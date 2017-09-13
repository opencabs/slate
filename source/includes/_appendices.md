# Appendices

## Bookings

### Status

Id   | Description
---- | -----------
0 | BOOKING_STATUS_NEW
1 | BOOKING_STATUS_BOOKED
2 | BOOKING_STATUS_IN_EXECUTION
3 | BOOKING_STATUS_EXECUTED
4 | BOOKING_STATUS_CANCELLED_BY_USER
44 | BOOKING_STATUS_CANCELLED_BY_ACCOUNT
45 | BOOKING_STATUS_CANCELLED_BY_COMPANY
46 | BOOKING_STATUS_CANCELLED_BY_ADMIN
5 | BOOKING_STATUS_EXPIRED
6 | BOOKING_STATUS_BILLED
71 | BOOKING_STATUS_REFUNDED_BY_COMPANY
72 | BOOKING_STATUS_REFUNDED_BY_ADMIN
73 | BOOKING_STATUS_REFUND_REQUESTED_BY_COMPANY
74 | BOOKING_STATUS_REFUND_CANCELLED_BY_ADMIN

## Fare types

Id  | Description
--- | -------
fixed_point_to_point | Fixed fare for standard A to B journey or transfer
metered | Metered fare (in development)
as_directed | As Directed - pickup and duration of service (in development)

## Payment types

Id | Description
---------- | -------
card_or_epayment | Credit card, debit card, electronic payments
cash | Cash payment
on_account | Account payment, i.e. deferred payment after invoice

## Vehicle categories

Id  | Code | Pax | Bags-L | Bags-S | Name
--- | --- | --- | ------ | ------ | ----
__1__ | standard_car | 4 | 2 | 2 | __Saloon__ (Prius)
__2__ | estate_car | 4 | 4 | 2 | __Estate__ (Prius+)
__3__ | small_mpv |5 | 4 | 3 | __Small MPV__ (Zafira)
__4__ | mpv | 6 | 4 | 3 | __Large MPV__ (Galaxy, 6 pax + hand luggage or 5 pax + bags
__11__ | small_van | 8 | 2 | 2 | __8 seaters__ (Transporter)
__6__ | executive_car | 4 | 2 | 2 | __Executive Saloon__ (E-Class)
__634__ | executive_van | 6 | 6 | 3 | __Executive MPV__ (V-Class)
__49__ | luxury_car | 3 | 2 | 2 | __Luxury Saloon__ (S-Class)

## VehicleOnRoad status

Id   | Description
---- | -----------
10 | Clear
20 | Busy
40 | Driving to pickup
50 | At pickup
60 | Passenger on board
70 | Soon to clear

## Errors

The OpenCabs platform uses primarily the following error codes.

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is not correct.
401 | Unauthorized -- Your API key is wrong or the user does not have access to the resource.
403 | Forbidden -- The operation is not allowed.
404 | Not Found -- The specified resource could not be found.
500 | Internal Server Error -- We had a problem with our server. Try again later.
