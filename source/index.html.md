---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='mailto:developers@paycargo.com'>Sign Up for Developer Credentials</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Paycargo REST API! You can use our API to create and void transactions, submit payments and get transaction details.

You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

We have 2 environments:

* Sandbox: https://apidev.paycargo.com
* Production: https://api.paycargo.com

This API supports 2 authentication types both using JWT token:

* User Authentication - perform actions on behalf of single user
* Developer Authentication - perform actions on behalf of multiple payer / vendor accounts

Most of the endpoints described below are accessible to both User and Developer (exceptions are marked)

# User Authentication

> To authenticate and get JWT token for further requests, use the following:

```shell
curl --request POST \
  --url 'https://apidev.paycargo.com/login' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data 'username={{username}}&password={{password}}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/login",
  "method": "POST",
  "headers": {
    "Content-Type": "application/x-www-form-urlencoded"
  },
  "data": {
    "username": "{{username}}",
    "password": "{{password}}"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON with the Authentication token

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6Nzc4MTQwLCJhY2NvdW50VHlwZSI6InNoaXBwZXIiLCJpYXQiOjE1MTcwOTUyODAsImV4cCI6MTUxNzcwMDA4MH0.HUeTs-Lb6AQsZjt_KEnjEljAXsXce0Y-zfQc5AI7oVc"
}
```
To authenticate as a user, you will need a properly registered and active Paycargo username and password.

The Paycargo API expects for the JWT token retrieved from this request to be included in all subsequent requests to the server in a header that looks like the following:

`Authorization: JWT {{token}}`

The token is longer than 256 characters as it contains information about the user account used for authentication.

# Developer Authentication

> Once you have the Key and Secret you can obtain a JWT token for further requests, using the following:

```shell
curl --request POST \
  --url 'https://apidev.paycargo.com/login/developer' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data 'apiKey={{apiKey}}&apiSecret={{apiSecret}}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/login/developer",
  "method": "POST",
  "headers": {
    "Content-Type": "application/x-www-form-urlencoded"
  },
  "data": {
    "apiKey": "{{apiKey}}",
    "apiSecret": "{{apiSecret}}"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns a JSON string with the Authentication token

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6Nzc4MTQwLCJhY2NvdW50VHlwZSI6InNoaXBwZXIiLCJpYXQiOjE1MTcwOTUyODAsImV4cCI6MTUxNzcwMDA4MH0.HUeTs-Lb6AQsZjt_KEnjEljAXsXce0Y-zfQc5AI7oVc"
}
```
To authenticate as a developer, you will need an API Key and Shared Secret. You can obtain them by emailing developers@paycargo.com.

Paycargo API expects for the JWT token retrieved from this request to be included in all subsequent requests to the server in a header that looks like the following:

`Authorization: JWT {{token}}`

The token can be longer than 256 charachters as it contains information about developer account.

# Transaction

## Get Transaction Details

```shell
curl --request GET \
  --url 'https://apidev.paycargo.com/transaction/{{transactionId}}' \
  --header 'Authorization: JWT {{token}}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/transaction/478169",
  "method": "GET",
  "headers": {
    "Authorization": "JWT {{token}}"
  }
}
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "transactionId": 478169,
    "payerId": 281390,
    "vendorId": 279404,
    "status": 222455,
    "type": 222899,
    "number": "89080934",
    "departureDate": null,
    "arrivalDate": null,
    "paymentDueDate": "2017-05-16T00:00:00.000Z",
    "approvalDate": "2018-01-24T22:05:18.257Z",
    "hasArrived": true,
    "total": 4,
    "direction": "Outbound",
    "createdDate": "2018-01-22T11:07:24.280Z",
    "lastModifiedDate": "2018-01-24T22:05:18.273Z",
    "userId": 778042,
    "modifiedByUserId": 778140,
    "customerRefNumber": "customerrefnum",
    "subcategory": "subcat",
    "relatedNumber": "relatedbol"
  },
  "result": {
    "code": 200,
    "msg": "successful"
  }
}
```

This endpoint gives you the details for the transaction.

### HTTP Request

`GET https://apidev.paycargo.com/transaction/{{transactionId}}`

### Request Parameter

Parameter | Required | Description
--------- | ------- | -----------
transactionId | true | Unique identifier of the transaction in Paycargo system.

