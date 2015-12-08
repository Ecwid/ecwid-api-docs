# Categories

## Get categories

### Request

> Request example

```http
GET /api/v3/4870020/categories?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/categories?parent={parent}&hidden_categories={hidden_categories}&offset={offset}&limit={limit}&productIds={productIds}&token={token}`

Query field | Type    | Description
----------- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `100`
parent | number | ID of the parent category. Set to `0` to get the list of root categories. Leave empty to get all store categories.
hidden_categories | boolean | By default, Ecwid returns only enabled categories. Set this parameter to `true` if you want hidden (disabled) categories to be returned. `false` is default
productIds | boolean | Set to `true` if you want the results to contain a list of product IDs assigned to category. `false` is default

### Response

> Response example

```json
{
    "total": 2,
    "count": 2,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            "id": 9691094,
            "orderBy": 10,
            "thumbnailUrl": "https://app.ecwid.com/default-store/fruit-230-sq.jpg",
            "originalImageUrl": "https://app.ecwid.com/default-store/fruit-230-sq.jpg",
            "name": "Fruit",
            "url": "http://app.ecwid.com/store/4870020#!/Fruit/c/9691094",
            "productCount": 6,
            "description": "",
            "enabled": true
        },
        {
            "id": 9691095,
            "orderBy": 20,
            "thumbnailUrl": "https://app.ecwid.com/default-store/vegetables-230-sq.jpg",
            "originalImageUrl": "https://app.ecwid.com/default-store/vegetables-230-sq.jpg",
            "name": "Vegetables",
            "url": "http://app.ecwid.com/store/4870020#!/Vegetables/c/9691095",
            "productCount": 4,
            "enabled": true
        }
    ]
}
```

A JSON object of type 'SearchResult' with the following fields:

#### SearchResult
Field | Type | Description
----- | ---- | -----------
total | number | The total number of found items (might be more than the number of returned items)
count | number | The total number of the items returned in this batch
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `100`
items | Array<Category> | The categories list

#### Category
Field | Type | Description
----- | ---- | -----------
id | number | Internal category ID
parentId | number  | ID of the parent category, if any
orderBy | number | Sort order of the category in the parent category subcategories list
thumbnailUrl | string  | Category thumbnail URL. The thumbnail size is specified in the store settings
originalImageUrl | string  | Link to the original (not resized) category image
name | string | Category name
url | string | Category page URL in the store
productCount | number | Number of products in the category and its subcategories
description | string  | The category description in HTML
enabled | boolean | `true` if the category is enabled, `false` otherwise. Use `hidden_categories` in request to get disabled categories
productIds | Array\<number\>  | IDs of the products assigned to the category


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
500 | Cannot retrieve the categories info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Get category

> Request example

```http
GET /api/v3/4870020/categories/10861116?token=abcdn339900932 HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/categories/{categoryId}?token={token}`

Query field | Type    | Description
----------- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**categoryId** | number | Category internal ID

### Response

> Response example

```json
{
    "id": 10861116,
    "parentId": 9691094,
    "orderBy": 20,
    "thumbnailUrl": "http://images-cdn.ecwid.com/images/4870020/244778352.jpg",
    "originalImageUrl": "http://images-cdn.ecwid.com/images/4870020/244778351.jpg",
    "name": "Subfruit2",
    "url": "http://app.ecwid.com/store/4870020#!/Subfruit2/c/10861116",
    "productCount": 0,
    "description": "<p>arf34</p>",
    "enabled": true
}
```

A JSON object of type 'Category' with the following fields:

#### Category

Field | Type | Description
----- | ---- | -----------
id | number | Internal category ID
parentId | number  | ID of the parent category, if any
orderBy | number | Sort order of the category in the parent category subcategories list
thumbnailUrl | string  | Category thumbnail URL. The thumbnail size is specified in the store settings
originalImageUrl | string  | Link to the original (not resized) category image
name | string | Category name
url | string | Category page URL in the store
productCount | number | Number of products in the category and its subcategories
description | string  | The category description in HTML
enabled | boolean | `true` if the category is enabled, `false` otherwise.
productIds | Array\<number\>  | IDs of the products assigned to the category

### Errors

> Error response example

