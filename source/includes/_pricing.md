# Pricing

## Get the price for a journey

> Request

```json
{  
   "isAsap":true,  
   "date":"2015-03-03T17:54:00Z",  
   "fareTypeId":"fixed_point_to_point",  
   "clientBookingReference":"12345",
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
   "vehicleCategoryCommonName": "standard_car",  
   "customer":{  
      "firstName":"John",  
      "lastName":"Doe",  
      "phoneNumberCountryCode":"44",  
      "phoneNumber":"12345"  
   }  
 }
```

The quote API enables the client to obtain either:

 - a price: for fixed price journeys,

 - or the pricing structure: for a journey that has a metered fare.

 `POST /pricing/estimate`

### Request

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

> Response

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
      "clientBookingReference":"12345",
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

- The `origin` fields and `destination` fields (if a destination is required) need to have <u>at least</u>: latitude, longitude, city and (i) POI or (ii) street address with either the streetNumber or the addressDetails.
