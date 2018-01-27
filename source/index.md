---
title: API Reference

language_tabs:
  - shell
  - python

toc_footers:
  - <a href='http://www.bookalimo.com/connect-with-us/'>Contact Us for a Developer Key</a>

includes:

search: true
---

# Introduction

Welcome to the International Chauffeured Service API! You can use our API to get our rates for Point to Point, Hourly and Daily car bookings. You can make a booking via ICS API.

We have language bindings in Shell and Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Get Car Prices
```shell
curl -X POST  "https://e-zbookings.com/api/v1/carprice" \
-H "Content-Type: application/json" \
-d '
{
	"type": "PP",
	"passengers": 1,
	"luggage": 1,
	"time": "13:52",
	"pickup": {
		"isairport": "true",
		"airportcode": "JFK",
		"airlinecode": "DL",
		"flightno": "234",
		"countrycode": "US"
	},
	"date": "2015-06-29",
	"dropoff": {
		"countrycode": "US",
		"address": "32 bedford rd",
		"zipcode": "10514",
		"state": "NY",
		"isairport": "false",
		"locality": "Chappaqua"
	}
}
'
```
```python
def get_car_prices():
    data = {
       "type": "PP",
       	"time": "13:52",
       	"pickup": {
       		"airportcode": "EWR",
       		"countrycode": "US",
       		"isairport": "true",
       		"locality": "NYC"
       	},
       	"date": "2015-10-29",
       	"dropoff": {
       		"countrycode": "US",
		"address": "73 S Bedford",
       		"zipcode": "10514",
       		"state": "NY",
       		"isairport": "false",
       		"locality": "Chappaqua"
       	}
    }
    
    json_data               = json.dumps(data)
    post_data               = json_data.encode('utf-8')
    headers                 = {}
    headers['Content-Type'] = 'application/json'
    url                     = 'https://e-zbookings.com/api/v1/carprice'
    req                     = urllib2.Request(url, post_data, headers)  
    res                     = urllib2.urlopen(req)
    result                  = res.read()

    print 'POST https://e-zbookings.com/api/v1/carprice' 
    pp                      = pprint.PrettyPrinter(indent=4)
    json_object             = json.loads(result)
    pp.pprint(json_object)
    print "\n"

    if 'token' in json_object:
        result_token        = json_object ['token']
        global token_id 
        token_id            = result_token
    else:
         print 'Token is null'
```

> The above command returns JSON structured like this:

```json
{
  "token": "5v72fh56ia8epinqnmunpg53ej",
  "prices": [
    {
      "id": "1",
      "carClassId": "SPT",
      "type": "SPRINTER VAN 14 PASSENGERS",
      "price": "179.00",
      "final_price": "243.76",
      "max_pass": "14",
      "max_lugg": "14",
      "image": "https://e-zbookings.com/includes/images/api/US_SPT_150x80.png"
    },
    {
      "id": "2",
      "carClassId": "MBUS",
      "type": "MINIBUS 22-36 PASSENGERS",
      "price": "344.00",
      "final_price": "461.36",
      "max_pass": "24",
      "max_lugg": "24",
      "image": "https://e-zbookings.com/includes/images/api/US_MBUS_150x80.png"
    },
    {
      "id": "3",
      "carClassId": "BUS",
      "type": "MOTORCOACH PREVOST",
      "price": "459.00",
      "final_price": "613.02",
      "max_pass": "55",
      "max_lugg": "55",
      "image": "https://e-zbookings.com/includes/images/api/US_BUS_150x80.png"
    }
  ],
  "options": [
    {
      "code": "B",
      "price": "15",
      "parking": "3",
      "name": "Baggage Claim",
      "instructions": "Meet your chauffeur by Baggage Claim even if you do not have any luggage"
    },
    {
      "code": "C",
      "price": "0",
      "parking": "0",
      "name": "Curb Side",
      "instructions": "*Please call upon arrival 1-800-266-5254.  We monitor the flight*"
    },
    {
      "code": "I",
      "price": "15",
      "parking": "3",
      "name": "International",
      "instructions": "Chauffeur will be waiting with the sign at the Outside Custom Area"
    },
    {
      "code": "B",
      "price": "11",
      "parking": "5",
      "name": "Baggage Claim",
      "instructions": "Meet your chauffeur by Baggage Claim even if you do not have any luggage"
    },
    {
      "code": "C",
      "price": "0",
      "parking": "0",
      "name": "Curb Side",
      "instructions": "*Please call upon arrival 1-800-266-5254.  We monitor the flight*"
    },
    {
      "code": "G",
      "price": "11",
      "parking": "5",
      "name": "Gate",
      "instructions": "Chauffeur will be waiting with the sign at the Gate Area"
    },
    {
      "code": "I",
      "price": "0",
      "parking": "5",
      "name": "International",
      "instructions": "Chauffeur will be waiting with the sign at the Outside Custom Area"
    }
  ]
}
```

