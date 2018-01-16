# Appendices

## Bookings

### Status

Id   | Description
---- | -----------
<code>0</code> | BOOKING_STATUS_NEW
<code>1</code> | BOOKING_STATUS_BOOKED
<code>2</code> | BOOKING_STATUS_IN_EXECUTION
<code>3</code> | BOOKING_STATUS_EXECUTED
<code>4</code> | BOOKING_STATUS_CANCELLED_BY_USER
<code>44</code> | BOOKING_STATUS_CANCELLED_BY_ACCOUNT
<code>45</code> | BOOKING_STATUS_CANCELLED_BY_COMPANY
<code>46</code> | BOOKING_STATUS_CANCELLED_BY_ADMIN
<code>5</code> | BOOKING_STATUS_EXPIRED
<code>6</code> | BOOKING_STATUS_BILLED
<code>71</code> | BOOKING_STATUS_REFUNDED_BY_COMPANY
<code>72</code> | BOOKING_STATUS_REFUNDED_BY_ADMIN
<code>73</code> | BOOKING_STATUS_REFUND_REQUESTED_BY_COMPANY
<code>74</code> | BOOKING_STATUS_REFUND_CANCELLED_BY_ADMIN

## Fare types

Id  | Description
--- | -------
<code>fixed_point_to_point</code> | Fixed fare for standard A to B journey or transfer
<code>metered</code> | Metered fare (in development)
<code>as_directed</code> | As Directed - pickup and duration of service (in development)

## Payment types

Id | Description | Notes
---------- | ------- | --------
<code>cash</code> | Cash payment | This is when the customer pays the driver directly.
<code>on_account</code> | Account payment, i.e. deferred payment after invoice | Use this payment type every time the customer pays you and you pay the supplier (operator/driver).
<code>card_or_epayment</code> | Credit card, debit card, electronic payments | This is for internal use OpenCabs use only (card payments taken via the OpenCabs platform).

## Vehicle categories

CommonName | Pax | Bags-L | Bags-S | Name
--- | --- | ------ | ------ | ----
<code>standard_car</code> | 4 | 2 | 2 | __Saloon__ (Prius)
<code>estate_car</code> | 4 | 4 | 2 | __Estate__ (Prius+)
<code>small_mpv</code> |5 | 4 | 3 | __Small MPV__ (Zafira)
<code>mpv</code> | 6 | 4 | 3 | __Large MPV__ (Galaxy, 6 pax + hand luggage or 5 pax + bags
<code>small_van</code> | 8 | 2 | 2 | __8 seaters__ (Transporter)
<code>executive_car</code> | 4 | 2 | 2 | __Executive Saloon__ (E-Class)
<code>executive_van</code> | 7 | 6 | 3 | __Executive MPV__ (V-Class)
<code>luxury_car</code> | 4 | 2 | 2 | __Luxury Saloon__ (S-Class)

## VehicleOnRoad status

Id   | Description
---- | -----------
<code>10</code> | Clear
<code>20</code> | Busy
<code>40</code> | Driving to pickup
<code>50</code> | At pickup
<code>60</code> | Passenger on board
<code>70</code> | Soon to clear

## Errors

The OpenCabs platform uses primarily the following error codes.

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is not correct.
401 | Unauthorized -- Your API key is wrong or the user does not have access to the resource.
403 | Forbidden -- The operation is not allowed.
404 | Not Found -- The specified resource could not be found.
500 | Internal Server Error -- We had a problem with our server. Try again later.
