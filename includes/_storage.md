# Application storage

Ecwid API allows your application to store application data with us in a simple key-value format. You may want to use this to keep user profile data, application settings, etc. specific to a particular store ID. For example, let's say you ask users to enter their birth date and sex in your application. Ecwid Storage API allows you to save the data on our side as follows:

store ID: 11111 | store ID: 22222 | ...
--- | ---- | ---
birth_date: 18.11.1987 | birth_date: 07.02.1981 | ...
sex: male | sex: female | ...

The application storage can be accessed either via REST API or client-side Javascript code. The latter will be especially useful for client-side applications workin with Ecwid storefront or Ecwid control panel. 

* To use the Storage API in the storefront, refer to the "EcwidApp.getAppPublicConfig" method in the [Ecwid Javascript API](#storefront-js-api). 
* To manage the storage from the application tab in Ecwid control panel, use [Ecwid Javascript SDK](#ecwid-javascript-sdk).

The HTTP ednpoints below describe the storage REST API.

<aside class="notice">
Scope required: *update_store_profile*
</aside>

## Get all storage data

Retrieves all stored data for the given store ID.

### Request

> Request example

```http
GET /api/v3/805056/storage?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/storage?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token


### Response

> Response example (JSON)

```json
[
  {
    "key":"birth_date",
    "value":"18.11.1987"
  },
  {
    "key":"sex",
    "value":"male"
  }
]
```

A JSON array of the objects in the following format

#### StorageElement
Field | Type | Description
----- | ---- | -----------
key | string | The key you set for the stored data
value | string | The stored data





## Get storage data by key

Retrieves the stored data for the given store ID by the given key

### Request

> Request example

```http
GET /api/v3/805056/storage/birth_date?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) of the stored data
**token** |  string | oAuth token


### Response

> Response example (JSON)

```json
{
  "key":"birth_date",
  "value":"18.11.1987"
}
```

A JSON object of the following format

#### StorageElement
Field | Type | Description
----- | ---- | -----------
key | string | The key you set for the stored data
value | string | The stored data


### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8

{
    "error": "OTHER",
    "errorMessage": "Key not found"
}
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
404 | The key is not found
500 | Cannot retrieve the data because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
error | string | Type of the error
errorMessage | string | Error message






## Add data to storage

Use this method to put a new data into the storage. If the key you specify in the request already exists, the corresponding value in the storage will be replaced with the submitted one. 

### Request

> Request example

```http
POST /api/v3/805056/storage/my_key?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

Value of the new stored parameter
```

`POST https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) for the stored data (Limit: 1024 symbols)
**token** |  string | oAuth token


A key for the new key-value pair will be taken from the request URL. The data (value) is the request body (**1MB** max)


### Response

> Response example (JSON)

```json
{
  success: true
}
```

A JSON object with the status of operation

#### Status
Field | Type | Description
----- | ---- | -----------
success | boolean | `true` if the data has been added to the storage, `false` otherwise




## Edit storage data

Use this method to update data in the storage. If the key you specify in the request doesn't yet exists, the corresponding value in the storage will be created. 

### Request

> Request example

```http
PUT /api/v3/805056/storage/my_key?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

Updated value of the new stored parameter
```

`PUT https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) for the stored data (Limit: 1024 symbols)
**token** |  string | oAuth token


A key for the new key-value pair will be taken from the request URL. The data (value) is the request body (**1MB** max)


### Response

> Response example (JSON)

```json
{
  success: true
}
```

A JSON object with the status of operation

#### Status
Field | Type | Description
----- | ---- | -----------
success | boolean | `true` if the data has been changed in the storage, `false` otherwise




## Delete storage data

Use this method to delete data in the storage by key. 

### Request

> Request example

```http
DELETE /api/v3/805056/storage/my_key?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) of the stored data
**token** |  string | oAuth token


### Response

> Response example (JSON)

```json
{
  success: true
}
```

A JSON object with the status of operation

#### Status
Field | Type | Description
----- | ---- | -----------
success | boolean | `true` if the data has been removed from the storage, `false` otherwise. 