# Tracking

## Get the drivers in a defined zone

`GET /tracking/drivers/byBoundingBox`

> Response

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

> Response

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

## Get the drivers for job request

> Response

```json
{
    "vehicleOnRoad": [
        {
          "id": 186452,
          "bearing": 347.12,
          "latitude": 51.5596,
          "longitude": -0.0988429,
          "driverId": 800,
          "speed": 0,
          "available": false,
          "vehicleCategory": {
                "id": 3,
                "commonName": "mpv"
            },
            "currentBooking": {
                "id": 394449,
                "date": "2016-05-19 00:25:45",
                "destination": {
                    "latitude": 51.531,
                    "longitude": -0.1237
                },
                "distance": 4.7,
                "duration": 16
            },
            "nextBooking": {
                "id": 394349,
                "date": "2016-05-19 05:00:00",
                "origin": {
                    "latitude": 51.5604,
                    "longitude": -0.0988141
                },
                "distance": 35.402,
                "duration": 57
            },
            "timeUntilNextBooking": 247,
            "company": {
                "latitude": 51.5309,
                "longitude": -0.117274,
                "orderPhone": "+447720700000",
                "id": 308,
                "name": "CabCo 1"
            }
        },
        {
            "(more results": "here)"
        }
    ]
}
```

`GET /tracking/drivers/availableForImminentJob`

Returns the list of live drivers who are expected to be compatible with an 'imminent journey' request (pickup in less than 90 minutes from now).

### Query parameters

Name | Type | Required | Description
---- | ---- | -------- | -----------
date | string | yes | Pickup date in local time at the format `yyyy-MM-ddTHH:mm:ss` (e.g. '2016-05-19T01:15:00')
originLat | float | yes | Latitude of the pickup location.
originLng | float | yes | Longitude of the pickup location.
destinationLat | float | no | Latitude of the destination.
destinationLng | float | no | Longitude of the destination.
vehicleCategoryId | float | no | `id` of the required vehicle category for the journey.
duration | float | yes | Expected duration of the journey in minutes.
searchRadius | float | no | Maximum distance of the current driver location from the pickup of the journey (default=25).
max | float | no | Number of results to return (default=10).

Currently the system tests for time overlap between the (1) requested journey and (2.a) the current job of the driver (if any) and (2.b) the next allocated job to the driver (if any).

Note: at this stage the system does not check whether the driver will materially have time to go from current location -> destination of current job -> pickup of requested journey -> destination of requested journey -> pickup of the next allocated job to the driver. This is something that is expected to be done visually by the operator when selecting the available vehicles.

### Response

HTTP Code   | Description
----------- | -----------
200 | Ok. The list of 'live drivers' (e.g. vehicles on road) likely to be compatible with the requested journey is returned.
40x | The server could not complete the request correctly for: user not authorized, booking not found, booking not currently in execution, driver not connected etc.
50x | Server error


## Track the driver of a booking

> Response

```json
{
  "latitude": 51.1559,
  "longitude": -0.158816,
  "locationAccuracy": 24,
  "distanceFromPickup": 1.41327956,
  "vehicleOnRoadId": 945422,
  "vehicleOnRoadDeviceTime": 1463007423000,
  "available": false,
  "status": 40,
  "locationAgeInMillis": 24607
}
```

`GET /bookings/{bookingId}/tracking`

Returns the current position of the driver of a booking.

### Response

HTTP Code   | Description
----------- | -----------
200 | Ok. The current position of the driver is returned in the body.
40x | The server could not complete the request correctly for: user not authorized, booking not found, booking not currently in execution, driver not connected etc.
50x | Server error
