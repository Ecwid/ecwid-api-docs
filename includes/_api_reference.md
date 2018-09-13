# REST API Reference

Ecwid REST API allows your application to manage Ecwid store on behalf of an Ecwid user. Create products, update orders, delete a customer and many many more.

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

**List of available endpoints**:

- [Store information](https://developers.ecwid.com/api-documentation/store-information)
- [Products](https://developers.ecwid.com/api-documentation/products)
- [Categories](https://developers.ecwid.com/api-documentation/categories)
- [Product Variations](https://developers.ecwid.com/api-documentation/product-variations)
- [Product Types](https://developers.ecwid.com/api-documentation/product-types)
- [Orders](https://developers.ecwid.com/api-documentation/orders)
- [Carts](https://developers.ecwid.com/api-documentation/carts)
- [Customers](https://developers.ecwid.com/api-documentation/customers)
- [Customer Groups](https://developers.ecwid.com/api-documentation/customer-groups)
- [Discount Coupons](https://developers.ecwid.com/api-documentation/discount-coupons)
- [Application](https://developers.ecwid.com/api-documentation/application)
- [Starter Site](https://developers.ecwid.com/api-documentation/starter-site)

#### API basics

**RESTful / oAuth2**

Ecwid API is a **RESTful** API with **oAuth2** authentication. As any RESTful service, Ecwid REST API use the standard HTTP codes in requests: 

* `GET` to read store data
* `PUT` to update store data
* `POST` to create entries
* `DELETE` to remove entries

**HTTPS**
All requests are done via HTTPs. Requests via insecure HTTP are not supported.

**UTF-8**
Ecwid API works with UTF-8 encoded data. Please make sure everything you send over in API calls also uses UTF-8.

**Content Type**
All data received from API and submitted to API is JSON, so the content type should be: `application/json;charset=utf-8`

**UTC**
Date/time values returned by Ecwid API are in UTC.

**API Version**
This document describes *Ecwid REST API v.3* 

**API calls limits**

Ecwid REST API has the following limits: 

- 800 requests per minute per store ID + burst 100
- 6000 requests per minute per IP + burst 50

This means that in one minute you can make 6050 requests to Ecwid REST API from one IP in total. 

If you exceed 6050 requests per minute, all following requests face **429 Too Many Requests** response. 

In the next minute and during the whole time you get close to the limitation, we will limit total request number for this IP to 6000 requests (no burst).

Once the request number gets lower, the burst request amount will return and you will be able to make 50 more requests per minute.

When building your app, keep in mind the [usage policy](#usage-policy) for the best experience. 

#### Usage policy

To protect us and our users from abusing, we strongly advise that you optimize your app code to make fewer API requests. For example:

- Cache store data locally if you need to use or display it many times in your app
- If you need to synchronize store data with your database, use Webhooks to get timely updates about changes in a store. More details: [Webhooks](#webhooks)
– To get multiple product details at once (knowing their `productId`s), specify them in a corresponding filter – `productId`. More details: [Searching Products](#search-products)
- To get multiple order details at once (knowing their `orderNumber`s), specify them in a corresponding filter – `orderNumber`. More details: [Searching Orders](#search-orders)

We constantly monitor API activity and servers load on our side to make sure every application uses API properly. In case an app abuses Ecwid API by generating huge amount of requests every day, we'll attempt to get in touch with the developer to talk about the issue. 

Don't worry, you will unlikely face such trouble and even if you do, we will advice on how to fix that. But of course, if the usage is high enough to significantly affect other users of the platform and you don't react on our warnings, we can temporarily disable your application. 

#### Q: How can I make the requests?

You can use any library or software (capable of making HTTP requests) you are familiar with. 

To make a basic API request you will need to know: 

- Ecwid Store ID
- [Access token](#access-tokens) (private or public)

These details are provided at the end of the app installation in an Ecwid store. Ways to get them depend on the app you are using, see the [Authentication section](#authentication) for more details.

Here are some resources we can recommend to get started:

- [Making HTTP requests with cURL in PHP](http://codular.com/curl-with-php)
- [Making HTTP requests with cURL in terminal](https://quickleft.com/blog/command-line-tutorials-curl/)
- [Making HTTP requests with wget in terminal](http://techs-tricks.blogspot.ru/2008/12/test-http-request-with-wget.html)

For testing out the API functionality we can also recommend using [Ecwid API Playground](#api-playground).

#### Using REST API in storefront

When working on a custom storefront functionality, applications can require getting up-to-date catalog information from Ecwid store.

> Get 3 products from Ecwid REST API using JavaScript with public token

```js
var xhttp = new XMLHttpRequest();
var storeId = 1003;
var token = 'public_qKDUqKkNXzcj9DejkMUqEkYLq2E6BXM9';

var requestURL = 'https://app.ecwid.com/api/v3/'+storeId+'/products?limit=3&token='+token;

xhttp.open("GET", requestURL, true);
xhttp.send();

xhttp.onreadystatechange = function() {
  if (xhttp.readyState == 4 && xhttp.status == 200) {
    var apiResponse = xhttp.responseText;
    console.log(apiResponse); // prints response in format of Search Products request in Ecwid API
  }
};
```

We suggest using [public access token](#access-tokens) to get information about: store details, enabled products, enabled categories, variations of enabled products, visible product types and deleted items statistics on demand with Ecwid REST API. Check out example on how to do this on the right.

With public access token you can safely make requests to Ecwid REST API without creating a buffer in a form of a server-side code, which requested information for your client-side code. You can make an Ajax request to Ecwid API with your JavaScript code and have a completely serverless application.

For more information on using custom JavaScript in Ecwid storefront, see [Customize Storefront](/api-documentation/logic-and-code#add-custom-javascript-code) section

#### Static information in Ecwid 

In Ecwid there are different formats and units used for each store. 

To make sure your solution supports them and provides a great service, use the spreadsheet below: 

[Static information in Ecwid](https://docs.google.com/spreadsheets/d/1UAYgxdNFpdUAcZ1AXRGBCT-rE4_wu8jR0yUt6ZNVnso/edit?usp=sharing)





