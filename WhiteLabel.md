# API documentation for INLOCK:White-Label service
version: 0.1

RESTful API for INLOCK (INLOCK.io) white-label integration. Default output is NON pretty printed JSON.

## API calls list for publicly available calculator requests
**Functions:**   
    [Public:getPubCustIndLoanOffers](#publicgetpubcustindloanoffers)  
    [Public:getAutoLendApr](#publicgetautolendapr)  

## API calls list to submit financial request via white-label service
**Functions:**   
(soon) 


# Public:getPubCustIndLoanOffers

Calculate an indicative loan offer

**URL** : `https://api.inlock.io/inlock/api/v1.0/public/getPubCustIndLoanOffers`

**Method** : `GET`

**Auth required** : NO

**Data constraints (HTML get parameters)**

```
    "coin_id": "[integer, id of currency]"
    "amt": "[float, requested amount of loan]"
    "dur": "[int, days until repay]"
    "coll": "[int, coin_id of collateral]"
    "oc": [int, overcollaterization rate in percent format]"
```

Available values of different parameteres:

    coin_id: should be 9 (9 = USDC)
    oc: should be at least 110 (110% overcollaterization)
    dur: following values only supported: 10, 30, 45, 60, 120, 180
    coll:
        0: Bitcoin (BTC, XBT)
        2: Ethereum (ETH)
        4: Litecoin (LTC)
        12: Binance Coin (BNB)


**Success result (json) (HTTP/200)**

```json
{
    "result": {
        "status": "ok",
        "getPubCustIndLoanOffers": {
            "loan_amt": "[float, capital part of debt]",
            "int_debt": "[float, interest part of debt]",
            "apr": "[float, interest rate (%/100)]",
            "coll_id": "[int, collateral coin_id]"  ,
            "coll_amt": "[float, amount collateral to get the loan",
            "cost_ilk": "[float, cost option of the contract in ILK]",
            "cost_usd": "[float, cost option of the contract in USDC]"
        }
    },
    "error": {
        "code": "none",
        "message": "none"
    }
}
```
**Rate limit (HTTP/429)**

This API request is rate limited. A customer cannot request more then 10 calculation per 10 seconds. If so, HTTP 429 error will be generated until 1 minute time period for new requests

**Error results (HTTP/422)**
* EGEN_badreq_E001 / Bad request or missing field, please check the API documentation
* EGEN_dbresp_E001 / Internal error occured, please contact product support
* EBRW_notavail / Borrow service is unavailable
* EBRW_invamount / Invalid loan amount (<100USDC)
* EBRW_invovercoll / Invalid overcollaterization rate (<110%)
* EBRW_invduration / Invalid duration
* EBRW_invcoll / Invalid collateral coin id

# Public:getAutoLendApr

Calculate fix yield/interest of a USDC managed lending. 

**URL** : `https://api.inlock.io/inlock/api/v1.0/public/getAutoLendApr`

**Method** : `GET`

**Auth required** : No

**Success result**

```json
{
    "result": {
        "status": "ok",
        "getAutoLendApr": {
            "apr_usdc": "[float, apr% in USDC]",
            "apr_ilk": "[float, apr% in ILK token]",
            "apy_usdc": "[float, apy% in USDC]",
            "apy_ilk": "float, apy% in ILK token"
        }
    }
}
```

Definitions:
* APR: Annual Percentage Rate. INLOCK pays interest on every weeks. 
* APY: Compounded interest rate if customer keep in lending position for a complete year.

