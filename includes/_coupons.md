# Discount Coupons

## Search coupons

### Request

> Request example

```http
GET /api/v3/4870020/discount_coupons?discount_type=ABS%2CPERCENT&availability=ACTIVE%2CEXPIRED&token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/discount_coupons?code={code}&discount_type={discount_type}&availability={availability}&limit={limit}&offset={offset}&token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items in one batch. Maximum allowed value: `100`. Default value: `10`
code | string | Coupon code
discount_type | string | Comma separated list of discount types. Supported values: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
availability |  string | Comma separated list of coupon states. Supported values: `ACTIVE`, `PAUSED`, `EXPIRED`, `USEDUP`

<aside class="notice">
If no filters are set in the URL, API will return all discount coupons
</aside>

<aside class="notice">
Parameters in bold are mandatory
</aside>

### Response

> Response example (JSON)

```json
{
    "total": 3,
    "count": 3,
    "limit": 10,
    "offset": 0,
    "items": [
        {
            "name": "Coupon # 1",
            "code": "MOXQ3YCWXRXA",
            "discountType": "ABS",
            "status": "ACTIVE",
            "discount": 1,
            "launchDate": "2014-06-06 08:00:00 +0400",
            "usesLimit": "UNLIMITED",
            "repeatCustomerOnly": false,
            "creationDate": "2014-06-06 18:57:19 +0400",
            "orderCount": 0,
            "catalogLimit": {
                "products": [
                    37208342,
                    37208338
                ],
                "categories": []
            }
        },
        {
            "name": "Coupon # 3",
            "code": "O3Q4AP5FKXJ1",
            "discountType": "PERCENT",
            "status": "PAUSED",
            "discount": 5,
            "launchDate": "2014-06-06 08:00:00 +0400",
            "usesLimit": "UNLIMITED",
            "repeatCustomerOnly": false,
            "creationDate": "2014-06-06 18:57:19 +0400",
            "orderCount": 0
        }
    ]
}
```

A JSON object of type 'DiscountCouponSearchResults' with the following fields:

#### DiscountCouponSearchResults
Field | Type | Description
----- | ---- | -----------
total | number | The total number of found items (might be more than the number of returned items)
count | number | The number of returned items
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
items | Array\<*CouponSearchEntry*\> | The items list

#### CouponSearchEntry
Field | Type  | Description
----- | ----- | -----------
name |  string | Coupon title
code |  string | Unique coupon code
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP`
discount | number | Discount amount
launchDate | string | The date of coupon launch
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0400`
totalLimit | number | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE`
repeatCustomerOnly | boolean | Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | The products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

### Errors

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
500 | Cannot retrieve the coupon info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Get coupon

### Request

> Request example

```http
GET /api/v3/4870020/discount_coupons/MOXQ3YCWXRXA?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/discount_coupons/{couponCode}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**couponCode** | number | Coupon ID
**token** |  string |  oAuth token

### Response

> Response example (JSON)

```json
{
    "name": "Coupon # 1",
    "code": "MOXQ3YCWXRXA",
    "discountType": "ABS",
    "status": "ACTIVE",
    "discount": 1,
    "launchDate": "2014-06-06 08:00:00 +0400",
    "usesLimit": "UNLIMITED",
    "repeatCustomerOnly": false,
    "creationDate": "2014-06-06 18:57:19 +0400",
    "orderCount": 0,
    "catalogLimit": {
        "products": [
            37208342,
            37208338
        ],
        "categories": []
    }
}
```


A JSON object of type 'Coupon' with the following fields:

#### Coupon
Field | Type  | Description
----- | ----- | -----------
name |  string | Coupon title
code |  string | Unique coupon code
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP`
discount | number | Discount amount
launchDate | string | The date of coupon launch
expirationDate | string | Coupon expliration date, e.g. `2014-06-06 08:00:00 +0400`
totalLimit | number  | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE`
repeatCustomerOnly | boolean | Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | The products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
404 | Coupon is not found
400 | Malformed request parameters


## Create coupon

### Request

> Request body

```http
POST /api/v3/4870020/discount_coupons?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

