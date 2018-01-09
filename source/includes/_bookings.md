# Bookings

## Create a booking (with quote)

`POST /book`

This API enables to submit new bookings to the platform where you first have obtained a price by calling the <a href="#pricing">pricing</a> service (which returns a quote with a <code>quoteId</code> to be used in this submission).

If you need to post a job without a quote from the system, then please look at the following section <a href="#create-a-booking-without-quote">Create a booking (without quote)</a>.

### Request

> Request

```json
{  
   "quoteId":306007,
   "isAsap":true,
   "paymentTypeId":"cash",
   "clientBookingReference":"12345",
   "date":"2017-03-03T17:54:00Z",
   "journeyTimeMinutes":51,
   "journeyDistance":28.192,
   "originPOI":"Hotel of the North",
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
   "vehicleCategoryCommonName": "standard_car",  
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
<code>quoteId</code> | yes | You can find the <code>quoteId</code> from the response of the <a href="#pricing">Pricing</a> service.
<code>isAsap</code> | yes | This field is used to tell the supplier to send a car as soon as they can, regardless of actual booking time. I.e. if the booking is for 20 minutes from now (which could be the contractual notice time agreed) and the supplier has a car available that can be sent immediately, then if <code>isAsap=true</code> the supplier knows that they have to send the car without waiting.
<code>journeyTimeMinutes</code> | yes | Driving time (in minutes, integer value) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
<code>journeyDistance</code> | yes | Driving distance (in kilometres) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
<code>date</code> | yes | Format: "YYYY-MM-DD’T'hh:mm:ss'Z’". E.g.: "2017-03-03T17:54:00Z".
<code>paymentTypeId</code> | yes | You will get the available payment types in the response from the <a href="#pricing">Pricing</a> service.
origin | yes | The only strictly required fields for the origin are the <code>originLatitude</code> and <code>originLongitude</code>. The other fields are required in the measure that the supplier can uniquely identify the pickup location and meet the customer easily.
destination | <i>see comments</i> | The destination is required for fixed fares ("fareTypeId"="fixed_point_to_point"). If passed, the same validation rules as for the origin apply (i.e. the coordinates are required, and then 'enough' address fields so that the driver can correctly identify the locations).
customer | yes | The <code>customer</code> object is required. If the id matches an existing customer linked to the same details, then the booking will be linked to the existing customer profile; otherwise a new customer profile will be created. The <code>customer</code> needs to have at least: (i) firstName, (ii) phoneNumberCountryCode, (iii) phoneNumber.
Passengers/luggage | yes | <code>numPassengers</code> is required. The luggage fields are optional.
<code>flightNumber</code> | <i>see comments</i> | Required for all pickups at airport terminals.

Note:

 * The 'via point' is now available. Please contact us for more details if needed.

### Response

> Response

```json
{
  "nbAllItems": 1,
  "booking": [
    {
      "id": 444411,
      "clientBookingReference":"12345",
      "timezoneOffset": 1,
      "currency": "GBP",
      "bookingStops": [],
      "showDestination": true,
      "showPrice": true,
      "created": "2017-05-11 14:02:19",
      "originPOI": "London Heathrow (Terminal 1) (LHR)",
      "originAddress": "Croydon Rd",
      "originCity": "Hounslow",
      "originPostcode": "UB3 5AP",
      "originLatitude": 51.4723,
      "originLongitude": -0.451795,
      "destinationPOI": "",
      "destinationAddress": "Little Portland Street",
      "destinationCity": "Greater London",
      "destinationPostcode": "W1W 7JB",
      "destinationLatitude": 51.517353,
      "destinationLongitude": -0.140401,
      "comments": "Meet and Greet pickup.",
      "flightNumber": "AF123",
      "numPassengers": 3,
      "numLargeItems": 2,
      "numSmallItems": 1,
      "vehicleCategory": {},
      "journeyDistance": 31.314,
      "journeyTimeMinutes": 48,
      "paymentType": {},
      "fareType": "fixed_point_to_point",
      "discount": 0,
      "expiryDate": "2017-05-11 14:07:19",
      "originCountry": "UK",
      "isAsap": false,
      "destinationCountry": "GB",
      "destinationStreetNumber": "3",
      "statusId": 0,
      "acknowledgedByCompany": false,
      "isHailing": false,
      "netPrice": 44.5,
      "vat": 0,
      "price": 44.5,
      "totalCustomerPrice": 44.5,
      "airportBooking": true,
      "createdByUserCode": "",
      "automaticallyAccepted": false,
      "currencyXml": {},
      "typeId": 80,
      "vehicleCategoryName": "Saloon",
      "date": "2017-05-18 23:55:00",
      "timeZone": "Europe/London"
    }
  ]
}


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
    "date": "2017-03-03 17:54:00",
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

