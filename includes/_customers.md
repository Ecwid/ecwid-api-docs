## Customers

It is possible to place orders without creating an account in a store – it depends on the store settings. Using the methods below you can get information about registered customers and modify them.

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

### Search customers

Search for customers by a keyword or basic filters: number of orders made, name, email, customer group and more.

#### Request

> Request examples

```http
GET /api/v3/4870020/customers?email=johnsmith@example.com&token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/customers?keyword={keyword}&name={name}&email={email}&customerGroup={groupId}&minOrderCount={minOrderCount}&maxOrderCount={maxOrderCount}&sortBy={sortBy}&offset={offset}&limit={limit}&token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
keyword |  string | Search term that Ecwid looks for in all customer fields
name | string | Customer name
email | string | Customer email
groupId | number | Customer group ID
minOrderCount |  number | Minimum number of order a customer placed
maxOrderCount | number | Maximum number of order a customer placed
createdFrom | string | Customer register date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li></ul> Examples: <ul><li>`1447804800`</li></ul>
createdTo | string | Customer register date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedFrom | string | Customer last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedTo | string | Customer last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
sortBy |  string | Sort order. Supported values: <ul><li>`NAME_ASC` *default*</li> <li>`NAME_DESC`</li> <li>`EMAIL_ASC`</li> <li>`EMAIL_DESC`</li> <li>`ORDER_COUNT_ASC`</li> <li>`ORDER_COUNT_DESC`</li><li>`REGISTERED_DATE_DESC`</li><li>`REGISTERED_DATE_ASC`</li><li>`UPDATED_DATE_DESC`</li><li>`UPDATED_DATE_ASC`</li></ul>
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`

<aside class="notice">
If no filters are set in the URL, API will return all store customers
</aside>

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (JSON)

```json
{
    "total": 3,
    "count": 3,
    "limit": 100,
    "offset": 0,
    "items": [
        {
            "id": 24623050,
            "name": "Jane Roe",
            "email": "jr@example.com",
            "registered": "2016-01-27 14:06:28 +0000",
            "updated": "2016-01-27 14:06:28 +0000",
            "totalOrderCount": 0,
            "customerGroupId": 0,
            "customerGroupName": "General",
            "billingPerson": {
                "name": "Jane Roe"
            },
            "shippingAddresses": [],
            "taxExempt": false,
            "taxIdValid": true
        },
        {
            "id": 14444116,
            "name": "John Darling [Sample]",
            "email": "demo4@ecwid.com",
            "registered": "2009-07-23 04:59:49 +0000",
            "updated": "2009-07-23 04:59:49 +0000",
            "totalOrderCount": 0,
            "customerGroupId": 2595001,
            "customerGroupName": "Platinum 228",
            "billingPerson": {
                "name": "John Darling [Sample]",
                "street": "12840 Pennridge Dr",
                "city": "Bridgeton",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "63044",
                "stateOrProvinceCode": "MO",
                "stateOrProvinceName": "Missouri",
                "phone": "314-209-0075"
            },
            "shippingAddresses": [],
            "taxExempt": false,
            "taxIdValid": true
        },
        {
            "id": 24623047,
            "name": "John Doe",
            "email": "john@example.com",
            "registered": "2016-01-27 14:06:23 +0000",
            "updated": "2016-01-27 14:06:23 +0000",
            "totalOrderCount": 0,
            "customerGroupId": 0,
            "customerGroupName": "General",
            "billingPerson": {
                "name": "John Doe"
            },
            "shippingAddresses": [],
            "taxExempt": false,
            "taxIdValid": true
        },
        {
            "id": 24623053,
            "name": "John Doe",
            "email": "john@cool.com",
            "registered": "2016-01-27 14:09:36 +0000",
            "updated": "2016-01-27 14:09:36 +0000",
            "totalOrderCount": 0,
            "customerGroupId": 0,
            "customerGroupName": "General",
            "billingPerson": {
                "name": "John Doe"
            },
            "shippingAddresses": [],
            "taxExempt": false,
            "taxIdValid": true
        },
    ]
}
```

A JSON object of type 'CustomerSearchResult' with the following fields:

#### CustomerSearchResult
Field | Type | Description
----- | ---- | -----------
total | number | The total number of found items (might be more than the number of returned items)
count | number | The number of returned items
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
items | Array<CustomerSearchEntry> | The items list

