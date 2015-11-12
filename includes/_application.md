# Application

One of the most important things about applications in Ecwid app market is to know the exact status of it in a specific Ecwid store. You can do that using the Applcation endpoint.

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
subscriptionStatus | string | Application status in Ecwid store

> Response example

```json
{
  "subscriptionStatus":"ACTIVE"
}
```

This endpoint works in a following way: 

**Free apps** 
Ecwid will always return `ACTIVE`

**Apps that use Ecwid billing scheme**
Ecwid will return `ACTIVE` if the app is paid by the user and it is active in their store. Also `ACTIVE` will be returned if the user is on a grace period. 
Ecwid will return `SUSPENDED` if there was an issue with prolongating of the subscription of this app

### How can I know if the app was uninstalled?

You can get such information when your applicaiton makes any requests to Ecwid API, for example [Get product](#get-a-product) or [Search Orders](#search-orders)

Status code | Applicaiton status 
------------| ------------------
200 | Application is installed and active
402 | Store doesn't have access to Ecwid API or didn't pay for application
403 | Application was unistalled by the user

