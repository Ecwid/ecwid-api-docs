# Application

This endpoint allows you to get the status of the app in a specific Ecwid store. It's now only identifying the billing state (subscription status) of the application. We will be adding more information to this endpoint in the future.

## Get application status 

Get application status in an Ecwid store.

> Request example

```http
GET /api/v3/4870020/application?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/application?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

### Response

> Response example

```json
{
	"subscription":
	{
		"startDate":"2016-06-30 08:28:28 +0000",
		"expirationDate":"2016-07-14 09:27:56 +0000",
		"status":"TRIAL"
	},
	"subscriptionStatus":"TRIAL"
}
```

A JSON object of type 'Application' with the following fields:

#### Application

Field | Type | Description
------| ------| -----------
subscription | \<*SubscriptionDetails*\> | Subscription details for the application
subscriptionStatus | string | Application status in Ecwid store. One of `ACTIVE`, `SUSPENDED` or `TRIAL`

#### SubscriptionDetails

Field | Type | Description
------| ------| -----------
startDate | string | App subscription start timestamp
expirationDate | string | App subscription end timestamp
status | string | Application status in Ecwid store. One of `ACTIVE`, `SUSPENDED` or `TRIAL`

#### HTTP codes

HTTP Status | Meaning
------------|--------
415 | Unsupported content-type: expected `application/json` or `text/json`

### How it works


**Free apps**: 

- Ecwid will always return `ACTIVE` unless application is uninstalled. 

**Paid apps using external billing**: 

- Ecwid will always return `ACTIVE` unless application is uninstalled. 

**Paid apps using Ecwid billing**: 

- Ecwid will return `ACTIVE` if application is successfully installed and paid by a user. 
- Ecwid will return `SUSPENDED` if there was an issue with prolongating subscription of this app.

**Paid apps using Ecwid billing with free trial**: 

- Ecwid will return `TRIAL` if this Ecwid store uses free trial of your application at the moment. 
- Ecwid will return `ACTIVE` if application is successfully installed and paid by a user. 
- Ecwid will return `SUSPENDED` if there was an issue with prolongating subscription of this app.

### What is Ecwid billing and how do I use it? 

Ecwid billing is a feature that allows to accept payments for applications. It works out of the box, so no additional development is required to use it. 

You can specify free trial period for new installs and active users pay on a monthly basis. To find out more details and how to use it, please visit: [http://developers.ecwid.com/make-money](http://developers.ecwid.com/make-money)
