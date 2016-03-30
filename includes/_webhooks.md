# Webhooks

# What is webhook

## Webhooks
So what is a webhook? Basically, a WebHook is an HTTP POST request that occurs when something happens, i.e. it's a simple event-notification via HTTP POST. Ecwid uses webhooks to notifiy your application in real time about event in the merchant store. 

This is how your application can use webhooks:

* Receive a request from Ecwid every time a new product is created or an exsiting product is changed to let you synchronize the store catalog with your local database. Get the updated product data and make your app in sync in one HTTP request instead of downloading the whole catalog.
* Get notified about every new order in the store to send a custom email or text message, or generate a custom receipt or subscribe the customer to your newsletter.

<aside class="notice">
Don't use webhooks themselves as actionable items – please see the "Webhooks security" notes below for details on working with webhooks.
</aside>

## How it works in Ecwid

In a nustshell, webhooks in Ecwid work this way:

* In your application settings, you specify an URL, which Ecwid will use to send webhooks to
* When a user (merchant) installs your application, the webhooks for this store are automatically enabled
* Each supported event in the store (e.g. new order is placed) triggers an HTTP POST request to the URL your specified
* Your application receives the requests and replies with `200 OK` to identify that it's received
* Then your app processes the webhook request and performs further steps to handle the event


## Supported events

The following events are supported:

* Unfinished order is created
* Unfinished order is updated
* Unfinished order is deleted
* New order is placed
* Order is changed
* Order is deleted
* New product is created
* Product is updated
* Product is deleted


## Setting up webhooks

Setup process is easy. Once your application has a webhook URL specified in the settings and has a token with appropriate access level for the store, it will receive notifications automatically. More details on these are below.


