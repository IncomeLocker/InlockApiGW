# IncomeApiGW - Income Locker frontend Application Programable Interface 
version: 0.1

API categories:<br>
<a name='usermgmt'>User management (registration, login, etc.)</a><br>
<a href='#collat-handler' name='collat-handler'>Collateral Inventory (preparation for contracting)</a>
 

# Create User's Account (#usermgmt)

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

# [](#collat-handler)Lock Collateral 

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

