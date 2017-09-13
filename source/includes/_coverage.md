# Coverage

## Get the available services

This services is used to test which services area available in a given location, so that you can display the correct UI to the user.

> Response

```json
{
  "coverageFound": true,
  "hailingEnabled": true,
  "preBookingEnabled": true,
  "defaultMinutesForEta": 10,
  "displayBookingCentre": false,
  "fareTypeHailing": [
    {
      "requireDestination": false,
      "id": "metered",
      "description": "metered fare",
      "isDefault": false
    }
  ],
  "vehicleCategoryHailing": [
    {
      "id": 3,
      "rank": 30,
      "capacityPassengers": 6,
      "capacityLargeLuggage": 4,
      "capacitySmallLuggage": 2,
      "commonName": "mpv",
      "name": "MPV"
    },
    {
      "id": 1,
      "rank": 10,
      "capacityPassengers": 4,
      "capacityLargeLuggage": 2,
      "capacitySmallLuggage": 2,
      "commonName": "standard_car",
      "name": "Locomote"
    }
  ],
  "vehicleOnRoad": [
    {
      "vehicleOnRoadId": 185952,
      "available": true,
      "status": 10,
      "latitude": 51.1598,
      "longitude": -0.1989709,
      "locationAccuracy": 30.521,
      "vehicleOnRoadDeviceTime": 1463077414000,
      "vehicleCategory": {
        "id": 3,
        "commonName": "mpv"
      }
    }
  ],  
  "fareType": [
    {
      "requireDestination": true,
      "id": "fixed_point_to_point",
      "description": "fixed fare - for journey (point to point)",
      "isDefault": true
    },
    {
      "requireDestination": false,
      "id": "metered",
      "description": "metered fare",
      "isDefault": false
    }
  ],
  "vehicleCategory": [
    {
      "id": 1,
      "rank": 10,
      "requiredNoticeTime": 10,
      "capacityPassengers": 4,
      "capacityLargeLuggage": 2,
      "capacitySmallLuggage": 2,
      "commonName": "standard_car",
      "name": "Locomote"
    },
    {
      "id": 2,
      "rank": 20,
      "requiredNoticeTime": 10,
      "capacityPassengers": 4,
      "capacityLargeLuggage": 4,
      "capacitySmallLuggage": 2,
      "commonName": "estate_car",
      "name": "Loco X+"
    }
  ]
}
```

`GET /coverage/forLocation/`

### Query parameters

Name | Type | Description
---- | ---- | -----------
`lat` | float | <b>Required</b>. Latitude of the location for which you need to know if online bookings are available.
`lng` | float | <b>Required</b>. Longitude of the location for which you need to know if online bookings are available.
`notice` | float | Notice time, in minutes (i.e. "are there any cars available for a pickup within 30 minutes?").
`toLat` | float | Latitude of the destination of a journey, to know if a journey is covered. In some case only pre-defined routes are allowed from certain locations. Hence even if the coverage on the pickup only had returned that online bookings are available, in some cases it's useful to re-test this after the destination has been entered so that the user can be redirected to the correct UI before actually attempting to get a price and place a booking.
`toLng` | float | <i>(see above)</i>
`paymentTypeId` | string | See the list of possible values on the <a href="#appendices">Appendices</a>
`fareTypeId` | string | See the list of possible values on the <a href="#appendices">Appendices</a>
`vehicleCategoryId` | integer | Id of the specific vehicle category for which we want to know if online bookings are available.
`forPreBooking` | boolean | <i>Default value=true</i>
`forHailing` | boolean | <i>Default value=false</i>

### Response details

Field | Comments
----- | --------
`coverageFound` | <i>true</i>: online booking are expected to be available. The user flow can continue.<br><i>false</i>: the location is not covered and a user message should be displayed.
`hailingEnabled` | <i>true</i>: there are drivers currently online and available to accept bookings.<i>false</i>: bookings sent directly to drivers are not possible as there are no drivers online at the moment. Note: bookings could be possible via the cab offices, with potentially only a few additional minutes for the ETA to pickup.
`preBookingEnabled` | <i>true</i>: advance bookings (via the cab office who will then dispatch the jobs to the drivers) are possible.
`defaultMinutesForEta` | ???.
`fareTypeHailing` | (for hailing) <i>Array</i> with the list of fare allowed fare types for a hailing request. When making the pricing request, use the first fare type with the field `isDefault=true` (or simply the first fare type if there are no <i>default</i> fare types).<br>Use the `requireDestination` property to know if you need to ask the destination to the user in the UI or whether the destination is optional and you can proceed without it.
`vehicleCategoryHailing` | (for hailing) <i>Array</i> with the list of vehicle categories available for hailing in the selected location.
`vehicleOnRoad` | (for hailing) <i>Array</i> with the list of drivers currently online and close to the selected location. You can use this information to display the position of the current drivers or (preferred option) call the `/tracking/liveDrivers/xxx` service (<i>to be documented</i>).
`fareType` | (for pre-bookings) <i>Array</i> with the list of fare allowed fare types for a pre-booking request. When making the pricing request, use the first fare type with the field `isDefault=true` (or simply the first fare type if there are no default fare types).
`vehicleCategory` | (for pre-bookings) <i>Array</i> with the list of vehicle categories available for pre-booking in the selected location.
`displayBookingCentre` | When `coverageFound=false` then some apps offer the option to call an operator to assist the customer. This field tell the user app if the 'call center operator' option is available. If yes, an object `bookingCentre` will appear in the response and will provide the call center information.
