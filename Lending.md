# API documentation for INLOCK:Lending
version: 1.0

RESTful API for INLOCK (INLOCK.io) Lending frontend. Default output is NON pretty printed JSON.

##API calls list<br>
**Functions:**   
    [Private:newLend](#privatenewlend)  
    [Private:setReinvestInterest](#privatesetreinvestinterest)  
    [Private:getRunningLends](#privategetrunninglends)  
    [Private:getClosedLends](#privategetclosedlends)  
    [Private:withdrawFromLend](#privatewithdrawfromlend)  
    [Private:setLendingNotifications](#privatesetlendingnotifications)  
    [Private:getLendingNotifications](#privategetlendingnotifications)  
    [Private:getLendingContractEvents](#privategetlendingcontractevents)  
    [Private:getLendDetails](#privategetlenddetails) 
 

# Private:newLend

Create a new lending position.

**URL** : `https://inlock.io/inlock/api/v0.1/private/newLend`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Provide necessary information of lending to be requested.

```json
{
    "coin_id": "[integer, id of the currency]",
    "amt": "[float, amount of lending position]",
    "apr": "[float, value of annual percent rate]",
    "duration": "[integer, value of lending contractible period in days]",
    "reinvest_interest": "[integer, 1 if lending is reinvestable, 0 if lending is not reinvestable]"
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
    "newLend": {
      "lend_id": "[integer, id of newly created lending position]"
    }, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* ELEND_invcoin / Lending is not possible with this coin
* ELEND_invamount / Lending amount is too low
* ELEND_invapr / APR is invalid
* ELEND_invduration / Duration is invalid
* ELEND_invreinv / Reinvest interest is invalid
* ELEND_nobalance / User balance is too low


# Private:setReinvestInterest

Set reinvestment

**URL** : `https://inlock.io/inlock/api/v0.1/private/setReinvestInterest`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Provide necessary information of lending to be requested.

```json
{
    "lend_id": "[integer, id of lending position]",
    "reinvest": "[integer, 1 if lending is reinvestable, 0 if lending is not reinvestable]"
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
    "setReinvestInterest": {},
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* ELEND_invreinv / Reinvest interest is invalid
* ELEND_unknown / Unknown lend ID
* ELEND_expired / Lending is expired


# Private:getRunningLends

Get running lends 

**URL** : `https://inlock.io/inlock/api/v0.1/private/getRunningLends`

**Method** : `GET`

**Auth required** : YES

**Data constraints**

```
    /private/getRunningLends?page={page_num}&perPage={per_page}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": { 
    "getRunningLends": {
      "pagination": {
        "page": "[integer, current page]", 
        "perPage": "[integer, item per page]", 
        "total": "[integer, total number of pages]"
      }, 
    "lends": [
        {
          "lend_id": "[integer, id of lending position]", 
          "apr": "[float, value of annual percent rate]", 
          "expiration": "[string, exipration date of lend]", 
          "current_amt": "[float, current amount]", 
          "orig_amt": "[float, original amount]", 
          "allocated_amt": "[float, allocated amount]", 
          "coin_id": "[integer, id of currency]"
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


# Private:getClosedLends

Get closed lends

**URL** : `https://inlock.io/inlock/api/v0.1/private/getClosedLends`

**Method** : `GET`

**Auth required** : YES

**Data constraints**

```
    /private/getClosedLends?page={page_num}&perPage={per_page}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": { 
    "getRunningLends": {
      "pagination": {
        "page": "[integer, current page]", 
        "perPage": "[integer, item per page]", 
        "total": "[integer, total number of pages]"
      }, 
    "lends": [
        {
          "lend_id": "[integer, lending position id]", 
          "apr": "[float, value of annual percent rate]", 
          "expiration": "[string, exipration date of lend]", 
          "current_amt": "[float, current amount]", 
          "orig_amt": "[float, original amount]", 
          "allocated_amt": "[float, allocated amount, if closed this value always 0]", 
          "coin_id": "[integer, coin id]"
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


# Private:withdrawFromLend

Withdraw from lending position

**URL** : `https://inlock.io/inlock/api/v0.1/private/withdrawFromLend`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "lend_id": "[integer, id of lending position]",
    "amount": "[float, amount of withdrawal from lending position]"
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
    "withdrawFromLend": {
      "withdrawed_amt": "[float, amount withdrawn form lending position]", 
      "remaining_amt": "[float, remaining amount in lending position]",
      "release_due_date": "[string, release due date]"
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* ELEND_notavail / Reinvest interest is invalid
* ELEND_unknown / Unknown lend ID
* ELEND_expired / Lending is expired
* ELEND_withdraw_001 / Amount is too low
* ELEND_withdraw_002 / Remaining amount is too low


# Private:setLendingNotifications

Set lending notification for the user

**URL** : `https://inlock.io/inlock/api/v0.1/private/setLendingNotifications`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "approve": "[integer, 1 if notification requested, 0 if notification is not requested]"
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
    "setLendingNotifications": {},
    "status": "ok"
  }
}
```


**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support


# Private:getLendingNotifications

Get user lending notification state

**URL** : `https://inlock.io/inlock/api/v0.1/private/getLendingNotifications`

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
    "getLendingNotifications": {
      "approve": "[integer, 1 if notification requested, 0 if notification is not requested]"
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support


# Private:getLendingContractEvents

Get lending events

**URL** : `https://inlock.io/inlock/api/v0.1/private/getLendingContractEvents`

**Method** : `GET`

**Auth required** : YES

**Data constraints**

```
    /private/getLendingContractEvents?lend_id={lend_id}&page={page_num}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": { 
    "getLendingContractEvents": {
      "pagination": {
        "page": "[integer, number of current page]", 
        "perPage": "[integer, number of item per page]", 
        "total": "[integer, total number of pages]"
      }, 
    "events": [
        {
          "id": "[integer, event id]",
          "type": "[string, event type]",
          "created": "[string, creation date of event]",
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
        "coin_id": "[integer, id of currency]",
        "amt": "[float, amount of lending position]",
        "apr": "[float, value of annual percent rate]",
        "duration": "[integer, value of lending contractible period in days]",
        "reinvest_interest": "[integer, 1 if lending is reinvestable, 0 if lending is not reinvestable]"
    }
```

if event `type` is `expired` or `terminated`
```json
    {}
```

if event `type` is `interest_income`
```json
    {
      "amt": "[float, amount of lending position]", 
      "coin_id": "[integer, id of currency]",
      "reinvest_interest": "[integer, 1 if lending is reinvestable, 0 if lending is not reinvestable]"
    }
```

if event `type` is `withdraw_req` or `withdraw`
```json
    {
      "amt": "[float, amount of lending position]", 
      "coin_id": "[integer, coin id]"
    }
```

if event `type` is `reinv_interest_on` or `reinv_interest_off`
```json
    {
      "reinvest_interest": "[integer, 1 if lending is reinvestable, 0 if lending is not reinvestable]"
    }
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* ELEND_unknown / Unknown lend ID


# Private:getLendDetails

**URL** : `https://inlock.io/inlock/api/v0.1/private/getLendDetails`

**Method** : `GET`

**Auth required** : YES

**Data constraints**

Provide necessary information of lending to be requested.

```
    /private/getLendDetails?lend_id={lend_id}
```

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": { 
    "getLendDetails": {
      "lend_id": "[integer, id of requested lending position]", 
      "apr": "[float, value of annual percentage rate]", 
      "expiration": "[string, end of lending contractible period]",
      "current_amt": "[float, current amount of the lending]", 
      "orig_amt": "[float, original amount of lending position]",
      "alloc_amt": "[float, allocated amount of lending position]", 
      "coin_id": "[integer, id of the currency]",
      "duration": "[integer, value of lending contractible period in days]",
      "creation": "[string, creation date of lending position]", 
      "interest_income": "[float, ]",
      "reinvest_interest": "[integer, 1 if lending is reinvestable, 0 if lending is not reinvestable]", 
      "pending_withdraw": "[float, ]",
      "withdraw_due_date": "[string, ]", 
      "chart": {
        "amt": [
          {
            "t": "[integer, unix timestamp]", 
            "v": "[float, ]"
          }
        ], 
        "int": [
          {
            "t": "[integer, unix timestamp]", 
            "v": "[float, ]"
          }
        ], 
        "pend_int": [
          {
            "t": "[integer, unix timestamp]", 
            "v": "[float, ]"
          }
        ], 
        "alloc": [
          {
            "t": "[integer, unix timestamp]", 
            "v": "[float, ]"
          }
        ]
      }
    },
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* ELEND_unknown / Unknown lend ID
