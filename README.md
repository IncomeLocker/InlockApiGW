# IncomeApiGW - Income Locker frontend Application Programable Interface 
version: 0.1

# Create User's Account

Create an Account for the authenticated User if an Account for that User does
not already exist. Each User can only have one Account.

**URL** : `https://api.inlock.io/inlock/api/v0.1/register`

**Method** : `POST`

**Auth required** : NO

**Permissions required** : None

**Data constraints**

Provide name of Account to be created.

```json
{
    "username": "[unicode 64 chars max]"
    "email": "[unicode 64 chars max]"
    "greetname": "[unicode 64 chars max]"
    "password": "[unicode 64 chars max]"
    "kyctoken": "[unicode 64 chars max]"
}
```
