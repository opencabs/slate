---
title: OpenCabs API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='mailto:support@opencabs.com'>Sign Up for a Developer Key</a>

includes:
  - bookings
  - coverage
  - pricing
  - tracking
  - appendices

search: true
---

# Introduction

Welcome to the OpenCabs <b>'Publishers API'</b>

This document describes the API to be used by 3rd parties (publishers) submitting bookings to the platform on behalf of their customers.

The main difference with the “Customers APIs” is that the end customer is not required to register within the OpenCabs platform. It's a user authorized by the publisher who is in charge of creating the booking for the end customer.

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
