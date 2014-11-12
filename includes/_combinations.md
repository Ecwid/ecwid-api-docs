# Product combinations

## Get all product combinations

> Request example

```http
GET /api/v3/4870020/products/8383892837/combinations?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID


### Response

> Response example (JSON)

```json
[
  {
    "id":7084071,
    "combinationNumber":6,
    "options":[
      {
        "name":"Size",
        "value":"Large"
      },
      {
        "name":"Color",
        "value":"White"
      }
    ],
    "sku":"000076",
    "smallThumbnailUrl":"http://images-cdn.ecwid.com/images/4870020/249912079.jpg",
    "thumbnailUrl":"http://images-cdn.ecwid.com/images/4870020/249912077.jpg",
    "imageUrl":"http://images-cdn.ecwid.com/images/4870020/249912061.jpg",
    "originalImageUrl":"http://images-cdn.ecwid.com/images/4870020/249912061.jpg",
    "quantity":21,
    "unlimited":false,
    "price":1.45,
    "wholesalePrices":[
      {
        "quantity":10,
        "price":1.05
      }
    ],
    "warningLimit":1
  },
  {
    "id":7084072,
    "combinationNumber":5,
    "options":[
      {
        "name":"Color",
        "value":"Red"
      },
      {
        "name":"Size",
        "value":"Large"
      }
    ],
    "sku":"000075",
    "quantity":0,
    "unlimited":false,
    "warningLimit":0
  },
  {
    "id":7084075,
    "combinationNumber":2,
    "options":[
      {
        "name":"Size",
        "value":"Small"
      },
      {
        "name":"Color",
        "value":"White"
      }
    ],
    "sku":"000072",
    "smallThumbnailUrl":"http://images-cdn.ecwid.com/images/4870020/249912078.jpg",
    "thumbnailUrl":"http://images-cdn.ecwid.com/images/4870020/249912076.jpg",
    "imageUrl":"http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "originalImageUrl":"http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "quantity":62,
    "unlimited":false,
    "price":1.45,
    "wholesalePrices":[
      {
        "quantity":10,
        "price":1.35
      },
      {
        "quantity":20,
        "price":1.25
      }
    ],
    "warningLimit":10
  },
  {
    "id":7084076,
    "combinationNumber":1,
    "options":[
      {
        "name":"Size",
        "value":"Small"
      },
      {
        "name":"Color",
        "value":"Red"
      }
    ],
    "sku":"000071",
    "quantity":61,
    "unlimited":false,
    "warningLimit":0
  }
]
```

An array of JSON objects of type 'Combination' with the following fields:

#### Combination
Field | Type  | Description
------| ----- | -----------
id |  number |  Combination ID
combinationNumber | number |  Combination # number, which is displayed in the combinations table in Control panel
options | Array\<*OptionValue*\> | Set of options that identifies this combination. An array of name-value pairs
sku | string  | Combination SKU. Omitted if the combination inherits the base product's SKU
smallThumbnailUrl | string  | URL of the combination thumbnail resized to fit 80x80 px box. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
thumbnailUrl |  string  | URL of the combination thumbnail displayed on the product list pages if the combination is default one. Thumbnails size is defined in the store settings and the same as the product thumbnail size. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the combination image resized to fit 500x500. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
originalImageUrl | string | URL of the original not resized combination image. Omitted if the combination inherits the base product's image.
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this combination, if set. When the combination in stock amount reaches this level, the store administrator gets an email notification. Omitted if the combination inherits the base product's settings.

#### OptionValue
Field | Type | Description
----- | -----| -----------
name |  string | Option name
value | string | Option value

#### WholesalePrice
Field | Type  | Description
----- | ----- | -----------
quantity |  number |  Number of product items on this wholesale tier
price | number |  Product price on the tier

### Errors

> Error response example

```http
HTTP/1.1 400 Wrong numeric parameter 'id' value: not a number or a number out of range
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
402 | The "Product Combinations" feature are not available on the merchant plan
500 | Cannot get combinations because of an error on the server


## Get product combination

> Request example

