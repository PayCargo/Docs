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

# Authentication

> To authenticate and get JWT token for further requests, use following:

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
Paycargo API expects for the JWT token retrieved from this request to be included in all subsequent requests to the server in a header that looks like the following:

`Authorization: JWT {{token}}`

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

This endpoint gives you the Details for the transaction. 

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
payerId | Int | Payer identifier
vendorId | Int | Vendor (funds reciever) identifier
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
userId | Int | Unique identfier of the user in Paycargo system who created the transaction
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

> if makePayment is specified and set as 'true', but for some reason payment fails while transaction is still created, following result will be returned with code 201:

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
paymentType | OVERNIGHT - we currently only support payments with ACH bank account

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

