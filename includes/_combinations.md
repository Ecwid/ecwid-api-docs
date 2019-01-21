## Product variations

Using the methods below you can get/update/delete variations and their details in an Ecwid store. Learn more about product variations in the [Ecwid Help Center](https://support.ecwid.com/hc/en-us/articles/207100299-Product-Combinations).

<aside class='note'>
Product Variations are available on the Ecwid's <a href='https://www.ecwid.com/pricing'>Business plan and higher</a>.
</aside>

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

### Get all product variations

Get all variations of a specific product in an Ecwid store.

> Request example

```http
GET /api/v3/4870020/products/8383237/combinations?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
Accept-Encoding: gzip
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID


#### Response

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
    "hdThumbnailUrl": "https://images-cdn.ecwid.com/images/4870020/397690841.jpg",
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
    "warningLimit":1,
    "compareToPrice": 2,
    "attributes": [
        {
            "id": 9998010,
            "name": "UPC",
            "value": "0435943543594395",
            "show": "DESCR",
            "type": "UPC"
        }
    ]
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
    "hdThumbnailUrl": "https://images-cdn.ecwid.com/images/4870020/397690841.jpg",
    "thumbnailUrl":"http://images-cdn.ecwid.com/images/4870020/249912076.jpg",
    "imageUrl":"http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "originalImageUrl":"http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "quantity":62,
    "unlimited":false,
    "price":1.45,
    "compareToPrice": 2.22,
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

An array of JSON objects of type 'Variation' with the following fields:

#### Variation
Field | Type  | Description
------| ----- | -----------
id |  number |  Variation ID
combinationNumber | number |  Variation # number, which is displayed in the variations table in Control panel
options | Array\<*OptionValue*\> | Set of options that identifies this variation. An array of name-value pairs
sku | string  | Variation SKU. Omitted if the variation inherits the base product's SKU
thumbnailUrl |  string | URL of the product variation thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. Default size of biggest dimension is 400px. Omitted if the variation inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product variation image resized to fit 1500x1500px. Omitted if the variation inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product variation thumbnail resized to fit 160x160px. Omitted if the variation inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product variation HD thumbnail URL resized to fit 800x800px. Omitted if the variation inherits the base product's image.
originalImageUrl |  string  | URL of the original not resized product variation image. Omitted if the variation inherits the base product's image.
quantity | number | Amount of the variation items in stock. Omitted if the variation inherits the base product's quantity.
unlimited | boolean | `true` if the variation has unlimited stock (that is, never runs out)
price | number | Variation price. Omitted if the variation inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the variation's wholesale price tiers (quantity limit and price). Omitted if the variation inherits the base product's tiered price settings. 
weight | number | Variation weight in the units defined in store settings. Omitted if the variation inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this variation, if set. When the variation in stock amount reaches this level, the store administrator gets an email notification. Omitted if the variation inherits the base product's settings
attributes | Array\<*AttributeValue*\> | Variation's UPC attribute and its value
compareToPrice | number | Variation's sale price displayed strike-out in the customer frontend *Omitted if empty*

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

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-types) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit’ attributes. The 'type’ field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines where to display the product attribute value:. Supported values: `NOTSHOW`, `DESCR`, `PRICE` 

#### Errors

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
402 | The "Product Variations" feature are not available on the merchant plan
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get variations because of an error on the server


### Get product variation

Get a specific product variation details referring to its ID.

> Request example

```http
GET /api/v3/4870020/products/8392837/combinations/772828388?token=1234567890qwqeertt HTTP/1.1
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
**combinationId** | number | Internal variation ID


#### Response

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
    "hdThumbnailUrl": "https://images-cdn.ecwid.com/images/4870020/397690841.jpg",
    "thumbnailUrl": "http://images-cdn.ecwid.com/images/4870020/249919680.jpg",
    "imageUrl": "http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "originalImageUrl": "http://images-cdn.ecwid.com/images/4870020/249912060.jpg",
    "quantity": 62,
    "unlimited": false,
    "price": 1.45,
    "compareToPrice": 2.22,
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

An array of JSON objects of type 'Variation' with the following fields:

#### Variation
Field | Type  | Description
------| ----- | -----------
id |  number |  Variation ID
combinationNumber | number |  Combination # number, which is displayed in the variations table in Control panel
options | Array\<*OptionValue*\> | Set of options that identifies this variation. An array of name-value pairs
sku | string  | Variation SKU. Omitted if the variation inherits the base product's SKU
thumbnailUrl |  string | URL of the product variation thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. Default size of biggest dimension is 400px. Omitted if the variation inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product variation image resized to fit 1500x1500px. Omitted if the variation inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product variation thumbnail resized to fit 160x160px. Omitted if the variation inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product variation HD thumbnail URL resized to fit 800x800px. Omitted if the variation inherits the base product's image.
originalImageUrl |  string  | URL of the original not resized product variation image. Omitted if the variation inherits the base product's image.
quantity | number | Amount of the variation items in stock. Omitted if the variation inherits the base product's quantity.
unlimited | boolean | `true` if the variation has unlimited stock (that is, never runs out)
price | number | Variation price. Omitted if the variation inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the variation's wholesale price tiers (quantity limit and price). Omitted if the variation inherits the base product's tiered price settings. 
weight | number | Variation weight in the units defined in store settings. Omitted if the variation inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this variation, if set. When the variation in stock amount reaches this level, the store administrator gets an email notification. Omitted if the variation inherits the base product's settings
attributes | Array\<*AttributeValue*\> | Variation's UPC attribute and its value
compareToPrice | number | Variation's sale price displayed strike-out in the customer frontend *Omitted if empty*

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

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-types) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit’ attributes. The 'type’ field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines where to display the product attribute value:. Supported values: `NOTSHOW`, `DESCR`, `PRICE` 

