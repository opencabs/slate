# Customers

## Get the Customers list

> Response

```json
{
  "nbItems": 8,
  "nbAllItems": 8,
  "customer": [
    {
      "verified": false,
      "phoneNumber": "7700002",
      "customerId": 4664,
      "email": "john.doe@mymail.com",
      "firstName": "john",
      "lastName": "doe",
      "phoneNumberCountryCode": "44",
      "vipScore": 0,
      "noShowCount": 0,
      "hasUserProfile": false,
      "enabled": true,
      "account": {
        "name": "test account",
        "id": 32
      }
    },
    {
      "...":"..."      
    },
    {
      "verified": false,
      "phoneNumber": "7700005",
      "customerId": 4671,
      "email": "john.doe2@mymail.com",
      "firstName": "john",
      "lastName": "doe",
      "phoneNumberCountryCode": "44",
      "vipScore": 0,
      "noShowCount": 0,
      "hasUserProfile": true,
      "account": {
        "name": "test account",
        "id": 32
      },
      "enabled": true
    }
  ]
}
```

`GET /customers`

### Query parameters

All parameters are optional. The maximum number of results returned is 20.

Name | Type | Description
---- | ---- | -----------
`q` | string |
`phoneNumberCountryCode` | string |
`phoneNumber` | string |
`firstName` | string |
`customerId` | integer | Not implemented yet.
`accountId` | integer |
`format` | string | Set 'autocomplete' for a short version of the response (faster).
`sortBy` | string | Partially implemented
`sortOrder` | string | Partially implemented.
`start` | integer |
`max` | integer |


## Create a new Customer

> Request

```json
{
    "firstName": "john",
    "lastName": "doe",
    "phoneNumberCountryCode": "+44",
    "phoneNumber": "7700001",
    "email": "john.doe@mymail.com",
    "accountId": 32
}
```

> Response

```json
{
  "customerId": 4653,
  "firstName": "john",
  "lastName": "doe",
  "phoneNumberCountryCode": "44",
  "phoneNumber": "7700001",
  "email": "john.doe@mymail.com",
  "enabled": true,
  "hasUserProfile": true,
  "verified": false,
  "vipScore": 0,
  "noShowCount": 0,
  "account": {
    "id": 32,
    "name": "John and John LLP"
  }
}
```


`POST /customers`

Enables the creation of a new customer profile.

***Important definitions***

 - `customer`: means a passenger profile. This represents the details of a passenger for a ride and it is not necessarily connected to a `user`;
 - `user`: means an online profile (username + password) that enables a person to connect to the system and interact with it. `Users` can be linked to `customers`, but not only. For example a controller in a cab office has a `user` profile that is not connected to a `customer`.

### Response details

Field | Comments
----- | --------
`enabled` | <i>true</i>: it means the the customer profile (i) can be used when creating bookings and (ii) the customer profile will  appear in the customers search when creating a booking. If a `user` is linked to a `customer` that is not enabled, the user will not be able to place bookings.
`hasUserProfile` | <i>true</i>: it means that the customer is connected to a `user` profile.
`verified` | the phone number is considered safe. The verification of `customers` linked to `users` is done via a short code sent by SMS. If the customer is not linked to a user, then it is generally set as `verified` automatically.
`account` | Short details of the `account` (generally a corporate account) connected to the `customer` (used in particular to display the account name in the views).

## Update an existing Customer

> Request

```json
{
    "firstName": "john",
    "lastName": "doe",
    "phoneNumberCountryCode": "+44",
    "phoneNumber": "7700001",
    "email": "john.doe@mymail.com",
    "accountId": 32,
    "enables": false
}
```

`PUT /customers/{id}`

Update the details of a `customer`.

### Request details

The request requires a JSON body with the fields to update.

Notes:

 - Any missing fields in the JSON are discarded and the old values are kept in the `customer` entity being updated.

 - If the phone number is changed, the customer verification status is reset and the `customer` will need to run again through an SMS verification process. For security reasons `customers` cannot be verified via this API.

Field | Comments
----- | --------
`firstName` | New value for the first name.
`lastName` | New value for the last name.
`email` | New value for the email.
`phoneNumberCountryCode` | New value for the country code.
`phoneNumber` | New value for the phone number (local number, e.g. for a mobile like '+44-7712341234' set `"phoneNumberCountryCode": "44"` and `"phoneNumber": "7712341234"`).
`accountId` | Id of the `Account` to be linked to the `Customer`. Set to '0' to unlink the existing `Account`.
