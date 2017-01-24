# Categories

## Get categories

### Request

> Request example

```http
GET /api/v3/4870020/categories?hidden_categories=true&token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/categories?parent={parent}&hidden_categories={hidden_categories}&offset={offset}&limit={limit}&productIds={productIds}&baseUrl={baseUrl}&cleanUrls={cleanUrls}&token={token}`

Query field | Type    | Description
----------- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `100`
parent | number | ID of the parent category. Set to `0` to get the list of root categories. Leave empty to get all store categories.
hidden_categories | boolean | By default, Ecwid returns only enabled categories. Set this parameter to `true` if you want hidden (disabled) categories to be returned. `false` is default
productIds | boolean | Set to `true` if you want the results to contain a list of product IDs assigned to category. `false` is default
baseUrl | string | Storefront URL for Ecwid to use when returning category URLs in the `url` field. If not specified, Ecwid will use the storefront URL specified in the [store settings](#get-store-profile)
cleanUrls | boolean | If `true`, Ecwid will return the SEO-friendly clean URL (without hash `'#'`) in the `url` field. If `false`, Ecwid will return URL in the old format (with hash `'#'`). We recommend using `true` value if merchant's website supports clean [SEO-friendly URL feature](#seo-friendly-urls)

<aside class='notice'>
To get a list of products in results for each category, set `productIds` parameter to `true` when making a request.
</aside>

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
            "hdThumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/397690775.jpg",
            "thumbnailUrl": "https://dqzrr9k4bjpzk.cloudfront.net/1003/123412341234.jpg",
            "imageUrl": "https://dqzrr9k4bjpzk.cloudfront.net/images/1003/461717703.jpg",
            "originalImageUrl": "https://dqzrr9k4bjpzk.cloudfront.net/1003/124124125.jpg",
            "originalImage": {
                "url": "https://app.ecwid.com/default-store/fruit-230-sq.jpg",
                "width": 123,
                "height": 456
            },            
            "name": "Fruit",
            "url": "http://app.ecwid.com/store/4870020#!/Fruit/c/9691094",
            "productCount": 6,
            "enabledProductCount": 5,
            "description": "",
            "enabled": true
        },
        {
            "id": 9691095,
            "orderBy": 20,
            "hdThumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/397690772.jpg",
            "thumbnailUrl": "https://dqzrr9k4bjpzk.cloudfront.net/1003/123412341254.jpg",
            "imageUrl": "https://dqzrr9k4bjpzk.cloudfront.net/images/1003/461717702.jpg",
            "originalImageUrl": "https://dqzrr9k4bjpzk.cloudfront.net/1003/124124124.jpg",
            "originalImage": {
                "url": "https://dqzrr9k4bjpzk.cloudfront.net/1003/124124124.jpg",
                "width": 123,
                "height": 456
            },
            "name": "Vegetables",
            "url": "http://app.ecwid.com/store/4870020#!/Vegetables/c/9691095",
            "productCount": 4,
            "enabledProductCount": 3,
            "enabled": false
        }
    ]
}
```

> Public token request example

```http
GET /api/v3/4870020/categories?hidden_categories=true&token=public_123abcdaASasdASjndasAnsdu HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

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
            "hdThumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/397690775.jpg",
            "thumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/12321312.jpg",
            "imageUrl": "https://dqzrr9k4bjpzk.cloudfront.net/images/1003/461717702.jpg",
            "originalImageUrl": "https://dpbfm6h358sh7.cloudfront.net/1003/1231231231.jpg",
            "originalImage": {
                "url": "https://app.ecwid.com/default-store/fruit-230-sq.jpg",
                "width": 123,
                "height": 456
            },            
            "name": "Fruit",
            "url": "http://app.ecwid.com/store/4870020#!/Fruit/c/9691094",
            "productCount": 6,
            "enabledProductCount": 5,
            "description": "",
            "enabled": true
        },
        {
            "id": 9691095,
            "enabled": false
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
id | number | Internal unique category ID
parentId | number  | ID of the parent category, if any
orderBy | number | Sort order of the category in the parent category subcategories list
hdThumbnailUrl | string  | Category HD thumbnail URL resized to fit 800x800px
thumbnailUrl | string  | Category thumbnail URL. The thumbnail size is specified in the store settings. Resized to fit 400x400px by default
imageUrl | string | Category image URL. A resized original image to fit 1500x1500px
originalImageUrl | string  | Link to the original (not resized) category image
originalImage | \<ImageDetails\> | Details of the category image
name | string | Category name
url | string |  URL of the category page in the store
productCount | number | Number of products in the category and its subcategories
enabledProductCount | number | Number of enabled products in the category (excluding its subcategories)
description | string  | The category description in HTML
enabled | boolean | `true` if the category is enabled, `false` otherwise. Use `hidden_categories` in request to get disabled categories
productIds | Array\<number\>  | IDs of products assigned to the category as they appear in Ecwid Control Panel > Catalog > Categories

#### ImageDetails 
Field | Type  | Description
----- | ----- | -----------
url | string | Image URL
width | integer | Image width
height | integer | Image height

### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning | Code (optional)
------------|--------|-----------
400 | Request parameters are malformed | 
400 | The cleanUrls value is invalid. It must be either `true` or `false` | `CLEAN_URLS_PARAMETER_IS_INVALID`
415 | Unsupported content-type: expected `application/json` or `text/json` | 
500 | Cannot retrieve the categories info because of an error on the server | 

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Q: How to get URLs for categories?

Direct URL for each category is always available in the `url` field once you make a request to the Ecwid REST API. 

In any Ecwid store there is a [storefront URL](#get-store-profile) field, where store owners can specify their storefront location. In case if it's empty, Ecwid will use their starter site URL to provide category URLs in the REST API and other connected services.

**When a store is embedded into multiple websites**

For this situation you may need to generate a category feed for each of those websites (building a sitemap, etc.), hence there will be multiple storefront URLs to process. In this case you can use `baseUrl` request parameter to get a working category URL in a response from the Ecwid REST API. 

Let's see how it works: 

If `baseUrl` request parameter is specified, then the `url` field will be generated according to that URL as a storefront URL. 

**Examples**

Ecwid store has a storefront URL set in store settings as: `"https://mdemo.ecwid.com"`:

- `baseUrl` parameter is not set in a request:

Example category URL in the `url` field will be: `"https://mdemo.ecwid.com#!/apple/p/70445445"`

- `baseUrl` parameter is set as `"https://mycoolstore.com"`:

Example category URL in the `url` field will be: `"https://mycoolstore.com#!/apple/p/70445445"`

As you can see, the category URL in a response from Ecwid API changes based on the value of the `baseUrl` request parameter. So now you can use it to get category URLs of the same store for any number of storefront URLs.

It is possible to use the `baseUrl` parameter together with the `cleanUrls` parameter. See below for more details on the `cleanUrls` parameter.

**Receiving SEO-friendly (clean) URLs from the Ecwid REST API**

By default, Ecwid's category URLs use hash-based format: `"https://mdemo.ecwid.com#!/apple/p/70445445"`. In case if a website supports the [SEO-friendly (clean) URLs](#seo-friendly-urls), you will need to use the `cleanUrls` request parameter in order to get URLs in that format.

<aside>
  In order for SEO-friendly (clean) URLs to be enabled on your website, please follow the instructions in the <a href="#seo-friendly-urls">SEO-friendly URLs section</a>.
</aside>

Let's see how it works: 

If `cleanUrls` request parameter is set to `true`, then `url` field will have the SEO-friendly format in the response (clean URL, no hash "#").

**Examples**

Ecwid store has a storefront URL set in store settings as: `"https://mdemo.ecwid.com"`

- `cleanUrls` parameter is set to `false` or not set

Example category URL in the `url` field will be: `"https://mdemo.ecwid.com#!/apple/p/70445445"`

- `cleanUrls` parameter is set to `true`

Example category URL in the `url` field will be: `"https://mdemo.ecwid.com/apple-p70445445"`

As you can see, the format of a category URL returned from the Ecwid API changes based on the `cleanUrls` request parameter. So now you can use it to get URLs of any of the two supported URL formats - SEO-friendly (clean) URLs or hash-based URLs. 

It is possible to use the `cleanUrls` parameter together with the `baseUrl` parameter. See above for more details on the `baseUrl` parameter.

## Get category

> Request example

```http
GET /api/v3/4870020/categories/10861116?token=abcdn339900932 HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/categories/{categoryId}?token={token}&baseUrl={baseUrl}&cleanUrls={cleanUrls}`

Query field | Type    | Description
----------- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**categoryId** | number | Internal category ID
baseUrl | string | Storefront URL for Ecwid to use when returning category URLs in the `url` field. If not specified, Ecwid will use the storefront URL specified in the [store settings](#get-store-profile)
cleanUrls | boolean | If `true`, Ecwid will return the SEO-friendly clean URL (without hash `'#'`) in the `url` field. If `false`, Ecwid will return URL in the old format (with hash `'#'`). We recommend using `true` value if merchant's website supports clean [SEO-friendly URL feature](#seo-friendly-urls)

### Response

> Response example

```json
{
    "id": 10861116,
    "parentId": 9691094,
    "orderBy": 20,
    "hdThumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/397690775.jpg",
    "thumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/12321312.jpg",
    "imageUrl": "https://dqzrr9k4bjpzk.cloudfront.net/images/1003/461717702.jpg",
    "originalImageUrl": "https://dpbfm6h358sh7.cloudfront.net/1003/1231231231.jpg",
    "originalImage": {
        "url": "https://dpbfm6h358sh7.cloudfront.net/1003/1231231231.jpg",
        "width": 123,
        "height": 456
    },
    "name": "Subfruit2",
    "url": "http://app.ecwid.com/store/4870020#!/Subfruit2/c/10861116",
    "productCount": 4,
    "enabledProductCount": 3,
    "description": "<p>arf34</p>",
    "enabled": true
}
```

> Disabled category and public token request example

```http
GET /api/v3/4870020/categories/10861117?token=public_123abcdaASasdASjndasAnsdu HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

> Response

```json
{
    "id": 10861117,
    "enabled": false
}
```

A JSON object of type 'Category' with the following fields:

#### Category

Field | Type | Description
----- | ---- | -----------
id | number | Internal unique category ID
parentId | number  | ID of the parent category, if any
orderBy | number | Sort order of the category in the parent category subcategories list
hdThumbnailUrl | string  | Category HD thumbnail URL resized to fit 800x800px
thumbnailUrl | string  | Category thumbnail URL. The thumbnail size is specified in the store settings. Resized to fit 400x400px by default
imageUrl | string | Category image URL. A resized original image to fit 1500x1500px
originalImageUrl | string  | Link to the original (not resized) category image
originalImage | \<ImageDetails\> | Details of the category image
name | string | Category name
url | string |  URL of the category page in the store
productCount | number | Number of products in the category and its subcategories
enabledProductCount | number | Number of enabled products in the category (excluding its subcategories)
description | string  | The category description in HTML
enabled | boolean | `true` if the category is enabled, `false` otherwise.
productIds | Array\<number\>  | IDs of products assigned to the category as they appear in Ecwid Control Panel > Catalog > Categories

#### ImageDetails 
Field | Type  | Description
----- | ----- | -----------
url | string | Image URL
width | integer | Image width
height | integer | Image height

### Errors

> Error response example

```http
HTTP/1.1 400 Wrong numeric parameter 'id' value: not a number or a number out of range
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning | Code (optional)
------------|--------|-----------
400 | Request parameters are malformed | 
400 | The cleanUrls value is invalid. It must be either `true` or `false` | `CLEAN_URLS_PARAMETER_IS_INVALID`
404 | Category is not found | 
415 | Unsupported content-type: expected `application/json` or `text/json` | 
500 | Server error | 

### Q: How to get URLs for categories?

Direct URL for each category is always available in the `url` field once you make a request to the Ecwid REST API. 

In any Ecwid store there is a [storefront URL](#get-store-profile) field, where store owners can specify their storefront location. In case if it's empty, Ecwid will use their starter site URL to provide category URLs in the REST API and other connected services.

**When a store is embedded into multiple websites**

For this situation you may need to generate a category feed for each of those websites (building a sitemap, etc.), hence there will be multiple storefront URLs to process. In this case you can use `baseUrl` request parameter to get a working category URL in a response from the Ecwid REST API. 

Let's see how it works: 

If `baseUrl` request parameter is specified, then the `url` field will be generated according to that URL as a storefront URL. 

**Examples**

Ecwid store has a storefront URL set in store settings as: `"https://mdemo.ecwid.com"`:

- `baseUrl` parameter is not set in a request:

Example category URL in the `url` field will be: `"https://mdemo.ecwid.com#!/apple/p/70445445"`

- `baseUrl` parameter is set as `"https://mycoolstore.com"`:

Example category URL in the `url` field will be: `"https://mycoolstore.com#!/apple/p/70445445"`

As you can see, the category URL in a response from Ecwid API changes based on the value of the `baseUrl` request parameter. So now you can use it to get category URLs of the same store for any number of storefront URLs.

It is possible to use the `baseUrl` parameter together with the `cleanUrls` parameter. See below for more details on the `cleanUrls` parameter.

**Receiving SEO-friendly (clean) URLs from the Ecwid REST API**

By default, Ecwid's category URLs use hash-based format: `"https://mdemo.ecwid.com#!/apple/p/70445445"`. In case if a website supports the [SEO-friendly (clean) URLs](#seo-friendly-urls), you will need to use the `cleanUrls` request parameter in order to get URLs in that format.

<aside>
  In order for SEO-friendly (clean) URLs to be enabled on your website, please follow the instructions in the <a href="#seo-friendly-urls">SEO-friendly URLs section</a>.
</aside>

Let's see how it works: 

If `cleanUrls` request parameter is set to `true`, then `url` field will have the SEO-friendly format in the response (clean URL, no hash "#").

**Examples**

Ecwid store has a storefront URL set in store settings as: `"https://mdemo.ecwid.com"`

- `cleanUrls` parameter is set to `false` or not set

Example category URL in the `url` field will be: `"https://mdemo.ecwid.com#!/apple/p/70445445"`

- `cleanUrls` parameter is set to `true`

Example category URL in the `url` field will be: `"https://mdemo.ecwid.com/apple-p70445445"`

As you can see, the format of a category URL returned from the Ecwid API changes based on the `cleanUrls` request parameter. So now you can use it to get URLs of any of the two supported URL formats - SEO-friendly (clean) URLs or hash-based URLs. 

It is possible to use the `cleanUrls` parameter together with the `baseUrl` parameter. See above for more details on the `baseUrl` parameter.

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
402 | The merchant plan category limit is reached
404 | The parent category or one of the assigned products is not found
409 | Data validation error: the category name or description exceed the max allowed length of characters
415 | Unsupported content-type: expected `application/json` or `text/json`
449 | Store catalog cannot be modified at the moment because import is in progress. Retry later
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
409 | There was a conflict modifying the store (updating a category while it's being edited elsewhere). Retry later.
415 | Unsupported content-type: expected `application/json` or `text/json`
449 | Store catalog cannot be modified at the moment because import is in progress. Retry later.
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
415 | Unsupported content-type: expected `application/json` or `text/json`
449 | Store catalog cannot be modified at the moment because import is in progress. Retry later.
500 | Server error


## Upload category image

Upload category image: if the category already has an image attached, the uploaded image will replace the existing one. Maximum allowed file size is 20Mb.

> Request example

```http
POST /api/v3/4870020/categories/1234657/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/categories/123456/image?token=abcdefg123456';

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

request_url = "https://app.ecwid.com/api/v3/1003/categories/123456/image?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
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
415 | Unsupported content-type: expected `application/octet-stream`
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