#### Errors

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
402 | The "Product Variations" feature are not available on the merchant plan
404 | The variation is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get the variation because of an error on the server




### Create product variation

> Request example

```http
POST /api/v3/4870020/products/8392837/combinations?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
```

```json
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
    "compareToPrice": 15,
    "sku": "combination-sku",
    "attributes": [
        {
            "id": 9998010,
            "name": "UPC",
            "value": "0435943543594395",
            "show": "DESCR",
            "type": "UPC"
        }
    ]
}
```

You can create a new product variation using this method. If the options you specify in request don't exist, they will be created automatically with the type of dropdown. If you want to create options explicitly, use the [Update product](#update-a-product) call to create them. 

<aside class="notice">
You can create only one variation per API request. To create several variations, send several separate requests for each coupon.
</aside>

`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID

#### Request body

A JSON object of type 'Variation' with the following fields:

#### Variation
Field | Type  | Description
------| ----- | -----------
options | Array\<*OptionValue*\> | Set of options that identifies this variation. An array of name-value pairs
sku | string  | Variation SKU. Omitted if the variation inherits the base product's SKU
quantity | number | Amount of the variation items in stock. Omitted if the variation inherits the base product's quantity.
unlimited | boolean | `true` if the variation has unlimited stock (that is, never runs out)
price | number | Variation price. Omitted if the variation inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the variation's wholesale price tiers (quantity limit and price). Omitted if the variation inherits the base product's tiered price settings. 
weight | number | Variation weight in the units defined in store settings. Omitted if the variation inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this variation, if set. When the variation in stock amount reaches this level, the store administrator gets an email notification. Omitted if the variation inherits the base product's settings
attributes | Array\<*AttributeValue*\> | Variation's UPC attribute and its value
compareToPrice | number | Variation's sale price displayed strike-out in the customer frontend *Omitted if empty*

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

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-types) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit’ attributes. The 'type’ field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines where to display the product attribute value:. Supported values: `NOTSHOW`, `DESCR`, `PRICE` 

#### Response

> Response example

```json
{
    "id": 397662764
}
```

A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type  | Description
----- | ----- | -----------
id | number | ID of the created variation


#### Errors

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
402 | The "Product Variations" feature are not available on the merchant plan
404 | The product is not found
409 | The specified sku or options variation already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get the variation because of an error on the server




### Update product variation

Update a specific product variation details referring to its ID.

> Request example

```http
PUT /api/v3/4870020/products/8392837/combinations/7728288?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
```

```json
{
    "price": 10,
    "quantity": 4,
    "weight": 0.5,
    "sku": "combination-new-sku",
    "compareToPrice": 15,
    "attributes": [
        {
            "id": 9998010,
            "name": "UPC",
            "value": "0435943543594395",
            "show": "DESCR",
            "type": "UPC"
        }
    ]
}
```

`PUT https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}?checkLowStockNotification={checkLowStockNotification}&token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID
**combinationId** | number | Internal variation ID
checkLowStockNotification | boolean | If `true`, makes Ecwid check whether the low stock email notification needs to be sent to merchant after request is sent

#### Request body

A JSON object of type 'Variation' with the following fields:

#### Variation
Field | Type  | Description
------| ----- | -----------
options | Array\<*OptionValue*\> | Set of options that identifies this variation. An array of name-value pairs
sku | string  | Variation SKU. Omitted if the variation inherits the base product's SKU
quantity | number | Amount of the variation items in stock. Omitted if the variation inherits the base product's quantity.
unlimited | boolean | `true` if the variation has unlimited stock (that is, never runs out)
price | number | Variation price. Omitted if the variation inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the variation's wholesale price tiers (quantity limit and price). Omitted if the variation inherits the base product's tiered price settings. 
weight | number | Variation weight in the units defined in store settings. Omitted if the variation inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this variation, if set. When the variation in stock amount reaches this level, the store administrator gets an email notification. Omitted if the variation inherits the base product's settings
attributes | Array\<*AttributeValue*\> | Variation's UPC attribute and its value
compareToPrice | number | Variation's sale price displayed strike-out in the customer frontend *Omitted if empty*

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

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-types) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit’ attributes. The 'type’ field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines where to display the product attribute value:. Supported values: `NOTSHOW`, `DESCR`, `PRICE` 