`POST https://e-zbookings.com/api/v1/carprice`

Gets prices for specified pick up and drop off location for different Car Classes and Meet and Greet Options

### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
type | true | Point to Point (PP) or Hourly (HR)
hours | type==HR | Number of hours for hourly jobs
date | true | Date of the Pick Up
time | true | Time of the Pick Up
pickup | true | Pick Up Location
dropoff | true | Drop Off Location
passengers | false | Number of Passengers, defaults to 1 if skipped
luggage | false | Number of Luggage itesm, defaults to 1 if skipped
option | false | Meet and Greet Option
passenger | false | Passenger information that includes his name and contact info (see below)
addl_option | false | Additional options include Animal, Car Seat and Booster
airlinecode | false | Airline code for trips from/to Airport
flightno | false | Flight Number for trip from/to airport
finalPrice | false | whether to give back the final_price that includes Meet & Greet, Parking, Gratuity Tolls and Tax
job_id | false | Pass it if you would like to edit reservation, then follow same steps as for reservation creation

### Passenger Parameters

Parameter | Required | Description
--------- | ------- | -----------
name_last | true | Passenger Last Name
name_first | true | Passenger First Name
email | true | Passenger email address
phone | true | Passenger phone number


### Additional Option Parameters

Parameter | Required | Description
--------- | ------- | -----------
animal | true | Transporting an Animal in the car true/false
carseat | true | Carseat needed true/false
booster | true | Booster seat needed true/false