## Create a booking (without quote)

`POST /book`

This API enables to submit new bookings to the platform in the case where you have pre-agreed arrangements with the suppliers and you do not need to obtain a quote from the system.

Note: you can <u>only</u> post jobs without a quote if your account has been previously authorised to do so.

### Request

> Request

```json
{
    "isAsap": false,
    "preAccepted": false,
    "managingPublisherId": 5,
    "paymentTypeId": "on_account",
    "fareTypeId": "fixed_point_to_point",
    "clientBookingReference":"12345",
    "price": 44.50,
    "journeyTimeMinutes":51,
    "journeyDistance":28.192,
    "date": "2017-05-18T23:55:00Z",
    "originPOI": "London Heathrow (Terminal 1) (LHR)",
    "originAddress": "Croydon Rd",
    "originCity": "Hounslow",
    "originPostcode": "UB3 5AP",
    "originLatitude": 51.4723,
    "originLongitude": -0.451795,
    "originCountry": "UK",
    "destinationPOI": "",
    "destinationStreetNumber": "3",
    "destinationAddress": "Little Portland Street",
    "destinationCity": "Greater London",
    "destinationPostcode": "W1W 7JB",
    "destinationLatitude": 51.517352,
    "destinationLongitude": -0.1404009999999971,
    "destinationCountry": "GB",
    "bookingStops": [{
        "index": 0,
        "POI": "",
        "streetNumber": "14",
        "address": "Woronzow Road",
        "city": "London",
        "postcode": "NW8 6",
        "latitude": 51.536430,
        "longitude": -0.168620,
        "country": "GB"
      }, {
        "index": 1,
        "POI": "",
        "addressDetails": "",
        "streetNumber": "1",
        "address": "Molesford Road",
        "city": "London",
        "postcode": "SW6 4",
        "latitude": 51.536430,
        "longitude": -0.168620,
        "country": "GB"
    }],  
    "comments": "Meet and Greet pickup.",
    "flightNumber": "AF123",
    "numPassengers": "3",
    "numSmallItems": "1",
    "numLargeItems": "2",
    "vehicleCategoryCommonName": "standard_car",  
    "customer": {
        "firstName": "John",
        "lastName": "Doe",
        "phoneNumberCountryCode": "44",
        "phoneNumber": "12345"
    },
    "createdByUserCode": "",
    "forceAnonymousCustomer": 0
}
```

Fields details:

Name   | Required | Comments
----------- | ----------- | -----------
<code>preAccepted</code> | yes | This fields defines whether the job is considered accepted by the supplier or whether the supplier has to explicitly accept it in the dispatch. If you set <code>preAccepted</code>='false' then the booking will be created in status NEW (0). At this point the supplier is notified and if they accept the booking, the status is set to BOOKED (1), otherwise the booking status is set to EXPIRED (5). If you set <code>preAccepted</code>='true' then the booking is created directly in status BOOKED (1) and there are no actions required by the supplier.
<code>managingPublisherId</code> | yes | The id of the supplier you are sending the job to. To get this fields you need to ask your supplier for their publisher code (which they can easily find in the URL when they connect into their dispatch system).
<code>journeyTimeMinutes</code> | yes | Driving time (in minutes, integer value) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
<code>journeyDistance</code> | yes | Driving distance (in kilometres) calculated by the routing system to go from the pickup to the destination (and passing through any 'via points')
<code>price</code> | yes | Price payable to the supplier.
<code>fareTypeId</code> | yes | We currently support only 'fixed_point_to_point' for this API.
<code>paymentTypeId</code> | yes | Only 'cash' and 'on_account' are accepted in this API.
origin | yes | The only strictly required fields for the origin are the <code>originLatitude</code> and <code>originLongitude</code>. The other fields are required in the measure that the supplier can uniquely identify the pickup location and meet the customer easily.
<code>isAsap</code> | yes | This field is used to tell the supplier to send a car as soon as they can, regardless of actual booking time. I.e. if the booking is for 20 minutes from now (which could be the contractual notice time agreed) and the supplier has a car available that can be sent immediately, then if <code>isAsap=true</code> the supplier knows that they have to send the car without waiting.
<code>date</code> | yes | Format: "YYYY-MM-DD’T'hh:mm:ss'Z’". E.g.: "2017-03-03T17:54:00Z".
destination | <i>see comments</i> | The destination is required for fixed fares ("fareTypeId"="fixed_point_to_point"). If passed, the same validation rules as for the origin apply (i.e. the coordinates are required, and then 'enough' address fields so that the driver can correctly identify the locations).
customer | yes | The <code>customer</code> object is required. If the id matches an existing customer linked to the same details, then the booking will be linked to the existing customer profile; otherwise a new customer profile will be created. The <code>customer</code> needs to have at least: (i) firstName, (ii) phoneNumberCountryCode, (iii) phoneNumber.
Passengers/luggage | yes | <code>numPassengers</code> is required. The luggage fields are optional.
<code>flightNumber</code> | <i>see comments</i> | Required for all pickups at airport terminals.

