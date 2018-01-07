---
title: OpenCabs API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='mailto:support@opencabs.com'>Sign Up for a Developer Key</a>

includes:
  - bookings
  - coverage
  - customers
  - pricing
  - tracking
  - appendices

search: true
---

# Introduction

Welcome to the OpenCabs <b>'Publishers API'</b>

This document describes the API to be used by 3rd parties (publishers) submitting bookings to the platform on behalf of their customers.

The main difference with the “Customers APIs” is that the end customer is not required to register within the OpenCabs platform. It's a user authorized by the publisher who is in charge of creating the booking for the end customer.

# Aggregators integration

If your company is an aggregator (such as a travel site), there are two integration types to send the bookings to your suppliers, whether <b>(1) <u>you set</u> the price</b> or <b>(2) if <u>you get</u> the price from the suppliers dynamically</b>.

## (1) Bookings integration, with price set by you

If the prices are calculated by you, the aggregator (typically based on prices pre-agreed with the suppliers), here are the APIs to use:

* <a href="#create-a-booking-without-quote">Posting a booking with set price (i.e. without a quote)</a>
* <a href="#cancel-a-booking">Cancelling a job</a>

## (2) Bookings integration, with live prices from your suppliers

If you want to let your suppliers to set the prices, you then need to call the 'pricing' API to get a price (and a quoteId for the requested journey), so that you can then use this quoteId to post the job.

The list of APIs needed in this case is:

* <a href="#get-the-price-for-a-journey">Getting the price for a journey</a>
* From the service above, you will get a quoteId that you can use to in the <a href="#create-a-booking-with-quote">post a booking with a quote</a> API
* <a href="#cancel-a-booking">Cancelling a job</a>

## Additional useful APIs

In addition to the 'booking integration' to send jobs to your suppliers, the following APIs can be particularly useful:

* <a href="#get-the-details-of-a-booking">Get the details of a job</a> to check the status and the driver details;
* <a href="#track-the-driver-of-a-booking">Track the position of the driver</a> of a job, if the job is in execution.

# Authentication

OpenCabs uses API keys to allow access to the API. You can obtain an ApiKey by contacting us on support@opencabs.com.

OpenCabs expects for the API key to be included in all API requests. You also need to provide a token, which will be used for authenticating your user profile, and the context (i.e. environment) for the execution of the request. This is required as the same user can be connected to several contexts (several publisher accounts for example).

`ApiKey: <your-api-key>`  
`Token: <your-token>`  

OpenCabs supports JSON by default for the body of the requests / responses. You need to specify this in the headers with:

`Content-Type: application/json`

<aside class="notice">
You must replace <code>your-api-key</code>, <code>your-api-key</code> and <code>your-publisher-id</code> with your personal values.
</aside>
