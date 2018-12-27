# API documentation for INLOCK:TokenMarket
version: 1.0

RESTful API for INLOCK (INLOCK.io) TokenMarket frontend. Default output is pretty printed JSON.

##API calls list<br>
**Public API requests for TokenMarket information:**
<a href='#privategettokenmarket' name='get-token-market'>Private:getTokenMarket</a>, 
<a href='#privateselltoken' name='sell-token'>Private:sellToken</a>, 
<a href='#privatebuytoken' name='buy-token'>Private:buyToken</a>,
<a href='#privatecancelselltoken' name='cancel-sell-token'>Private:cancelsellToken</a>,
<a href='#privatelistmytokenmarketorders' name='list-my-tokenmarket-orders'>Private:listMyTokenmarketOrders</a>,
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

# [](#publicgettokenmarket)Private:getTokenMarket

Get orderbook of INLOCK TokenMarket for a selected trading pair.

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/public/getTokenMarket`

**Method** : `GET`

**Auth required** : NO

**Data constraints**

```json
{
    "pair": "[int, identifier of tradingpair]",
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
    "getTokenMarket": {
      "orderbook": {
        "marketid": 50, 
        "market": "ETHILK", 
        "sell": [
          {
            "timestamp": "2018-12-26 23:43:18", 
            "amount": 100.00000000, 
            "price": 0.00310000
          }, 
          {
            "timestamp": "2018-12-26 23:43:36", 
            "amount": 72.15000000, 
            "price": 0.00335000
          }
        ]
      }
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

# [](#privateselltoken)Private:sellToken

List a sell order of selected amount of ILK tokens on specified price level

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/sellToken`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "pair": "[int, identifier of tradingpair]",
    "amount": "[decimal with 8 precision, amount of ILK tokens want to sell]",
    "limit": "[decimal with 8 precision, price limit]",
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
    "sellToken": {
      "orderid": 11
    }
  }
}
```

**Error results**<br>
EGEN_dbresult_E001 / General background database error. unexpected result<br>

# [](#privatebuytoken)Private:buyToken

Buy specified amount of ILK tokens from TokenMarket.

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/buyToken`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

```json
{
    "pair": "[int, identifier of tradingpair]",
    "amount": "[decimal with 8 precision, amount of ILK tokens want to buy]",
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
    "buyToken": {
      "bought": 19.23000000, 
      "spent": 0.05961300
    }
  }
}
```

**Error results**<br>
EGEN_dbresult_E001 / General background database error. unexpected result<br>


# [](#privatecancelselltoken)Private:cancelsellToken

Cancel an existing ILK token sell order

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/cancelsellToken`

**Method** : `POST`

**Auth required** : YES

**Data constraints**

Provide the trading pair and interval to be requested.

```json
{
    "orderid"    : "[int, ID of sell order]",
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
    "cancelsellToken": {
      "orderid": 11, 
      "reason": "canceled", 
      "remaining_amount": 72.15000000
    }
  }
}
```

**Error results**<br>
EGEN_badreq_E001 / Bad request or missing field, please check the API documentation<br>

# [](#privatelistmytokenmarketorders)Private:listMyTokenmarketOrders

Cancel an existing ILK token sell order

**URL** : `https://prod.inlock.io:2096/inlock/api/v1.0/private/listMyTokenmarketOrders`

**Method** : `GET`

**Auth required** : YES

**Data constraints**

Provide the trading pair and interval to be requested.

```json
{
    "pair"    : "[int, identifier of tradingpair]",
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
    "listMyTokenmarketOrders": {
      "my_orders": {
        "marketid": 50, 
        "market": "ETHILK", 
        "sell": [
          {
            "timestamp": "2018-12-26 23:43:18", 
            "ttl_minutes": 405, 
            "remaining_amount": 80.77000000, 
            "orig_amount": 100.00000000, 
            "price": 0.00035000, 
            "status": "partiallysold"
          }
        ]
      }
    }
  }
}
```

**Error results**<br>
EGEN_badreq_E001 / Bad request or missing field, please check the API documentation<br>
