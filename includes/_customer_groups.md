## Customer groups

In Ecwid stores merchants assign customers to specific groups in order to treat them differently. For example, for some group of people, that purchased a membership in their store, they would like to provide a discount based on subtotal at all times. Another way of using customer groups is to to show them a hidden content of a store, in case if these customers are wholesalers.

To manage customer groups in Ecwid stores, use this endpoint described below and to find out more about customer groups in Ecwid stores, check out [this page](https://help.ecwid.com/customer/en/portal/articles/1345509-customer-groups)

### Get all customer groups

Get information about all customer groups in an Ecwid store.

#### Request

> Request example

```http
GET /api/v3/4870020/customer_groups?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/customer_groups?offset={offset}&limit={limit}&token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (JSON)

```json
{
    "total": 2,
    "count": 2,
    "limit": 10,
    "offset": 0,
    "items": [
        {
            "id": 0,
            "name": "General"
        },
        {
            "id": 12345,
            "name": "VIP"
        }
    ]
}
```

A JSON object of type 'CustomerGroupsSearchResult' with the following fields:

#### CustomerGroupsSearchResult
Field | Type | Description
----- | ---- | -----------
total | number | The total number of found items (might be more than the number of returned items)
count | number | The number of returned items
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
offset | number | Offset from the beginning of the returned items list (for paging)
items | Array<CustomerGroupsSearchEntry> | The items list

#### CustomerGroupsSearchEntry
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique internal customer group ID
name | string | Customer group name

#### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
402 | Store doesn't have access to customer groups feature, it's available on Business plan an higher
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the customer groups info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Get customer group

Get information about a specific customer group in an Ecwid store referring to its ID.

#### Request

> Request example

```http
GET /api/v3/4870020/customer_groups/123123?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/customer_groups/{groupId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**groupId** | number | Customer group ID
**token** |  string |  oAuth token

#### Response

> Response example (JSON)

```json
{
    "id": 12345,
    "name": "VIP"
}
```

A JSON object of type 'CustomerGroup' with the following fields:

#### CustomerGroup

Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique internal customer group ID
name | string | Customer group name

#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
402 | Store doesn't have access to customer groups feature, it's available on Business plan and higher
404 | Customer group is not found
500 | Cannot retrieve the customer info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Create customer group

Create a brand new customer group in an Ecwid store.

#### Request

> Request body

```http
POST /api/v3/4870020/customer_groups?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

{
    "name": "Platinum"
}
```


`POST https://app.ecwid.com/api/v3/{storeId}/customer_groups?token={token}`


Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token


#### Request body

A JSON object of type 'CustomerGroup' with the following fields:

#### CustomerGroup

Field | Type  | Description
-------------- | -------------- | --------------
**name** | string | Customer group name

#### Response


> Response example

```json
{
    "id": 123456
}
```

A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type |  Description
-------------- | -------------- | --------------
id | number | ID of the created customer group


#### Errors

> Error response example

```http
HTTP/1.1 400 Field CustomerMembership.name is absent
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
402 | Store doesn't have access to customer groups feature, it's available on Business plan and higher
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The creation request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Update customer group

Update an existing customer group in an Ecwid store referring to its ID.

#### Request

> Request body

```http
PUT /api/v3/4870020/customer_groups?123456&token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

{
    "name": "VIP Plus"
}
```

`PUT https://app.ecwid.com/api/v3/{storeId}/customer_groups/{groupId}?token={token}`


Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**groupId** | number | Internal customer group ID

#### Request body

A JSON object of type 'CustomerGroup' with the following fields:

#### CustomerGroup

Field | Type  | Description
-------------- | -------------- | --------------
**name** | string | Customer group name

#### Response


> Response example

```json
{
    "updateCount": 1
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus

Field | Type |  Description
-------------- | -------------- | --------------
updateCount | number | The number of update customers (`0` or `1` depending on whether the request was successful)


#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
402 | Store doesn't have access to customer groups feature, it's available on Business plan and higher
404 | The customer group with given ID is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The update request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Delete customer group

Delete a customer group in an Ecwid store referring to its ID.

> Request example

```http
DELETE /api/v3/4870020/customer_groups/39847403?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/customer_groups/{groupId}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**groupId** | number | Customer ID
**token** |  string |  oAuth token

<aside class="notice">
If a customer group is deleted, all customers of that group will be moved to the General group, ID: 0
</aside>

#### Response

> Response example

```json
{
    "deleteCount": 1
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | ------------
deleteCount | number | The number of deleted customers (`1` or `0` depending on whether the request was successful)


#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
402 | Store doesn't have access to customer groups feature, it's available on Business plan and higher
404 | The customer group with given ID is not found
500 | The update request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
