# IncomeApiGW - Income Locker frontend Application Programable Interface 
version: 0.1

RESTful API for Income Locker (inlock.io) frontend. Based on flask microframework. Default JSON output is 'pritty' printed, but you can switch to XML compressed output with "X-Requested-With: XMLHttpRequest" request header. Please use the compressed mode in normal cases!

API categories:<br>
<a href='#create-users-account' name='create-users-account'>User management (registration, login, etc.)</a><br>
<a href='#list-collaterals' name='list-collaterals'>Collateral Inventory (preparation for contracting)</a><br>
Request handling<br>
Offer handling (borrower side)<br>
Offer accepting (borrower side)<br>
Contracting<br>
Managing contracts<br>
Contract close with payback<br>
Contract close with termination<br>


# [](#create-users-account)Create User's Account

Create an Account for the authenticated User if an Account for that User does
not already exist. Each User can only have one Account.

**URL** : `https://api.inlock.io/inlock/api/v0.1/register`

**Method** : `POST`

**Auth required** : NO

**Permissions required** : None

**Data constraints**

Provide necessary information of Account to be created.

```json
{
    "username": "[unicode 64 chars max]",
    "email": "[standard email format]",
    "greetname": "[short greating name, 28 chars max]",
    "password": "[hashed password]",
    "kyctoken": "[preauthentication KYC token]"
}
```

## Success Response

**Condition** : If everything is OK and an Account didn't exist for this User.

**Code** : `201 CREATED`

**Content example**

```json
{
    "result": "ok"
}
```

## Error Responses

**Condition** : If Account already exists for User.

**Code** : `400 BAD REQUEST`

**Content** : 
```json
{
    "result": "user already exists"
}
```

### Or

**Condition** : If fields are missed.

**Code** : `400 BAD REQUEST`

**Content example**

```json
{
    "name": [
        "This field is required."
    ]
}
```

# [](#list-collaterals)List Collaterals 

List all existing locked collateras for authenticated user

**URL** : `https://api.inlock.io/inlock/api/v0.1/listCollaterals`

**Method** : `GET`

**Auth required** : YES

**Permissions required** : None

**Data constraints**

List all locked colleterals of authenticated user

## Success Response

**Condition** : If everything is OK and an Account didn't exist for this User.

**Code** : `200 OK`

**Content example**

```json
{
    "collaterals": [
        {
          "collat_id": 10, 
          "status": "prepared",
          "collat_mgmt_id": 0, 
          "created": "Fri, 30 Mar 2018 13:46:52 GMT", 
          "lastmodified": "Fri, 30 Mar 2018 13:46:52 GMT", 
          "margincall": 90.0,                                      ### margin call percentage(%)
          "cvalue_usd": 32059.8,                                   ### collateral's value in USD
          "c1_cointype": 2,                                        ### coin ID
          "c1_amount": 81.0                                        ### amount
        }, 
    ]
}
```


## Error Responses

**Condition** : There is no any collateral for requested user

**Code** : `400 BAD REQUEST`

**Content** : 
```json
{
    "lockCollateral": "*message*"
}
```

### Or

**Condition** : If fields are missed.

**Code** : `400 BAD REQUEST`

**Content example**

```json
{
    "name": [
        "This field is required."
    ]
}
```

# [](#lock-collateral)Lock Collateral 

Reserve collateral from user's balance and set the required parameters for further collateral management.

**URL** : `https://api.inlock.io/inlock/api/v0.1/lockCollateral`

**Method** : `POST`

**Auth required** : YES

**Permissions required** : None

**Data constraints**

Provide necessary information of Account to be created.

```json
{
    "coinid": "[int, based on /balance request's result]",
    "amount": "[float, amount of locked collateral]",
    "margincall": "[int, percentage of margin call after successful contracting]",
}
```

## Success Response

**Condition** : If everything is OK and an Account didn't exist for this User.

**Code** : `200 OK`

**Content example**

```json
{
    "result": "ok"
}
```

## Error Responses

**Condition** : Invalid coin referer or cannot mapped to requested user!<br>
**Condition** : Not enought available balance to lock collateral<br>
**Condition** : Internal error (invalid price information), please contact InLock support!

**Code** : `400 BAD REQUEST`

**Content** : 
```json
{
    "lockCollateral": "*message*"
}
```

### Or

**Condition** : If fields are missed.

**Code** : `400 BAD REQUEST`

**Content example**

```json
{
    "name": [
        "This field is required."
    ]
}
```

# [](#cancel-collateral)Cancel Collateral 

Cancel an existing and 'prepared' stated collateral. 

**URL** : `https://api.inlock.io/inlock/api/v0.1/cancelCollateral`

**Method** : `POST`

**Auth required** : YES

**Permissions required** : None

**Data constraints**

Provide necessary information of Account to be created.

```json
{
    "cid": "[int, collateral id]"
}
```

## Success Response

**Condition** : Existing collateral succesfully canceled. Pending balance also refunded and available for User

**Code** : `200 OK`

**Content example**

```json
{
    "result": "ok"
}
```

## Error Responses

**Condition** : There is no any collateral for requested user<br>
**Condition** : Cannot found balance of the collateral owner
**Condition** : Cannot cancel running or terminated collateral!<br>
**Condition** : Collateral is not found, or maybe invalid

**Code** : `400 BAD REQUEST`

**Content** : 
```json
{
    "cancelCollateral": "*message*"
}
```

### Or

**Condition** : If fields are missed.

**Code** : `400 BAD REQUEST`

**Content example**

```json
{
    "name": [
        "This field is required."
    ]
}
```
