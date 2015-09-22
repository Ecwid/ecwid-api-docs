# Webhooks [work in progress]

# What is webhook

## Webhooks
So what is a webhook? Basically, a WebHook is an HTTP POST request that occurs when something happens, i.e. it's a simple event-notification via HTTP POST. Ecwid uses webhooks to notifiy your application in real time about event in the merchant store. 

This is how your application can use webhooks:

* Receive a request from Ecwid every time a new product is created or an exsiting product is changed to let you synchronize the store catalog with your local database. Get the updated product data and make your app in sync in one HTTP request instead of downloading the whole catalog.
* Get notified about every new order in the store to send a custom email or text message, or generate a custom receipt or subscribe the customer to your newsletter.

<aside class="notice">
Don't use webhooks themselves as actionable items – please see the ["Webhooks security"](#webhooks-security) notes below for details on working with webhooks.
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


# Setting up webhooks

Setup process is easy. Once your application has a webhook URL specified in the settings and has a token with appropriate access level for the store, it will receive notifications automatically. More details on these are below.


## Set webhook URL
When you [register your application](#register-your-app-in-ecwid) with Ecwid, please specify a webhook URL – Ecwid will send a request to this URL each time a supported event occurs. To enable webhooks for existing application, please contact us. 

<aside class="notice">
This must be a valid HTTPS URL. 
</aside>


## Get access
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
eventCreated | Unix Timestamp | Unix timestamp of the occurred event.
storeId | number | Store ID of the store where the event occured.
entityId | number | Id of the updated entity. For example, if a product was updated, then the entityId will be the ID of that product

<aside class="notice">
The `eventType` field is also duplicated in the request GET parameters. This allows you to filter our the webhooks you don't want to handle. For example, if you only need to listen to order updates, reply `200 OK` to every request containing products updates, e.g.  `https://www.myapp.com/callback?eventType=product.updated`
</aside>


## Request headers
Among the other headers, the webhook HTTP request includes the `X-Ecwid-Webhook-Signature` header that can be used to verify the webhook. See more details in the ["Webhooks security"](#webhooks-security) below.


## Retries
If the request fails to connect (wrong URL or problems on your server) in the time span of 30 seconds, Ecwid will attempt to re-send it every 15 minutes. If all attempts were not successful, a message gets removed from the queue as an undelivered one.

The request is considered successful if an endpoint/URL returns HTTP 200 or HTTP 201 status header. 





## Webhooks security

**Workflow of a webhook**

Your script gets a notification about an event. Only the necessary information like order number, date and store ID is sent. Ecwid will not send any private and sensitive information.

Then your script should use the [Order API endpoint](#orders) or [Product API endpoint](#products) to validate and get more information about this event. I.e. check that the placed/updated order exists and get necessary sensitive order details. If no such order exists in a database or it exists, but its current order status doesn't match a status in the notification, then the notification should be ignored as a fake one. 


**Security**

This approach makes the whole process to be secure by design. For example:

1) If you set the incorrect endpoint URL or use a 3d-party URL to test notifications, they will not get a private information about your customers, because they don't know your API access token.

2) This design forces application to always verify each notification request which comes to the endpoint. So even if somebody tries to "fake" a notification, it will not be validated and will be ignored. This is a great approach, which enhances the security of the app greatly. 