### Location Parameters
Parameter | Required | Description
--------- | ------- | -----------
isairport | true | Location is an airport "true" or "false"
countrycode | true | Country Code
airportcode | isairport | Airport IATA code, only required when isairport="true"
airlinecode | false | Airline IATA code, please refer to [IATA Airline/Airport Search](http://www.iata.org/publications/Pages/code-search.aspx)
flightno | false | Flight no, only required when isairport="true"
state | only if countrycode="US" and !isairport | US State, required only when isairport="false" and countrycode is "US"
locality | !isairport | City or Locality
zipcode | !isairport | Postal or Zip Code


Returns a Car Classes with Prices and Meet and Greet Options along with the token that serves as Booking identifier for further requests


This endpoint provides quotes for different car classes and meet and greet options


# Authentication

ICS uses API keys to allow access to the API. You can get ICS API key by contacting us at http://bookalimo.com/connect-with-us

We expect for the API credentials to be included in every request to the server in a header that looks like the following:

`Authorization: Basic base64encodedkeyandsecret`

Get Car Prices is the only request that do not require authentication header
<aside class="notice">
You must replace <code>base64encodedkeyandsecret</code> with your personal API credentials.
</aside>


# Preview Booking

`POST https://e-zbookings.com/api/v1/book`

```shell
curl -X POST "https://e-zbookings.com/api/v1/book" \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic base64encodedkeyandsecret" \
  -d '
{
    "token": "t122so3d90pj9hrqbb4nv7lv2f",
    "car": "SD",
    "option": "C",
    "passenger":
    {
      "name_first":"Nick",
      "name_last":"Jones",
      "phone":"555-555-5555",
      "email":"test@test.com"
    },
    "addl_option":
    {
      "animal": true,
      "carseat": false,
      "booster": true
    }
}
'
```
```python
def get_preview_booking():
    data      = token_id
    udata     = data.decode("utf-8")
    asciidata = udata.encode("ascii","ignore")
    data      = {
        "token": asciidata,
        "car": "SD",
        "option": "B",
        "passenger":
        {
          "name_first":"Nick",
          "name_last":"Jones",
          "phone":"555-555-5555",
          "email":"test@test.com"
        },
        "addl_option":
        {
          "animal": true,
          "carseat": false,
          "booster": true
        }
    }
    
    json_data               = json.dumps(data)
    post_data               = json_data.encode('utf-8')
    headers                 = {}
    headers['Content-Type'] = 'application/json'
    headers['Authorization'] = 'Basic base46encodedkeyandsecret'
    url                     = 'https://e-zbookings.com/api/v1/book'
    req                     = urllib2.Request(url, post_data, headers)  
    res                     = urllib2.urlopen(req)
    result                  = res.read()
    pp = pprint.PrettyPrinter(indent = 4)
    print 'POST https://e-zbookings.com/api/v1/book'
    json_object             = json.loads(result)
    pp.pprint(json_object)
    print "\n"

```
Requests a final quote breakdown

### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | A token of the reservation provided by Get Car Prices
car | true | Selected Car Class to be used as means of transpiration
option | false | Selected Airport Meet & Greet Option
passenger | false | Passenger information that includes his name and contact info
addl_option | false| Additional Options include Animal, Car Seat, Booster
airlinecode | false | Airline code for trips from/to Airport
flightno | false | Flight Number for trip from/to airport

### Passenger Parameters

Parameter | Required | Description
--------- | ------- | -----------
name_last | true | Passenger Last Name
name_first | true | Passenger First Name
email | true | Passenger email address
phone | true | Passenger phone number


### Additional Option Parameters

Parameter | Required | Description
--------- | ------- | -----------
animal | true | Transporting an Animal in the car true/false
carseat | true | Carseat needed true/false
booster | true | Booster seat needed true/false



> The above command returns JSON structured like this:

```json
{
  "token": "t122so3d90pj9hrqbb4nv7lv2f",
  "amount_units": "USD",
  "price": {
    "base_fare": "127.00",
    "gratuity": "25.40",
    "toll_misc": "9.50",
    "parking": "0.00",
    "meet_greet": "0.00",
    "stc_charge": "16.03",
    "wc_tax": "4.45",
    "misc": "0.00",
    "total": "182.38"
  }
}
```

# Confirm Booking
```shell
curl -X POST "https://e-zbookings.com/api/v1/pay" \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic base64encodedkeyandsecret" \
  -d '
{
    "token": "t122so3d90pj9hrqbb4nv7lv2f",
    "payment": {
        "type": "VI",
        "cc": "4111111111111111",
        "exp": "1115",
        "zip": "10001",
        "cvv": "123"
    }
}
'
```
```python
def get_confirm_booking():
    data = {
        "token": token_id,
            "payment": 
            {
                "type": "VI",
                "cc": "4111111111111111",
                "exp": "1115",
                "zip": "10001",
                "cvv": "123"
           }
    }
            
    json_data                = json.dumps(data)
    post_data                = json_data.encode('utf-8')
    headers                  = {}
    headers['Content-Type']  = 'application/json'
    headers['Authorization'] = 'Basic base46encodedkeyandsecret'
    url                      = 'https://e-zbookings.com/api/v1/pay'
    req                      = urllib2.Request(url, post_data, headers)  
    res                      = urllib2.urlopen(req)
    result                   = res.read()
    pp                       = pprint.PrettyPrinter(indent = 4)
    print 'POST https://e-zbookings.com/api/v1/pay' 
    json_object              = json.loads(result)
    pp.pprint(json_object)
```
> Upon Successful booking the Request above will return following JSON:

```json
{
  "jobId": 658068
}
```
> Where jobId is reservation confirmation number
> Upon a Credit Card Validation problem the JSON response:

```json
{
  "errors":
	[ "Invalid Credit Card" ]
}
```

`POST https://e-zbookings.com/api/v1/pay`



This request requires Authentication Header comprised of your API key and secret

### Request Parameters

Parameter | Required | Description
--------- | ------- | -----------
token | true | A token of the reservation provided by Get Car Prices Response
payment | true | Payment details
passenger | false | Passenger information that includes his name and contact info 
addl_option | false | Additional Options include Animal, Car Seat and Booster
airlinecode | false | Airline code for trips from/to Airport
flightno | false | Flight Number for trip from/to airport

### Payment Details

Parameter | Required | Description
--------- | ------- | -----------
type | true | Payment type could be VI, MC, AE, JB for Credit Cards and CH for Charge accounts
cc | true for CC types | Credit Card number
exp | true for CC types | Expiration date in MMYY format
zip | true for CC types | Billing Zip Code
cvv | true for CC types | Security code on the back of the card

### Passenger Parameters

Parameter | Required | Description
--------- | ------- | -----------
name_last | true | Passenger Last Name
name_first | true | Passenger First Name
email | true | Passenger email address
phone | true | Passenger phone number


### Additional Option Parameters

Parameter | Required | Description
--------- | ------- | -----------
animal | true | Transporting an Animal in the car true/false
carseat | true | Carseat needed true/false
booster | true | Booster seat needed true/false

# View Reservations

```shell
curl -X GET "https://e-zbookings.com/api/v1/reslist"
-H "Authorization Basic base64encodedkeyandsecret"
```

```python
def get_reslist():
    
    headers                  = {}
    headers['Content-Type']  = 'application/json'
    headers['Authorization'] = 'Basic base46encodedkeyandsecret'
    url                      = 'https://e-zbookings.com/api/v1/reslist'
    req                      = urllib2.Request(url, post_data, headers)
    res                      = urllib2.urlopen(req)
    result                   = res.read()
    pp                       = pprint.PrettyPrinter(indent = 4)
    print 'GET https://e-zbookings.com/api/v1/reslist'
    json_object              = json.loads(result)
    pp.pprint(json_object)
```

> The request will return the following JSON


```json
{
  "jobs": [
    {
      "jobId": 762760,
      "type": "PP",
      "date": "12/18/2015",
      "time": "1:30 PM",
      "pickup": {
        "addressFull": "EWR (C) ",
        "isairport": "true",
        "airportcode": "EWR"
      },
      "dropoff": {
        "addressFull": "Chappaqua",
        "locality": "Chappaqua",
        "address": "null null",
        "state": "NY",
        "isairport": "false"
      },
      "passenger": {
        "name_first": "Mozio",
        "name_last": "Test",
        "phone": "877.666.5544",
        "email": "test@mozio.com"
      },
      "passengers": 1,
      "luggage": 1,
      "car": "BUS",
      "option": "C",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    },
    {
      "jobId": 762865,
      "type": "PP",
      "date": "12/18/2015",
      "time": "2:20 PM",
      "pickup": {
        "addressFull": "Chappaqua",
        "locality": "Chappaqua",
        "address": "null null",
        "state": "NY",
        "isairport": "false"
      },
      "dropoff": {
        "addressFull": "EWR",
        "isairport": "true",
        "airportcode": "EWR"
      },
      "passenger": {
        "name_first": "Mozio",
        "name_last": "Test",
        "phone": "877.666.5544",
        "email": "test@mozio.com"
      },
      "passengers": 1,
      "luggage": 1,
      "car": "BUS",
      "option": "C",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    },
    {
      "jobId": 762970,
      "type": "PP",
      "date": "12/18/2015",
      "time": "2:20 PM",
      "pickup": {
        "addressFull": "Chappaqua",
        "locality": "Chappaqua",
        "address": "null null",
        "state": "NY",
        "isairport": "false"
      },
      "dropoff": {
        "addressFull": "EWR",
        "isairport": "true",
        "airportcode": "EWR"
      },
      "passenger": {
        "name_first": "Mozio",
        "name_last": "Test",
        "phone": "877.666.5544",
        "email": "test@mozio.com"
      },
      "passengers": 1,
      "luggage": 1,
      "car": "BUS",
      "option": "C",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    },
    {
      "jobId": 762925,
      "type": "PP",
      "date": "12/18/2015",
      "time": "5:00 PM",
      "pickup": {
        "addressFull": "EWR (C) ",
        "isairport": "true",
        "airportcode": "EWR"
      },
      "dropoff": {
        "addressFull": "Chappaqua",
        "locality": "Chappaqua",
        "address": "null null",
        "state": "NY",
        "isairport": "false"
      },
      "passenger": {
        "name_first": "Mozio",
        "name_last": "Test",
        "phone": "877.666.5544",
        "email": "test@mozio.com"
      },
      "passengers": 1,
      "luggage": 1,
      "car": "BUS",
      "option": "C",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    },
    {
      "jobId": 762874,
      "type": "PP",
      "date": "12/18/2015",
      "time": "5:00 PM",
      "pickup": {
        "addressFull": "EWR (C) ",
        "isairport": "true",
        "airportcode": "EWR"
      },
      "dropoff": {
        "addressFull": "Chappaqua",
        "locality": "Chappaqua",
        "address": "null null",
        "state": "NY",
        "isairport": "false"
      },
      "passenger": {
        "name_first": "Mozio",
        "name_last": "Test",
        "phone": "877.666.5544",
        "email": "test@mozio.com"
      },
      "passengers": 1,
      "luggage": 1,
      "car": "BUS",
      "option": "C",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    },
    {
      "jobId": 762857,
      "type": "PP",
      "date": "12/18/2015",
      "time": "5:00 PM",
      "pickup": {
        "addressFull": "EWR (C) ",
        "isairport": "true",
        "airportcode": "EWR"
      },
      "dropoff": {
        "addressFull": "Chappaqua",
        "locality": "Chappaqua",
        "address": "null null",
        "state": "NY",
        "isairport": "false"
      },
      "passenger": {
        "name_first": "Mozio",
        "name_last": "Test",
        "phone": "877.666.5544",
        "email": "test@mozio.com"
      },
      "passengers": 1,
      "luggage": 1,
      "car": "BUS",
      "option": "C",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    },
    {
      "jobId": 763030,
      "type": "PP",
      "date": "01/03/2016",
      "time": "5:00 PM",
      "pickup": {
        "addressFull": "EWR (C) ",
        "isairport": "true",
        "airportcode": "EWR"
      },
      "dropoff": {
        "addressFull": "Chappaqua",
        "locality": "Chappaqua",
        "address": "null null",
        "state": "NY",
        "isairport": "false"
      },
      "passenger": {
        "name_first": "Mozio",
        "name_last": "Test",
        "phone": "877.666.5544",
        "email": "test@mozio.com"
      },
      "passengers": 1,
      "luggage": 1,
      "car": "BUS",
      "option": "C",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    }
  ]
}
```
`GET https://e-zbookings.com/api/v1/reslist`

Gets the list of future reservations

This request requires Authentication Header comprised of your API key and secret

# Get Customer Information
```shell
curl -X GET "https://e-zbookings.com/api/v1/profile"
-H "Authorization Basic base64encodedcustomercredentials"
```

> Request above will return the following JSON:

```json
{
  "profile": {
    "name_first": "JOHN",
    "name_last": "DOE",
    "phone": "212.547.8146",
    "email": "JOHN.DOE@GMAIL.COM"
  }
}
```

`GET https://e-zbookings.com/api/v1/profile`

Gets Customer information

This request requires Authentication Header comprised of base64encoded customer credentials:

Authorization Basic Base64Encode(customer_email:customer_password)


# Cancel Confirmed Booking
```shell
curl -X GET "https://e-zbookings.com/api/v1/cancel/[jobId]"
-H "Authorization Basic base64encodedkeyandsecret"
```
> Upon successfull cancellation, the Request above will return following JSON:

```json
{
  "success": true,
  "message": "Successfully cancelled reservation: 760855"
}
```

> Upon un-successfull cancellation, the JSON will look like this:


```json
{
  "success": false,
  "message": "RESERVATION IS NOT WITHIN CANCELLATION POLICY -- MUST CALL 1-800-266-5254"
}
```

`GET https://e-zbookings.com/api/v1/cancel/[jobId]`

This request requires Authentication Header comprised of your API key and secret

Parameter | Required | Description
--------- | ------- | -----------
jobId | true | an identifier for the job to be cancelled


# Driver get List of Reservations

`GET https://e-zbookings.com/api/v1/driver/reslist`

This request requires Authentication Header comprised of Driver API key and secret

```shell
curl -X GET "https://e-zbookings.com/api/v1/driver/reslist"
-H "Authorization Basic base64encodedkeyandsecret"
```

> The Request above returns following JSON:

```json
{
  "jobs": [
    {
      "jobId": 843250,
      "confirmable": true,
      "type": "PP",
      "date": "04/25/2016",
      "time": "6:10 AM",
      "pickup": {
        "addressFull": "New Rochelle NY: 94 DAVIS AVE",
        "locality": "New Rochelle NY",
        "address": "94 DAVIS AVE",
        "state": "NY",
        "isairport": "false"
      },
      "dropoff": {
        "addressFull": "LGA:DL - Delta Airlines",
        "isairport": "true",
        "airportcode": "LGA",
        "airlinecode": "DL"
      },
      "passenger": {
        "name_first": ".",
        "name_last": "HUGH DOYLE SENIOR CENTER"
      },
      "passengers": 1,
      "car": "SD",
      "addl_option": {
        "animal": false,
        "carseat": false,
        "booster": false
      }
    },
    {
      "jobId": 860777,
      "confirmable": false,
      "type": "PP",
      "date": "06/30/2016",
      "time": "5:00 AM",
      "pickup": {
        "addressFull": "Washington: White House, Pennsylvania Avenue Northwest, Washington, D.C., DC, United States: 1600 Pennsylvania Ave NW",
        "locality": "Washington",
        "address": "1600 Pennsylvania Ave NW",
        "state": "DC",
        "isairport": "false"
      },
      "dropoff": {
        "addressFull": "Washington DC: 1404 Highland Ave, Ashland, KY, United States: 1404 Highland Ave",
        "locality": "Washington DC",
        "address": "1404 Highland Ave",
        "state": "KY",
        "isairport": "false"
      },
      "passenger": {
        "name_first": "Katherine",
        "name_last": "Sierra",
        "phone": "606.324.8398",
        "email": "Katherine@bookalimo.com"
      },
      "passengers": 2,
      "luggage": 4,
      "car": "MBUS",
      "addl_option": {
        "animal": false,
        "carseat": true,
        "booster": false
      }
    }
  ]
}
```

# Driver Confirm Reservation

`POST https://e-zbookings.com/api/v1/driver/confirm` 

This request requires Authentication Header comprised of Driver API key and secret

```shell
curl -X POST "https://e-zbookings.com/api/v1/driver/reslist" \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic base64encodedkeyandsecret" \
  -d '
{
    "jobId": "860777",
    "confirmation": "c1"
}
'
```
> Upon Successful confirmation, the succes message is returned:

```json
{
  "result": "OK"
}
```

> Upon un-successful confirmation, the error list is returned:

```json
{
  "errors": [
    "Already confirmed"
  ]
}
```
