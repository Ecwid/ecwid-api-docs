# Webhooks [coming soon]

So what is a webhook? Basically, a WebHook is an HTTP callback: an HTTP POST request that occurs when something happens, or in other words it's a simple event-notification via HTTP POST.

Using webhooks, Ecwid can notify another application about important store events as they happen in near real-time.

A developer can use webhooks to extend and customize the checkout process of Ecwid stores. For example send a custom e-mail notification, inform your supplier about a sale, generate a pin code or a license key for a sold software, send a text message, add a new order to your accounting software or subscribe a new customer to your newsletter.

## How it works

All you have to do is provide a URL and Ecwid will call this URL (via HTTP POST request) as soon as things happen.

The following events are supported:

* New order has been placed
* Order status has been changed
* Order has been deleted
* New product has been created
* Product has been updated
* Product has been deleted

In order to understand, which entity has been updated/created/deleted, your application will need to check value of the field "eventType" in a request. Ecwid sends the *eventType* as a URL parameter for your convenience as well, for example: https://www.app.com/callback?eventtype=product.created So, your endpoint can only listen for specific type of events, regarding updates in product or order entity only.

Any Ecwid application has a set of access scopes that it can use for requests in Ecwid API. Some applications need access to orders in Ecwid stores, some of them need access to the product catalog. In terms of the access scopes, webhooks in Ecwid work like this: 
- Access token A has access to store's catalog and orders: Ecwid will send webhooks for any event in store's products or orders
- Access token B has access to store's catalog: Ecwid will send webhooks for any event in store's products only
- Access token C has access to store's orders: Ecwid will send webhooks for any event in store's orders only

So Ecwid will send only the relevant information to your application's endpoint and as soon as it happens.

## How to configure

When you are [registering your application](#register-your-app-in-ecwid) you will need to specify a single URL where Ecwid will send the updates about the events in the store. 

### Response

> Response example (JSON)

```json
{
	"eventId": 12345,
	"eventType": "product.updated",
	"eventCreated": 1230329329,
	"storeId": 1003,
	"entityId": 130030039,
}
```

The following parameters are sent in an HTTP POST request to the endpoint URL as soon as one of the supported events occurs: 

Name | Type    | Description
---- | ------- | --------------
eventId | number | unique webhook ID
eventType | string | Type of the occurred event. Possible values: **order.created** or **product.updated**.
eventCreated | Unix Timestamp | Unix timestamp of the occurred event.
storeId | number | Store ID of the store where the event occured.
entityId | number | Id of the entity, where the event occured. For example, if a product was updated, then the entityId will be the ID of that product

If the request fails to connect (wrong URL or problems on your server) in the time span of 30 seconds, Ecwid will attempt to re-send it every 15 minutes. If all attempts were not successful, a message gets removed from the queue as an undelivered one.

The request is considered successful if an endpoint/URL returns HTTP 200 or HTTP 201 status header. 

## Securing Webhooks and correct workflow

**Workflow of a webhook**

Your script gets a notification about an event. Only the necessary information like order number, date and store ID is sent. Ecwid will not send any private and sensitive information.

Then your script should use the [Order API endpoint](#orders) or [Product API endpoint](#products) to validate and get more information about this event. I.e. check that the placed/updated order exists and get necessary sensitive order details. If no such order exists in a database or it exists, but its current order status doesn't match a status in the notification, then the notification should be ignored as a fake one. 


**Security**

This approach makes the whole process to be secure by design. For example:

1) If you set the incorrect endpoint URL or use a 3d-party URL to test notifications, they will not get a private information about your customers, because they don't know your API access token.

2) This design forces application to always verify each notification request which comes to the endpoint. So even if somebody tries to "fake" a notification, it will not be validated and will be ignored. This is a great approach, which enhances the security of the app greatly. 