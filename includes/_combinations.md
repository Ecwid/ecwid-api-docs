## Product combinations

Using the methods below you can get/update/delete combinations and their details in an Ecwid store. Learn more about product combinations in the [Ecwid Help Center](https://support.ecwid.com/hc/en-us/articles/207100299-Product-Combinations).

<aside class='note'>
Product Combinations are available on the Ecwid's <a href='https://www.ecwid.com/pricing'>Business plan and higher</a>.
</aside>

### Get all product combinations

Get all combinations of a specific product in an Ecwid store.

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
    "hdThumbnailUrl": "https://images-cdn.ecwid.com/images/4870020/397690841.jpg",
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
thumbnailUrl |  string | URL of the product combination thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. Default size of biggest dimension is 400px. Omitted if the combination inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product combination image resized to fit 1500x1500px. Omitted if the combination inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product combination thumbnail resized to fit 160x160px. Omitted if the combination inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product combination HD thumbnail URL resized to fit 800x800px. Omitted if the combination inherits the base product's image.
originalImageUrl |  string  | URL of the original not resized product combination image. Omitted if the combination inherits the base product's image.
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
402 | The "Product Combinations" feature are not available on the merchant plan
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get combinations because of an error on the server


### Get product combination

Get a specific product combination details referring to its ID.

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
thumbnailUrl |  string | URL of the product combination thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. Default size of biggest dimension is 400px. Omitted if the combination inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product combination image resized to fit 1500x1500px. Omitted if the combination inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product combination thumbnail resized to fit 160x160px. Omitted if the combination inherits the base product's image. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product combination HD thumbnail URL resized to fit 800x800px. Omitted if the combination inherits the base product's image.
originalImageUrl |  string  | URL of the original not resized product combination image. Omitted if the combination inherits the base product's image.
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
402 | The "Product Combinations" feature are not available on the merchant plan
404 | The combination is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get the combination because of an error on the server




### Create product combination

You can create a new product combination using this method. If the options you specify in request don't exist, they will be created automatically with the type of dropdown. If you want to create options explicitly, use the [Update product](#update-a-product) call to create them. 

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

#### Request body

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
id | number | ID of the created combination


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
402 | The "Product Combinations" feature are not available on the merchant plan
404 | The product is not found
409 | The specified sku or options variation already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get the combination because of an error on the server




### Update product combination

Update a specific product combination details referring to its ID.

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

#### Request body

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
updateCount | number | The number of updated combinations (`1` or `0` depending on whether the update was successful)

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
402 | The "Product Combinations" feature are not available on the merchant plan
404 | The product or combination is not found
409 | The specified sku or options variation already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot get the combination because of an error on the server




### Delete product combination

Delete a specific product combination referring to its ID.

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
deleteCount | number | The number of deleted combinations (`1` or `0` depending on whether the request was successful)


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
402 | The "Product Combinations" feature is not available on the merchant plan
404 | The combination is not found
500 | Cannot remove the combination because of an error on the server



### Delete all product combinations

Delete all product combinations of a product in an Ecwid store.

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
deleteCount | number | The number of deleted combinations


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
402 | The "Product Combinations" feature is not available on the merchant plan
500 | Cannot delete combinations because of an error on the server



### Adjust combination inventory

> Request example

```http
PUT /api/v3/4870020/products/8383892837/combinations/772828388/inventory?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
    "quantityDelta": -10
}
```
When your integration changes in stock quantity of product combination in a store pretty often, it becomes harder and harder to keep track of how many items are actually in stock. For example, when at one point of time you have 3 items in stock and 5 in the very next second, then using the specific values can result in incorrect stock quantity.

This method solves this very problem: you can increase or decrease the product combination's stock quantity by a delta quantity. For example, if you need to decrease quantity by 10 items, you can use this method. 

This method is also available for [products](https://developers.ecwid.com/api-documentation/products#adjust-product-inventory).

`PUT https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}/inventory?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**combinationId** | number | Combination ID
**token** |  string |  oAuth token

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
400 | Request parameters are malformed
402 | This functionality is not available on this plan
404 | Product not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Could not process the request, internal server error

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message





### Upload combination image

Upload a custom image for a specific product combination referring to its ID.

> Request example

```http
POST /api/v3/4870020/products/1234657/combinations/463737373/image?token=123456789abcd HTTP/1.1
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

`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}/image?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
**combinationId** | number | Internal combination ID

When uploading an image for a combination, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

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
404 | Product or combination in request are not found
413 | The image file is too large (Maximum allowed file size is 20Mb)
422 | The uploaded file is not an image
415 | Unsupported content-type: expected `application/octet-stream`
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message






### Delete combination image

Delete an image of a specific product combination in an Ecwid store.

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
402 | The Product combinations are not available on the merchant plan
404 | Product or combination in request are not found
500 | Request failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