```http
GET /api/v3/4870020/products/8383892837/combinations/772828388?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID
**combinationId** | number | Internal combination ID


### Response

> Response example (JSON)

```json
{
    "id": 7084075,
    "combinationNumber": 2,
    "options": [
        {
            "name": "Size",
            "value": "Small"
        },
        {
            "name": "Color",
            "value": "White"
        }
    ],
    "sku": "000072",
    "smallThumbnailUrl": "http://images-cdn.ecwid.com/images/4870020/249919682.jpg",
    "thumbnailUrl": "http://images-cdn.ecwid.com/images/4870020/249919680.jpg",
    "imageUrl": "http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "originalImageUrl": "http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "quantity": 62,
    "unlimited": false,
    "price": 1.45,
    "wholesalePrices": [
        {
            "quantity": 10,
            "price": 1.35
        },
        {
            "quantity": 20,
            "price": 1.25
        }
    ],
    "warningLimit": 10
}
```

An array of JSON objects of type 'Combination' with the following fields:

#### Combination
Field | Type  | Description
------| ----- | -----------
id |  number |  Combination ID
combinationNumber | number |  Combination # number, which is displayed in the combinations table in Control panel
options | Array\<*OptionValue*\> | Set of options that identifies this combination. An array of name-value pairs
sku | string  | Combination SKU. Omitted if the combination inherits the base product's SKU
smallThumbnailUrl | string  | URL of the combination thumbnail resized to fit 80x80 px box. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
thumbnailUrl |  string  | URL of the combination thumbnail displayed on the product list pages if the combination is default one. Thumbnails size is defined in the store settings and the same as the product thumbnail size. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the combination image resized to fit 500x500. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
originalImageUrl | string | URL of the original not resized combination image. Omitted if the combination inherits the base product's image.
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this combination, if set. When the combination in stock amount reaches this level, the store administrator gets an email notification. Omitted if the combination inherits the base product's settings.

#### OptionValue
Field | Type | Description
----- | -----| -----------
name |  string | Option name
value | string | Option value

#### WholesalePrice
Field | Type  | Description
----- | ----- | -----------
quantity |  number |  Number of product items on this wholesale tier
price | number |  Product price on the tier

### Errors

> Error response example

```http
HTTP/1.1 404 Not found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
402 | The "Product Combinations" feature are not available on the merchant plan
404 | The combination is not found
500 | Cannot get the combination because of an error on the server




## Create product combination