### Response Fields
Response Field | Type | Description
-------------- | ---- | -----------
transactionId | Int | Unique transaction
payerId | Int | Required (Optional for user authenticated payers) Payer identifier
vendorId | Int | Required (Optional for user authenticated vendors) Vendor (funds reciever) identifier
status | String | Error / Paid / Created / Disputed / Cleared / Void
type | String | Invoice / Bill of Lading / Terminal Fee / AWB
number | String | Transaction number, usually an AWB or BOL, accepts stars (*) at the end for duplicate payments
departureDate | Date | Cargo departure date
arrivalDate | Date | Cargo arrival date
paymentDueDate | Date | The date when payment is completed (may be in the future)
approvalDate | Date | Payment Authorization date
total | Numeric | Total payment(s) amount
hasArrived | Boolean | true / false
direction | String | "Inbound" / "Outbound"
createdDate | Date | Date transaction was created
lastModifiedDate | Date | Date transaction was last modified
userId | Int | Required (Optional for user authenticated accounts) Unique identfier of the user in Paycargo system who created the transaction
modifiedByUserId | Int | Unique identifier of the user in Paycargo system who last modified the transaction
shipperRefNumber | String | Optional reference number used by Payers / Vendors
customerRefNumber | String | Optional reference number used by Payers / Vendors
subcategory | String | Optional reference nuber used by Payers / Vendors
relatedNumber | String | Optional reference number used by Payers / Vendors

## Create a Transaction

```shell
curl --request POST \
  --url 'https://apidev.paycargo.com/transaction' \
  --header 'Authorization: JWT {{token}}' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data 'payerId=281390&vendorId=279404&type=Invoice&number=8908aaaaaa&total=4&userId=778042&direction=Outbound&paymentDueDate=2018-05-16&hasArrived=Y'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/transaction",
  "method": "POST",
  "headers": {
    "Content-Type": "application/x-www-form-urlencoded",
    "Authorization": "JWT {{token}}"
  },
  "data": {
    "payerId": "281390",
    "vendorId": "279404",
    "type": "Invoice",
    "number": "8908aaaaaa",
    "total": "4",
    "userId": "778042",
    "direction": "Outbound",
    "paymentDueDate": "2018-05-16",
    "hasArrived": "Y"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON structured like this:

```json
{
  "data": {
    "transactionId": 478565
  },
  "result": {
    "msg": "Transaction created",
    "code": 201
  }
}
```

> On failure the above command returns JSON like this:

```json
{
  "msg": "Transaction Number already exists. If you need to make additional payments to the same number, please add an asterisk (* up to 5) at the end for each additional payment.",
  "code": 400
}
```

> If `makePayment` is specified and set as 'true', but for some reason payment fails while transaction is still created, following result will be returned with code 201:

```json
{
  "data": {
    "transactionId": 478565
  },
  "result": {
    "code": 201,
    "msg": "Overnight availability reached, insufficient funds, cannot be approved"
  }
}
```

This endpoint creates a transaction, and optionally makes a payment (if makePayment: true)

### HTTP Request

`POST https://apidev.paycargo.com/transaction`

### Form Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
payerId | Yes | Int | Unique identifier of the Payer in Paycargo system
vendorId | Yes | Int | Unique identifier of the Vendor in Paycargo system
type | Yes | String | Invoice / Bill of Lading / Terminal Fee / AWB
number | Yes | String | Transaction number, usually an AWB or BOL, accepts stars (*) at the end for duplicate payments
paymentDueDate | Yes | Date | The date when payment is completed (can be in the future)
departureDate | No | Date | Cargo departure date
arrivalDate | No | Date | Cargo arrival date
total | Yes | Numeric | Total payment(s) amount
hasArrived | Yes | Boolean | 'Y' / 'N'
direction | Yes | String | "Inbound" / "Outbound"
userId | Yes | Int | Unique identfier of the user in Paycargo system who created the transaction
shipperRefNumber | No | String | Optional reference number used by Payers / Vendors
customerRefNumber | No | String | Optional reference number used by Payers / Vendors
subcategory | No | String | Optional reference nuber used by Payers / Vendors
relatedNumber | No | String | Optional reference number used by Payers / Vendors
makePayment | No | Boolean | true / false, If specified authrizes payment on this transaction
paymentType | *No | String | OVERNIGHT / PAYCARGO_CREDIT *(Required when makePayment=true)

