## Discount coupons

Discount coupons allow customers to get discounts on their orders in the checkout process. For store owners it's a great way to increase sales or make a temporary promotion.

Learn more about discount coupons in our [Help Center](https://support.ecwid.com/hc/en-us/articles/207806525-Discount-Coupons).

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

### Search coupons

Search or filter discount coupons in an Ecwid store. Filters include: discount coupon code, type, availability.

#### Request

> Request example

```http
GET /api/v3/4870020/discount_coupons?discount_type=ABS%2CPERCENT&availability=ACTIVE%2CEXPIRED&token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
Accept-Encoding: gzip
```

`GET https://app.ecwid.com/api/v3/{storeId}/discount_coupons?code={code}&discount_type={discount_type}&availability={availability}&limit={limit}&offset={offset}&token={token}`

<aside class="notice">
Parameters in <strong>bold</strong> are mandatory
</aside>

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items in one batch. Maximum allowed value: `100`. Default value: `100`
code | string | Coupon code
discount_type | string | Comma separated list of discount types. Supported values: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
availability |  string | Comma separated list of coupon states. Supported values: `ACTIVE`, `PAUSED`, `EXPIRED`, `USEDUP`
createdFrom | string | Coupon creation date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul> Examples: <ul><li>`1447804800`</li> </ul>
createdTo | string | Coupon creation date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedFrom | string | Coupon last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedTo | string | Coupon last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>

<aside class="notice">
If no filters are set in the URL, API will return all discount coupons
</aside>

#### Response

> Response example (JSON)

```json
{
    "total": 3,
    "count": 3,
    "limit": 10,
    "offset": 0,
    "items": [
        {
            "id": 1231421413,
            "name": "Coupon # 1",
            "code": "MOXQ3YCWXRXA",
            "discountType": "ABS",
            "status": "ACTIVE",
            "discount": 1,
            "launchDate": "2014-06-06 08:00:00 +0400",
            "usesLimit": "ONCEPERCUSTOMER",
            "applicationLimit": "NEW_CUSTOMER_ONLY",
            "creationDate": "2014-06-06 18:57:19 +0400",
            "updateDate": "2017-01-10 02:03:43 +0000",
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
            "id": 12314211242,
            "name": "Coupon # 3",
            "code": "O3Q4AP5FKXJ1",
            "discountType": "PERCENT",
            "status": "PAUSED",
            "discount": 5,
            "launchDate": "2014-06-06 08:00:00 +0400",
            "usesLimit": "UNLIMITED",
            "applicationLimit": "UNLIMITED",
            "creationDate": "2014-06-06 08:00:00 +0400",
            "updateDate": "2017-02-10 08:03:43 +0000",
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
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `100`
items | Array\<*CouponSearchEntry*\> | The items list

#### CouponSearchEntry
Field | Type  | Description
----- | ----- | -----------
id | number | Internal unique coupon ID
name |  string | Coupon title
code |  string | Unique coupon code
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP`
discount | number | Discount amount
launchDate | string | The date of coupon launch, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
totalLimit | number | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE`
repeatCustomerOnly | boolean | **Deprecated. Use applicationLimit instead**. Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`. If you are creating or updating a discount coupon, make sure to use the old `repeatCustomerOnly` field or the new `applicationLimit` field **only**. When both these fields are sent, the priority will be given to the new field – `applicationLimit`
creationDate |  string | Coupon creation date. Format example: `2016-06-29 11:36:55 +0000`
updateDate | string | Coupon update date. Format example: `2016-06-29 11:36:55 +0000`
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | The products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

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
403 | Access token doesn't have `read_discount_coupons` scope
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the coupon info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Get coupon

#### Request

> Request example

```http
GET /api/v3/4870020/discount_coupons/MOXQ3YCWXRXA?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/discount_coupons/{couponIdentifier}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**couponIdentifier** | number/string | Coupon identifier in a store. It can be either unique internal coupon `id` OR a discount coupon `code`.
**token** |  string |  oAuth token

#### Response

> Response example (JSON)

```json
{
    "id": 12314211242,
    "name": "Coupon # 1",
    "code": "MOXQ3YCWXRXA",
    "discountType": "ABS",
    "status": "ACTIVE",
    "discount": 1,
    "launchDate": "2014-06-06 08:00:00 +0400",
    "usesLimit": "UNLIMITED",
    "applicationLimit": "UNLIMITED",
    "creationDate": "2014-06-06 18:57:19 +0400",
    "updateDate": "2017-02-10 08:03:43 +0000",
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
id | number | Internal unique coupon ID
name |  string | Coupon title
code |  string | Unique coupon code
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP`
discount | number | Discount amount
launchDate | string | The date of coupon launch, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
totalLimit | number  | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE`
repeatCustomerOnly | boolean | **Deprecated. Use applicationLimit instead**. Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`. If you are creating or updating a discount coupon, make sure to use the old `repeatCustomerOnly` field or the new `applicationLimit` field **only**. When both these fields are sent, the priority will be given to the new field – `applicationLimit`
creationDate |  string | Coupon creation date. Format example: `2016-06-29 11:36:55 +0000`
updateDate | string | Coupon update date. Format example: `2016-06-29 11:36:55 +0000`
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | The products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

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
400 | Malformed request parameters
403 | Access token doesn't have `read_discount_coupons` scope
404 | Coupon is not found
415 | Unsupported content-type: expected `application/json` or `text/json`


### Create coupon

Create a new discount coupon in an Ecwid store.

<aside class="notice">
You can create only one coupon per API request. To create several coupons, send several separate requests for each coupon.
</aside>

#### Request

> Request body

```http
POST /api/v3/4870020/discount_coupons?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

```json
{
    "name": "Coupon # 1",
    "code": "MOXQ3YCWXRXA",
    "discountType": "ABS",
    "status": "ACTIVE",
    "discount": 1,
    "launchDate": "2014-06-06 08:00:00 +0400",
    "usesLimit": "UNLIMITED",
    "applicationLimit": "UNLIMITED",
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

#### Request body

A JSON object of type 'Coupon' with the following fields:

#### Coupon
Field | Type  | Description
----- | ----- | -----------
**name** |  string | Coupon title
**code** |  string | Unique coupon code, length limit is 128 characters.
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING` . Default is `ABS`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`. Discount coupon states `EXPIRED` and `USEDUP` are set by Ecwid store only and not available in API
discount | number | Discount amount . `0` is default
launchDate | string | The date of coupon launch, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
totalLimit | number  | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE` . `UNLIMITED` is default
repeatCustomerOnly | boolean | **Deprecated. Use applicationLimit instead**. Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`. If you are creating or updating a discount coupon, make sure to use the old `repeatCustomerOnly` field or the new `applicationLimit` field **only**. When both these fields are sent, the priority will be given to the new field – `applicationLimit`
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


#### Response


> Response example

```json
{
    "id": 21871001,
    "code": 9698215
}
```


A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type |  Description
-------------- | -------------- | --------------
id | number | Internal unique coupon ID
code | number | Code of the created coupon


#### Errors

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
403 | Access token doesn't have `create_discount_coupons` scope
409 | The coupon with the given code already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The creation request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Update coupon

Update an existing discount coupon in an Ecwid store referring to its code or ID.

#### Request

> Request body for discount coupon code

```http
PUT /api/v3/4870020/discount_coupons/12MOXQ3YCWXRXA?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

```json
{
    "name": "Coupon # 1",
    "code": "MOXQ3YCWXRXA",
    "discountType": "ABS",
    "status": "ACTIVE",
    "discount": 1,
    "launchDate": "2014-06-06 08:00:00 +0400",
    "usesLimit": "UNLIMITED",
    "applicationLimit": "NEW_CUSTOMER_ONLY",
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

> Request body for discount coupon id

```http
PUT /api/v3/4870020/discount_coupons/123213123?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

```json
{
    "name": "Coupon # 1",
    "code": "MOXQ3YCWXRXA",
    "discountType": "ABS",
    "status": "ACTIVE",
    "discount": 1,
    "launchDate": "2014-06-06 08:00:00 +0400",
    "usesLimit": "UNLIMITED",
    "applicationLimit": "REPEAT_CUSTOMER_ONLY",
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

`PUT https://app.ecwid.com/api/v3/{storeId}/discount_coupons/{couponIdentifier}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
**couponIdentifier** | number/string | Coupon identifier in a store. It can be either unique internal coupon `id` OR a discount coupon `code`.

#### Request body

A JSON object of type 'Coupon' with the following fields:

#### Coupon
Field | Type  | Description
----- | ----- | -----------
name |  string | Coupon title
code |  string | Unique coupon code, length limit is 128 characters.
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING` . 
status | string | Discount coupon state: `ACTIVE`, `PAUSED`. Discount coupon states `EXPIRED` and `USEDUP` are set by Ecwid store only and not available in API
discount | number | Discount amount
launchDate | string | The date of coupon launch, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0400`. Any date provided will be corrected to the UTC timezone
totalLimit | number  | The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE` . 
repeatCustomerOnly | boolean | **Deprecated. Use applicationLimit instead**. Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`. If you are creating or updating a discount coupon, make sure to use the old `repeatCustomerOnly` field or the new `applicationLimit` field **only**. When both these fields are sent, the priority will be given to the new field – `applicationLimit`
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
------| ---- | --------------
updateCount | number | The number of updated coupons (`1` or `0` depending on whether the update was successful)


#### Errors

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
403 | Access token doesn't have `update_discount_coupons` scope
409 | The coupon with the given code already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Delete coupon

Delete a specific coupon from an Ecwid store referring to its code or ID.

> Request example

```http
DELETE /api/v3/4870020/discount_coupons/MOXQ3Y222CWXRXA?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/discount_coupons/{couponIdentifier}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**couponIdentifier** | number/string | Coupon identifier in a store. It can be either unique internal coupon `id` OR a discount coupon `code`.
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
deleteCount | number | The number of deleted coupons (`1` or `0` depending on whether the request was successful). It returns `0` when the coupon with the given code is not found


#### Errors

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
403 | Access token doesn't have `update_discount_coupons` scope
500 | The request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