You can create a new product combination using this method. Please make sure the product options your refer to when creating a combo exist. You can create product options using the [Update product](#update-a-product) call

> Request example

```http
POST /api/v3/4870020/products/8383892837/combinations?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8

{
    "options": [
        {
            "name": "Size",
            "value": "L"
        },
        {
            "name": "Color",
            "value": "Red"
        }
    ],
    "price": 10,
    "quantity": 4,
    "weight": 0.5,
    "sku": "combination-sku"
}
```

`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID

### Request body

A JSON object of type 'Combination' with the following fields:

#### Combination
Field | Type  | Description
------| ----- | -----------
options | Array\<*OptionValue*\> | Set of options that identifies this combination. An array of name-value pairs
sku | string  | Combination SKU. Omitted if the combination inherits the base product's SKU
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this combination, if set. When the combination in stock amount reaches this level, the store administrator gets an email notification. Omitted if the combination inherits the base product's settings.

#### OptionValue
Field | Type | Description
----- | -----| -----------
**name** |  string | Option name
**value** | string | Option value

#### WholesalePrice
Field | Type  | Description
----- | ----- | -----------
**quantity** |  number |  Number of product items on this wholesale tier
**price** | number |  Product price on the tier

### Response

> Response example

```json
{
    "id": 397662764,
    "message": "Successfully created",
    "success": true
}
```

A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type  | Description
----- | ----- | -----------
id | number | ID of the created combination
message | string | Status message 
success | boolean | `true` if the combination has been created, `false` otherwise


### Errors

> Error response example

```http
HTTP/1.1 404 Not found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
402 | The "Product Combinations" feature are not available on the merchant plan
404 | The product is not found
409 | The specified sku or options variation already exists
500 | Cannot get the combination because of an error on the server




## Update product combination

> Request example

```http
PUT /api/v3/4870020/products/8383892837/combinations/772828388?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8

{
    "price": 10,
    "quantity": 4,
    "weight": 0.5,
    "sku": "combination-new-sku"
}
```

`PUT https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID
**combinationId** | number | Internal combination ID

### Request body

A JSON object of type 'Combination' with the following fields:

#### Combination
Field | Type  | Description
------| ----- | -----------
options | Array\<*OptionValue*\> | Set of options that identifies this combination. An array of name-value pairs
sku | string  | Combination SKU. Omitted if the combination inherits the base product's SKU
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this combination, if set. When the combination in stock amount reaches this level, the store administrator gets an email notification. Omitted if the combination inherits the base product's settings.
inventoryDelta | number | Amount by which to increase the combination inventory. A negative number decreases inventory.

#### OptionValue
Field | Type | Description
----- | -----| -----------
**name** |  string | Option name
**value** | string | Option value

#### WholesalePrice
Field | Type  | Description
----- | ----- | -----------
**quantity** |  number |  Number of product items on this wholesale tier
**price** | number |  Product price on the tier



### Response


> Response example

```json
{
    "message": "Combination was successfully updated",
    "updateCount": 1,
    "success": true
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus

Field | Type | Description
----- | ---- | -----------
message | string | Status message 
updateCount | number | The number of updated combinations (`1` or `0` depending on whether the update was successful)
success | boolean | `true` if the combination has been updated, `false` otherwise

### Errors

> Error response example

```http
HTTP/1.1 404 Not found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
402 | The "Product Combinations" feature are not available on the merchant plan
404 | The product or combination is not found
409 | The specified sku or options variation already exists
500 | Cannot get the combination because of an error on the server




## Delete product combination

> Request example

```http
DELETE /api/v3/4870020/products/8383892837/combinations/772828388?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID
**combinationId** | number | Internal combination ID


### Response

> Response example

```json
{
    "deleteCount": 1,
    "success": true
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted combinations (`1` or `0` depending on whether the request was successful)
success | boolean | `true` if the combination has been deleted, `false` otherwise


### Errors

> Error response example

```http
HTTP/1.1 404 Not found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
402 | The "Product Combinations" feature is not available on the merchant plan
404 | The combination is not found
500 | Cannot remove the combination because of an error on the server













## Delete all product combinations

> Request example

```http
DELETE /api/v3/4870020/products/8383892837/combinations?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID


### Response

> Response example

```json
{
    "deleteCount": 4,
    "success": true
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted combinations
success | boolean | `true` if the operation has been successful, `false` otherwise


### Errors

> Error response example

```http
HTTP/1.1 404 Not found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
402 | The "Product Combinations" feature is not available on the merchant plan
500 | Cannot delete combinations because of an error on the server










## Upload combination image

> Request example

```http
POST /api/v3/4870020/products/1234657/combinations/463737373/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

binary data
```

`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}/image?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
**combinationId** | number | Internal combination ID

### Response

> Response example

```json
{
    "id": 240198557
}
```

A JSON object of type 'UploadStatus' with the following fields:

#### UploadStatus
Field | Type |  Description
----- | -----| ------------
id | number | Internal image ID


### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8

{
    "errorMessage": "Internal server error"
}
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Uploading of the image file failed or there was an internal server error while processing a file
404 | Product or combination in request are not found
413 | The image file is too large (Maximum allowed file size is 20Mb)
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message






## Delete combination image

> Request example

```http
DELETE /api/v3/4870020/products/1234657/combinations/463737373/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}/image?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
**combinationId** | number | Internal combination ID

### Response

> Response example

```json
{
    "deleteCount": 1,
    "success": true
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted images (`1` if the image has been removed, `0` otherwise)
success | boolean | `true` if the operation has been successful, `false` otherwise


### Errors

> Error response example 

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, a JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Request failed or there was an internal server error while processing a file
404 | Product or combination in request are not found
400 | Request parameters are malformed
402 | The Product combinations are not available on the merchant plan

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
