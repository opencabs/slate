# Pricing

## Get the price for a journey

> Request

```json
{  
   "isAsap":true,  
   "date":"2015-03-03T17:54:00Z",  
   "fareTypeId":"fixed_point_to_point",  
   "clientBookingReference":"12345",
   "journeyTimeMinutes":51,
   "journeyDistance":28.192,
   "origin": {
     "POI":"some poi",
     "addressDetails":"York House",  
     "streetNumber":"262",  
     "address":"Kentish Town Road",  
     "city":"Greater London",  
     "postcode":"NW5 2AA",  
     "country":"UK",
     "latitude":51.55051,  
     "longitude":-0.1407013000000461  
   },
   "bookingStops": [{
       "index": 0,
       "POI": "",
       "streetNumber": "14",
       "address": "Woronzow Road",
       "city": "London",
       "postcode": "NW8 6",
       "country": "UK",
       "latitude": 51.536430,
       "longitude": -0.168620
     }, {
       "index": 1,
       "POI": "",
       "addressDetails": "",
       "streetNumber": "1",
       "address": "Molesford Road",
       "city": "London",
       "postcode": "SW6 4",
       "country": "UK",
       "latitude": 51.473350,
       "longitude": -0.197480
   }],   
   "destination": {
     "POI":"London Heathrow (Terminal 1)",  
     "addressDetails":"",
     "streetNumber":"",  
     "address":"Croydon Rd",  
     "city":"Hayes",  
     "postcode":"UB3 5AP",  
     "country":"UK",
     "latitude":51.4723,  
     "longitude":-0.451795
   },
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
<code>isAsap</code> | yes | If set to true, the result will also include cats
<code>date</code> | yes | In local time with the format yyyy-mm-dd’T’hh:mm:ss’Z’
<code>journeyTimeMinutes</code> | yes | Driving time (in minutes, integer value) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
<code>journeyDistance</code> | yes | Driving distance (in kilometres) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
<code>fareTypeId</code> | yes | Possible values: 'fixed_point_to_point', 'metered', 'as_directed' (not implemented yet)
<code>flightNumber</code> | yes, for airport pickups | Flight number (e.g. BA123) which will enable suppliers to monitor flights for delays and cancellations

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
      "typeId":38,
      "isAsap":true,
      "statusId":0,
      "clientBookingReference":"12345",
      "origin": {
        "POI":"some poi",
        "addressDetails":"Red building",
        "streetNumber":"262",
        "address":"Kentish Town Road",
        "city":"Greater London",
        "postcode":"NW5 2AA",
        "country":"GB",
        "latitude":51.55051,
        "longitude":-0.1407013
      },
      "bookingStops":[  
      ],
      "destination": {
        "POI":"London Heathrow (Terminal 1)",
        "address":"Croydon Rd",
        "city":"Hayes",
        "postcode":"UB3 5AP",
        "country":"UK",
        "latitude":51.4723,
        "longitude":-0.451795
      },
      "journeyDistance":31.766,
      "numPassengers":1,
      "journeyTimeMinutes":38,
      "comments":"some notes",
      "flightNumber":"AB12345",
      "timezoneOffset":0.0,
      "numLargeItems":0,
      "numSmallItems":0,
      "totalCustomerPrice":38.50,
      "timeZone":"Europe/London",
      "date":"2015-03-03 12:28:50"
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

- `quoteId`: if this is available and not null, then a price has been obtained. Otherwise check the <code>responseCode</code> and <code>text</code> fields for more information.

- `price` object contains the quoted price for a journey with fixed price. If the

- `meteredPricingStructure` object (not available in the example) contains the pricing structure for a metered journey;

- `availablePaymentTypes`: the list of the payment types that are acceptable for booking the journey;

- The `origin` fields and `destination` fields (if a destination is required) need to have <u>at least</u>: latitude, longitude, city and (i) POI or (ii) street address with either the streetNumber or the addressDetails.

## Get multiple prices for a journey

> Request

```json
{
    "origin": {
        "poi": "",
        "addressDetails": "3",
        "streetNumber": "",
        "address": "Little portland street",
        "city": "Greater London",
        "postcode": "W1W 7JB",
        "country": "United Kingdom",
        "latitude": 51.5174,
        "longitude": -0.140674
    },
    "bookingStops": [{
        "index": 0,
        "POI": "",
        "streetNumber": "14",
        "address": "Woronzow Road",
        "city": "London",
        "postcode": "NW8 6",
        "country": "GB",
        "latitude": 51.536430,
        "longitude": -0.168620
    }, {
        "index": 1,
        "POI": "",
        "addressDetails": "",
        "streetNumber": "1",
        "address": "Molesford Road",
        "city": "London",
        "postcode": "SW6 4",
        "country": "GB",
        "latitude": 51.473350,
        "longitude": -0.197480
    }],
    "destination": {
        "poi": "London City Airport (LCY)",
        "addressDetails": "",
        "streetNumber": "",
        "address": "Hartmann Road",
        "city": "Royal Docks",
        "postcode": "E16 2PX",
        "country": "UK",
        "latitude": 51.5032,
        "longitude": 0.04994
    },
    "paymentTypeId": "on_account",
    "numPassengers": 1,
    "numSmallItems": 0,
    "numLargeItems": 0,
    "date": "2018-02-12T13:11:00Z",
    "isAsap": false,
    "customer": {
        "firstName": "tudor",
        "lastName": "secri",
        "phoneNumberCountryCode": "33",
        "phoneNumber": "741577391"
    }
}
```

With the search API you can get the prices for a given journey and for each available vehicle category.

 `POST /pricing/search`

### Request

The JSON request example shows the fields required for the pricing search.

Please note that this API currently works only for 'fixed_point_to_point' fares (i.e. no metered fares).

The additional comments below apply:

Parameter | Required | Description
--------- | -------- | -----------
`fareTypeId` | yes | Currently only 'fixed_point_to_point' is supported
`paymentTypeId` | yes | Please see <a href="#payment-types">here</a> for the list of possible payment types
`date` | yes | In local time with the format yyyy-mm-dd’T’hh:mm:ss’Z’
`journeyTimeMinutes` | yes | Driving time (in minutes, integer value) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
`journeyDistance` | yes | Driving distance (in kilometres) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
`customer{}` | no | The customer object is optional and useful only for existing customers (i.e. we do not use the customer data for requests with new or anonymous customers)

### Response

> Response

```json
{
    "prices": [
        {
            "quoteId": 532043,
            "price": {
                "netPrice": 129,
                "vat": 0,
                "totalPrice": 129
            },
            "vehicleCategory": {
                "commonName": "executive_car",
                "code": "EXEC"
            },
            "paymentType": [
                {
                    "id": "on_account",
                    "description": "Account payment (after invoice)",
                    "shortDescription": "Account"
                }
            ],
            "fareType": "fixed_point_to_point",
            "responseCode": "Q200",
            "automaticallyAccepted": false
        },
        {
            "quoteId": 532045,
            "price": {
                "netPrice": 89.3,
                "vat": 0,
                "totalPrice": 89.3
            },
            "vehicleCategory": {
                "commonName": "standard_car",
                "code": "STD"
            },
            "paymentType": [
                {
                    "id": "on_account",
                    "description": "Account payment (after invoice)",
                    "shortDescription": "Account"
                }
            ],
            "fareType": "fixed_point_to_point",
            "responseCode": "Q200",
            "automaticallyAccepted": false
        },
        {
            "quoteId": 532044,
            "price": {
                "netPrice": 100,
                "vat": 0,
                "totalPrice": 100
            },
            "vehicleCategory": {
                "commonName": "mpv",
                "code": "MPV"
            },
            "paymentType": [
                {
                    "id": "on_account",
                    "description": "Account payment (after invoice)",
                    "shortDescription": "Account"
                }
            ],
            "fareType": "fixed_point_to_point",
            "responseCode": "Q200",
            "automaticallyAccepted": false
        }
    ],
    "journeyDetails": {
        "origin": {
            "POI": "",
            "addressDetails": "3",
            "streetNumber": "",
            "address": "Little portland street",
            "city": "Greater London",
            "postcode": "W1W 7JB",
            "country": "United Kingdom",
            "latitude": 51.5174,
            "longitude": -0.140674
        },
        "bookingStops": [
            {
                "index": 0,
                "POI": "",
                "streetNumber": "14",
                "address": "Woronzow Road",
                "city": "London",
                "postcode": "NW8 6",
                "country": "GB",
                "latitude": 51.53643,
                "longitude": -0.16862
            },
            {
                "index": 1,
                "POI": "",
                "addressDetails": "",
                "streetNumber": "1",
                "address": "Molesford Road",
                "city": "London",
                "postcode": "SW6 4",
                "country": "GB",
                "latitude": 51.47335,
                "longitude": -0.19748
            }
        ],        
        "destination": {
            "POI": "London City Airport (LCY)",
            "addressDetails": "",
            "streetNumber": "",
            "address": "Hartmann Road",
            "city": "Royal Docks",
            "postcode": "E16 2PX",
            "country": "UK",
            "latitude": 51.5032,
            "longitude": 0.04994
        },
        "date": "2018-02-12 13:11:00",
        "timeZone": "Europe/London",
        "dateUTC": 1518441060000,
        "timezoneOffset": 0,
        "isAsap": false,
        "numPassengers": 1,
        "numLargeItems": 0,
        "numSmallItems": 0,
        "journeyDistance": 34.729,
        "journeyTimeMinutes": 67,
        "created": "2018-01-16 22:37:38",
        "publisher": {
            "id": 5,
            "shortName": "Operator1",
            "code": "OP1",
            "name": "Operator 1"
        },
        "airportBooking": false
    }
}
```

HTTP Code   | Description
----------- | -----------
200 | The request has been completed correctly. Check the content to know if a price is actually available (see further).
40x | The request has not been completed correctly. Some of the reasons are: incorrect data passed, user not authorized, etc.
50x | Server error

The response contains the details of the journey with the price, with:

- <code>prices[]</code>: array with the prices by vehicle category;
- <code>journeyDetails{}</code>: object confirming the details of the requested journey to be estimated.
