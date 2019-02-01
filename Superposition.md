# API documentation for INLOCK:Superposition
version: 1.0

RESTful API for INLOCK (INLOCK.io) Superposition frontend. Default output is NON pretty printed JSON.

##API calls list<br>
**Public API requests for Superposition information:**
<a href='#privatelocksuperposition' name='lock-superposition'>Private:lockSuperposition</a>, 
<a href='#privateunlocksuperposition' name='unlock-superposition'>Private:unlockSuperposition</a>, 
<a href='#privategetmyspstat' name='get-my-sp-stats'>Private:getMySPStats</a>,
<a href='#privategetmyrunningspcontracts' name='get-my-running-sp-contracts'>Private:getMyRunningSpContracts</a>,
<a href='#privategetmyclosesSpcontracts' name='get-my-closed-sp-contracts'>Private:getMyClosedSpContracts</a>,
<a href='#privategetspserviceavailability' name='get-sp-service-availability'>Private:GetSPServiceAvailability</a>,
<a href='#privategetcontractevents' name='get-contract-events'>Private:getContractEvents</a>,
<a href='#privategetcontractdetails' name='get-contract-details'>Private:getContractDetails</a>,
<a href='#privategetcontractchart' name='get-contract-chart'>Private:getContractChart</a>,
<br>

## Authentication
Authenticated requests require the following HTTP headers:
* 'Authentication': 'token [access token]'

## HTTP Status codes
The general rule is the following:
* 200 as a response for successful requests.
* 201 as a response for create object.
* 422 in case of error.

Exceptions for the above defined rules are specified at the corresponding API description.

# [](#privatelocksuperposition)Private:lockSuperposition

Lock selected amount of crypto into a superposition contract

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/lockSuperposition`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "pair": "[int, identifier of tradingpair]",
    "orig_coin_id": "[int, identifier of locked crypto id]",
    "locked_coin_id": "[int, identifier of stable coin id]",
    "sp_coin_id": "[int, identifier of superpositioned coin id, only ILK available right now]",
    "amount": "[float, amount to sell from orig_coin_id]", 
    "ordertype": "c2s",
    "apr_payment": "locked-coin" OR "ilk"
}
```
**Limitations**: "ordertype" should be "c2s" only.  

**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  },
  "result": {
    "status": "ok", 
    "lockSuperposition": {
      "contract_id": 177
    }
  }
}

```

**Error results**<br>
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGP_missing_E001 / Missing tradingpair reference
* ETM_unsuppair_E001 / 
* ETM_unsupmrkt_E001 /
* ETM_notallwd_E001 /

# [](#privateunlocksuperposition)Private:unlockSuperposition

Unlock a Superposition contract 

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/unlockSuperposition`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "contract_id": "[int, identifier of a contract]",
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
    "status": "ok", 
    "unlockSuperposition": {
    }
  }
}
```

**Error results**<br>
EGEN_dbresult_E001 / General background database error. unexpected result<br>

# [](#privategetmyspstat)Private:getMySPStats

Get your own Superposition statistics

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/getMySPStats`

**Method** : `POST`

**Auth required** : YES

**Success result**

```json
{
  "result": {
    "status":"ok",
    "getMySPStats": {
      "general": {
        "bond_contract":0.006,
        "contract_apr":0.043,
        "sp_pot_percent":0.05
      },
      "customer": {
        "contracts": {
          "running":1,
          "closed":0,
          "sum_values": {
            "ILK":766.65196671,
            "USDC":0,"
            USDt":0,
            "PAX":0,
            "DAI":0
          }
        }
      }
    }
  },
  "error": {
    "code":"none",
    "message":"none"
  }
}
```

**Error results**<br>
EGEN_dbresult_E001 / General background database error. unexpected result<br>


# [](#privategetcontractdetails)Private:getContractEvents

Cancel an existing ILK token sell order

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/getContractEvents`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Provide the orderid to be requested.

```json
{
    "contract_id": "[int, identifier of a contract]",
    "page"    : "[int, pagination: num of page]",
    "perPage"    : "[int, pagination: items per page]",
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
    "status": "ok", 
    "getContractEvents": {
      "coin_id": "[int, id of affected coin]",
      "amount": "[float, amount to change]",
      "created": "[timestamp, when event happened]",
      "event": "[string, identifier of event]",
      "event_id": "[int, identifier of event]"
    }
  }
}
```
**Event values**: 'init', 'locked', 'partiallyunlocked',  'fullyunlocked', 'exhausted', 'canceled', 'declined', 'failed', 'unlocking', 'fee', 'apr'

# [](#privategetcontractdetails)Private:getContractDetails

Cancel an existing ILK token sell order

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/getContractDetails`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Provide the orderid to be requested.

```json
{
    "contract_id": "[int, identifier of a contract]",
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
    "status": "ok", 
    "getContractDetails": {
      "details": {
        "contract_id": 10,
        "request_id": 31210,
        "created": 1549028473,
        "closed": 1549039431, 
        "orig_coin_id":2,
        "orig_amt": 0.10000000,
        "locked_coin_id": 9,
        "locked_orig_amt": 10.60999999,
        "locked_remain_amt": 7.58014099,
        "sp_coin_id": 8,
        "sp_orig_amt": 1073.09050977,
        "sp_remain_amt": 766.65196671,
        "apr": 0.04300000,
        "apr_payment": "locked-coin",
        "status": "partiallyunlocked",
        "ordertype": "c2s"
      }    
    }
  }
}
```

# [](#privategetcontractchart)Private:getContractChart

Cancel an existing ILK token sell order

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/getContractChart`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Provide the orderid to be requested.

```json
{
    "contract_id": "[int, identifier of a contract]",
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
    "status": "ok", 
    "getContractDetails": {
       "details":{
       [...]
       "chart": {
         "IBI":
           [{"ts":1549026000,"p_usd":0.01063590},
            {"ts":1549047507,"p_usd":0.01063590},
            {"ts":1549051119,"p_usd":0.01037831},
            {"ts":1549054729,"p_usd":0.01069797},
            {"ts":1549058334,"p_usd":0.01094563}
           ],
        "origin":
           [{"ts":1549039431,"p_usd":10.60998821}
           ],
        "value":
           [{"ts":1549028473,"p_usd":10.60999999},
            {"ts":1549039431,"p_usd":10.60998821}
           ]}
       }
    }
  }
}
```