Note:

 * The 'via point' is now available. Please contact us for more details if needed.

### Response

> Response

```json
{  
    "id": 4098755,
    "clientBookingReference":"12345",
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

> Response

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
    "driver": {
        "id": 45583,
        "firstName": "Haroon",
        "lastName": "Kahn",
        "phonePrefix": "44",
        "phone": "7943214321",
        "shortId": "D007"
    },
    "vehicle": {
        "vehicleId": 46258,
        "makeName": "Citroen",
        "numberPlate": "LK62HDE",
        "model": "CITROEN C4",
        "capacity": 6
    },
    "vehicleOnRoad": {
        "vehicleOnRoadId": 1008655,
        "status": 40,
        "available": false,
        "locationAgeInMillis": 61267,
        "latitude": 51.5469,
        "longitude": 0.0731435,
        "locationAccuracy": 16.0
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

If the booking has a driver allocated to it, then the driver information will be returned in the <code>driver</code> JSON node.

If the booking is 'in execution' by the driver ("statusId": 2 - see also <a href="#appendices">Appendices</a>), then the following JSON nodes are also returned:

 * <code>vehicle</code> (self explanatory),
 * <code>vehicleOnRoad</code>: this object contains the information the live position and status of the driver + vehicle combination. The list of possible statuses can be found here in <a href="#vehicleonroad-status">VehicleOnRoad Statuses</a>)

## Cancel a booking

> Request

```json
{
    "cancellationReason": "Flight cancelled."
}
```

> Response

```json
{
    "text": "The booking has been cancelled"
}
```

`POST /bookings/{id}/cancel`

### Request

The cancellation API accepts a body with a single text field <code>cancellationReason</code> containing the reason of the cancellation. This field can be empty.

### Response

HTTP Code   | Description
----------- | -----------
200 | Ok. The booking has been cancelled.
40x | The server could not complete the request correctly for: user not authorized, booking not found, etc.
50x | Server error

## Update the destination

> Request

```json
{
  "destinationPOI":"London Heathrow (Terminal 1)",
  "destinationAddress":"Croydon Rd",
  "destinationCity":"Hayes",
  "destinationPostcode":"UB3 5AP",
  "destinationLatitude":51.4723,
  "destinationLongitude":-0.451795,
  "destinationCountry":"UK",
  "journeyDistance": 27.9,
  "journeyTimeMinutes": 64
}
```

> Response

```json
{
    "<booking JSON object>": "the standard booking JSON object is returned"
}
```

`POST /bookings/{id}/update/destination`

### Request

*Important*: this API is currently enabled only for drivers via the drivers apps.

The update destination API takes body that is a subset of the booking JSON, with:

 * destination fields;
 * journey distance in KM: calculated by the client app calling a mapping API before making the update destination call;
 * journey time (in minutes): this is set by the driver app.

### Response

HTTP Code   | Description
----------- | -----------
200 | Ok. The destination has been update and the booking JSON is returned in the body.
40x | The server could not complete the request correctly for: user not authorized, booking not found, etc.
50x | Server error
