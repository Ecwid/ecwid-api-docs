# Products

## Search products

Search/filter store products

### Request

> Request examples

```http
GET /api/v3/4870020/products?keyword=apple&token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products?keyword={keyword}&priceFrom={priceFrom}&priceTo={priceTo}&category={category}&withSubcategories={withSubcategories}&sortBy={sortBy}&offset={offset}&limit={limit}&createdFrom={createdFrom}&createdTo={createdTo}&updatedFrom={updatedFrom}&updatedTo={updatedTo}&enabled={enabled}&inStock={inStock}&field{attributeName}={attributeValues}&field{attributeId}={attributeValues}&token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
keyword |  string | Search term. Ecwid searches products over multiple fields: title, description, SKU, product options, category name, gallery image descriptions, values of attributes
priceFrom |  number | Minimum product price
priceTo | number | Maximum product price
category | number | Category ID
withSubcategories |  boolean | `true`/`false`: defines whether Ecwid should search in subcategories of the category you set in `category` field. Ignored if `category` field is not set . `false` is the default value
sortBy |  string | Sort order. Supported values: <ul><li>`RELEVANCE` *default*</li> <li>`ADDED_TIME_DESC`</li> <li>`ADDED_TIME_ASC`</li> <li>`NAME_ASC`</li> <li>`NAME_DESC`</li> <li>`PRICE_ASC`</li> <li>`PRICE_DESC`</li></ul>
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
createdFrom | string | Product creation date (lower bound) Format: YYYY-MM-DD
createdTo | string | Product creation date (upper bound) Format: YYYY-MM-DD
updatedFrom | string | Product last update date (lower bound) Format: YYYY-MM-DD
updatedTo | string | Product last update date (upper bound) Format: YYYY-MM-DD
enabled | boolean | `true` to get only enabled products, `false` to get only disabled products
inStock | boolean | `true` to get only products in stock, `false` to get out of stock products
field{attributeName} | string | Filter by product attribute values. Format: field{attributeName}=param[,param], where "attributeName" is the attribute name and "param" is the attribute value. You can place several values separated by comma. In that case values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `fieldBrand=Apple&fieldCapacity=32GB,64GB`
field{attributeId} | string | Filter by product attribute values. Works the same as the filter by `field{attributeName}` but attribute IDs are used instead of attribute names. This way is insensitive to attributes renaming.

<aside class="notice">
If no filters are set in the URL, API will return all products
</aside>

<aside class="notice">
Parameters in bold are mandatory
</aside>

### Response

> Response example (JSON)

```json
{
    "total": 2,
    "count": 2,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            "id": 37208338,
            "sku": "00000",
            "smallThumbnailUrl": "http://app.ecwid.com/default-store/00000-80-sq.jpg",
            "thumbnailUrl": "http://app.ecwid.com/default-store/00000-230-sq.jpg",
            "imageUrl": "http://app.ecwid.com/default-store/00000-sq.jpg",
            "unlimited": true,
            "inStock": true,
            "name": "Apple",
            "price": 1.99,
            "priceInProductList": 1.99,
            "weight": 0.32,
            "url": "http://app.ecwid.com/store/4870020#!/~/product/id=37208338",
            "created": "2014-06-06 18:57:19 +0400",
            "updated": "2014-06-06 18:57:19 +0400",
            "productClassId": 0,
            "enabled": true,
            "description": "<h5>Apple</h5>\n<p>The apple is the pomaceous fruit of the apple tree, species Malus domestica in the rose family Rosaceae. It is one of the most widely cultivated tree fruits. The tree is small and deciduous, reaching 3 to 12 metres (9.8 to 39 ft) tall, with a broad, often densely twiggy crown. The leaves are alternately arranged simple ovals 5 to 12 cm long and 3–6 centimetres (1.2–2.4 in) broad on a 2 to 5 centimetres (0.79 to 2.0 in) petiole with an acute tip, serrated margin and a slightly downy underside. Blossoms are produced in spring simultaneously with the budding of the leaves. The flowers are white with a pink tinge that gradually fades, five petaled, and 2.5 to 3.5 centimetres (0.98 to 1.4 in) in diameter. The fruit matures in autumn, and is typically 5 to 9 centimetres (2.0 to 3.5 in) diameter. The center of the fruit contains five carpels arranged in a five-point star, each carpel containing one to three seeds.</p>\n<p>The tree originated from Central Asia, where its wild ancestor is still found today. There are more than 7,500 known cultivars of apples resulting in a range of desired characteristics. Cultivars vary in their yield and the ultimate size of the tree, even when grown on the same rootstock.</p>\n<p>vAt least 55 million tonnes of apples were grown worldwide in 2005, with a value of about $10 billion. China produced about 35% of this total. The United States is the second leading producer, with more than 7.5% of the world production. Turkey, France, Italy, and Iran are also among the leading apple exporters.</p>\n<p> </p>\n<div style=\"padding: 24px 24px 24px 21px; display: block; background-color: #ececec;\">From <a style=\"color: #1e7ec8; text-decoration: underline;\" title=\"Wikipedia\" href=\"http://en.wikipedia.org\">Wikipedia</a>, the free encyclopedia</div>",
            "descriptionTruncated": false
        },
        {
            "id": 37208344,
            "sku": "00003",
            "smallThumbnailUrl": "http://app.ecwid.com/default-store/00003-80-sq.jpg",
            "thumbnailUrl": "http://app.ecwid.com/default-store/00003-230-sq.jpg",
            "imageUrl": "http://app.ecwid.com/default-store/00003-sq.jpg",
            "unlimited": true,
            "inStock": true,
            "name": "Orange",
            "price": 2.99,
            "priceInProductList": 3.39,
            "weight": 0.32,
            "url": "http://app.ecwid.com/store/4870020#!/~/product/id=37208344",
            "created": "2014-06-06 18:57:19 +0400",
            "updated": "2014-06-06 18:57:19 +0400",
            "productClassId": 0,
            "enabled": true,
            "description": "<h5>Orange</h5>\n<p>An orange—specifically, the sweet orange—is the citrus Citrus ×sinensis (syn. Citrus aurantium L. var. dulcis L., or Citrus aurantium Risso) and its fruit. The orange is a hybrid of ancient cultivated origin, possibly between pomelo (Citrus maxima) and tangerine (Citrus reticulata). It is a small flowering tree growing to about 10 m tall with evergreen leaves, which are arranged alternately, of ovate shape with crenulate margins and 4–10 cm long. The orange fruit is a hesperidium, a type of berry.</p>\n<p>Oranges originated in Southeast Asia. The fruit of Citrus sinensis is called sweet orange to distinguish it from Citrus aurantium, the bitter orange. The name is thought to ultimately derive from the Dravidian and Telugu word for the orange tree, with its final form developing after passing through numerous intermediate languages.</p>\n<p>In a number of languages, it is known as a \"Chinese apple\" (e.g. Dutch Sinaasappel, \"China's apple\", or \"Apfelsine\" in German).</p>\n<p> </p>\n<div style=\"padding: 24px 24px 24px 21px; display: block; background-color: #ececec;\">From <a style=\"color: #1e7ec8; text-decoration: underline;\" title=\"Wikipedia\" href=\"http://en.wikipedia.org\">Wikipedia</a>, the free encyclopedia</div>",
            "descriptionTruncated": false,
            "categoryIds": [
                1234567,
                39303939
            ],
            "defaultCategoryId": 1234567,
            "favorites": {
                "count":3201,
                "displayedCount":"3K"
            }
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
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
items | Array<ProductEntry> | The items list

#### ProductEntry
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique integer product identifier
sku | string |  Product SKU. Items with options can have several SKUs specified in the product combinations.
quantity |  number | Amount of product items in stock. *This field is omitted for the products with unlimited stock*
unlimited | boolean | `true` if the product has unlimited stock
inStock | boolean | `true` if the product or any of its combinations is in stock (quantity is more than zero) or has unlimited quantity. `false` otherwise.
name |  string |  Product title
price | number |  Base product price
priceInProductList | number |  Product price displayed in the product list. May differ from the *price* value when the product has combinations and the default combination's price is different from the base product price
compareToPrice |  number | Product's sale price displayed strike-out in the customer frontend
weight |  number | Product weight in the units defined in store settings. *Omitted for intangible products*
url | string |  URL of the product's details page in the store
created | string | Date and time of the product creation. Example: `2014-07-30 10:32:37 +0400`
updated |  string | Product last update date/time
productClassId |  number | Id of the class (type) that this product belongs to. `0` value means the product is of the default 'General' class. See also: [Product types and attributes in Ecwid](http://help.ecwid.com/customer/portal/articles/1167365-product-types-and-attributes)
enabled | boolean | `true` if product is enabled, `false` otherwise. Disabled products are not displayed in the store front.
thumbnailUrl |  string | URL of the product thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product image resized to fit 500x500. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product thumbnail resized to fit 80x80. *The original uploaded product image is available in the `originalImageUrl` field.*
description | string  | Product description *in HTML* TODO: limit
descriptionTruncated | boolean | Identifies whether the returned description has been truncated
categoryIds | Array\<number\> | List of the categories, which the product belongs to
defaultCategoryId | number  | Identifier of the default category of the product
favorites | \<FavoritesStats\>  | Product favorites stats

#### FavoritesStats
Field | Type  | Description
----- | ----- | -----------
count | number | The actual number of 'likes' of this product
displayedCount | number | The displayed number of likes. May differ from the `count` if, for example, the value is more than 1000 -- it will show 1K instead of precise number

### Errors

> Error response example

```http
HTTP/1.1 400 Wrong parameter 'inStock' value: expected one of 'true', 'false', 'yes', 'no', 'on', 'off', '1', '0
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
500 | Cannot get the product because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Get a product

### Request

> Request example

```http
GET /api/v3/4870020/products/123123?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/{productId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
<aside class="notice">
Parameters in bold are mandatory
</aside>

### Response

> Response example (JSON)

```json
{
    "id": 37208339,
    "sku": "00007",
    "thumbnailUrl": "http://app.ecwid.com/default-store/00007-230-sq.jpg",
    "quantity": 67,
    "inStock": true,
    "name": "Radish",
    "price": 1.15,
    "priceInProductList": 1.15,
    "wholesalePrices": [
        {
            "quantity": 10,
            "price": 1.05
        }
    ],
    "compareToPrice": 1.34,
    "weight": 0.31,
    "url": "http://app.ecwid.com/store/4870020#!/~/product/id=37208339",
    "created": "2009-07-23 17:22:37 +0400",
    "updated": "2014-07-30 10:32:37 +0400",
    "productClassId": 0,
    "enabled": true,
    "options": [
        {
            "type": "RADIO",
            "name": "Size",
            "choices": [
                {
                    "text": "Small",
                    "priceModifier": 0,
                    "priceModifierType": "ABSOLUTE"
                },
                {
                    "text": "Large",
                    "priceModifier": 0.5,
                    "priceModifierType": "ABSOLUTE"
                }
            ],
            "defaultChoice": 0,
            "required": false
        },
        {
            "type": "SELECT",
            "name": "Color",
            "choices": [
                {
                    "text": "Red",
                    "priceModifier": 0,
                    "priceModifierType": "ABSOLUTE"
                },
                {
                    "text": "White",
                    "priceModifier": 0,
                    "priceModifierType": "ABSOLUTE"
                }
            ],
            "defaultChoice": 0,
            "required": false
        }
    ],
    "warningLimit": 0,
    "fixedShippingRateOnly": false,
    "fixedShippingRate": 0,
    "defaultCombinationId": 7084076,
    "imageUrl": "http://app.ecwid.com/default-store/00007-sq.jpg",
    "smallThumbnailUrl": "http://app.ecwid.com/default-store/00007-80-sq.jpg",
    "description": "<h5>Radish</h5>\n<p>The radish (Raphanus sativus) is an edible root vegetable of the Brassicaceae family that was domesticated in Europe in pre-Roman times. They are grown and consumed throughout the world. Radishes have numerous varieties, varying in size, color and duration of required cultivation time. There are some radishes that are grown for their seeds; oilseed radishes are grown, as the name implies, for oil production.</p>\n<p> </p>\n<div style=\"padding: 24px 24px 24px 21px; display: block; background-color: #ececec;\">From <a style=\"color: #1e7ec8; text-decoration: underline;\" title=\"Wikipedia\" href=\"http://en.wikipedia.org\">Wikipedia</a>, the free encyclopedia</div>",
    "galleryImages": [
        {
            "id": 18276483,
            "alt": "Radish with friends",
            "url": "http://images-cdn.ecwid.com/images/4870020/237132118.jpg",
            "thumbnail": "http://images-cdn.ecwid.com/images/4870020/237132119.jpg",
            "width": 220,
            "height": 293
        }
    ],
    "categoryIds": [
        9691095
    ],
    "defaultCategoryId": 9691095,
    "attributes": [
        {
            "id": 5029057,
            "name": "Brand",
            "value": "SuperVegetables"
        }
    ],
    "files": [
        {
            "id": 7215101,
            "name": "pic_200_200.jpg",
            "description": "",
            "size": 54492,
            "adminUrl": "https://app.ecwid.com/api/v3/4870020/products/37208340/files/7215101?token=abcd123456"
        },
        {
            "id": 7215102,
            "name": "14293004.zip",
            "description": "Files archive",
            "size": 18955,
            "adminUrl": "https://app.ecwid.com/api/v3/4870020/products/37208340/files/7215102?token=abcd1234"
        }
    ],
    "relatedProducts": {
        "productIds": [
            37208340
        ],
        "relatedCategory": {
            "enabled": true,
            "categoryId": 9691095,
            "productCount": 1
        }
    },
    "combinations": [
        {
            "id": 7084071,
            "combinationNumber": 6,
            "options": [
                {
                    "name": "Color",
                    "value": "White"
                },
                {
                    "name": "Size",
                    "value": "Large"
                }
            ],
            "sku": "000076",
            "quantity": 1,
            "unlimited": false,
            "weight": 0.41,
            "warningLimit": 1
        },
        {
            "id": 7084072,
            "combinationNumber": 5,
            "options": [
                {
                    "name": "Color",
                    "value": "Red"
                },
                {
                    "name": "Size",
                    "value": "Large"
                }
            ],
            "sku": "000075",
            "quantity": 0,
            "unlimited": false,
            "weight": 0.41,
            "warningLimit": 0
        },
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
            "quantity": 67,
            "unlimited": true,
            "warningLimit": 0
        },
        {
            "id": 7084076,
            "combinationNumber": 1,
            "options": [
                {
                    "name": "Size",
                    "value": "Small"
                },
                {
                    "name": "Color",
                    "value": "Red"
                }
            ],
            "sku": "000071",
            "quantity": 61,
            "unlimited": false,
            "warningLimit": 0
        }
    ]
}
```


A JSON object of type 'Product' with the following fields:

#### Product
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique integer product identifier
sku | string |  Product SKU. Items with options can have several SKUs specified in the product combinations.
quantity |  number | Amount of product items in stock. *This field is omitted for the products with unlimited stock*
unlimited | boolean | `true` if the product has unlimited stock
inStock | boolean | `true` if the product or any of its combinations is in stock (quantity is more than zero) or has unlimited quantity. `false` otherwise.
name |  string |  Product title
price | number |  Base product price
priceInProductList | number |  Product price displayed in the product list. May differ from the *price* value when the product has combinations and the default combination's price is different from the base product price
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of wholesale price tiers (quantity limit and price pairs)
compareToPrice |  number | Product's sale price displayed strike-out in the customer frontend
weight |  number | Product weight in the units defined in store settings. *Omitted for intangible products*
url | string |  URL of the product's details page in the store
created | string | Date and time of the product creation. Example: `2014-07-30 10:32:37 +0400`
updated |  string | Product last update date/time
productClassId |  number | Id of the class (type) that this product belongs to. `0` value means the product is of the default 'General' class. See also: [Product types and attributes in Ecwid](http://help.ecwid.com/customer/portal/articles/1167365-product-types-and-attributes)
enabled | boolean | `true` if product is enabled, `false` otherwise. Disabled products are not displayed in the store front.
options | Array\<*ProductOption*\> | A list of the product options. Empty (`[]`) if no options are specified for the product. 
warningLimit | number | The minimum 'warning' amount of the product items in stock, if set. When the product quantity reaches this level, the store administrator gets an email notification.
fixedShippingRateOnly | boolean | `true` if shipping cost for this product is calculated as *'Fixed rate per item'* (managed under the "Tax and Shipping" section of the product management page in Ecwid Control panel). `false` otherwise. With this option on, the `fixedShippingRate` field specifies the shipping cost of the product
fixedShippingRate | number |  When `fixedShippingRateOnly` is `true`, this field sets the product fixed shipping cost per item. When `fixedShippingRateOnly` is `false`, the value in this field is treated as an extra shipping cost the product adds to the global calculated shipping
defaultCombinationId |  number |  Identifier of the default product combination, which is defined by the default values of product options.
thumbnailUrl |  string | URL of the product thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product image resized to fit 500x500. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product thumbnail resized to fit 80x80. *The original uploaded product image is available in the `originalImageUrl` field.*
originalImageUrl |  string  | URL of the original not resized product image
description | string  | Product description *in HTML*
galleryImages | Array\<*GalleryImage*\> |  List of the product gallery images
categoryIds | Array\<number\> | List of the categories, which the product belongs to
defaultCategoryId | number  | Identifier of the default category of the product
favorites | \<FavoritesStats\>  | Product favorites stats
attributes | Array\<*AttributeValue*\> | Product attributes and their values
files | Array\<*ProductFile*\> | Downloadable files (E-goods) attached to the product
relatedProducts | \<*RelatedProducts*\>  | Related or "You may also like" products of the product
combinations | Array\<*Combination*\> | List of the product combinations

#### FavoritesStats
Field | Type  | Description
----- | ----- | -----------
count | number | The actual number of 'likes' of this product
displayedCount | number | The displayed number of likes. May differ from the `count` if, for example, the value is more than 1000 -- it will show 1K instead of precise number

#### WholesalePrice
Field | Type  | Description
----- | ----- | -----------
quantity |  number |  Number of product items on this wholesale tier
price | number |  Product price on the tier

#### ProductOption
Field | Type  | Description
----- | ----- | -----------
type |  string | One of `SELECT`, `RADIO`, `CHECKBOX`, `TEXTFIELD`, `TEXTAREA`, `DATE`, `FILES`
name |  string |  Product option name, e.g. `Color`
choices | Array\<*ProductOptionChoice*\> | All possible option selections for the types `SELECT`, `CHECKBOX` or `RADIO`. *This field is omitted for the product option with no selection (e.g. text, datepicker or upload file options)*
defaultChoice | number  | The number, starting from `0`, of the option's default selection. Only presents if the type is `SELECT`, `CHECKBOX` or `RADIO`.
required |  boolean | `true` if this option is required, `false` otherwise. Default is `false`

#### GalleryImage
Field | Type  | Description
----- | ----- | -----------
id | number | Internal gallery image ID
alt | string |  Image description, displayed in the image tag's *alt* attribute
url | string |  Image URL
thumbnail | string  | Image thumbnail URL resized to fit 46x42px box
width | number |  Image width
height |  number |  Image height

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-classes) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value

#### ProductFile
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Internal ID of the file 
name |  string |  File name
description | string |  File description defined by the store administrator
size |  number |  File size, bytes (64-bit integer)
adminUrl | string | Link to the file. Be careful: the link contains the API access token so make sure the link is not displayed as is in your application

#### RelatedProducts
Field | Type  | Description
-------------- | -------------- | --------------
productIds | Array\<number\>  | IDs of the related products
relatedCategory | *RelatedCategory*  | Describes the "N random related products from a category" option

#### RelatedCategory
Field | Type  | Description
-------------- | -------------- | --------------
enabled | boolean | `true` if the "N random related products from a category" option is enabled. `false` otherwise
categoryId |  number |  Id of the related category. Zero value means "any category", that is, random products from the whole store.
productCount |  number |  Number of random products from the given category to be shown as related

#### Combination
Field | Type  | Description
------| ----- | -----------
id |  number |  Combination ID
combinationNumber | number |  Combination # number, which is displayed in the combinations table in Control panel
options | Array\<*OptionValue*\> | Set of options that identifies this combination. An array of name-value pairs
sku | string  | Combination SKU. `null` means the combinations inherits the base product's SKU
smallThumbnailUrl | string  | URL of the combination thumbnail resized to fit 80x80 px box. `null` means the combinations inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
thumbnailUrl |  string  | URL of the combination thumbnail displayed on the product list pages if the combination is default one. Thumbnails size is defined in the store settings and the same as the product thumbnail size. `null` means the combinations inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the combination image resized to fit 500x500. `null` means the combinations inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
originalImageUrl |  string  | URL of the original not resized combination image. `null` means the combinations inherits the base product's image.
quantity |  number  | Amount of the combination items in stock. `null` means the combinations inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number  | Combination price. `null` means the combinations inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). `null` means the combinations inherits the base product's tiered price settings. 
weight |  number  |  Combination weight in the units defined in store settings. `null` means the combinations inherits the base product's weight.
warningLimit | number | The minimum 'warning' amount of the product items in stock for this combination, if set. When the combination in stock amount reaches this level, the store administrator gets an email notification. `null` means the combinations inherits the base product's settings.


#### OptionValue
Field | Type  | Description
-------------- | -------------- | --------------
name |  string |  Option name
value | string |  Option value

#### ProductOptionChoice
Field | Type  | Description
-------------- | -------------- | --------------
text |  string | Option selection text, e.g. 'Green'.
priceModifier | number | Percent or absolute value of the option's price markup. Positive, negative and zero values are allowed. Default is `0`
priceModifierType | string | Option markup calculation type. `PERCENT` or `ABSOLUTE`. Default is `ABSOLUTE`.

### Errors

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
404 | Product is not found
500 | Cannot get the product because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Add a product

### Request

> Request body

```http
POST /api/v3/4870020/products?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

{
  "sku": "000012199",
  "quantity": 10,
  "name": "New Product",
  "price": 20.99,
  "compareToPrice": 24.99,
  "categoryIds": [
    9691094
  ],
  "weight": 10,
  "enabled": true,
  "description": "A <b>new</b> product description",
  "productClassId": 0,
  "created":"2014-01-01",
  "fixedShippingRateOnly": false,
  "fixedShippingRate": 1.2
}
```


`POST https://app.ecwid.com/api/v3/{storeId}/products?token={token}`


Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token


### Request body

A JSON object of type 'Product' with the following fields:

#### Product
Field | Type |  Description
------| ---- | ------------
**sku** | string |  Product SKU
**name** |  string |  Product title
quantity |  number | Amount of product items in stock. Leave empty for the products with unlimited stock
price | number |  Base product price
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of wholesale price tiers (quantity limit and price pairs)
compareToPrice |  number | Product's sale price displayed strike-out in the customer frontend
weight |  number | Product weight in the units defined in store settings. *Leave empty for intangible products*
productClassId |  number | Id of the class (type) that this product belongs to. `0` value means the product is of the default 'General' class. See also: [Product types and attributes in Ecwid](http://help.ecwid.com/customer/portal/articles/1167365-product-types-and-attributes)
enabled | boolean | `true` to make product enabled, `false` otherwise. Disabled products are not displayed in the store front.
options | Array\<*ProductOption*\> | List of the product options. 
warningLimit | number | The minimum 'warning' amount of the product items in stock, if set. When the product quantity reaches this level, the store administrator gets an email notification.
fixedShippingRateOnly | boolean | `true` if shipping cost for this product is calculated as *'Fixed rate per item'* (managed under the "Tax and Shipping" section of the product management page in Ecwid Control panel). `false` otherwise. With this option on, the `fixedShippingRate` field specifies the shipping cost of the product
fixedShippingRate | number |  When `fixedShippingRateOnly` is `true`, this field sets the product fixed shipping cost per item. When `fixedShippingRateOnly` is `false`, the value in this field is treated as an extra shipping cost the product adds to the global calculated shipping
description | string  | Product description *in HTML*
categoryIds | Array\<number\> | List of the categories, which the product belongs to
defaultCategoryId | number  | Identifier of the default category of the product
attributes | Array\<*AttributeValue*\> | Product attributes and their values
relatedProducts | \<*RelatedProducts*\>  | Related or "You may also like" products of the product


#### WholesalePrice
Field | Type  | Description
-------------- | -------------- | --------------
**quantity** |  number |  Number of product items on this wholesale tier
**price** | number |  Product price on the tier


#### ProductOption
Field | Type  | Description
-------------- | -------------- | --------------
type |  string | One of `SELECT`, `RADIO`, `CHECKBOX`, `TEXTFIELD`, `TEXTAREA`, `DATE`, `FILES`
**name** |  string |  Product option name, e.g. `Color`
choices | Array\<*ProductOptionChoice*\> | All possible option selections for the types `SELECT`, `CHECKBOX` or `RADIO`. *Omit this field for product options with no selection (e.g. text, datepicker or upload file options)*
defaultChoice | number  | The number, starting from `0`, of the option's default selection for the options types `SELECT`, `CHECKBOX` or `RADIO`.
required |  boolean | `true` if this option is required, `false` otherwise. Default is `false`


#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
**id** |  number |  Unique attribute ID. See [Product Classes](#product-classes) for the information on attribute IDs. <br /> **Note**: to set values of the general attributes UPC and BRAND, it's possible to specify `UPC` and `BRAND` correspondingly in this field. This allows you to add/update the values for these attributes even without numeric IDs.
value | string  | Attribute value

#### RelatedProducts
Field | Type  | Description
-------------- | -------------- | --------------
**productIds** | Array\<number\>  | IDs of the related products
relatedCategory | *RelatedCategory*  | Describes the "N random related products from a category" option

#### RelatedCategory
Field | Type  | Description
-------------- | -------------- | --------------
enabled | boolean | `true` if the "N random related products from a category" option is enabled. `false` otherwise
**categoryId** |  number |  Id of the related category. Zero value means "any category", that is, random products from the whole store.
productCount |  number |  Number of random products from the given category to be shown as related

#### OptionValue
Field | Type |  Description
-------------- | -------------- | --------------
**name** |  string |  Option name, as in Product.options[i].name
value | string |  Option value one of Product.options[i].choices[j].text

#### ProductOptionChoice
Field | Type  | Description
-------------- | -------------- | --------------
**text** |  string | Option selection text, e.g. 'Green'.
priceModifier | number | Percent or absolute value of the option's price markup. Positive, negative and zero values are allowed. Default is `0`
priceModifierType | string | Option markup calculation type. `PERCENT` or `ABSOLUTE`. Default is `ABSOLUTE`.

<aside class="notice">
Parameters in bold are mandatory
</aside>


### Response


> Response example

```json
{
    "id": 39766764,
    "message": "Successfully created",
    "success": true
}
```

A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type |  Description
-------------- | -------------- | --------------
id | number | ID of the created product
message | string | Status message 
success | boolean | `true` if the product has been created, `false` otherwise


### Errors

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
409 | The product with such SKU already exists
402 | The functionality/method is not available on the merchant plan
402 | The merchant plan product limit is reached
404 | Some of the linked entities in the request doesn't exist. For example, the product class is not found

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Update a product

> Request example

```http
PUT /api/v3/4870020/products/39766764?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
  "compareToPrice": 24.99,
  "categoryIds": [
    9691094
  ]
}
```

`PUT https://app.ecwid.com/api/v3/{storeId}/products/{productId}?token={token}`


Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token


### Request body

A JSON object of type 'Product' with the following fields:

#### Product
Field | Type |  Description
------| ---- | ------------
sku | string |  Product SKU
name |  string |  Product title
quantity |  number | Amount of product items in stock. Leave empty for the products with unlimited stock
price | number |  Base product price
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of wholesale price tiers (quantity limit and price pairs)
compareToPrice |  number | Product's sale price displayed strike-out in the customer frontend
weight |  number | Product weight in the units defined in store settings. *Leave empty for intangible products*
productClassId |  number | Id of the class (type) that this product belongs to. `0` value means the product is of the default 'General' class. See also: [Product types and attributes in Ecwid](http://help.ecwid.com/customer/portal/articles/1167365-product-types-and-attributes)
enabled | boolean | `true` to make product enabled, `false` otherwise. Disabled products are not displayed in the store front.
options | Array\<*ProductOption*\> | List of the product options. 
warningLimit | number | The minimum 'warning' amount of the product items in stock, if set. When the product quantity reaches this level, the store administrator gets an email notification.
fixedShippingRateOnly | boolean | `true` if shipping cost for this product is calculated as *'Fixed rate per item'* (managed under the "Tax and Shipping" section of the product management page in Ecwid Control panel). `false` otherwise. With this option on, the `fixedShippingRate` field specifies the shipping cost of the product
fixedShippingRate | number |  When `fixedShippingRateOnly` is `true`, this field sets the product fixed shipping cost per item. When `fixedShippingRateOnly` is `false`, the value in this field is treated as an extra shipping cost the product adds to the global calculated shipping
description | string  | Product description *in HTML*
categoryIds | Array\<number\> | List of the categories, which the product belongs to
defaultCategoryId | number  | Identifier of the default category of the product
attributes | Array\<*AttributeValue*\> | Product attributes and their values
relatedProducts | \<*RelatedProducts*\>  | Related or "You may also like" products of the product

<aside class="notice">
All fields are optional
</aside>


#### WholesalePrice
Field | Type  | Description
-------------- | -------------- | --------------
**quantity** |  number |  Number of product items on this wholesale tier
**price** | number |  Product price on the tier


#### ProductOption
Field | Type  | Description
-------------- | -------------- | --------------
type |  string | One of `SELECT`, `RADIO`, `CHECKBOX`, `TEXTFIELD`, `TEXTAREA`, `DATE`, `FILES`
**name** |  string |  Product option name, e.g. `Color`
choices | Array\<*ProductOptionChoice*\> | All possible option selections for the types `SELECT`, `CHECKBOX` or `RADIO`. *Omit this field for product options with no selection (e.g. text, datepicker or upload file options)*
defaultChoice | number  | The number, starting from `0`, of the option's default selection for the options types `SELECT`, `CHECKBOX` or `RADIO`.
required |  boolean | `true` if this option is required, `false` otherwise. Default is `false`


#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
**id** |  number |  Unique attribute ID. See [Product Classes](#product-classes) for the information on attribute IDs. <br /> **Note**: to set values of the general product attributes UPC and BRAND, it's possible to specify `UPC` and `BRAND` correspondingly in this field. This allows you to add/update the values for these attributes even without numeric IDs.
value | string  | Attribute value

#### RelatedProducts
Field | Type  | Description
-------------- | -------------- | --------------
**productIds** | Array\<number\>  | IDs of the related products
relatedCategory | *RelatedCategory* | Describes the "N random related products from a category" option


#### RelatedCategory
Field | Type  | Description
-------------- | -------------- | --------------
enabled | boolean | `true` if the "N random related products from a category" option is enabled. `false` otherwise
**categoryId** |  number |  Id of the related category. Zero value means "any category", that is, random products from the whole store.
productCount |  number |  Number of random products from the given category to be shown as related


#### OptionValue
Field | Type |  Description
-------------- | -------------- | --------------
**name** |  string |  Option name, as in Product.options[i].name
value | string |  Option value one of Product.options[i].choices[j].text

#### ProductOptionChoice
Field | Type  | Description
-------------- | -------------- | --------------
**text** |  string | Option selection text, e.g. 'Green'.
priceModifier | number | Percent or absolute value of the option's price markup. Positive, negative and zero values are allowed. Default is `0`
priceModifierType | string | Option markup calculation type. `PERCENT` or `ABSOLUTE`. Default is `ABSOLUTE`.


### Response

> Response example (JSON)

```json
{
  "message": "Product was successfully updated",
  "updateCount": 1,
  "success": true
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus

Field | Type |  Description
-------------- | -------------- | --------------
message | string | Status message 
updateCount | number | The number of updated products (`1` or `0` depending on whether the update was successful)
success | boolean | `true` if the product has been updated, `false` otherwise

### Errors

```http
HTTP/1.1 400 Bad Request
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
-------------- | --------------
400 | Request parameters are malformed
409 | The product with such SKU already exists
402 | The functionality/method is not available on the merchant plan
402 | The merchant plan product limit is reached
404 | Some of the linked entities in the request doesn't exist. For example, the product class is not found

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Delete a product

> Request example

```http
DELETE /api/v3/4870020/products/39847403?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token

### Response

> Response example

```json
{
    "message":"",
    "deleteCount":1,
    "success":true
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | --------------
message | string | Status message 
deleteCount | number | The number of deleted products (`1` or `0` depending on whether the request was successful)
success | boolean | `true` if the product has been deleted, `false` otherwise


### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
404 | The product with given ID is not found
500 | The delete request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Upload product image

Upload product image: if the product already has an image attached, the uploaded image will replace the existing one.

> Request example

```http
POST /api/v3/4870020/products/1234657/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

binary data
```

`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/image?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token

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
404 | Product is not found
413 | The image file is too large
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Delete product image

> Request example

```http
DELETE /api/v3/4870020/products/1234657/image?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/image?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token

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
deleteCount | number | The number of deleted images (`1` or `0` depending on whether the request was successful)
success | boolean | `true` if the image has been deleted, `false` otherwise

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
500 | Deleting of the image file failed or there was an internal server error
404 | Product is not found

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message





<!--
---------------------------------------------------------------------------------------------------------
    Upload product gallery image
---------------------------------------------------------------------------------------------------------
-->

## Upload gallery image

Add image to the product images gallery. Request parameters specify which product should be updated and what title should the uploaded image have. Request body is the image file itself (binary data).

> Request example

```http
POST /api/v3/4870020/products/1234657/gallery?fileName=Extra%20Image&token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

binary data
```

`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/gallery?fileName={fileName}token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
fileName |  string |  Image title

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
--------- | ---------| -----------
id | number | Internal image file ID

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
404 | Product is not found
413 | The image file is too large
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


<!--
---------------------------------------------------------------------------------------------------------
    Delete product image
---------------------------------------------------------------------------------------------------------
-->

## Delete gallery image

> Request example

```http
DELETE /api/v3/4870020/products/1234657/gallery/1234657345/?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/gallery/{fileId}/?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
**fileId** | number | Internal image file ID

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
deleteCount | number | The number of deleted images (`1` or `0` depending on whether the request was successful)
success | boolean | `true` if the image has been deleted, `false` otherwise

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
500 | Deleting of the image file failed or there was an internal server error
404 | Product is not found
400 | Request parameters are malformed

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message






<!--
---------------------------------------------------------------------------------------------------------
    Delete all product gallery images
---------------------------------------------------------------------------------------------------------
-->

## Delete all gallery images

> Request example

```http
DELETE /api/v3/4870020/products/39847403/gallery?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

*Remove all gallery images attached to the product*

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/gallery?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token

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
deleteCount | number | The number of deleted gallery images
success | boolean | `true` if the deletion was successful, `false` otherwise

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
500 | Deleting of the files failed or there was an internal server error
404 | Product is not found
400 | Request parameters are malformed

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


<!--
---------------------------------------------------------------------------------------------------------
    Upload product file
---------------------------------------------------------------------------------------------------------
-->

## Upload product file

> Request example

```http
POST /api/v3/4870020/products/1234567/files?token=123456789abcd&fileName=photo+large.psd&description=Item+photo+in+psd+format HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

binary data
```

Uploading a product file (e-goods)


`POST https://app.ecwid.com/api/v3/{storeId}/products/{productId}/files?token={token}&fileName={fileName}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token
**fileName** | string | Name of the file that customers will see
description | string | A short description of the uploaded file


### Response

> Response example

```json
{
    "id": 6738222
}
```

A JSON object of type 'UploadStatus' with the following fields:

#### UploadStatus
Field | Type |  Description
--------- | ---------| -----------
id | number | Internal file ID

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8

{
    "errorMessage": "Internal server error"
}
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Uploading of the file failed or there was an internal server error while processing a file
404 | Product is not found
413 | The file is too large
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



<!--
---------------------------------------------------------------------------------------------------------
    Delete product egoods file
---------------------------------------------------------------------------------------------------------
-->

## Delete product file

> Request example

```http
DELETE /api/v3/4870020/products/1234657/files/193639?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

*Delete product's file (e-goods) by the file ID*

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/files/{fileId}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**fileId** | number | Internal file ID
**token** |  string |  oAuth token

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
deleteCount | number | The number of deleted files (`1` or `0` depending on whether the request was successful)
success | boolean | `true` if the file has been deleted, `false` otherwise

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
500 | Deleting of the file failed or there was an internal server error
404 | Product is not found

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



<!--
---------------------------------------------------------------------------------------------------------
    Delete all product egoods files
---------------------------------------------------------------------------------------------------------
-->

## Delete all product files

> Request example

```http
DELETE /api/v3/4870020/products/39847403/files?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

*Remove all downloadable files attached to the product (e-goods)*

`DELETE https://app.ecwid.com/api/v3/{storeId}/products/{productId}/files?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string |  oAuth token

### Response

> Response example

```json
{
    "deleteCount": 3,
    "success": true
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted files
success | boolean | `true` if the deletion was successful, `false` otherwise

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
500 | Deleting of the files failed or there was an internal server error
404 | Product is not found

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

