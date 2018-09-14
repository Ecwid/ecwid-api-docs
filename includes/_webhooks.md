# Webhooks

A webhook is an HTTP POST request that occurs when something happens. 

In other words, it’s a simple event notification via HTTP POST. Ecwid uses webhooks to notify your application in real time about events in the merchant store.

**Table of contents**

Use these links to quickly get started with webhooks: 

- [Set up webhooks for your app](https://developers.ecwid.com/api-documentation/set-up-webhooks)
- [Troubleshooting webhooks](https://developers.ecwid.com/api-documentation/processing-webhooks#troubleshooting-webhooks)
- [Webhook structure](https://developers.ecwid.com/api-documentation/webhook-structure)
- [Webhooks best practices](https://developers.ecwid.com/api-documentation/webhooks-best-practices)

<aside class="notice">
Don’t use webhooks themselves as actionable items – please see the <a href="https://developers.ecwid.com/api-documentation/webhooks#processing-webhooks">Processing Webhooks</a> notes below for details on working with webhooks.
</aside>

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

**How to use webhooks**

This is how your application can use webhooks:

* Receive a request from Ecwid every time a new product is created or an existing product is changed so you can synchronize the store catalog with your local database. Get the updated product data from Ecwid REST API and make your app in sync in one HTTP request instead of downloading the whole catalog.
* Get notified about every new order in the store so you can send a custom email or text message, generate a custom receipt, or subscribe the customer to your newsletter. Once webhook is received, get order details from Ecwid REST API and continue your app flow

**What entities are supported**

You can receive webhooks about these store entities: 

- Abandoned sale (created, updated, deleted)
- Order (created, updated, deleted)
- Product (created, updated, deleted)
- Category (created, updated, deleted)
- Customer (created, updated, deleted)
- Store profile (updated, subscription plan updated)
- Application (installed, subscription status updated, uninstalled)

[Learn more about webhooks and supported events](https://developers.ecwid.com/api-documentation/webhooks-overview)

## Webhooks overview

### How webhooks work

In a nutshell, webhooks in Ecwid work this way:

* Ecwid will use the `Webhook URL` from your application details to send any available webhooks to that single URL
* When a user (merchant) installs your application, the webhooks for this store are automatically enabled
* Each supported event in the store (e.g. new order is placed) triggers an HTTP POST request to the URL your specified
* Your application receives the requests and replies with `200 OK` to identify that it's received
* Then your app processes the webhook request and performs further steps to handle the event

<aside class="notice">
Don’t use webhooks themselves as actionable items – please see the <a href="https://developers.ecwid.com/api-documentation/processing-webhooks">Processing Webhooks</a> notes below for details on working with webhooks.
</aside>

### Supported events

The following events are supported:

#### Orders

* New order was placed
* Order was updated
* Order was deleted
* Unfinished order was created
* Unfinished order was updated
* Unfinished order was deleted

`order.updated` and `unfinished_order.updated` events are triggered when any changes are made to an unfinished or completed order. [Orders endpoint](#orders) allows you to control and get information about unfinished and completed orders in a store. 

<aside class="notice">
Access scope required: <strong>read_orders</strong> (see <a href="https://developers.ecwid.com/api-documentation/external-applications#access-scopes">Access scopes</a>)
</aside>

#### Products

* New product was created
* Product was updated
* Product was deleted

`product.updated` events are triggered when any part of a product is updated: quantity, categories assigned, product options, variations, attributes, images, pricing, etc. [Products endpoint](#products) allows you to control and get information about products in a store. 

<aside class="notice">
Access scope required: <strong>read_products</strong> (see <a href="https://developers.ecwid.com/api-documentation/external-applications#access-scopes">Access scopes</a>)
</aside>

#### Categories

* New category created
* Category was updated
* Category was deleted

`category.updated` events are triggered when any [part of category](https://developers.ecwid.com/api-documentation/categories#get-category) is updated: parentId, order by index, category image, name, product list, description, enabled status. [Category endpoint](https://developers.ecwid.com/api-documentation/categories) allows you to control and get category information in a store.

<aside class="notice">
Access scope required: <strong>read_catalog</strong> OR <strong>create_catalog</strong> OR <strong>update_catalog</strong> (see <a href="https://developers.ecwid.com/api-documentation/external-applications#access-scopes">Access scopes</a>).
</aside>

#### Application

* Application was installed
* Application subscription status was updated
* Application was deleted

[Application endpoint](#application) allows you to check status of your application.

#### Profile

* Store information updated
* Store subscription plan was updated

[Store profile endpoint](#store-information) allows you to get and update store information. 

#### Customer

* Customer was created
* Customer was updated
* Customer was deleted

[Customers endpoint](#customers) allows you to get and update customer information.

<aside class="notice">
Access scope required: <strong>read_customers</strong> (see <a href="https://developers.ecwid.com/api-documentation/external-applications#access-scopes">Access scopes</a>)
</aside>

## Set up webhooks

To start sending webhooks to an app, Ecwid needs this information: 

- [Webhook URL](https://developers.ecwid.com/api-documentation/set-up-webhooks#set-webhook-url)
- [Webhook events](https://developers.ecwid.com/api-documentation/set-up-webhooks#set-webhook-events)

[Provide Ecwid team](/contact) with webhook URL and webhook events to set for your application. 

Learn more about setting up webhooks below.

### Set webhook URL

After you [registered your application](/register) with Ecwid, please contact us and provide a **single webhook URL** for all events your app subscribed to.

Ecwid will send a request to this URL each time a supported event occurs. To enable or modify webhooks for existing application, please contact us as well.

<aside class='notice'>
If your application is for public use, the request URL must be working <strong>via HTTPS</strong>. Also, the certificate can only be from <strong>trusted CA's and not self-signed</strong>.
</aside>

<aside class='notice'>
If you are planning to use <strong>specific ports in your URL</strong>, make sure you are using: '80' or '443', or anything between '1024:65535' ports. Otherwise, the webhooks <strong>will not be sent to your URL</strong>. 
</aside>

### Set webhook events

There are several types of events in the store that Ecwid can notify your application about, check out [Event types](https://developers.ecwid.com/api-documentation/webhooks-overview#supported-events) section for more details.

Please specify the exact event types you wish to be notified about upon registering your application or [contact us](/contact) if you already have an app.

### Set custom HTTP headers (optional)

You are also able to specify your custom HTTP headers to be provided by Ecwid when sending webhooks to your URL. If you want to add custom headers to your app, please [contact us](/contact). Learn more about [custom HTTP headers](https://developers.ecwid.com/api-documentation/webhook-structure#request-headers)

### Get access

Each application requests access scopes from a store owner. The same set of access scopes is used to send webhooks to your app.

To be notified of updates to products, make sure your app has `read_catalog` access scope. The `read_orders` scope allows to get order-related webhooks and so on. 

See [supported events](https://developers.ecwid.com/api-documentation/webhooks-overview#supported-events) for more details. 



## Webhook structure


### Webhook request body

> Webhook header example

```http
POST https://www.myapp.com/callback?eventtype=order.updated HTTP/1.1
Host: www.myapp.com
Content-Type: application/json; charset=UTF-8
Content-Length: 243
Cache-Control: no-cache
X-Ecwid-Webhook-Signature: MeV28XtFal4HCkYFvdilwckJinc6Dtp4ZWpPhm/pzd4=
```

> Unfinished order created webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":-1, // order number of the unfinished order (for older stores)
  "eventType":"unfinished_order.created"
}
```

> Unfinished order updated webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":-1, // order number of the unfinished order (for older stores)
  "eventType":"unfinished_order.updated"
}
```

> Unfinished order #105 deleted webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":-1, // order number of the unfinished order (for older stores)
  "eventType":"unfinished_order.deleted"
}
```


> Order #100 created webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":103, // this is the number of the placed order
  "eventType":"order.created",
  "data":{
    "newPaymentStatus":"PAID",
    "newFulfillmentStatus":"SHIPPED"
  }
}
```

> Order #103 updated webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":103, // this is the number of the updated order
  "eventType":"order.updated",
  "data":{
    "oldPaymentStatus":"PAID",
    "newPaymentStatus":"PAID",
    "oldFulfillmentStatus":"PROCESSING",
    "newFulfillmentStatus":"SHIPPED"
  }
}
```

> Order #102 deleted webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":102, // this is the number of the deleted order
  "eventType":"order.deleted"
}
```

> Product with ID 667251253 created webhook body example

```json
{
  "eventId":"08a78904-4c1a-0aa0-953a-2e33c56236f1",
  "eventCreated":1469429915,
  "storeId":1003, 
  "entityId":667251253, // this is the id of the created product
  "eventType":"product.created"
}
```

> Product with ID 66722483 updated webhook body example

```json
{
  "eventId":"08a78904-0aa0-4c1a-953a-2e33c56236f0",
  "eventCreated":1469429912,
  "storeId":1003, 
  "entityId":66722483, // this is the id of the updated product
  "eventType":"product.updated"
}
```

> Product with ID 667251253 deleted webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1469429915,
  "storeId":1003, 
  "entityId":667251253, // this is the id of the deleted product
  "eventType":"product.deleted"
}
```

> Category with ID 66722483 created webhook body example

```json
{
  "eventId":"08a78904-0aa0-4c1a-953a-2e33c56236f0",
  "eventCreated":1469429912,
  "storeId":1003, 
  "entityId":66722483, 
  "eventType":"category.created"
}
```

> Category with ID 66722483 updated webhook body example

```json
{
  "eventId":"08a78904-0aa0-4c1a-953a-2e123c236f0",
  "eventCreated":1469429912,
  "storeId":1003, 
  "entityId":66722483, 
  "eventType":"category.updated"
}
```

> Category with ID 66722483 deleted webhook body example

```json
{
  "eventId":"08a78904-0aa0-4c1a-953a-2e33c562336f0",
  "eventCreated":1469429912,
  "storeId":1003, 
  "entityId":66722483, 
  "eventType":"category.deleted"
}
```

> Application was installed webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":"1003", // this is the id of a store that installed the app
  "eventType":"application.installed"
}
```

> Application subscription status changed webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":"1003", // this is the id of a store where app subscription status was changed
  "eventType":"application.subscriptionStatusChanged",
  "data":{
    "oldSubscriptionStatus":"TRIAL",
    "newSubscriptionStatus":"ACTIVE"
    }
}
```

> Application was deleted webhook body example

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1234567,
  "storeId":1003,
  "entityId":"1003", // this is the id of a store that deleted the app
  "eventType":"application.uninstalled"
}
```

> Store has switched from Business to Unlimited plan

```json
{
  "eventId":"80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated":1494503041,
  "storeId":1421002,
  "entityId":1421002, // this is the id of a store that switched to a different Ecwid plan
  "eventType":"profile.subscriptionStatusChanged",
  "data":{
    "oldSubscriptionName":"ECWID_BUSINESS",
    "newSubscriptionName":"ECWID_UNLIMITED"
  }
}
```

> Customer was created 

```json
{
  "eventId": "80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated": 7891234567,
  "storeId": 1003,
  "entityId": 1663830, // customer ID
  "eventType": "customer.created",
  "data": {
    "customerEmail":"user@example.com" // customer email
  }
}
```

> Customer was updated

```json
{
  "eventId": "80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated": 7891234567,
  "storeId": 1003,
  "entityId": 1663830, // customer ID
  "eventType": "customer.updated",
  "data": {
    "customerEmail":"user@example.com" // customer email
  }
}
```

> Customer was deleted

```json
{
  "eventId": "80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated": 7891234567,
  "storeId": 1003,
  "entityId": 1663830, // customer ID
  "eventType": "customer.deleted",
  "data": {
    "customerEmail":"user@example.com" // customer email
  }
}
```

> Store information updated

```json
{
  "eventId": "80aece08-40e8-4145-8764-6c2f0d38678",
  "eventCreated": 7891234567,
  "storeId": 1003,
  "entityId": 1003
  "eventType": "profile.updated"
}
```

The request body is a JSON object with the following fields:

Name | Type | Description
---- | -----| -----------
**eventId** | number | Unique webhook ID
**eventType** | string | Type of the occurred event.
**eventCreated** | timestamp | Unix timestamp of the occurred event.
**storeId** | number | Store ID of the store where the event occured.
**entityId** | number | Id of the updated entity. Can contain `productId`, `categoryId`, `orderNumber`, `storeId` depending on the `eventType`. Is `-1` for `unfinished_order.*` webhook events for newer stores
data | \<*WebhookData*\> | Optional field. Describes changes made to an entity. Is provided for `order.*` and `application.subscriptionStatusChanged` event types.

#### WebhookData

Name | Type | Description
---- | -----| -----------
oldPaymentStatus | string | Payment status of **order** before changes occurred
newPaymentStatus | string | Payment status of **order** after changes occurred
oldFulfillmentStatus | string | Fulfillment status of **order** before changes occurred
newFulfillmentStatus | string | Fulfillment status of an **order** after changes occurred
oldSubscriptionName | string | Previous *Ecwid store premium plan* name
newSubscriptionName | string | New *Ecwid store premium plan* name
oldSubscriptionStatus | string | Previous **application** subscription status before changes occurred
newSubscriptionStatus | string | New **application** subscription status after changes occurred
customerEmail | string | Email of a **customer**

<aside class='note'>
Fields sent with all webhook requests are highlighted in <strong>bold</strong>.
</aside>

<aside class="notice">
Don’t use webhooks themselves as actionable items – please see the <a href="https://developers.ecwid.com/api-documentation/webhook-structure#processing-webhooks">Processing Webhooks</a> notes below for details on working with webhooks.
</aside>

The `eventType` field is also duplicated in the request GET parameters. This allows you to filter our the webhooks you don't want to handle. For example, if you only need to listen to order updates, you can just reply `200 OK` to every request containing products updates, e.g.  `https://www.myapp.com/callback?eventtype=product.updated`, and avoid further processing. 

#### Event types
* `unfinished_order.created` Unfinished order is created
* `unfinished_order.updated` Unfinished order is updated
* `unfinished_order.deleted` Unfinished order is deleted
* `order.created` New order is placed
* `order.updated` Order is changed
* `order.deleted` Order is deleted
* `product.created` New product is created
* `product.updated` Product is updated
* `product.deleted` Product is deleted
* `category.created` New category is created
* `category.updated` Category is updated
* `category.deleted` Category is deleted
* `application.installed` Application is installed
* `application.uninstalled` Application is deleted
* `application.subscriptionStatusChanged` Application status changed
* `profile.updated` Store information updated
* `profile.subscriptionStatusChanged` Store premium subscription status changed
* `customer.created` Customer is created
* `customer.updated` Customer is updated
* `customer.deleted` Customer is deleted

All order-related webhooks require `read_orders` access scope, all product- and category-related webhooks require `read_catalog` access scope, all customer-related webhooks require `read_customers` [access scope](https://developers.ecwid.com/api-documentation/external-applications#access-scopes) to be requested from the store

#### Q: What is an unfinished order and how it works? 

The typical process of ordering in Ecwid store is:

- customer adds products to cart
- goes to the checkout pages
- fills in shipping and billing details
- goes to payment processor and pays for order
- customer returns to store

An [unfinished order](https://support.ecwid.com/hc/en-us/articles/115004446065) (or abandoned cart) is created when a customer goes to payment processor or order confirmation page. In case if customer pays for their order, then it is placed among other successful orders in store sales history. However, if customer fails to make a payment and never completes the order, then it will stay as unfinished order. 

There is a separate tab in Ecwid control panel > My Sales > Abandoned carts where merchants can see all those orders and their details.

`unfinished_order.*` webhook event types are providing timely updates about unfinished orders in Ecwid store. 

For newer stores, the `entityId` field contains `-1` value as the order number hasn't been assigned yet. For older stores, the `entityId` field contains actual order number that your app can reference the order with. 

Here's how the process works in technical terms:
 
- when customer goes to payment processor, `unfinished_order.created` webhook is sent
(if customer was able to pay for an order within **1 second** of that,`order.created` is sent instead)

- if customer paid later for their order, Ecwid sends `order.created` webhook after it happens

Webhooks about unfinished orders can be a great help for applications, which goal is track cart abandonment and get back those lost sales. 

We recommend the following workflow: 

- app recieves `unfinished_order.created` webhook about order A
- app waits for `order.created` webhook about order A for 5-10 minutes
- in case if there was no `order.created` webhook received, consider this order unfinished

After that, application can send an email to that person, following up on that unfinished order or provide some other functionality.

#### Q: When 'unfinished_order.deleted' event type is sent?

When a merchant deletes an unfinished order in their Ecwid control panel, `hidden` field in order details becomes `true`. After that update `unfinished_order.updated` webhook is sent. So the details of this order are still available to be accessed [via Ecwid API](https://developers.ecwid.com/api-documentation/orders#get-order-details).

To completely delete an unfinished order, make a delete order request to Ecwid API. If your app is set up to receive `unfinished_order.deleted` webhooks, Ecwid will send that webhook to endpoint of your application.

#### Q: How can I know if order status was changed?
Webhooks in Ecwid have a specific field for that: `data`. This field provides information about changes to both payment and fulfilment status of orders. 

Contents of `data` field also lets you know the details about old status (before the changes) and the new one (after the changes) at all times. For example, your application can send a note to your warehouse if it received a webhook about order payment status changes from `Awaiting payment` to `Paid`.

#### Q: How can I know the current subscription status of a store?

Once you received `application.subscriptionStatusChanged` webhook, you can make a request to [Application endpoint](https://developers.ecwid.com/api-documentation/application#get-application-status) to get the current subscription status of your app in that store.

### Request headers

Among the other headers, the webhook HTTP request includes the `X-Ecwid-Webhook-Signature` header that can be used to verify the webhook. See more details in the ["Webhooks security"](https://developers.ecwid.com/api-documentation/webhooks-best-practices#webhooks-security) below.

#### Custom HTTP Headers

> Custom webhook headers example

```http
POST https://www.myapp.com/callback?eventtype=order.updated HTTP/1.1
Host: www.myapp.com
Content-Type: application/json; charset=UTF-8
Content-Length: 243
Cache-Control: no-cache
X-Ecwid-Webhook-Signature: MeV28XtFal4HCkYFvdilwckJinc6Dtp4ZWpPhm/pzd4=
Custom-Webhook-Header-Name: custom webhook header value
```

Webhooks also allow you to specify custom headers that Ecwid will use when sending a webhook to your endpoint. For example, if you have only one endpoint and several applications sending webhooks to that endpoint, you may want to specify a custom HTTP header to know the application this webhook was sent to.

The custom HTTP headers specified for webhooks will be **added** to the default list of headers Ecwid is sending. In case if a custom webhook HTTP header is duplicating a default header, Ecwid will send **both the default and custom header** in a request.

To setup custom HTTP headers for your app webhooks, please [contact us](/contact).

## Processing webhooks

Webhooks are a way to get notified about events in an Ecwid store. So they shouldn't be used as actionable items. See processing flow examples below.

`product.updated` webhook processing flow example:

- Receive a webhook from Ecwid
- Respond with 200OK HTTP status
- Parse the request information: identify `productId` from `entityId`, make sure the `eventType` is correct and get `storeId` value
- Get up-to-date information about the product from Ecwid via [the Ecwid REST API](#rest-api-reference)
- Update your local database with latest stock quantity of that product

`order.updated` webhook processing flow example: 

- Receive a webhook from Ecwid
- Respond with 200OK HTTP status
- Parse the request information: identify `orderNumber` from `entityId`, make sure the `eventType` is correct and get `storeId` value, check if `newPaymentStatus` value is "PAID"
- Get order details from Ecwid via [the Ecwid REST API](#rest-api-reference)
- Send order details to a fulfillment center
- Set order fulfillment status as "PROCESSING" via [the Ecwid REST API](#rest-api-reference)

See also [the webhooks best practices](#webhooks-best-practices) on webhooks security and processing examples.

### Responding to webhooks

When a webhook is sent to your URL, your app must return a `200 OK` HTTP status code in reply to a webhook. This acknowledges Ecwid that you received the webhook. 

Any other response (e.g. `3xx`), will indicate that the webhook is not received. In this case, we will re-send a webhook every 15 minutes for 48 hours until the time is out or the resource responds with 200OK HTTP response code. Once the 48 hour limit is reached, the webhook will be removed from the queue and will not be sent again.

### Troubleshooting webhooks

#### Q: Webhooks to my endpoint are not delivered. Why?

There are several factors that can prevent you from getting webhooks from Ecwid. 

**Webhooks are added to an existing application**

If you registered your app without webhooks functionality and added it later on, please make sure to reinstall the app for the changes to apply faster in Ecwid.

**Application is not installed**

When you are expecting a webhook from Ecwid after a certain event, please make sure that you have a registered app that has all webhook details specified. [More details](#setting-up-webhooks)

**Application is missing the right webhook events**

Webhooks in Ecwid are sent to an endpoint of an application only when event occurs and the application is set up for those webhook event types. Make sure that you specified webhook event types that you want to receive when setting up webhooks. [More details](#setting-up-webhooks)

**Application is missing access scopes**

Webhooks also depend not only on the event types specified for them, but also for access scopes that your application has. For example, if you want to receive webhooks about new orders, then your app must request `read_orders` access scope from a store.

**Your endpoint is not responding to requests with 200 OK status**

When an event occurs, Ecwid will immediately try to send a webhook to your endpoint. However, if it fails to respond with 200OK response status or it has errors in the response (from PHP code, for example). Ecwid will not be able to deliver this webhook to your endpoint, because it failed to accept it.

This case also includes situations when your endpoint is performing redirects to other pages. In that case, it responds with 301 HTTP code, thus the webhook isn't delivered properly.

Try sending a dummy POST request to your webhooks URL and see if it accepts the request correctly with 200OK HTTP status code. [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en) is great for testing the requests.

**Ecwid can't access your endpoint**

When [setting up webhooks](#setting-up-webhooks), make sure that your endpoint is publicly accessible by any resource (no local servers, etc.). This way, Ecwid services can successfully send and deliver POST requests to your endpoint.

If you made sure that all of the above steps are not concerning your case, please contact us and we will help you.

#### Q: I receive webhooks for events that already happened. Why?

When your application isn't sending `200OK` HTTP response back to Ecwid, the webhook event counts by Ecwid as not delivered. Such webhooks are sent again to the application URL according to the scheme described in [Responding to webhooks](https://developers.ecwid.com/api-documentation/processing-webhooks#responding-to-webhooks). 

Hence, if your app somehow got the webhook the first time, it will receive more of the same webhooks until Ecwid receives back the `200OK` response from your app or the 48-hour timeout is over.

So please make sure that your webhooks endpoint always respods with `200OK` HTTP response upon successfully receiving the webhook from Ecwid.

#### Q: I receive webhooks with a delay. Why? 

Webhooks in Ecwid are sent out to many applications every minute. We have several servers processing the delivery of webhook requests. 

When there are too many events that need to be processed, a queue is created to deliver those requests to their respective URLs. Also, sometimes the application URLs are not responding in a proper way, so [we perform retries](https://developers.ecwid.com/api-documentation/processing-webhooks#responding-to-webhooks) to make sure we deliver the webhooks. 

All these factors can influence on when your app is going to get the webhook. So it may be delivered with a delay, which size depends on the load of our servers.

## Webhooks best practices

### Webhooks security

We recommend verifying each webhook request to make sure it comes from Ecwid and not altered or corrupted during transmission. You can do that by validating the webhook signature coming with each webhook.


**Where can I find the signature?**

A webhook signature is sent in the `X-Ecwid-Webhook-Signature` HTTP header with each webhook request


**How is the signature generated** 

The signature is an encoded string generated by concatenating the following webhook data (delimiter is a dot `.`):

* eventCreated (webhook event timestamp)
* eventId (webhook event ID)

The resulting string is encoded using HMAC SHA-256 and using `client_secret` as the shared secret key.

**How to validate the signature**

To verify a webhook in your appliciation:

1. Get the signature from the request headers
2. Get `eventCreated` and `eventId` values from the request body
3. Encode the string *'{eventCreated}.{eventId}'* using HMAC SHA256 (using `client_secret` as the shared secret key) and pass it through Base64 encoding
4. Compare the resulting string with the received webhook signature. 


See the example in the [webhook processing example code](https://developers.ecwid.com/api-documentation/webhooks-best-practices#webhook-processing-example). 


### Webhook processing example

> Webhooks processing PHP example

```php
<?php 
// Get contents of webhook request
$requestBody = file_get_contents('php://input');
$client_secret = 'abcde123456789'; // your client_secret value sent to you after the app registration

// Parse webhook data
$decodedBody = json_decode($requestBody, true);

$eventId = $decodedBody['eventId'];
$eventCreated = $decodedBody['eventCreated'];
$storeId = $decodedBody['storeId'];
$entityId = $decodedBody['entityId'];
$eventType = $decodedBody['eventType'];
$data = $decodedBody['data'];

// Reply with 200OK to Ecwid
http_response_code(200);

// Filter out the events we're not interested in
if ($eventType !== 'order.updated') {
    exit;
}

// Continue if eventType is order.updated
// Verify the webhook (check that it is sent by Ecwid)
foreach (getallheaders() as $name => $value) {
    if ($name == "X-Ecwid-Webhook-Signature") {
        $headerSignature = "$value";
        
        $hmac_result = hash_hmac("sha256", "$eventCreated.$eventId", $client_secret, true);
        $generatedSignature = base64_encode($hmac_result);
        
        if ($generatedSignature !== $headerSignature) {
            echo 'Signature verification failed';
            exit;
        }
  }
}

// Handle the event
// ...

?>
```

Here's an example of implementing all of the above described guidelines and recommendations in order to process webhooks from Ecwid in the most efficient way.
