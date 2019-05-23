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
    "apr": "[integer, value of annual percent rate]",
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
    "newlend": {
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

Set reinvestment rate 

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
          "apr": "[integer, value of annual percent rate]", 
          "expiration": "[datetime, exipration date of lend]", 
          "current_amt": "[integer, current amount]", 
          "orig_amt": "[integer, original amount]", 
          "allocated_amt": "[integer, allocated amount]", 
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
          "apr": "[integer, value of annual percent rate]", 
          "expiration": "[datetime, exipration date of lend]", 
          "current_amt": "[integer, current amount]", 
          "orig_amt": "[integer, original amount]", 
          "allocated_amt": "[integer, allocated amount]", 
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


# Private:withdrawFromLend

Withdraw from lending position

**URL** : `https://inlock.io/inlock/api/v0.1/private/withdrawFromLend`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Lend id and amount

```json
{
    "lend_id": "[integer, id of lending position]",
    "amount": "[integer, amount of withdrawal from lending position]"
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
      "withdrawed_amt": "[integer, amount withdrawn form lending position]", 
      "remaining_amt": "[integer, remaining amount in lending position]",
      "release_due_date": "[datetime, release due date]"
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

Set a lending notification for the user

**URL** : `https://inlock.io/inlock/api/v0.1/private/setLendingNotifications`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Provide necessary information of lending to be requested.

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

Get user lending notification

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

Get user lending notification

**URL** : `https://inlock.io/inlock/api/v0.1/private/getLendingContractEvents`

**Method** : `GET`

**Auth required** : YES

**Data constraints**

Provide necessary information of lending to be requested.

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
          "created": "[datetime, creation date of event]",
          "type": "[string, event type]",
          "id": "[integer, event id]",
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
        "apr": "[integer, value of annual percent rate]",
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
      "amt": "[integer, amount of lending position]", 
      "coin_id": "[integer, id of currency]",
      "reinvest_interest": "[integer, 1 if lending is reinvestable, 0 if lending is not reinvestable]"
    }
```

if event `type` is `withdraw_req` or `withdraw`
```json
    {
      "amt": "[integer, amount of lending position]", 
      "coin_id": "[integer, id of currency]"
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
    "lend_id": "[integer, id of requested lending position]", 
    "apr": "[integer, value of annual percentage rate]", 
    "expiration": "[datetime, end of lending contractible period]",
    "current_amt": "[integer, current amount of the lending]", 
    "orig_amt": "[integer, original amount of lending position]",
    "alloc_amt": "[integer, allocated amount of lending position]", 
    "coin_id": "[integer, id of the currency]",
    "duration": "[integer, value of lending contractible period in days]",
    "creation": "[datetime, creation date of lending position]", 
    "interest_income": "[integer, ]",
    "reinvest_interest": "[integer, ]", 
    "pending_withdraw": "[integer, ]",
    "withdraw_due_date": "[datetime, ]", 
    "chart": {
      "amt": [
        {
          "t": "[integer, unix time]", 
          "v": "[integer, ]"
        }
      ], 
      "int": [
        {
          "t": "[integer, unix time]", 
          "v": "[integer, ]"
        }
      ], 
      "pend_int": [
        {
          "t": "[integer, unix time]", 
          "v": "[integer, ]"
        }
      ], 
      "alloc": [
        {
          "t": "[integer, unix time]", 
          "v": "[integer, ]"
        }
      ],
  }
}
```


**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Backend's response is invalid, please contact support
* ELEND_unknown / Unknown lend ID