```http
HTTP/1.1 400 Wrong numeric parameter 'id' value: not a number or a number out of range
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
404 | Category is not found
400 | Malformed request parameters
500 | Server error


## Add new category

### Request

> Request body

```http
POST /api/v3/4870020/categories?token=alads043043lk0ds0 HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
    "name": "New Cool Category",
    "description": "Hey, this is my <b>new</b> category!",
    "enabled": true,
    "orderBy": 10,
    "parentId": 9691094
}
```


`POST https://app.ecwid.com/api/v3/{storeId}/categories?token={token}`


Query field | Type    | Description
----------- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

### Request body

A JSON object of type 'Category' with the following fields:

#### Category

Field | Type | Description
----- | ---- | -----------
**name** | string | Category name
parentId | number  | ID of the parent category. Omit this field to add root category
orderBy | number | Sort order of the category in the parent category subcategories list
description | string  | The category description in HTML
enabled | boolean | `true` to make category enabled, `false` otherwise. `true` is default
productIds | Array\<number\>  | IDs of the products to assign to the category

### Response

> Response example

```json
{
    "id": 10869029
}
```


A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus

Field | Type |  Description
-------------- | -------------- | --------------
id | number | ID of the created category


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
400 | Malformed request parameters
404 | The parent category or one of the assigned products is not found
449 | Store catalog cannot be modified at the moment because import is in progress. Retry later.
402 | The merchant plan category limit is reached
409 | Data validation error
409 | There was a conflict modifying the store. Retry later.
500 | Server error





## Update category

### Request

> Request body

```http
PUT /api/v3/4870020/categories/10869029?token=34534509340sdsreoiweurt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
  "name": "Updated name",
  "description": "Updated <b>description</b>",
  "productIds": [
    37208339,
    37208345
  ]
}
```


`PUT https://app.ecwid.com/api/v3/{storeId}/categories/{categoryId}?token={token}`


Query field | Type    | Description
----------- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**categoryId** | number | Category internal ID

### Request body

A JSON object of type 'Category' with the following fields:

#### Category

Field | Type | Description
----- | ---- | -----------
name | string | Category name
parentId | number  | ID of the parent category
orderBy | number | Sort order of the category in the parent category subcategories list
description | string  | The category description in HTML
enabled | boolean | `true` to make category enabled, `false` to disable it. `true` is default
productIds | Array\<number\>  | IDs of the products to assign to the category

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
updateCount | number | The number of updated categories (`1` or `0` depending on whether the update was successful)


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
400 | Malformed request parameters
400 | Category name must not be empty
404 | The parent category or one of the assigned products is not found
449 | Store catalog cannot be modified at the moment because import is in progress. Retry later.
409 | Data validation error
409 | There was a conflict modifying the store. Retry later.
500 | Server error


## Delete a category

> Request example

```http
DELETE /api/v3/4870020/categories/10861116?token=abcdn339900932 HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/categories/{categoryId}?token={token}`

Query field | Type    | Description
----------- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**categoryId** | number | Category internal ID

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
deleteCount | number | The number of deleted categories (`1` or `0` depending on whether the request was successful)

### Errors

> Error response example

```http
HTTP/1.1 400 Bad request
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Malformed request parameters
449 | Store catalog cannot be modified at the moment because import is in progress. Retry later.
500 | Server error


## Upload category image

Upload category image: if the category already has an image attached, the uploaded image will replace the existing one. Maximum allowed file size is 20Mb.

> Request example

```http
POST /api/v3/4870020/categories/1234657/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

binary data
```

`POST https://app.ecwid.com/api/v3/{storeId}/categories/{categoryId}/image?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**categoryId** | number | Category ID
**token** |  string |  oAuth token

When uploading an image for a category, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

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
------| -----| -----------
id | number | Internal image ID

### Errors

> Error response example 

```http
HTTP/1.1 422 Uploaded file cannot be processed. Please make sure you upload an image file
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
404 | Category is not found
413 | The image file is too large (Maximum allowed size is 20Mb)
422 | The uploaded file is not an image
500 | Uploading of the image file failed or there was an internal server error while processing a file. On of possible reasons is image 


#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



## Delete category image

> Request example

```http
DELETE /api/v3/4870020/categories/1234657/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/categories/{categoryId}/image?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**categoryId** | number | Category ID
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
----- | ---- | --------------
deleteCount | number | The number of deleted images (`1` or `0` depending on whether the request was successful)

### Errors

> Error response example 

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
404 | Category is not found
500 | Deleting of the image file failed or there was an internal server error

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message