## Void Transaction

```shell
curl --request PUT \
  --url 'https://apidev.paycargo.com/transaction/void/478553' \
  --header 'Authorization: JWT {{token}}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/transaction/void/478553",
  "method": "PUT",
  "headers": {
    "Authorization": "JWT {{token}}"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON like this upon success:

```json
{
  "data": {
    "transactionId": "478569"
  },
  "result": {
    "code": 200,
    "msg": "Transaction Voided"
  }
}
```

This endpoint voids a transaction.

### HTTP Request

`PUT https://apidev.paycargo.com/transaction/void/{{transactionId}}`

### URL Parameters

Parameter | Description
--------- | -----------
transactionId | transactionId of previously created transaction




## Make Payment

```shell
curl --request PUT \
  --url 'https://apidev.paycargo.com/transaction/pay/478562' \
  --header 'Authorization: JWT {{token}}' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data paymentType=OVERNIGHT
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/transaction/pay/478562",
  "method": "PUT",
  "headers": {
    "Content-Type": "application/x-www-form-urlencoded",
    "Authorization": "JWT {{token}}"
  },
  "data": {
    "paymentType": "OVERNIGHT"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON like this upon success:

```json
{
  "result": {
    "msg": "Transaction paid",
    "code": 200
  }
}
```

You can make a payment on a transaction in status "Created". Making a payment will make transaction "Approved" status

### HTTP Request

`PUT https://apidev.paycargo.com/transaction/pay/{{transactionId}}`

### URL Parameters

Parameter | Description
--------- | -----------
transactionId | transactionId of previously created transaction

### Form Parameters
Parameter | Description
--------- | -----------
paymentType | OVERNIGHT / PAYCARGO_CREDIT - Pay by Overnight or Paycargo Credit


## Freight Correction

```shell
curl --request PUT \
  --url 'http://apidev.paycargo.com/transaction/487727' \
  --header 'Authorization: JWT {{token}}' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data 'total=20&paymentDueDate=2018-04-21'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "http://apidev.paycargo.com/transaction/487727",
  "method": "PUT",
  "headers": {
    "Content-Type": "application/x-www-form-urlencoded",
    "Authorization": "JWT {{token}}"
  },
  "data": {
    "total": "20",
    "paymentDueDate": "2018-04-21"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON like this upon success:

```json
{
    "result": {
        "msg": "Transaction freight corrected.",
        "code": 200
    },
    "data": {
        "transactionId": "487727",
        "total": "20",
        "paymentDueDate": "2018-04-21"
    }
}
```

This endpoint updates the total and payment due date for the paid transaction.

### HTTP Request

`PUT https://apidev.paycargo.com/transaction/{{transactionId}}`

### URL Parameters

Parameter | Description
--------- | -----------
transactionId | transactionId of previously created transaction

### Form Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
paymentDueDate | Yes | Date | The date when payment is completed (can only be before the current paymentDueDate)
total | Yes | Numeric | Total payment(s) amount (can only be less than the current total)



# Transaction Lines

## Get Transaction Lines