#### CustomerSearchEntry
Field | Type  | Description
----- | ----- | -----------
id |  number |  Unique internal customer ID
email | string |  Customer email
name | string | Customer full name
totalOrderCount | number | Number of customer's orders
registered | string | Registration date, e.g `2014-06-06 18:57:19 +0400`
updated | string | Last updated date, e.g `2014-06-06 18:57:19 +0400`
billingPerson | *Person* | Customer's billing name/address
shippingAddresses | Array\<*ShippingAddress*\> | Customer address book items
customerGroupId | number | Customer group ID
customerGroupName | string | Customer group name
taxId | string | Customer tax ID
taxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
taxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)

#### Person
Field | Type  | Description
----- | ----- | -----------
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country code (2-letter code)
countryName | string | Country name
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
stateOrProvinceName | string | State/province name
phone | string | Phone number

#### ShippingAddress
Field | Type  | Description
------| ----- | -----------
id | number | Internal address ID
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country (2-digits code)
countryName | string | Country name
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
stateOrProvinceName | string | State/province name
phone | string | Phone number

#### Q: When does the customer updated date change? 

The `updated` field is changed in these situations: 

- a new customer account created via storefront or via API
- customer was updated in storefront or via API
- customer group was changed in any way
- customer was deleted from Ecwid Control Panel of via API

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
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the customers info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Get customer

Get full customers details referring to their ID in an Ecwid store.

#### Request

> Request example

```http
GET /api/v3/4870020/customers/123123?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/customers/{customerId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**customerId** | number | Customer ID
**token** |  string |  oAuth token

#### Response

> Response example (JSON)

```json
{
    "id": 15319410,
    "email": "johnsmith@example.com",
    "registered": "2014-09-01 23:25:27 +0400",
    "customerGroupId": 12345,
    "customerGroupName": "Extra Gold",
    "billingPerson": {
        "name": "John Smith",
        "companyName": "Unreal Company",
        "street": "W 3d st",
        "city": "New York",
        "countryCode": "US",
        "countryName": "United States",
        "postalCode": "10001",
        "stateOrProvinceCode": "NY",
        "stateOrProvinceName": "New York",
        "phone": "+1234567890"
    },
    "shippingAddresses": [
        {
            "id": 8315122,
            "name": "John Smith",
            "companyName": "",
            "street": "W 3d st",
            "city": "New York",
            "countryCode": "US",
            "countryName": "United States",
            "postalCode": "10001",
            "stateOrProvinceCode": "NY",
            "phone": "123567890"
        },
        {
            "id": 8315123,
            "name": "Jane Smith",
            "companyName": "",
            "street": "1733 W Madison St",
            "city": "Chicago",
            "countryCode": "US",
            "countryName": "United States",
            "postalCode": "60612",
            "stateOrProvinceCode": "IL",
            "stateOrProvinceName": "Illinois",
            "phone": ""
        }
    ],
    "taxId": "GB999 9999 73",
    "taxExempt": true,
    "taxIdValid": true
}
```


A JSON object of type 'Customer' with the following fields:

#### Customer
Field | Type  | Description
----- | ----- | -----------
id |  number |  Unique internal customer ID
email | string |  Customer email
registered | string | Registration date, e.g `2014-06-06 18:57:19 +0400`
updated | string | Last updated date, e.g `2014-06-06 18:57:19 +0400`
billingPerson | *Person* | Customer's billing name/address
shippingAddresses | Array\<*ShippingAddress*\> | Customer address book items
customerGroupId | number | Customer group ID
customerGroupName | string | Customer group name
taxId | string | Customer tax ID
taxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
taxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)

#### Person
Field | Type  | Description
----- | ----- | -----------
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country code (2-letter code)
countryName | string | Country name
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
stateOrProvinceName | string | State/province name
phone | string | Phone number

#### ShippingAddress
Field | Type  | Description
------| ----- | -----------
id | number | Internal address ID
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country (2-digits code)
countryName | string | Country name
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
stateOrProvinceName | string | State/province name
phone | string | Phone number

#### Q: When does the customer updated date change? 

The `updated` field is changed in these situations: 

- a new customer account created via storefront or via API
- customer was updated in storefront or via API
- customer group was changed in any way
- customer was deleted from Ecwid Control Panel of via API

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
404 | Customer is not found
500 | Cannot retrieve the customer info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Create customer

Create a new customer in an Ecwid store.

#### Request

> Request body

```http
POST /api/v3/4870020/customers?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

