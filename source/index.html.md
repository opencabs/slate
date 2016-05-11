---
title: API Reference

language_tabs:
  - shell
  - java
  - ruby
  - python

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

`GET ../pricing/estimate`

### Request body

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
The JSON request example shows the fields required for the pricing estimate.

The destination fields are generally not required for requests with 'metered' fare type.

The additional comments below apply:

Parameter | Required | Default | Description
--------- | -------- | ------- | -----------
isAsap | yes | If set to true, the result will also include cats
date | yes | In local time with the format yyyy-mm-dd’T’hh:mm:ss’Z’
fareTypeId | yes | Possible values: fixed_point_to_point, metered, as_directed (not implemented yet)
flightNumber | yes, for airport pickups | Flight number (e.g. BA123) which will enable suppliers to monitor flights for delays and cancellations

> The above command returns JSON structured like this:

### Response

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
      "minPrice":38.50,
      "maxPrice":38.50,
      "netPrice":32.08,
      "vat":6.42,
      "minPriceNet":32.08,
      "minPriceVat":6.42,
      "maxPriceNet":32.08,
      "maxPriceVat":6.42,
      "numLargeItems":0,
      "numSmallItems":0,
      "totalCustomerPrice":38.50,
      "timeZone":"Europe/London",
      "date":"2015-03-03 12:28:50",
      "currency":"GBP",
      "bookingStops":[  
      ],
      "price":38.50
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
   "showPrice":true,
   "expiryDate":""
}
```

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

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