```shell
curl --request GET \
  --url 'https://apidev.paycargo.com/transactionLines/{{transactionId}}' \
  --header 'Authorization: JWT {{token}}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/transactionLines/497526",
  "method": "GET",
  "headers": {
    "Authorization": "JWT {{token}}"
  }
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
        "msg": "succesful",
        "code": 200
    },
    "data": [
        {
            "PC_TRANSACTION_LINE_ID": 2233,
            "PC_TRANSACTION_ID": 500311,
            "DESCRIPTION": "Initial value set for client",
            "START_DATE": null,
            "END_DATE": null,
            "QUANTITY": 2,
            "UNIT_PRICE": 2,
            "AMOUNT": 40,
            "VAT_PERCENT": 0,
            "VAT_AMOUNT": 0,
            "PIECES": 0,
            "WEIGHT": 0,
            "CREATED_BY": 778993,
            "DATE_CREATED": "2018-11-26T09:19:23.380Z",
            "MODIFIED_BY": null,
            "DATE_MODIFIED": "2018-11-26T09:20:26.260Z"
        },
        {
            "PC_TRANSACTION_LINE_ID": 2235,
            "PC_TRANSACTION_ID": 500311,
            "DESCRIPTION": "Correction",
            "START_DATE": null,
            "END_DATE": null,
            "QUANTITY": 2,
            "UNIT_PRICE": -2,
            "AMOUNT": -4,
            "VAT_PERCENT": 0,
            "VAT_AMOUNT": 0,
            "PIECES": 0,
            "WEIGHT": 0,
            "CREATED_BY": 778993,
            "DATE_CREATED": "2018-11-26T09:19:23.437Z",
            "MODIFIED_BY": null,
            "DATE_MODIFIED": null
        },
        {
            "PC_TRANSACTION_LINE_ID": 2236,
            "PC_TRANSACTION_ID": 500311,
            "DESCRIPTION": "Second correction",
            "START_DATE": null,
            "END_DATE": null,
            "QUANTITY": 2,
            "UNIT_PRICE": -2,
            "AMOUNT": -4,
            "VAT_PERCENT": 0,
            "VAT_AMOUNT": 0,
            "PIECES": 0,
            "WEIGHT": 0,
            "CREATED_BY": 778993,
            "DATE_CREATED": "2018-11-26T09:19:23.437Z",
            "MODIFIED_BY": null,
            "DATE_MODIFIED": null
        },
        {
            "PC_TRANSACTION_LINE_ID": 2237,
            "PC_TRANSACTION_ID": 500311,
            "DESCRIPTION": "New price applied including corrections",
            "START_DATE": null,
            "END_DATE": null,
            "QUANTITY": 2,
            "UNIT_PRICE": 30,
            "AMOUNT": 60,
            "VAT_PERCENT": 0,
            "VAT_AMOUNT": 0,
            "PIECES": 0,
            "WEIGHT": 0,
            "CREATED_BY": 778993,
            "DATE_CREATED": "2018-11-26T09:23:54.157Z",
            "MODIFIED_BY": null,
            "DATE_MODIFIED": null
        },
        {
            "PC_TRANSACTION_LINE_ID": 2238,
            "PC_TRANSACTION_ID": 500311,
            "DESCRIPTION": "Original price removed",
            "START_DATE": null,
            "END_DATE": null,
            "QUANTITY": 2,
            "UNIT_PRICE": -20,
            "AMOUNT": -40,
            "VAT_PERCENT": 0,
            "VAT_AMOUNT": 0,
            "PIECES": 0,
            "WEIGHT": 0,
            "CREATED_BY": 778993,
            "DATE_CREATED": "2018-11-26T09:23:54.157Z",
            "MODIFIED_BY": null,
            "DATE_MODIFIED": null
        }
    ]
}
```

This endpoint gives you the details for the transaction lines for a given transaction.



# Vendors

```shell
curl --request GET \
  --url 'https://apidev.paycargo.com/vendors' \
  --header 'Authorization: JWT {{token}}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/vendors",
  "method": "GET",
  "headers": {
    "Authorization": "JWT {{token}}"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON like this upon success:

```json
{
  "data": [
    {
      "vendorId": 278529,
      "name": "Air General - Ocean - DFW"
    },
    {
      "vendorId": 278545,
      "name": "CSAV-USA"
    }
  ]
}
```

This endpoint retrieves all vendors in Paycargo system and their corresponding IDs

### HTTP Request

`GET https://apidev.paycargo.com/vendors`


# Payers

```shell
curl --request GET \
  --url 'https://apidev.paycargo.com/payers' \
  --header 'Authorization: JWT {{token}}'
```

```javascript
var settings = {
  "async": true,
  "crossDomain": true,
  "url": "https://apidev.paycargo.com/payers",
  "method": "GET",
  "headers": {
    "Authorization": "JWT {{token}}"
  }
}

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> The above command returns JSON like this upon success:

```json
{
    "result": {
        "msg": "succesful",
        "code": 200
    },
    "data": [
        {
            "payerId": 279986,
            "payerName": "Essex Mfg Inc./Baum Bros"
        },
        {
            "payerId": 280289,
            "payerName": "Applause Source LLC"
        },
        {
            "payerId": 281919,
            "payerName": "Pasha Gemini Payer"
        }
    ]
}
```

This endpoint only works when logged in as a Developer and retrieves the payers associated with that developer account

### HTTP Request

`GET https://apidev.paycargo.com/payers` (developer auth only)
