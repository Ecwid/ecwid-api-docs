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
* Then your app processes the webhook request and perform additional API pull requests to detect what was changed in the store


## Supported events

The following events are supported:

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


### Get access
Each application has scope of access that controls the set of store resources and operations permitted for the application. The same set of access scopes is used to determine which events your application can be notified of. To be notified of the product updates, make sure your app has `read_catalog` access to the store. The `read_orders` scope allows to get order webhooks. See [Access scopes](#access-scopes) for more details. 



# Webhook structure


## Webhook request body

> Webhook body example

```
POST https://www.myapp.com/callback?eventType=product.updated

{
  "eventId": 12345,
  "eventType": "product.updated",
  "eventCreated": 1230329329,
  "storeId": 1003,
  "entityId": 130030039,
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


The `eventType` field is also duplicated in the request GET parameters. This allows you to filter our the webhooks you don't want to handle. For example, if you only need to listen to order updates, you can just reply `200 OK` to every request containing products updates, e.g.  `https://www.myapp.com/callback?eventType=product.updated`, and avoid further processing. 


## Request headers
Among the other headers, the webhook HTTP request includes the `X-Ecwid-Webhook-Signature` header that can be used to verify the webhook. See more details in the ["Webhooks security"](#webhooks-security) below.


# Responding to webhooks

Your app should return a `200 OK` HTTP status code in reply to a webhook. This acknowledges Ecwid that you received the webhook. Any other response (e.g. `3xx`), will indicate that the webhook is not received. In this case, we will re-send a webhook every 15 minutes the maximum retry limit is reached. Once the limit is reached, the webhook is removed from the queue and will not be send again.



# Webhooks security

<aside class="notice">
The documentation is in progress
</aside>

### Verifying webhook signature

### Best practices

So now that you have specified a URL for your application, it is time to start working on accepting Webooks of Ecwid users' stores.

The standard workflow for using Webhooks in Ecwid is the following: 
1. Something changes in Ecwid store
2. Ecwid sends a Webhook to your application
3. Application sends a reply to that request and verifies that this webhook is from Ecwid
4. Application checks for the updated data
5. Application reacts to changes based on the type of event and other factors

Let's break down each step in more detail:
1. Someting changes in Ecwid store
These changes can be initiated by many parties: store admin that changes the stock of items, a customer who places a new order in the store, or a 3rd party application that has changed something in a store.

2. Ecwid sends a Webhook to your application
This step is pretty simple, as Ecwid is just sending webhook to the URL that you specified for your application. 

3. Application sends a reply to that request and verifies that this webhook is from Ecwid
Every notification request will include a header called `X-Ecwid-Webhook-Signature` that includes an HMAC-SHA256 signature of the request body. To decrypt it, you will need to use your clientSecret, which we sent you upon app registration, as the signing key. This lets your app verify that the notification really came from Ecwid. It's not a required step, however we highly recommend that you check the validity of the signature before processing the notification.

4. Application checks for the updated data
The body of a Webhook does not include the actual file changes. It only informs your app of which entities (orders or products) were updated and how: a new element is created, existing element update or something was deleted from the store. 

So your app will need to dive into the details of the changes by using either [get order details](#get-order-details) or [get a product](#get-a-product) methods.

5. Application reacts to changes, based on the type of event and other factors
As a result of getting the exact entity that has changed (product or order) your app can do various actions. For example, send a push notification to a user who is actively browsing Ecwid storefront, update only one product in your local database or send a new info to your fulfilment center, send an email to a new customer offering them a discount and many more.

Once the app is done making changes, it will be ready to accept further updates from Ecwid, when the process will begin all over again.