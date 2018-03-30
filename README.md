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