{
    "name": "Coupon # 1",
    "code": "MOXQ3YCWXRXA",
    "discountType": "ABS",
    "status": "ACTIVE",
    "discount": 1,
    "launchDate": "2014-06-06 08:00:00 +0400",
    "usesLimit": "UNLIMITED",
    "repeatCustomerOnly": false,
    "creationDate": "2014-06-06 18:57:19 +0400",
    "orderCount": 0,
    "catalogLimit": {
        "products": [
            37208342,
            37208338
        ],
        "categories": []
    }
}
```

`POST https://app.ecwid.com/api/v3/{storeId}/discount_coupons?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token

### Request body

A JSON object of type 'Coupon' with the following fields:

#### Coupon
Field | Type  | Description
----- | ----- | -----------
**name** |  string | Coupon title
**code** |  string | Unique coupon code, length limit is 128 characters.
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING` . Default is `ABS`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP` . Default is `ACTIVE`
discount | number | Discount amount . `0` is default
launchDate | string | The date of coupon launch
expirationDate | string | Coupon expliration date, e.g. `2014-06-06 08:00:00 +0400`
totalLimit | number  | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE` . `UNLIMITED` is default
repeatCustomerOnly | boolean | Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers. `false` is default
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | The products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

<aside class="notice">
Parameters in bold are mandatory
</aside>


### Response


> Response example

```json
{
    "code": 9698215
}
```


A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type |  Description
-------------- | -------------- | --------------
code | number | Code of the created coupon


### Errors

> Error response example

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, a JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
409 | The coupon with the given code already exists
500 | The creation request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Update coupon

### Request

> Request body

```http
PUT /api/v3/4870020/discount_coupons/12MOXQ3YCWXRXA?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

{
    "name": "Coupon # 1",
    "code": "MOXQ3YCWXRXA",
    "discountType": "ABS",
    "status": "ACTIVE",
    "discount": 1,
    "launchDate": "2014-06-06 08:00:00 +0400",
    "usesLimit": "UNLIMITED",
    "repeatCustomerOnly": false,
    "creationDate": "2014-06-06 18:57:19 +0400",
    "orderCount": 0,
    "catalogLimit": {
        "products": [
            37208342,
            37208338
        ],
        "categories": []
    }
}
```

`PUT https://app.ecwid.com/api/v3/{storeId}/discount_coupons/{couponCode}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
**code** | string | Unique coupon code

### Request body

A JSON object of type 'Coupon' with the following fields:

#### Coupon
Field | Type  | Description
----- | ----- | -----------
name |  string | Coupon title
code |  string | Unique coupon code, length limit is 128 characters.
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING` . 
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP` .
discount | number | Discount amount .
launchDate | string | The date of coupon launch
expirationDate | string | Coupon expliration date, e.g. `2014-06-06 08:00:00 +0400`
totalLimit | number  | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE` . 
repeatCustomerOnly | boolean | Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers.
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | The products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

<aside class="notice">
All fields are optional
</aside>


### Response


> Response example

```json
{
    "updateCount": 1
}
```


A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus
Field | Type |  Description
------| ---- | --------------
updateCount | number | The number of updated coupons (`1` or `0` depending on whether the update was successful)


### Errors

> Error response example

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, a JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
409 | The coupon with the given code already exists
500 | The request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Delete coupon

> Request example

```http
DELETE /api/v3/4870020/discount_coupons/MOXQ3Y222CWXRXA?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/discount_coupons/{couponCode}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**couponCode** | number | Coupon code
**token** |  string |  oAuth token

### Response

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
deleteCount | number | The number of deleted coupons (`1` or `0` depending on whether the request was successful). It returns `0` when the coupon with the given code is not found


### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
500 | The request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message