#### Response

> Response example

```json
{
    "updateCount": 1
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus

Field | Type | Description
----- | ---- | -----------
updateCount | number | The number of updated variations (`1` or `0` depending on whether the update was successful)

#### Errors

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
402 | The "Product Variations" feature are not available on the merchant plan
404 | The product or variation is not found
409 | The specified sku or options variation already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get the variation because of an error on the server




### Delete product variation

Delete a specific product variation referring to its ID.

> Request example

```http
DELETE /api/v3/4870020/products/8392837/combinations/7728388?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID
**combinationId** | number | Internal variation ID


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
----- | ---- | --------------
deleteCount | number | The number of deleted variations (`1` or `0` depending on whether the request was successful)


#### Errors

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
402 | The "Product Variations" feature is not available on the merchant plan
404 | The variation is not found
500 | Cannot remove the variation because of an error on the server



### Delete all product variations

Delete all product variations of a product in an Ecwid store.

> Request example

```http
DELETE /api/v3/4870020/products/8392837/combinations?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**productId** | number | Internal product ID


#### Response

> Response example

```json
{
    "deleteCount": 4
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted variations


#### Errors

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
402 | The "Product Variations" feature is not available on the merchant plan
500 | Cannot delete variations because of an error on the server



### Adjust variation inventory

> Request example

```http
PUT /api/v3/4870020/products/8392837/combinations/7728288/inventory?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

```json
{
    "quantityDelta": -10
}
```
When your integration changes in stock quantity of product variation in a store pretty often, it becomes harder and harder to keep track of how many items are actually in stock. For example, when at one point of time you have 3 items in stock and 5 in the very next second, then using the specific values can result in incorrect stock quantity.

This method solves this very problem: you can increase or decrease the product variation's stock quantity by a delta quantity. For example, if you need to decrease quantity by 10 items, you can use this method. 

This method is also available for [products](https://developers.ecwid.com/api-documentation/products#adjust-product-inventory).

`PUT https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}/inventory?checkLowStockNotification={checkLowStockNotification}&token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**combinationId** | number | Variation ID
**token** |  string |  oAuth token
checkLowStockNotification | boolean | If `true`, makes Ecwid check whether the low stock email notification needs to be sent to merchant after request is sent

#### Request body

A JSON object of type 'Inventory' with the following fields:

#### Inventory
Field | Type |  Description
------| ---- | ------------
**quantityDelta** | number | Delta value used to update product quantity. Negative value will decrease quantity, positive one will increase it.

#### Response

> Response example (JSON)

```json
{
  "updateCount": 1
}
```

A JSON object of type 'InventoryAdjustmentStatus' with the following fields:

#### InventoryAdjustmentStatus

Field | Type |  Description
-------------- | -------------- | --------------
updateCount | number | The number of updated products (`1` or `0` depending on whether the update was successful)
warning | string | Inventory update warning(optional). For example, the warning will display if the stock became negative

#### Errors

```http
HTTP/1.1 400 Bad Request
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
-------------- | --------------
400 | Request parameters are malformed: SKU for combination is not set, etc.
402 | This functionality is not available on this plan
404 | Product not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Could not process the request, internal server error

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message





### Upload variation image

Upload a custom image for a specific product variation referring to its ID.

> Request example

```http
POST /api/v3/4870020/products/8392837/combinations/463737373/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/products/123456/combinations/1233541/image?token=abcdefg123456';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $file);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: image/jpeg;'));

$result = curl_exec($ch);
curl_close ($ch);
```

> Python Example

```python
import requests

request_url = "https://app.ecwid.com/api/v3/1003/products/123456/combinations/1233541/image?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}/image?token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
**combinationId** | number | Internal variation ID
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading an image for a variation, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

#### Response

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


#### Errors

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
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan
404 | Product or variation in request are not found
413 | The image file is too large (Maximum allowed file size is 20Mb)
422 | The uploaded file is not an image
415 | Unsupported content-type: expected `application/octet-stream`
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message






### Delete variation image

Delete an image of a specific product variation in an Ecwid store.

> Request example

```http
DELETE /api/v3/4870020/products/8392837/combinations/463737373/image?token=123456789abcd HTTP/1.1
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
**combinationId** | number | Internal variation ID

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
----- | ---- | --------------
deleteCount | number | The number of deleted images (`1` if the image has been removed, `0` otherwise)


#### Errors

> Error response example 

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, a JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
402 | The Product variations are not available on the merchant plan
404 | Product or variation in request are not found
500 | Request failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
