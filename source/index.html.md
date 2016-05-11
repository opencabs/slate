---
title: API Reference

language_tabs:
  - shell
  - java

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the OpenCabs 'Publishers API'!

This document describes the API to be used by 3rd parties (publishers) submitting bookings to the platform on behalf of their customers.

The main difference with the “Customers APIs” is that the end customer is not required to register within the OpenCabs platform as another authorized user will create the booking for the end customer.

# Authentication

OpenCabs uses API keys to allow access to the API. You can obtain an ApiKey by contacting us on support@opencabs.com.

OpenCabs expects for the API key to be included in all API requests. You also need to provide a token, which will be used for authenticating your user profile, and the context (i.e. environment) for the execution of the request. This is required as the same user can be connected to several contexts (several publisher accounts for example).

`ApiKey: <your-api-key>`  
`Token: <your-token>`  
`ContextType”: "publisher"`  
`ContextId”: "<your-publisher-id>"`

OpenCabs supports JSON by default for the body of the requests / responses. You need to specify this in the headers with:

`Content-Type”: “application/json"`

<aside class="notice">
You must replace <code>your-api-key</code>, <code>your-api-key</code> and <code>your-publisher-id</code> with your personal values.
</aside>

# Pricing

## Get the price for a journey

The quote API enables the client to obtain either:

 - a price: for fixed price journeys,

 - or the pricing structure: for a journey that has a metered fare.

### HTTP Request

> Example request

```json
{  
   "isAsap":true,  
   "date":"2015-03-03T17:54:00Z",  
   "fareTypeId":"fixed_point_to_point",  
   "originPOI":"some poi",  
   "originAddressDetails":"York House",  
   "originStreetNumber":"262",  
   "originAddress":"Kentish Town Road",  
   "originCity":"Greater London",  
   "originPostcode":"NW5 2AA",  
   "originLatitude":51.55051,  
   "originLongitude":-0.1407013000000461,  
   "originCountry":"GB",  
   "destinationPOI":"London Heathrow (Terminal 1)",  
   "destinationAddress":"Croydon Rd",  
   "destinationCity":"Hayes",  
   "destinationPostcode":"UB3 5AP",  
   "destinationLatitude":51.4723,  
   "destinationLongitude":-0.451795,  
   "destinationCountry":"UK",  
   "comments":"some notes",  
   "flightNumber":"AB12345",  
   "numPassengers":1,  
   "numLargeItems":0,  
   "numSmallItems":0,  
   "vehicleCategoryId":1,  
   "customer":{  
      "firstName":"John",  
      "lastName":"Doe",  
      "phoneNumberCountryCode":"44",  
      "phoneNumber":"12345"  
   }  
 }
```

`POST /pricing/estimate`

### Request body

The JSON request example shows the fields required for the pricing estimate.

The destination fields are generally not required for requests with 'metered' fare type.

The additional comments below apply:

Parameter | Required | Description
--------- | -------- | -----------
isAsap | yes | If set to true, the result will also include cats
date | yes | In local time with the format yyyy-mm-dd’T’hh:mm:ss’Z’
fareTypeId | yes | Possible values: fixed_point_to_point, metered, as_directed (not implemented yet)
flightNumber | yes, for airport pickups | Flight number (e.g. BA123) which will enable suppliers to monitor flights for delays and cancellations

### Response

> The above command returns JSON structured like this:

```json
{  
   "quoteId":306007,
   "price":{  
      "netPrice":32.08,
      "vat":6.42,
      "totalPrice":38.50
   },
   "quote":{  
      "created":"2015-03-03 12:18:50",
      "fareType":"fixed_point_to_point",
      "destinationAddress":"Croydon Rd",
      "typeId":38,
      "isAsap":true,
      "statusId":0,
      "originAddress":"Kentish Town Road",
      "originCity":"Greater London",
      "originCountry":"GB",
      "originPOI":"some poi",
      "originPostcode":"NW5 2AA",
      "destinationCity":"Hayes",
      "destinationCountry":"UK",
      "destinationPOI":"London Heathrow (Terminal 1)",
      "destinationPostcode":"UB3 5AP",
      "destinationLatitude":51.4723,
      "destinationLongitude":-0.451795,
      "journeyDistance":31.766,
      "originLatitude":51.55051,
      "originLongitude":-0.1407013,
      "numPassengers":1,
      "journeyTimeMinutes":38,
      "comments":"some notes",
      "flightNumber":"AB12345",
      "originStreetNumber":"262",
      "originAddressDetails":"12",
      "timezoneOffset":0.0,
      "price":38.50,
      "netPrice":32.08,
      "vat":6.42,
      "numLargeItems":0,
      "numSmallItems":0,
      "totalCustomerPrice":38.50,
      "timeZone":"Europe/London",
      "date":"2015-03-03 12:28:50",
      "bookingStops":[  
      ]
   },
   "availablePaymentTypes":[  
      {  
         "id":"card_or_epayment",
         "shortDescription":"Card",
         "description":"Credit card, debit card, electronic payments"
      },
      {  
         "id":"on_account",
         "shortDescription":"Account",
         "description":"Account payment (after invoice)"
      },
      {  
         "id":"cash",
         "shortDescription":"Cash",
         "description":"Cash payment"
      }
   ],
   "showPrice":true
}
```

HTTP Code   | Description
----------- | -----------
200 | The request has been completed correctly. Check the content to know if a price is actually available (see further).
40x | The request has not been completed correctly. Some of the reasons are: incorrect data passed, user not authorized, etc.
50x | Server error

The response contains the details of the journey with the price, with:

- <code>quoteId</code>: if this is available and not null, then a price has been obtained. Otherwise check the <code>responseCode</code> and <code>text</code> fields for more information.

- <code>price</code> object contains the quoted price for a journey with fixed price. If the

- <code>meteredPricingStructure</code> object (not available in the example) contains the pricing structure for a metered journey;

- <code>availablePaymentTypes</code>: the list of the payment types that are acceptable for booking the journey;

- The `origin` fields and `destination` fields (if a destination is required) need to have <u>at least</u>: latitude, longitude, city and (i) POI or (ii) street address with either the streetNumber or the addressDetails;

# Bookings

## Create a new booking

This API enables to submit new bookings to the platform.

Generally you will need to make a pricing request to obtain a <code>quoteId</code> before submitting your booking. This is not required however if you are a platform and have been authorised to post jobs with a price set by yourself.

### Request

> Example request

```json
{  
   "quoteId":306007,
   "isAsap":true,
   "paymentTypeId":"cash",
   "date":"2015-03-03T17:54:00Z",
   "originPOI":"some poi",
   "originAddressDetails":"12",
   "originStreetNumber":"262",
   "originAddress":"Kentish Town Road",
   "originCity":"Greater London",
   "originPostcode":"NW5 2AA",
   "originLatitude":51.55051,
   "originLongitude":-0.1407013000000461,
   "originCountry":"GB",
   "destinationPOI":"London Heathrow (Terminal 1)",
   "destinationAddress":"Croydon Rd",
   "destinationCity":"Hayes",
   "destinationPostcode":"UB3 5AP",
   "destinationLatitude":51.4723,
   "destinationLongitude":-0.451795,
   "destinationCountry":"UK",
   "comments":"some notes",
   "flightNumber":"AB12345",
   "numPassengers":1,
   "numSmallItems":0,
   "numLargeItems":0,
   "vehicleCategoryId":1,
   "customer":{  
      "customerId": 1234,
      "firstName":"John",
      "lastName":"Doe",
      "phoneNumberCountryCode":"44",
      "phoneNumber":"12345"
   },
   "forceAnonymousCustomer":0
}
```

`POST /book`

Key fields

Name   | Required | Comments
----------- | ----------- | -----------
quoteId | (see comments) | The quoteId is required in all cases except if the publisher has been explicitly authorised (set up done by OpenCabs) for being able to post bookings without quoteId. In this case the <code>price</code> has to be included in the JSON with the request.
customer | yes | The <code>customer</code> object is required. If the id matches an existing customer linked to the same details, then the booking will be linked to the existing customer profile; otherwise a new customer profile will be created. The <code>customer</code> needs to have at least: (i) firstName, (ii) phoneNumberCountryCode, (iii) phoneNumber.
isAsap | yes | This field is used to tell the supplier to send a car as soon as they can, regardless of actual booking time. I.e. if the booking is for 20 minutes from now (which could be the contractual notice time agreed), if the supplier has a car available that can be sent immediately, then if <code>isAsap=true</code> the supplier knows that they have to send the car without waiting.
flightNumber | <i>see comments</i> | Required for all pickups at airport terminals.

### Response

> Response

```json
{  
"booking":[  
      {  
         "id":306049,
         "customer":{  
            "customerId":3608,
            "email":"",
            "firstName":"John",
            "lastName":"Doe",
            "phoneNumber":"12345",
            "phoneNumberCountryCode":"44",
            "vipScore":0.0,
            "noShowCount":0,
            "verified":false,
            "enabled":false
         },
         "loyaltyTokenUsed":false,
         "typeId":38,
         "created":"2015-03-03 12:19:46",
         "paymentType":{  
            "id":"cash",
            "shortDescription":"Cash",
            "description":"Cash payment"
         },
         "fareType":"fixed_point_to_point",
         "vehicleCategory":{  
            "id":1,
            "defaultCategoryForCountry":true,
            "capacityPassengers":4,
            "capacityLargeLuggage":2,
            "capacitySmallLuggage":2,
            "name":"Saloon",
            "enabled":true,
         },
         "isAsap":true,
         "date":"2015-03-03 12:28:50",
         "timeZone":"Europe/London",
         "timezoneOffset":0.0,
         "statusId":0,
         "originPOI":"some poi",
         "originStreetNumber":"262",
         "originAddressDetails":"12",
         "originAddress":"Kentish Town Road",
         "originCity":"Greater London",
         "originCountry":"GB",
         "originPostcode":"NW5 2AA",
         "originLatitude":51.55051,
         "originLongitude":-0.1407013,
         "destinationPOI":"London Heathrow (Terminal 1)",
         "destinationAddress":"Croydon Rd",
         "destinationPostcode":"UB3 5AP",
         "destinationCity":"Hayes",
         "destinationCountry":"UK",
         "destinationLatitude":51.4723,
         "destinationLongitude":-0.451795,
         "journeyDistance":31.766,
         "journeyTimeMinutes":38,
         "comments":"some notes",
         "flightNumber":"AB12345",
         "netPrice":32.08,
         "vat":6.42,
         "price":38.50,
         "currency":"GBP",
         "totalCustomerPrice":38.50,
         "numPassengers":1,
         "numLargeItems":0,
         "numSmallItems":0,
         "bookingStops":[  
         ]
      }
   ]
}
```

HTTP Code   | Description
----------- | -----------
201 | The booking has been successfully created.
40x | The request has not been completed correctly: incorrect data passed, user not authorized, etc.
50x | Server error

## Get the details of a booking

## Cancel a booking

# Tracking

## Get the drivers online

## Get the details of a driver online

# Kittens

## Get All Kittens

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve
