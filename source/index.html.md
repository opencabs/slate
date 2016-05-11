---
title: API Reference

language_tabs:
  - shell
  - java

toc_footers:
  - <a href='mailto:support@opencabs.com'>Sign Up for a Developer Key</a>

includes:
  - lists
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

# Bookings

## Create a new booking

`POST /book`

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
    "id": 4098755,
    "statusId": 1,
    "customer": {
        "customerId": 434665,
        "firstName": "Moise",
        "lastName": "Errol",
        "phoneNumberCountryCode": "44",
        "phoneNumber": "78812341234",
        "vipScore": 0,
        "noShowCount": 0,
        "verified": false,
        "enabled": false
    },
    "originStreetNumber": "",
    "originAddressDetails": "",
    "originAddress": "Westminster Bridge Road",
    "originPOI": "St. Thomas' Hospital",
    "originCity": "Lambeth",
    "originCountry": "United Kingdom",
    "originPostcode": "SE1 7HY",
    "originLatitude": 51.4988,
    "originLongitude": -0.117893,
    "destinationStreetNumber": "22",
    "destinationAddressDetails": "",
    "destinationAddress": "Faircross Avenue",
    "destinationPOI": "",
    "destinationCity": "Barking",
    "destinationCountry": "United Kingdom",
    "destinationPostcode": "IG11 8RD",
    "destinationLatitude": 51.542,
    "destinationLongitude": 0.0831571,
    "isAsap": true,
    "date": "2016-05-11 17:05:00",
    "timezoneOffset": 1,
    "timeZone": "Europe/London",
    "numLargeItems": 0,
    "numSmallItems": 0,
    "numPassengers": 2,
    "journeyDistance": 18.843,
    "journeyTimeMinutes": 40,
    "comments": "",
    "airportBooking": false,
    "automaticallyAccepted": false,
    "acknowledgedByCompany": false,
    "bookingStops": [],
    "fareType": "fixed_point_to_point",
    "netPrice": 43.3,
    "vat": 0,
    "price": 43.3,
    "discount": 0,
    "totalCustomerPrice": 43.3,
    "showPrice": true,
    "currency": "GBP",
    "currencyXml": {
        "currencyId": "GBP",
        "currencyLongSymbol": "GBP",
        "currencyName": "Pound",
        "currencySymbol": "£"
    },
    "publisher": {
        "id": 1,
        "shortName": "ABC Cabs",
        "distance": 0,
        "name": "ABC Cabs "
    },
    "paymentType": {
        "id": "cash",
        "shortDescription": "Cash",
        "description": "Cash payment",
        "isDefault": false
    },
    "vehicleCategory": {
        "id": 1,
        "rank": 11,
        "defaultCategoryForCountry": true,
        "capacityPassengers": 4,
        "capacityLargeLuggage": 2,
        "descriptionLong": "Saloon",
        "descriptionShort": "Saloon",
        "capacitySmallLuggage": 2,
        "commonName": "standard_car",
        "name": "Saloon",
        "enabled": true
    },
    "managingCompany": {
        "companyId": 8647,
        "companyName": "East Ten Cars Limited",
        "companyLatitude": 51.5144,
        "companyLongitude": -0.0542385,
        "orderPhone": "+442077771234"
    },
    "created": "2016-05-11 16:48:14",
    "expiryDate": "2016-05-11 16:58:14"
}
```

HTTP Code   | Description
----------- | -----------
201 | The booking has been successfully created.
40x | The request has not been completed correctly: incorrect data passed, user not authorized, etc.
50x | Server error

## Get the details of a booking

```json
{
    "id": 4098755,
    "statusId": 1,
    "automaticallyAccepted": false,
    "customer": {
        "customerId": 434665,
        "firstName": "Moise",
        "lastName": "Errol",
        "phoneNumberCountryCode": "44",
        "phoneNumber": "78812341234",
        "vipScore": 0,
        "noShowCount": 0,
        "verified": false,
        "enabled": false
    },
    "publisher": {
        "id": 1,
        "shortName": "ABC Cabs",
        "distance": 0,
        "name": "ABC Cabs "
    },
    "originStreetNumber": "",
    "originAddressDetails": "",
    "originAddress": "Westminster Bridge Road",
    "originPOI": "St. Thomas' Hospital",
    "originCity": "Lambeth",
    "originCountry": "United Kingdom",
    "originPostcode": "SE1 7HY",
    "originLatitude": 51.4988,
    "originLongitude": -0.117893,
    "destinationStreetNumber": "92",
    "destinationAddressDetails": "",
    "destinationAddress": "Hillside Avenue",
    "destinationPOI": "",
    "destinationCity": "Barking",
    "destinationCountry": "United Kingdom",
    "destinationPostcode": "IG11 8RD",
    "destinationLatitude": 51.542,
    "destinationLongitude": 0.0831571,
    "bookingStops": [
    ],    
    "isAsap": true,
    "date": "2016-05-11 17:05:00",
    "timezoneOffset": 1,
    "timeZone": "Europe/London",
    "numLargeItems": 0,
    "numSmallItems": 0,
    "numPassengers": 2,
    "journeyDistance": 18.843,
    "journeyTimeMinutes": 40,
    "comments": "",
    "airportBooking": false,
    "fareType": "fixed_point_to_point",
    "netPrice": 43.3,
    "vat": 0,
    "price": 43.3,
    "discount": 0,
    "totalCustomerPrice": 43.3,
    "showPrice": true,
    "currency": "GBP",
    "currencyXml": {
        "currencyId": "GBP",
        "currencyLongSymbol": "GBP",
        "currencyName": "Pound",
        "currencySymbol": "£"
    },
    "paymentType": {
        "id": "cash",
        "shortDescription": "Cash",
        "description": "Cash payment",
        "isDefault": false
    },
    "vehicleCategory": {
        "id": 1,
        "rank": 11,
        "defaultCategoryForCountry": true,
        "capacityPassengers": 4,
        "capacityLargeLuggage": 2,
        "descriptionLong": "Saloon",
        "descriptionShort": "Saloon",
        "capacitySmallLuggage": 2,
        "commonName": "standard_car",
        "name": "Saloon",
        "enabled": true
    },
    "created": "2016-05-11 16:48:14",
    "expiryDate": "2016-05-11 16:58:14",
    "acknowledgedByCompany": false
}
```

`GET /bookings/{id}/details`

HTTP Code   | Description
----------- | -----------
200 | Ok. The details of the booking are returned in the body.
40x | The server could not complete the request correctly for: user not authorized, booking not found, etc.
50x | Server error

## Cancel a booking

`POST /bookings/{id}/cancel`

### Request

```json
{
    "cancellationReason": "Flight cancelled."
}
```

The cancellation API accepts a body with a single text field <code>cancellationReason</code>containing the reason of the cancellation.

### Response

```json
{
    "text": "The booking has been cancelled"
}
```

HTTP Code   | Description
----------- | -----------
200 | Ok. The booking has been cancelled.
40x | The server could not complete the request correctly for: user not authorized, booking not found, etc.
50x | Server error

# Pricing

## Get the price for a journey

`POST /pricing/estimate`

The quote API enables the client to obtain either:

 - a price: for fixed price journeys,

 - or the pricing structure: for a journey that has a metered fare.

### Request

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

# Tracking

## Get the drivers in a defined zone

`GET /tracking/drivers/byBoundingBox`

```json
{
  "vehicleOnRoad": [
    {
      "id": 185911,
      "available": true,
      "driverId": 800,
      "latitude": 51.5598,
      "longitude": -0.0989665,
      "bearing": 254,
      "speed": 0,
      "vehicleCategory": {
        "id": 3,
        "rank": 30,
        "commonName": "saloon",
        "name": "Saloon",
        "capacityPassengers": 6,
        "capacityLargeLuggage": 4,
        "capacitySmallLuggage": 2
      }
    },
    {
      "id": 185924,
      "available": false,
      "driverId": 127,
      "latitude": 52.0152,
      "longitude": -1.1985,
      "bearing": 254,
      "speed": 32.501,
      "vehicleCategory": {
        "id": 3,
        "rank": 30,
        "commonName": "mpv",
        "name": "MPV",
        "capacityPassengers": 6,
        "capacityLargeLuggage": 4,
        "capacitySmallLuggage": 2
      }
    }
  ]
}
```

Returns the list of available drivers within a geographic bounding box with information on:

 - Current availability,

 - Vehicle type currently in use,

 - Bearing (direction) and speed of the vehicle (if available).

### Query parameters

Name | Type | Required | Description
---- | ---- | -------- | -----------
minLat | float | yes | Latitude of the bottom left corner of the geographic bounding box.
minLng | float | yes | Longitude of the bottom left corner of the geographic bounding box.
maxLat | float | yes | Latitude of the top right corner of the geographic bounding box.
maxLng | float | yes | Longitude of the top right corner of the geographic bounding box.

## Get the details of a driver online

`GET /tracking/drivers/{driverId}/{vehicleOnRoadId}`

```json
{
  "latitude": 51.5598,
  "longitude": -0.0989642,
  "speed": 22,
  "bearing": 254,
  "available": false,
  "id": 185912,
  "driverId": 800,
  "vehicleCategory": {
    "id": 3,
    "capacityPassengers": 6,
    "capacityLargeLuggage": 4,
    "capacitySmallLuggage": 2,
    "rank": 30,
    "name": "MPV",
    "commonName": "mpv"
  },
  "timeUntilNextBooking": 1030,
  "company": {
    "orderPhone": "+917760420703",
    "id": 308,
    "name": "CAB Co 1"
  },
  "currentBooking": {
    "id": 392852,
    "date": "2016-05-11 18:53:00",
    "destination": {
      "latitude": 51.4595,
      "longitude": -0.44661
    },
    "origin": {
      "latitude": 51.5598,
      "longitude": -0.0989554
    },
    "duration": 61,
    "distance": 35.716
  },
  "nextBooking": {
    "id": 393249,
    "date": "2016-05-12 12:00:00",
    "destination": {
      "latitude": 51.4704,
      "longitude": -0.452056
    },
    "origin": {
      "latitude": 51.531,
      "longitude": -0.1237
    },
    "duration": 46,
    "distance": 30.819
  }
}
```

Returns additional information for a specific driver currently online.

### Path parameters

Name | Description
---- | -----------
driverId | The id of the connected driver.
vehicleOnRoadId | The id of the current driving session for the driver (every time a driver logs in, a new number is generated).

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>
