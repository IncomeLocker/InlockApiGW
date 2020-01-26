# API documentation for INLOCK:Staking (Staker) 
version: 1.0

RESTful API for INLOCK (INLOCK.io) staker frontend. Default output is NON pretty printed JSON.

## API calls list for Staker<br>
**Functions:**   
    [Public:getAvailableStakeOptions](#publicgetAvailableStakeOptions)  
    [Private:registerStake](#privateregisterStake)  
    [Private:listMyRunningStakes](#privatelistMyRunningStakes)  
    [Private:listMyClosedStakes](#privatelistMyClosedStakes)  
    [Private:cancelStake](#privatecancelStake)  
    [Private:modifyStakeRenewal](#privatemodifyStakeRenewal)  


# Public:getAvailableStakeOptions

Get all available stake options

**URL** : `https://api.inlock.io/inlock/api/v0.1/public/getAvailableStakeOptions

**Method** : `GET`

**Auth required** : NO

**Success result**

```json
{
  ...
  "result": {
    "getAvailableStakeOptions": {
      "prefered": "ilk",
      "available": {
        "ilk": {
          "visible":true,
          "active":true,
          "options":[
            {
              "amt": "[float, requested amount]",
              "coin_id": "[integer, id of currency]",
              "stake_reward": "[float, annual percentage yield]",
              "fee": "[float, contract fee in ILK]",
              "autolend_bonus": "[float, additional bonus]",
              ...
            },
            ...
          ]
        },
        ...
      }
    }, 
    "status": "ok"
  }
}
```


# Private:registerStake

Register a new Stake request

**URL** : `https://api.inlock.io/inlock/api/v0.1/private/registerStake

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "coin_id": "[integer, id of currency]",
    "amt": "[float, requested amount of loan]",
    "duration": "[int, days to lock stake]",
    "renewable": "[int, should be: 0 (maturing) or 1 (renewable)]"
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
    "registerStake": {
      "request_id": "[integer]"
    }, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Internal error occured, please contact product support

# Private:cancelStake

Cancel a Stake preliminary

**URL** : `https://api.inlock.io/inlock/api/v0.1/private/cancelStake

**Method** : `DELETE`

**Auth required** : YES

**Data constraints**

```json
{
    "request_id": "[integer, request id of a stake]",
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
    "cancelStake": {}, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Internal error occured, please contact product support

# Private:modifyStakeRenewal

Set or reset Stake auto-renewal state

**URL** : `https://api.inlock.io/inlock/api/v0.1/private/modifyStakeRenewal

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "request_id": "[integer, request id of a stake]",
    "renewable": "[int, should be: 0 (maturing) or 1 (renewable)]"
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
    "modifyStakeRenewal": {}, 
    "status": "ok"
  }
}
```

**Error results**  
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Internal error occured, please contact product support
* ESTK_notfnd_E001 / Requested stake doesn't found or not belonging to requested user

# Private:listMyRunningStakes
# Private:listMyClosedStakes

List my **running or closed** stake contracts with mandatory pagination 
**URL** : `https://api.inlock.io/inlock/api/v0.1/private/listMyRunningStakes

**Method** : `GET`

**Auth required** : YES

**Data constraints**

```
/private/listMy[RunningStakes/ClosedStakes]?page={page_num}&perPage={per_page}
```
**Success result**

```json
{
  "error": {
    "code": "none", 
    "message": "none"
  }, 
  "result": { 
    "listMy[RunningStakes/ClosedStakes]": {
      "pagination": {
        "page": "[integer, current page]", 
        "perPage": "[integer, item per page]", 
        "total": "[integer, total number of pages]"
        "stakes": [
          {
            "request_id": "[integer, id of stake contract]",
            "status": "[string, status of stake]",
            "created": "[string, create date of stake]",
            "coin_id": "[integer, id of currency]",
            "duration": "[int, days to lock stake]", 
            "expiration": "[string, exipration date of stake]",
            "amount": "[float, locked amount]",
            "apr": "[float, value of annual percent yield]",
            "renewable": "[int, value: 0 (maturing) or 1 (renewable)]"
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