### Set webhook URL
When you [register your application](#register-your-app-in-ecwid) with Ecwid, please specify a webhook URL – Ecwid will send a request to this URL each time a supported event occurs. To enable webhooks for existing application, please contact us. 

<aside class="notice">
This must be an HTTPS URL. 
</aside>

### Set webhook events
There are several types of events in the store that Ecwid can notify your application about, check out **Event type** section of webhook structure for more details. 

Please specify the exact event types you wish to be notified about upon registering your application or [contact us](http://developers.ecwid.com/contact) if you already have an app.

### Get access
Each application has scope of access that controls the set of store resources and operations permitted for the application. The same set of access scopes is used to determine which events your application can be notified of. To be notified of the product updates, make sure your app has `read_catalog` access to the store. The `read_orders` scope allows to get order webhooks. See [Access scopes](#access-scopes) for more details. 



# Webhook structure


## Webhook request body

> Webhook body example

```http
POST https://www.myapp.com/callback?eventType=order.updated HTTP/1.1
Host: www.myapp.com
Content-Type: application/json; charset=UTF-8
Cache-Control: no-cache

{
"eventId":"123456-1234-1234-1234-123412341234",
"eventCreated":1234567,
"storeId":1003,
"entityId":103,
"eventType":"order.updated",
"data":{
  "oldPaymentStatus":"PAID",
  "newPaymentStatus":"PAID",
  "oldFulfillmentStatus":"PROCESSING",
  "newFulfillmentStatus":"SHIPPED"
  }
}
```

The request body is a JSON object with the following fields:

Name | Type | Description
---- | -----| -----------
eventId | number | Unique webhook ID
eventType | string | Type of the occurred event.
eventCreated | timestamp | Unix timestamp of the occurred event.
storeId | number | Store ID of the store where the event occured.
entityId | number | Id of the updated entity. For example, if a product was updated, then the entityId will be the ID of that product
data | \<OrderStatuses\> | Describes changes made to order. Is provided for `order.updated` and `order.created` event types, regarding order statuses

#### OrderStatuses

Name | Type | Description
---- | -----| -----------
oldPaymentStatus | string | Payment status of order before changes occurred
newPaymentStatus | string | Payment status of order after changes occurred
oldFulfillmentStatus | string | Fulfillment status of order before changes occurred
newFulfillmentStatus | string | Fulfillment status of order after changes occurred

The `eventType` field is also duplicated in the request GET parameters. This allows you to filter our the webhooks you don't want to handle. For example, if you only need to listen to order updates, you can just reply `200 OK` to every request containing products updates, e.g.  `https://www.myapp.com/callback?eventType=product.updated`, and avoid further processing. 

### Event types
* `unfinished_order.created` Unfinished order is created
* `unfinished_order.updated` Unfinished order is updated
* `unfinished_order.deleted` Unfinished order is deleted
* `order.created` New order is placed
* `order.updated` Order is changed
* `order.deleted` Order is deleted
* `product.created` New product is created
* `product.updated` Product is updated
* `product.deleted` Product is deleted

All order related webhooks require `read_orders` access scope and all product related webhooks require `read_catalog` [access scope](#access-scopes) to be requested from the store.

### Q: What is an unfinished order and how it works? 

The typical process of ordering in Ecwid store is:

- customer adds products to cart
- goes to the checkout pages
- fills in shipping and billing details
- goes to payment processor and pays for order
- customer returns to store

An [unfinished order](https://help.ecwid.com/customer/en/portal/articles/1163955-orders#Unfinishedsales) is created when a customer goes to payment processor or order confirmation page. In case if customer pays for their order, then it is placed among other successful orders in store sales history. However, if customer fails to make a payment and never completes the order, then it will stay as unfinished order. 

There is a separate tab in Ecwid control panel > My Sales > Unfinished orders where merchants can see all those orders and their details.

`unfinished_order.*` webhook event types are providing timely updates about unfinished orders in Ecwid store. Here's how the process works in technical terms:
 
- when customer goes to payment processor, `unfinished_order.created` webhook is sent
(if customer was able to pay for an order within **1 second** of that,`order.created` is sent instead)

- if customer paid later for their order, Ecwid sends `order.created` webhook after it happens

Webhooks about unfinished orders can be a great help for applications, which goal is track cart abandonment and get back those lost sales. 

We recommend the following workflow: 

- app recieves `unfinished_order.created` webhook about order A
- app waits for `order.created` webhook about order A for 5-10 minutes
- in case if there was no `order.created` webhook received, consider this order unfinished

After that, application can send an email to that person, following up on that unfinished order or provide some other functionality.

### Q: When 'unfinished_order.deleted' event type is sent?

When a merchant deletes an unfinished order in their Ecwid control panel, `hidden` field in order details becomes `true`. After that update `unfinished_order.updated` webhook is sent. So the details of this order are still available to be accessed [via Ecwid API](#get-order-details).

To completely delete an unfinished order, make a delete order request to Ecwid API. If your app is set up to receive `unfinished_order.deleted` webhooks, Ecwid will send that webhook to endpoint of your application.

### Q: How can I know if order status was changed?
Webhooks in Ecwid have a specific field for that: `data`. This field provides information about changes to both payment and fulfilment status of orders. 

Contents of `data` field also lets you know the details about old status (before the changes) and the new one (after the changes) at all times. For example, your application can send a note to your warehouse if it received a webhook about order payment status changes from `Awaiting payment` to `Paid`.

## Request headers
Among the other headers, the webhook HTTP request includes the `X-Ecwid-Webhook-Signature` header that can be used to verify the webhook. See more details in the ["Webhooks security"](#webhooks-security) below.


# Responding to webhooks

Your app should return a `200 OK` HTTP status code in reply to a webhook. This acknowledges Ecwid that you received the webhook. Any other response (e.g. `3xx`), will indicate that the webhook is not received. In this case, we will re-send a webhook every 15 minutes the maximum retry limit is reached. Once the limit is reached, the webhook is removed from the queue and will not be sent again.



# Webhooks best practices

## Webhooks security

We recommend verifying each webhook request to make sure it comes from Ecwid and not altered or corrupted during transmission. You can do that by validating the webhook signature coming with each webhook.


**Where can I find the signature?**

A webhook signature is sent in the `X-Ecwid-Webhook-Signature` HTTP header with each webhook request


**How is the signature generated** 

The signature is an encoded string generated by concatenating the following webhook data (delimiter is a dot `.`):

* eventCreated (webhook event timestamp)
* eventId (webhook event ID)

The resulting string is encoded using HMAC SHA-256.

**How to validate the signature**

To verify a webhook in your appliciation:

1. Get the signature from the request headers
2. Get `eventCreated` and `eventId` values from the request body
3. Encode the string *'{eventCreated}.{eventId}'* using HMAC SHA256 and pass it through Base64 encoding
4. Compare the resulting string with the received webhook signature. 


See the example in the [webhook processing example code](#webhook-processing-example). 


## Webhook processing example

> Webhooks processing PHP example

```php
<?php 
// Get contents of webhook request
$requestBody = file_get_contents('php://input');
$client_secret = 'abcde123456789';
​
// Parse webhook data
$decodedBody = json_decode($requestBody, true);
​
$eventId = $decodedBody['eventId'];
$eventCreated = $decodedBody['eventCreated'];
$storeId = $decodedBody['storeId'];
$entityId = $decodedBody['entityId'];
$eventType = $decodedBody['eventType'];
$data = ​$decodedBody['data'];

// Reply with 200OK to Ecwid
http_response_code(200);
​
// Filter out the events we're not interested in
if ($eventType != 'order.updated') {
    exit;
}
​
// Continue if eventType is order.updated
// Verify the webhook (check that it is sent by Ecwid)
foreach (getallheaders() as $name => $value) {
    if ($name == "X-Ecwid-Webhook-Signature") {
        $headerSignature = "$value";
        
        $hmac_result = hash_hmac("sha256", "$eventCreated.$eventId", $client_secret, true);
        $generatedSignature = base64_encode($hmac_result);
  ​
        if ($generatedSignature != $headerSignature) {
            echo 'Signature verification failed';
            exit;
        }
  }
}
​
// Handle the event
// ...
​
?>
```

Here's an example of implementing all of the above described guidelines and recommendations in order to process webhooks from Ecwid in the most efficient way.
