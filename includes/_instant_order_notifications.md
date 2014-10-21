# Instant Order Notifications
Ecwid can notify another application about important store events as they happen in near real-time.  

A developer can use this API to extend and customize your checkout process. For example send a custom e-mail notification, inform your supplier about a sale, generate a pin code or a license key for a sold software, notify your IM client when you are offline, send a text message, add a new order to your accounting software or subscribe a new customer to your newsletter.

## How it works

All you have to do is provide a URL and Ecwid will call this URL (via HTTP POST request) as soon as things happen.

The following events are supported:
- New order has been placed.
- Order status has been changed.

##How to configure

- Log into your Ecwid account.
- Go to the System settings → API page. 
- Find the Instant Order Notifications section.
- Enter a valid URL for Ecwid to contact. Ecwid will send a request to this URL each time a supported event occurs. 
- Please note, that API is available for paid users only. 

##Request
The following parameters are sent in an HTTP POST request to the endpoint URL as soon as one of the supported events occurs: 

Name | Type    | Description
---- | ------- | --------------
owner_id |  number | Store ID, where the event occurred.
event_type |  Text | Type of the occurred event. Possible values: **new_order** or **order_status_change**. 
date |  Date | Order placement date or date of the order status change. Format is **YYYY-MM-DD hh:mm:ss**. 
order_id |  Text | Order ID without custom suffix and prefix.  
payment_status |  Text | Code of the new payment status. Possible values: QUEUED*, ACCEPTED, CHARGEABLE, DECLINED, CANCELLED, REFUNDED
fulfillment_status |  Text | Code of the new fulfillment status
old_payment_status | Text | The payment status before the update. This parameter will be sent, only if event type is "order_status_change". 
old_fulfillment_status |  Text | The fulfillment status before the update. This parameter will be sent, only if event type is "order_status_change". 



If the request fails to connect (wrong URL or problems on your server), Ecwid will attempt to re-send it using the following backoff logic: 1 minute, 5 minutes, 10 minutes, 15 minutes, 30 minutes, 1 hour, 2 hours, 5 hours, 12 hours, 24 hours, 36 hours, 48 hours, 60 hours, 72 hours. If all attempts were not successful, a message gets removed from the queue as an undelivered one.

The request is considered successful if an endpoint/URL returns HTTP 200 or HTTP 201 status header. 


##Securing Instant Order Notifications and correct Wwrkflow

The correct workflow is as follows: 

Your script gets a notification about an event. Only the necessary information like order status, order and store IDs is sent. No private sensitive information is sent.
Then your script should use Order API to validate and get more information about this event. I.e. check that the placed/updated order exists and gets necessary sensitive order details. If no such order exists in a database or it exists, but its current order status doesn't match a status in the notification, then the notification should be ignored as a fake one. 
This approach makes the whole process to be secure by design. For example:

1) If you set the incorrect endpoint URL or use a 3d-party URL to test notifications, they will not get a private information about your customers, because they don't know your API access token.

2) This design forces a developer to always verify each notification request which comes to the endpoint. So even if somebody tries to "fake" a notification, it will not be validated and will be ignored.

##How to test

For testing notifications use the "Test endpoint" link on the "API" page in the control panel. It allows to send custom notifications with any data you want, so it simplifies testing of your scripts.

Also we suggest to use the RequestBin tool: http://requestb.in/ It is an awesome utility to help see data come across as events happen in your system. Make a new bin and use its URL as the endpoint one. All notifications will appear in the same RequestBin page.
