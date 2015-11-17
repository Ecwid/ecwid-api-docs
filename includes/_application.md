# Application

This endpoint allows you to get the status of the app in a specific Ecwid store. It's now only identifying the billing state (subscription status) of the application. We will be adding more information to this endpoint in the future.

## Get application status 

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

A JSON object of type 'Application' with the following fields:

#### Application

Field | Type | Description
------| ------| -----------
subscriptionStatus | string | Application status in Ecwid store. One of `ACTIVE`, `SUSPENDED`

> Response example

```json
{
  "subscriptionStatus":"ACTIVE"
}
```

This endpoint works in a following way: 

**Free apps**: Ecwid will always return `ACTIVE` unless the app is uninstalled. 

**Paid apps with external billing**: Ecwid will always return `ACTIVE` unless the app is uninstalled. 

**Paid apps with Ecwid billing**: Ecwid will return `ACTIVE` if the app is paid by the user and it is active in their store.
Ecwid will return `SUSPENDED` if there was an issue with prolongating of the subscription of this app