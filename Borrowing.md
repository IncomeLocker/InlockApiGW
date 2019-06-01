# API documentation for INLOCK:Borrowing/Loans
version: 1.0

RESTful API for INLOCK (INLOCK.io) borrowing frontend. Default output is NON pretty printed JSON.

##API calls list<br>
**Functions:**   
    [Public:getAvailableCollateralCoins](#publicgetavailablecollateralcoins) 
    [Private:getLoanReqRateLimit](#privategetloadnreqratelimit)  
    [Private:requestNewLoanOffers](#privaterequestnewloanoffers)  
    [Private:getLoanOfferRequest](#privategetloadnofferrequest)  
    [Private:requestPriorityLoanOffer](#privaterequestpriorityloanoffer)  
    [Private:rejectLoanOffers](#privaterejectloanoffers)  
    [Private:acceptLoanOffer](#privateacceptloanoffer)  
    [Private:getRunningLoans](#privategetrunningloans)  
    [Private:getClosedLoans](#privategetclosedloans)  
    [Private:getLoanDetails](#privategetloandetails) 
    [Private:repayLoan](#privaterepayloan)  
    [Private:getLoanExtendOffers](#orivategetloanextendoffers)  
    [Private:terminateLoan](#privateterminateloan)    
    [Private:getLoanContractEvents](#privategetloancontractevents)  
    
    
# Public:getAvailableCollateralCoins

Get available collateral coins

**URL** : `https://inlock.io/inlock/api/v0.1/public/getAvailableCollateralCoins`

**Method** : `GET`

**Auth required** : NO

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "getAvailableCollateralCoins": {
      "coins": [
        "[integer, coin id]",
        ...
      ]
    }, 
    "status": "ok"
  }
}
```


# Private:getLoanReqRateLimit

Getting the rate limit status of loan offer requests. 10 requests are allowed in an hour

**URL** : `https://inlock.io/inlock/api/v0.1/public/getLoanReqRateLimit`

**Method** : `GET`

**Auth required** : YES

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "getAvailableCollateralCoins": {
      "available_from": "[integer, 0 if a new request is allowed, otherwise the time in seconds]"
    }, 
    "status": "ok"
  }
}
```


# Private:requestNewLoanOffers

Getting new loan offers 

**URL** : `https://inlock.io/inlock/api/v0.1/public/requestNewLoanOffers`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "coin_id": "[integer, coin id, only 9 can be]",
    "amt": "[float, requested amount of loan, min value is 100]",
    "overcoll": "[integer, overcollateralization rate in percent, minimum 110]",
    "duration": "[integer, value of borrowing contractible period in days]",
    "collaterals": [
      "[integer, coin id, optional]",
      ...
    ]
}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "requestNewLoanOffers": {
      "request_id": "[integer, id of the request]"
    }, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support  
* EBRW_notavail / Borrowing is not available right now. Please check back later!  
* EBRW_invcoin / Borrowing is not possible with this coin  
* EBRW_invamount / Borrow amount is too low  
* EBRW_invovercoll / Overcollateralization rate is invalid  
* EBRW_invduration / Duration is invalid  
* EBRW_invcoll / Invalid collateral  
* EBRW_reqrate / Loan offer request rate limit exceeded  
* EBRW_nocoll / Collateral value is too low  


# Private:getLoanOfferRequest

Getting request loan offers

**URL** : `https://inlock.io/inlock/api/v0.1/public/getLoanOfferRequest`

**Method** : `GET`

**Auth required** : YES

**Data constraints**
```
    /private/getLoanOfferRequest?request_id={request_id}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "getLoanOfferRequest": {
      "request_id": "[integer, request id]",
      "status": "[pending | completed, offer creation status ]",
      "validity": "[integer, offer validity in seconds]",
      "offers": [
        {
          "offer_id": "[integer, offer id]", 
          "loan_amt": "[float, offered loan amount]",
          "apr": "[float, annual percent rate of the offered loan in percent]",
          "duration": "[integer, duration of the offer in days]", 
          "cost_ilk": "[float, contract fee in ILK]", 
          "cost_usdc": "[float, contract fee in usdc, 0 if user has enough ILK]",
          "overcoll": "[float, overcollateralization rate of the offer]",
        },
        ...
      ],
      "prio_req": {
        "cost_ilk": "[float, cost of priority request]", 
        "cost_coin_id": "[integer, coin id]"
      }
    }, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  
* EBRW_requnknown / Unknown offer request ID  


# Private:requestPriorityLoanOffer



**URL** : `https://inlock.io/inlock/api/v0.1/public/getLoanOfferRequest`

**Method** : `POST`

**Auth required** : YES

**Data constraints**
```json
{
  "request_id": "[integer, request id]",
  "cost_coin_id": "[integer, value of the previous endpoint prio_req.cost_coin_id]"
}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "requestPriorityLoanOffer": {
      "request_id": "[integer, request id]",
      "cost_ilk": "[float, cost of priority request]", 
      "cost_coin_id": "[integer, coin id]"
      "cost": "[float, loan contract fee]",
      "overcoll": "[float, overcollateralization rate in percent]",
      "amt": "[float, loan amount]",
      "duration": "[integer, duration of the offer in days]",
      "req_date": "[datetime, date of request]",
      "valid_until": "[integer, days]",
      "collaterals": [ 
        {
          "coin_id": "[integer, cost of priority request]", 
          "amt": "[float, collateralized amount]",
          "p_usdc": "[float, collateralized amount in usdc]"
        },
        ...
      ]
    }, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* EBRW_notavail / Borrowing is not available right now. Please check back later!
* EBRW_requnknown / Unknown offer request ID
* EBRW_notoffer / Not an active loan offer
* EBRW_tmbuy_err / Unable to buy ILK on tokenmarket
* EBRW_prio_nobalance / Not enough balance to pay priority offer.


# Private:rejectLoanOffers

Rejecting all loan offers

**URL** : `https://inlock.io/inlock/api/v0.1/public/rejectLoanOffers`

**Method** : `POST`

**Auth required** : YES

**Data constraints**
```json
{
  "request_id": "[integer, request id]"
}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "rejectLoanOffers": {}, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  
* EBRW_notoffer / Not an active loan offer
* EBRW_requnknown / Unknown offer request ID


# Private:acceptLoanOffer

Accepting loan offer

**URL** : `https://inlock.io/inlock/api/v0.1/public/acceptLoanOffer`

**Method** : `POST`

**Auth required** : YES

**Data constraints**
```json
{
  "offer_id": "[integer, offer id]"
}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "acceptLoanOffer": {
      "cost": "[float, loan contract fee]",
      "requested_overcoll": "[float, in percent]",
      "curr_overcoll": "[float, current overcollateralization rate in percent]",
      "loan_amt": "[float, amount of loan]",
      "amt_to_repay": "[float, amount to repay]",
      "due_date": "[string, end date of loan contract]",
      "start_date": "[datetime, loan contract starting date]",
      "apr": "[float, annual percent rate]",
      "duration": "[integer, duration of the offer in days]",
      "collaterals": [
        {
          "coin_id": "[integer, collateralized coin id]",
          "amt": "[float, collateralized amount]",
          "p_usdc": "[float, collateralized amount in usdc ]",
        },
        ...
      ]
    }, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* EBRW_notavail / Borrowing is not available right now. Please check back later!
* EBRW_offunknown / Unknown offer ID
* EBRW_notoffer / Not an active loan offer
* EBRW_requnknown / Unknown offer request ID


# Private:getRunningLoans

Get running loans

**URL** : `https://inlock.io/inlock/api/v0.1/public/getRunningLoans`

**Method** : `GET`

**Auth required** : YES

**Data constraints**
```
    /private/getRunningLoans?page={page}&perPage={per_page}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "getRunningLoans": {
      "pagination": {
        "page": "[integer, number of current page]", 
        "perPage": "[integer, number of item per page]", 
        "total": "[integer, total number of pages]",
        "loans": [
          {
            "loan_id": "[integer, loan contract id]",
            "curr_overcoll": "[float, current overcollateralization rate]",
            "curr_debt": "[float, current debt]",
            "due_date": "[string, end date of loan contract]",
            "status": "[datetime, current status ]",
            "orig_capital": "[float, original capital]"
          },
          ...
        ]
      }
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  


# Private:getClosedLoans

Get closed loans

**URL** : `https://inlock.io/inlock/api/v0.1/public/getClosedLoans`

**Method** : `GET`

**Auth required** : YES

**Data constraints**
```
    /private/getClosedLoans?page={page}&perPage={per_page}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "getRunningLoans": {
      "pagination": {
        "page": "[integer, number of current page]", 
        "perPage": "[integer, number of item per page]", 
        "total": "[integer, total number of pages]",
        "loans": [
          {
            "loan_id": "[integer, loan id]",
            "curr_overcoll": "[float, current overcollateralization rate]",
            "curr_debt": "[float, current debt]",
            "due_date": "[string, end date of loan contract]",
            "status": "[string, status]",
            "orig_capital": "[float, original capital]"
          },
          ...
        ]
      }
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  


# Private:Private:getLoanDetails

Get running loans

**URL** : `https://inlock.io/inlock/api/v0.1/public/Private:getLoanDetails`

**Method** : `GET`

**Auth required** : YES

**Data constraints**
```
    /private/Private:getLoanDetails?loan_id={loan_id}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "getLoanDetails": {
      "loan_id": "[integer, loan id]",
      "coin_id": "[integer, coin id]",
      "req_id": "[integer, request id]",
      "curr_overcoll": "[float, current overcollateralization rate]",
      "curr_capital": "[float, current]",
      "curr_interest": "[float, current interest rate]",
      "due_date": "[string, end date of loan contract]",
      "orig_overcoll": "[float, original overcollateralization rate]",
      "start_date": "[string, starting date of the loan contract]",
      "apr": "[float, rate of annual percent rate]",
      "duration": "[integer, duration of the offer in days]",
      "original_loan_amt": "[float, original amount of loan]",
      "status": "[string, loan contract status]",
      "collaterals": [
        {
          "coin_id": "[integer, coin id]",
          "amt": "[float, collateralized amount]",
          "p_usdc": "[float, price in usdc (not implemented yet]",
          "limit_price": "[float, ]"
        },
        ...
      ],
      "chart": {
        "overcoll": [
          {
            "ts": "[integer, ]",
            "val": "[float, ]"
          },
          ...
        ],
      },
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  
* EBRW_unknown / Unknown loan ID


# Private:repayLoan

Get running loans

**URL** : `https://inlock.io/inlock/api/v0.1/public/repayLoan`

**Method** : `POST`

**Auth required** : YES

**Data constraints**
```json
{
  "loan_id": "[integer, loan id]",
  "amt": "[float, ]"
}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "repayLoan": {
      "repaid_amt": "[float, repaid amount of loan]",
      "remaining_amt": "[float, remaining amount of loan]"
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation  
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* EBRW_unknown / Unknown loan ID
* EBRW_repay_001 / Invalid repay amount, at least 100 USDC is required!
* EBRW_repay_002 / Not enough money to repay
* EBRW_repay_003 / Loan is currently unavailable, try again later.
* EBRW_notrunning / Not a running loan
* EBRW_repaying / Repayment is being processed, please try again later.
* EBRW_requnknown / Unknown offer request ID


# Private:getLoanExtendOffers

Extension of loan term

**URL** : `https://inlock.io/inlock/api/v0.1/public/getLoanExtendOffers`

**Method** : `POST`

**Auth required** : YES

**Data constraints**
```json
{
  "loan_id": "[integer, loan id]",
  "new_duration": "[integer, new extended duration of a loan]"
}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "getLoanExtendOffers": {
      "request_id": "[integer, request id]",
      "cost": "[float, cost of extension, always ILK]"
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* EBRW_notavail / Borrowing is not available right now. Please check back later!



# Private:terminateLoan

Terminate loan

**URL** : `https://inlock.io/inlock/api/v0.1/public/terminateLoan`

**Method** : `POST`

**Auth required** : YES

**Data constraints**
```json
{
  "loan_id": "[integer, loan id]"
}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": {
    "terminateLoan": {}, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* EBRW_unknown / Unknown loan ID
* EBRW_notrunning / Not a running loan


# Private:getLoanContractEvents

Get loan contract events

**URL** : `https://inlock.io/inlock/api/v0.1/private/getLoanContractEvents`

**Method** : `GET`

**Auth required** : YES

**Data constraints**

Provide necessary information of lending to be requested.

```
    /private/getLoanContractEvents?loan_id={loan_id}&page={page_num}&perPage={per_page}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": { 
    "getLoanContractEvents": {
      "pagination": {
        "page": "[integer, number of current page]", 
        "perPage": "[integer, number of item per page]", 
        "total": "[integer, total number of pages]"
      }, 
    "events": [
        {
          "id": "[integer, event id]",
          "type": "[string, event type]",
          "created": "[string, date of event creation]",
          "params": "{**}"
        },
        ...
      ]
    },
    "status": "ok"
  }
}
```

{**} Param value is  
if event `type` is `created`
```json
    {
        "amt": "[float, amount of loan]",
        "coin_id": "[integer, id of currency]",
        "apr": "[float, value of annual percent rate]",
        "duration": "[integer, value of lending contractible period in days]",
        "overcoll": "[float, overcollateralization rate of the offer]"
    }
```

if event `type` is `repay`
```json
    {
        "amt": "[float, repayed amount]",
        "coin_id": "[integer, coin id]",
        "interest_repay_amt": "[float, interest repay amount]",
        "capital_repay_amt": "[float, capital repay amount]",
        "remaining_amt": "[float, remaining amount]"
    }
```

if event `type` is `closed` or `accepted` or `rejected` or `margin_called`
```json
    {}
```

if event `type` is `creation_fee` or `extend_req_fee` or `add_coll_fee` or `withdraw_coll_fee` or `terminated` or `coll_terminated` or `missed_due_date`
```json
    {
      "amt": "[float, fee amount]", 
      "coin_id": "[integer, coin id]"
    }
```

if event `type` is `coll_soft_lock` or `coll_soft_unlock`
```json
    {
      "amt": "[float, collateral amount]", 
      "coin_id": "[integer, id of currency]",
      "full_amt": "[float, ]"
    }
```

if event `type` is `coll_hard_lock` or `coll_hard_unlock`
```json
    {
      "amt": "[float, collateral amount]", 
      "coin_id": "[integer, coin id]",
      "tx_id": "[integer, transaction id]",
      "wallet_address": "[string, wallet address]"
    }
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