{
    "email": "example@example.com",
    "password": "ecwidiscool",
    "customerGroupId": 12345,
    "billingPerson": {
        "name": "John Smith",
        "companyName": "Imaginary Company",
        "street": "Hedgehog Street, 1",
        "city": "Bucket",
        "countryCode": "US",
        "postalCode": "90002",
        "stateOrProvinceCode": "CA",
        "phone": "11111111111"
    },
    "shippingAddresses": [
        {
            "name": "John Smith",
            "companyName": "Imaginary Company",
            "street": "W 3d st",
            "city": "New York",
            "countryCode": "US",
            "postalCode": "10001",
            "stateOrProvinceCode": "NY",
            "phone": "11111111111"
        }
      ],
    "taxId": "GB999 9999 73",
    "taxExempt": true,
    "taxIdValid": true
}
```


`POST https://app.ecwid.com/api/v3/{storeId}/customers?token={token}`


Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token


#### Request body

A JSON object of type 'Customer' with the following fields:

#### Customer
Field | Type  | Description
-------------- | -------------- | --------------
**email** | string |  Customer email
password | string |  Customer password
customerGroupId | number | Customer group ID
billingPerson | <Person> | Customer's billing name/address
shippingAddresses | Array\<*ShippingAddress*\> | Customer address book items
taxId | string | Customer tax ID
taxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
taxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)

#### Person
Field | Type  | Description
-------------- | -------------- | --------------
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country (2-digits code)
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
phone | string | Phone number

#### ShippingAddress
Field | Type  | Description
------| ----- | -----------
id | number | Internal address ID
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country (2-digits code)
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
phone | string | Phone number

<aside class="notice">
Parameters in bold are mandatory
</aside>


#### Response


> Response example

```json
{
    "id": 15319442
}
```

A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type |  Description
-------------- | -------------- | --------------
id | number | ID of the created customer


#### Errors

> Error response example

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
409 | The customer with the given email already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The creation request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Update customer

Update details of an existing customer in an Ecwid store. 

#### Request

> Request body

```http
PUT /api/v3/4870020/customers?10293737&token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

{
    "email": "new-email@example.com",
    "password":"newpassword",
    "customerGroupId": 12345,
    "billingPerson": {
        "name": "New Name",
        "companyName": "Ecwid",
        "street": "Updated street",
        "city": "Bucket",
        "countryCode": "US",
        "postalCode": "90002",
        "stateOrProvinceCode": "CA",
        "phone": "11111111111"
    },
    "shippingAddresses": [
        {
            "name": "Eugene K",
            "companyName": "Hedgehog and Bucket",
            "street": "Hedgehog Street, 3",
            "city": "Bucket",
            "countryCode": "US",
            "postalCode": "90002",
            "stateOrProvinceCode": "CA",
            "phone": "11111111111"
        }
      ],
    "taxId": "GB999 9999 73",
    "taxExempt": true,
    "taxIdValid": true
}
```


`PUT https://app.ecwid.com/api/v3/{storeId}/customers/{customerId}?token={token}`


Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**customerId** | number | Internal customer ID


#### Request body

A JSON object of type 'Customer' with the following fields:

#### Customer
Field | Type  | Description
-------------- | -------------- | --------------
email | string |  Customer email
password | string |  Customer password
customerGroupId | number | Customer group ID
billingPerson | <Person> | Customer's billing name/address
shippingAddresses | Array\<*ShippingAddress*\> | Customer address book items
taxId | string | Customer tax ID
taxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
taxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)

#### Person
Field | Type  | Description
-------------- | -------------- | --------------
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country (2-digits code)
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
phone | string | Phone number

#### ShippingAddress
Field | Type  | Description
------| ----- | -----------
id | number | Internal address ID
name | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country (2-digits code)
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
phone | string | Phone number

<aside class="notice">
All fields are optional
</aside>


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
404 | The customer with given ID is not found
409 | The customer with the given email already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The update request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



### Delete customer

Delete a specific customer from an Ecwid store referring to their ID.

> Request example

```http
DELETE /api/v3/4870020/customers/39847403?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/customers/{customerId}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**customerId** | number | Customer ID
**token** |  string |  oAuth token

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
404 | The customer with given ID is not found
500 | The update request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
