# Errors

## Overview

To indicate status of your request, Ecwid API uses standard HTTP codes and text description. In your application, once you receive a response, you should handle the response code and give a feedback to the user. The following HTTP response code groups are used:

- 200 Success
- 4xx Error is on your side
- 5xx Error is on our side

Below, you will find the common errors and their descriptions.

## 400 Bad Request

```http
HTTP/1.1 400 Wrong parameter 'inStock' value: expected one of 'true', 'false', 'yes', 'no', 'on', 'off', '1', '0
Content-Type application/json; charset=utf-8
```

400 errors are returned when Ecwid cannot parse the request or the request parameters have wrong format. For example, if you submit a text value where it's expected a numeric one or if some required parameter is missing in request.  

## 402 Merchant Needs to Upgrade

```http
HTTP/1.1 402 Please upgrade your account to access API
Content-Type application/json; charset=utf-8
```

When the requested functionality or the API itself is not available on the merchant plan in Ecwid, Ecwid responds with 402 error code. Examples:

- You make an API request to the store on Free plan. API is not available on Free plan hence the error.
- You try to add a product combination using the 'Add combination' method in a store on the Venture plan. The Venture plan doesn't include product combinations (they are available on Business plans) so the API will response with a 402 error. 

<aside class="success">
It's very important to properly handle those Ecwid users who are on free plan. 
</aside>


### How to handle Free Ecwid users in your application
[Ecwid pricing](http://www.ecwid.com/pricing) currently include four tiers: Free, Venture, Business, Unlimited. Unlike many other services, Ecwid doesn't provide a trial – instead, there is a Free plan which stays free forever. Plus it is quite powerful and thus very popular among Ecwid users, even long-time ones. So, in your application, you should be ready that some of your users will be on Ecwid's Free plan and handle them properly. Here are the key points:

1. Ecwid 
Ecwid API is not available on free Ecwid plan.  paid plans and is not available on free Ecwid plan. 

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```


### Errors

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description.

#### HTTP codes

HTTP Status | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
409 | The product with such SKU already exists
402 | The functionality/method is not available on the merchant plan
402 | The merchant plan product limit is reached
404 | Some of the linked entities in the request doesn't exist. For example, the product class is not found
