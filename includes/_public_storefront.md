# Public REST API

# Overview

Public REST API allows you to get product, categories, store information and other publicly visible details of a store via REST interface. 

While the private REST API can change elements in the store, like update stock levels of products and change order details, public REST API provides information in read-only format. This opens up good possibilities for storefront customizations and even creating a completely custom storefront with the help of [Storefront JavaScript API](#storefront-js-api).

Let's dive in and see what data you can retrieve from the store from anywhere you have your code running: in storefront, in a native application or in insalled on a device application.

# Get public store information

For full management of an Ecwid store via REST API, please see [this section](#store-information)

> Request example

```http
GET /api/v3/4870020/profile?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/profile?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token

### Response

> Response example (JSON)

```json
{
    "generalInfo": {
        "storeId": 4870020,
        "storeUrl": "http://www.example.com",
        "starterSite": {
        	"ecwidSubdomain": "mysuperstore",
        	"generatedUrl": "http://mysuperstore.ecwid.com/"
    	}
    },
    "account": {
        "availableFeatures": []
    },
    "settings": {
        "closed": false,
        "storeName": "My Super Store",
        "invoiceLogoUrl": "https://dpbfm6h358sh7.cloudfront.net/images/4870020/253584290.jpg",
        "emailLogoUrl": "https://dpbfm6h358sh7.cloudfront.net/images/4870020/298177033.jpg",
        "googleAnalyticsId": "UA-123456-1"
    },
    "mailNotifications": {
        "customerNotificationFromEmail": "john@example.com"
    },
    "company": {
        "companyName": "My Super Store",
        "email": "example@example.com",
        "street": "W 3d st",
        "city": "New York",
        "countryCode": "US",
        "postalCode": "10001",
        "stateOrProvinceCode": "NY",
        "phone": "234324234"
    },
    "formatsAndUnits": {
        "currency": "USD",
        "currencyPrefix": "$",
        "currencySuffix": "",
        "currencyGroupSeparator": " ",
        "currencyDecimalSeparator": ".",
        "currencyTruncateZeroFractional": false,
        "currencyPrecision": 2,
        "currencyRate": 1,
        "weightUnit": "KILOGRAM",
        "weightGroupSeparator": " ",
        "weightDecimalSeparator": ".",
        "weightTruncateZeroFractional": false,
        "timeFormat": "hh:mm a",
        "dateFormat": "MMM d, yyyy",
        "timezone": "Europe/Moscow"
    },
    "languages": {
        "enabledLanguages": [
         	"en",
      		"es",
      		"pt",
      		"ru"
        ],
        "facebookPreferredLocale": "en_US"
    }
}
```

A JSON object of type 'Profile' with the following fields:

#### Profile
Field | Type | Description
----- | ---- | -----------
generalInfo | \<GeneralInfo\> | Store basic data
settings | \<Settings\> | Store general settings
mailNotifications | \<MailNotifications\> | Mail notifications settings
company | \<Company\> | Company info
formatsAndUnits | \<FormatsAndUnits\> | Store formats/untis settings
languages | \<Languages\> | Store language settings
shipping | \<Shipping\> | Store shipping settings (only handling fee is included at the moment)
starterSite | \<StarterSiteInfo\> | Starter site information

#### GeneralInfo
Field | Type | Description
----- | ---- | -----------
storeId | number | Ecwid Store ID
storeUrl | string | Storefront URL
starterSite | \<StarterSiteInfo\> | Starter Site settings

#### Account
Field | Type | Description
----- | ---- | -----------
availableFeatures | Array\<string\> | A list of the premium features available on the store's pricing plan

#### Settings
Field | Type | Description
----- | ---- | -----------
closed | boolean | `true` if the store is closed for maintenance, `false` otherwise
storeName | string | The store name displayed in Starter Site
invoiceLogoUrl | string | Company logo displayed on the invoice
emailLogoUrl | string | Company logo displayed in the store email notifications
googleAnalyticsId | string | [Google Analytics ID](https://help.ecwid.com/customer/en/portal/articles/1170264-google-analytics) connected to a store

#### MailNotifications
Field | Type | Description
----- | ---- | -----------
customerNotificationFromEmail | string | The email address used as the 'reply-to' field in the notifications to customers

#### Company
*System Settings → General → Store Profile*

Field | Type | Description
----- | ---- | -----------
companyName | string | The company name displayed on the invoice
email | string | Company (store administrator) email
street | string | Company address. 1 or 2 lines separated by a new line character
city    | string  | Company city
countryCode | string  | A two-letter ISO code of the country
postalCode  | string  | Postal code or ZIP code
stateOrProvinceCode | string  | State code (e.g. `NY`) or a region name
phone | string  | Company phone number

#### FormatsAndUnits
*System settings → General → Formats & Units.*

Field | Type | Description
----- | ---- | -----------
currency | string | 3-letters code of the store currency (ISO 4217). Examples: `USD`, `CAD`
currencyPrefix | string |  Currency prefix (e.g. $)
currencySuffix | string | Currency suffix
currencyPrecision | number  | Numbers of digits after decimal point in the store prices. E.g. `2` ($2.99) or `0` (¥500).
currencyGroupSeparator | string | Price thousands separator. Supported values: space ` `, dot `.`, comma `,`  or empty value ``.
currencyDecimalSeparator |  string | Price decimal separator. Possible values: `.` or `,` 
currencyTruncateZeroFractional | boolean | Hide zero fractional part of the prices in storefront. `true` or `false` . 
currencyRate | number | Currency rate in U.S. dollars, as set in the merchant control panel
weightUnit |    string |    Weight unit. Supported values: `CARAT`, `GRAM`, `OUNCE`, `POUND`, `KILOGRAM`
weightPrecision | number | Numbers of digits after decimal point in weights displayed in the store
weightGroupSeparator | string | Weight thousands separator. Supported values: space ` `, dot `.`, comma `,`  or empty value ``
weightDecimalSeparator | string | Weight decimal separator. Possible values: `.` or `,` 
weightTruncateZeroFractional |  boolean | Hide zero fractional part of the weight values in storefront. `true` or `false` . 
dateFormat | string | Date format, e.g. `MMM d, yyyy`
timeFormat | string | Time format, e.g. `hh:mm a`
timezone | string | Store timezone, e.g. `Europe/Moscow`

#### Languages
*System Settings → General → Languages*

Field | Type | Description
----- | ---- | -----------
enabledLanguages | Array\<string\> | A list of enabled languages in the storefront. First language code is the default language for the store.
facebookPreferredLocale | string | Language automatically chosen be default in Facebook storefront (if any)

#### StarterSiteInfo
*System Settings → General → Starter site*

Field | Type | Description
----- | ---- | -----------
ecwidSubdomain | string | Store subdomain on ecwid.com domain, e.g. `mysuperstore.ecwid.com`
generatedUrl | string | Starter Site generated URL, e.g. `http://mysuperstore.ecwid.com/`


### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
500 | Cannot retrieve the coupon info because of an error on the server

# Public products

Get public information of **enabled** products in a store. For full management products in a store, please see [this section](#products)

## Search public products

Search/filter store products

### Request

> Request examples

```http
GET /api/v3/4870020/products?limit=2&keyword=fruit&token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products?keyword={keyword}&priceFrom={priceFrom}&priceTo={priceTo}&category={category}&withSubcategories={withSubcategories}&sortBy={sortBy}&offset={offset}&limit={limit}&createdFrom={createdFrom}&createdTo={createdTo}&updatedFrom={updatedFrom}&updatedTo={updatedTo}&inStock={inStock}&field{attributeName}={attributeValues}&field{attributeId}={attributeValues}&sku={sku}&productId={productId}&token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token
keyword |  string | Search term. Use quotes to search for exact match. Ecwid searches products over multiple fields: <ul><li>title</li><li>description</li><li>SKU</li><li>product options</li><li>category name</li><li>gallery image descriptions</li><li>attribute values (except for hidden attributes)
priceFrom |  number | Minimum product price
priceTo | number | Maximum product price
category | number | Category ID
withSubcategories |  boolean | `true`/`false`: defines whether Ecwid should search in subcategories of the category you set in `category` field. Ignored if `category` field is not set . `false` is the default value
sortBy |  string | Sort order. Supported values: <ul><li>`RELEVANCE` *default*</li> <li>`ADDED_TIME_DESC`</li> <li>`ADDED_TIME_ASC`</li> <li>`NAME_ASC`</li> <li>`NAME_DESC`</li> <li>`PRICE_ASC`</li> <li>`PRICE_DESC`</li><li>`UPDATED_TIME_ASC`</li><li>`UPDATED_TIME_DESC`</li></ul>
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `100`
createdFrom | string | Product creation date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul> Examples: <ul><li>`1447804800`</li> <li>`2015-04-22 18:48:38 -0500`</li> <li>`2015-04-22` (this is 2015-04-22 00:00:00 UTC)</li></ul>
createdTo | string | Product creation date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
updatedFrom | string | Product last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
updatedTo | string | Product last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
inStock | boolean | `true` to get only products in stock, `false` to get out of stock products
field{attributeName} | string | Filter by product attribute values. Format: field{attributeName}=param[,param], where "attributeName" is the attribute name and "param" is the attribute value. You can place several values separated by comma. In that case values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `fieldBrand=Apple&fieldCapacity=32GB,64GB` 
field{attributeId} | string | Filter by product attribute values. Works the same as the filter by `field{attributeName}` but attribute IDs are used instead of attribute names. This way is insensitive to attributes renaming.
sku | string | Product or combination SKU. Ecwid will return details of a product containing that SKU, if SKU value is an exact match. If SKU is specified, other search parameters are ignored, except for product ID.
productId | number | Internal Ecwid product ID. If product ID is specified, other search parameters are ignored. 

<aside class="notice">
If no filters are set in the URL, API will return all enabled products. This request returns <strong>only enabled products</strong>. 
</aside>

<aside class="notice">
To search for exact match, put the keyword in quotes like this: "ABC123". For example, you can use this to get a product by SKU. 
</aside>

### Response

> Response example (JSON)

```json
{
    "total": 5,
    "count": 2,
    "offset": 0,
    "limit": 100,
    "items": [
        {
          "id": 37208339,
          "sku": "00099",
          "thumbnailUrl": "http://app.ecwid.com/default-store/00011-232-sq.jpg",
          "quantity": 11,
          "unlimited": false,
          "inStock": true,
          "name": "Orange",
          "price": 10,
          "priceInProductList": 10,
          "wholesalePrices": [
            {
              "quantity": 2,
              "price": 9
            },
            {
              "quantity": 4,
              "price": 8
            }
          ],
          "compareToPrice": 23,
          "isShippingRequired": true,
          "weight": 0,
          "url": "http://app.ecwid.com#!/Orange/p/37208339",
          "created": "2015-10-05 07:26:14 +0000",
          "updated": "2016-02-03 10:01:02 +0000",
          "createTimestamp": 1444029974,
          "updateTimestamp": 1454493662,
          "productClassId": 0,
          "options": [],
          "fixedShippingRateOnly": true,
          "fixedShippingRate": 0,
          "defaultCombinationId": 0,
          "imageUrl": "http://app.ecwid.com/default-store/00007123-12-sq.jpg",
          "smallThumbnailUrl": "http://app.ecwid.com/default-store/000017-sq.jpg",
          "hdThumbnailUrl": "https://app.ecwid.com/images/1003/397690783.jpg",
          "originalImageUrl": "http://app.ecwid.com/default-store/00005-sq.jpg",
          "originalImage": {
              "url": "http://app.ecwid.com/default-store/00005-sq.jpg",
              "width": 123,
              "height": 456
          },
          "description": "<p>It's a tasty fruit!</p>",
          "galleryImages": [
            {
              "id": 18481471,
              "alt": "AdditionalImage",
              "url": "https://dpbfm6h358sh7.cloudfront.net/images/5035009/312058848.jpg",
              "thumbnail": "https://dpbfm6h358sh7.cloudfront.net/images/5035009/351433814.jpg",
              "width": 473,
              "height": 545,
              "orderBy": 0
            },
            {
              "id": 18481472,
              "alt": "AdditionalImage",
              "url": "https://dpbfm6h358sh7.cloudfront.net/images/5035009/312058850.jpg",
              "thumbnail": "https://dpbfm6h358sh7.cloudfront.net/images/5035009/351433815.jpg",
              "width": 247,
              "height": 545,
              "orderBy": 1
            }
          ],
          "categoryIds": [],
          "seoTitle": "Orange",
          "seoDescription": "It's a tasty orange for you and your family!",
          "defaultCategoryId": 0,
          "favorites": {
            "count": 0,
            "displayedCount": "0"
          },
          "attributes": [
            {
              "id": 8258226,
              "name": "Width",
              "value": "61.47 mm",
              "show": "DESCR"
            },
            {
              "id": 8258231,
              "name": "Height",
              "value": "117.09 mm",
              "show": "DESCR"
            },
            {
              "id": 8258249,
              "name": "Depth",
              "value": "15.49 mm",
              "show": "DESCR"
            },
            {
              "id": 8258255,
              "name": "Net weight",
              "value": "153.2 g",
              "show": "DESCR"
            }
          ],
          "relatedProducts": {
            "productIds": [],
            "relatedCategory": {
              "enabled": false,
              "categoryId": 0,
              "productCount": 1
            }
          },
          "combinations": []
        },
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
            "isShippingRequired": true,
            "weight": 0.31,
            "url": "http://app.ecwid.com/store/4870020#!/~/product/id=37208339",
            "created": "2009-07-23 17:22:37 +0000",
            "updated": "2014-07-30 10:32:37 +0000",
            "createTimestamp": 1248340746,
            "updateTimestamp": 1428313104,
            "productClassId": 0,
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
            "fixedShippingRateOnly": false,
            "fixedShippingRate": 0,
            "defaultCombinationId": 7084076,
            "hdThumbnailUrl": "https://app.ecwid.com/images/1003/397690841.jpg",
            "originalImageUrl": "http://app.ecwid.com/default-store/00007-sq.jpg",
            "originalImage": {
              "url": "http://app.ecwid.com/default-store/00007-sq.jpg",
              "width": 123,
              "height": 456
            },
            "smallThumbnailUrl": "http://app.ecwid.com/default-store/00007-80-sq.jpg",
            "description": "<h5>Radish</h5>\n<p>The radish (Raphanus sativus) is an edible root vegetable of the Brassicaceae family that was domesticated in Europe in pre-Roman times. They are grown and consumed throughout the world. Radishes have numerous varieties, varying in size, color and duration of required cultivation time. There are some radishes that are grown for their seeds; oilseed radishes are grown, as the name implies, for oil production.</p>\n<p> </p>\n<div style=\"padding: 24px 24px 24px 21px; display: block; background-color: #ececec;\">From <a style=\"color: #1e7ec8; text-decoration: underline;\" title=\"Wikipedia\" href=\"http://en.wikipedia.org\">Wikipedia</a>, the free encyclopedia</div>",
            "galleryImages": [
                {
                    "id": 18276483,
                    "alt": "Radish with friends",
                    "url": "http://images-cdn.ecwid.com/images/4870020/237132118.jpg",
                    "thumbnail": "http://images-cdn.ecwid.com/images/4870020/237132119.jpg",
                    "width": 220,
                    "height": 293,
                    "orderby": 10
                }
            ],
            "categoryIds": [
                9691095
            ],
            "seoTitle": "Radish",
            "seoDescription": "It's an awesome radish just for you!",            
            "defaultCategoryId": 9691095,
            "attributes": [
                {
                    "id": 5029057,
                    "name": "Brand",
                    "value": "SuperVegetables",
                    "show": "DESCR"
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
                    "smallThumbnailUrl": "https://app.ecwid.com/images/1003/397690841.jpg",
                    "hdThumbnailUrl": "https://app.ecwid.com/images/1003/397690842.jpg",
                    "thumbnailUrl": "https://app.ecwid.com/images/1003/397690811.jpg",
                    "imageUrl": "https://app.ecwid.com/images/1003/397690844.jpg",
                    "originalImageUrl": "https://app.ecwid.com/images/1003/397690845jpg",
                    "unlimited": false,
                }
            ]
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
limit | number | Maximum possible number of returned items in this request. 
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
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of wholesale price tiers (quantity limit and price pairs)
compareToPrice |  number | Product's sale price displayed strike-out in the customer frontend *Omitted if empty*
isShippingRequired | boolean | `true` if product requires shipping, `false` otherwise
weight |  number | Product weight in the units defined in store settings. *Omitted for intangible products*
url | string |  URL of the product's details page in the store
created | string | Date and time of the product creation. Example: `2014-07-30 10:32:37 +0000`
updated |  string | Product last update date/time
createTimestamp | number | The date of product creation in UNIX Timestamp format, e.g `1427268654`
updateTimestamp | number | Product last update date in UNIX Timestamp format, e.g `1427268654`
productClassId |  number | Id of the class (type) that this product belongs to. `0` value means the product is of the default 'General' class. See also: [Product types and attributes in Ecwid](http://help.ecwid.com/customer/portal/articles/1167365-product-types-and-attributes)
options | Array\<*ProductOption*\> | A list of the product options. Empty (`[]`) if no options are specified for the product. 
fixedShippingRateOnly | boolean | `true` if shipping cost for this product is calculated as *'Fixed rate per item'* (managed under the "Tax and Shipping" section of the product management page in Ecwid Control panel). `false` otherwise. With this option on, the `fixedShippingRate` field specifies the shipping cost of the product
fixedShippingRate | number |  When `fixedShippingRateOnly` is `true`, this field sets the product fixed shipping cost per item. When `fixedShippingRateOnly` is `false`, the value in this field is treated as an extra shipping cost the product adds to the global calculated shipping
defaultCombinationId |  number |  Identifier of the default product combination, which is defined by the default values of product options.
thumbnailUrl |  string | URL of the product thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product image resized to fit 500x500. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product thumbnail resized to fit 80x80. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product HD thumbnail URL resized to fit 650x650px
originalImageUrl |  string  | URL of the original not resized product image
originalImage | \<ImageDetails\> | Details of the product image
description | string  | Product description *in HTML*
galleryImages | Array\<*GalleryImage*\> |  List of the product gallery images
categoryIds | Array\<number\> | List of the categories, which the product belongs to
seoTitle | string | Page title to be displayed in search results on the web. Recommended length is under 55 characters
seoDescription | string | Page description to be displayed in search results on the web. Recommended length is under 160 characters
defaultCategoryId | number  | Identifier of the default category of the product
favorites | \<FavoritesStats\>  | Product favorites stats
attributes | Array\<*AttributeValue*\> | Product attributes and their values
relatedProducts | \<*RelatedProducts*\>  | Related or "You may also like" products of the product
combinations | Array\<*Combination*\> | List of the product combinations

#### FavoritesStats
Field | Type  | Description
----- | ----- | -----------
count | number | The actual number of 'likes' of this product
displayedCount | string | The displayed number of likes. May differ from the `count` if, for example, the value is more than 1000, than it will show 1K instead of the precise number

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

#### ImageDetails 
Field | Type  | Description
----- | ----- | -----------
url | string | Image URL
width | integer | Image width
height | integer | Image height

#### GalleryImage
Field | Type  | Description
----- | ----- | -----------
id | number | Internal gallery image ID
alt | string |  Image description, displayed in the image tag's *alt* attribute
url | string |  Image URL
thumbnail | string  | Image thumbnail URL resized to fit 46x42px box
width | number |  Image width
height |  number |  Image height
orderby |  number |  The sort weight of the image in the gallery images list. The less the number, the closer the image to the beginning of the gallery

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-types) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value
show | string | Defines where to display the product attribute value:. Supported values: `DESCR`, `PRICE` .

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
sku | string  | Combination SKU. Omitted if the combination inherits the base product's SKU
smallThumbnailUrl | string  | URL of the combination thumbnail resized to fit 80x80 px box. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Combination HD thumbnail URL resized to fit 650x650px
thumbnailUrl |  string  | URL of the combination thumbnail displayed on the product list pages if the combination is default one. Thumbnails size is defined in the store settings and the same as the product thumbnail size. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the combination image resized to fit 500x500. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
originalImageUrl | string | URL of the original not resized combination image. Omitted if the combination inherits the base product's image.
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.

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


## Get a public product

### Request

> Request example

```http
GET /api/v3/4870020/products/123123?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/{productId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**productId** | number | Product ID
**token** |  string | Public oAuth token

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
    "isShippingRequired": true,
    "weight": 0.31,
    "url": "http://app.ecwid.com/store/4870020#!/~/product/id=37208339",
    "created": "2009-07-23 17:22:37 +0000",
    "updated": "2014-07-30 10:32:37 +0000",
    "createTimestamp": 1248340746,
    "updateTimestamp": 1428313104,
    "productClassId": 0,
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
    "fixedShippingRateOnly": false,
    "fixedShippingRate": 0,
    "defaultCombinationId": 7084076,
    "hdThumbnailUrl": "https://app.ecwid.com/images/1003/397690824.jpg",
    "originalImageUrl": "http://app.ecwid.com/default-store/00007-sq.jpg",
    "originalImage": {
      "url": "http://app.ecwid.com/default-store/00007-sq.jpg",
      "width": 123,
      "height": 456
    },
    "smallThumbnailUrl": "http://app.ecwid.com/default-store/00007-80-sq.jpg",
    "description": "<h5>Radish</h5>\n<p>The radish (Raphanus sativus) is an edible root vegetable of the Brassicaceae family that was domesticated in Europe in pre-Roman times. They are grown and consumed throughout the world. Radishes have numerous varieties, varying in size, color and duration of required cultivation time. There are some radishes that are grown for their seeds; oilseed radishes are grown, as the name implies, for oil production.</p>\n<p> </p>\n<div style=\"padding: 24px 24px 24px 21px; display: block; background-color: #ececec;\">From <a style=\"color: #1e7ec8; text-decoration: underline;\" title=\"Wikipedia\" href=\"http://en.wikipedia.org\">Wikipedia</a>, the free encyclopedia</div>",
    "galleryImages": [
        {
            "id": 18276483,
            "alt": "Radish with friends",
            "url": "http://images-cdn.ecwid.com/images/4870020/237132118.jpg",
            "thumbnail": "http://images-cdn.ecwid.com/images/4870020/237132119.jpg",
            "width": 220,
            "height": 293,
            "orderby": 10
        }
    ],
    "categoryIds": [
        9691095
    ],
    "seoTitle": "Radish",
    "seoDescription": "It's an awesome radish just for you!",     
    "defaultCategoryId": 9691095,
    "attributes": [
        {
            "id": 5029057,
            "name": "Brand",
            "value": "SuperVegetables",
            "show": "DESCR"
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
compareToPrice |  number | Product's sale price displayed strike-out in the customer frontend *Omitted if empty*
isShippingRequired | boolean | `true` if product requires shipping, `false` otherwise
weight |  number | Product weight in the units defined in store settings. *Omitted for intangible products*
url | string |  URL of the product's details page in the store
created | string | Date and time of the product creation. Example: `2014-07-30 10:32:37 +0000`
updated |  string | Product last update date/time
createTimestamp | number | The date of product creation in UNIX Timestamp format, e.g `1427268654`
updateTimestamp | number | Product last update date in UNIX Timestamp format, e.g `1427268654`
productClassId |  number | Id of the class (type) that this product belongs to. `0` value means the product is of the default 'General' class. See also: [Product types and attributes in Ecwid](http://help.ecwid.com/customer/portal/articles/1167365-product-types-and-attributes)
options | Array\<*ProductOption*\> | A list of the product options. Empty (`[]`) if no options are specified for the product. 
fixedShippingRateOnly | boolean | `true` if shipping cost for this product is calculated as *'Fixed rate per item'* (managed under the "Tax and Shipping" section of the product management page in Ecwid Control panel). `false` otherwise. With this option on, the `fixedShippingRate` field specifies the shipping cost of the product
fixedShippingRate | number |  When `fixedShippingRateOnly` is `true`, this field sets the product fixed shipping cost per item. When `fixedShippingRateOnly` is `false`, the value in this field is treated as an extra shipping cost the product adds to the global calculated shipping
defaultCombinationId |  number |  Identifier of the default product combination, which is defined by the default values of product options.
thumbnailUrl |  string | URL of the product thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product image resized to fit 500x500. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product thumbnail resized to fit 80x80. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product HD thumbnail URL resized to fit 650x650px
originalImageUrl |  string  | URL of the original not resized product image
originalImage | \<ImageDetails\> | Details of the product image
description | string  | Product description *in HTML*
galleryImages | Array\<*GalleryImage*\> |  List of the product gallery images
categoryIds | Array\<number\> | List of the categories, which the product belongs to
seoTitle | string | Page title to be displayed in search results on the web. Recommended length is under 55 characters
seoDescription | string | Page description to be displayed in search results on the web. Recommended length is under 160 characters
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
displayedCount | string | The displayed number of likes. May differ from the `count` if, for example, the value is more than 1000, than it will show 1K instead of the precise number

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

#### ImageDetails 
Field | Type  | Description
----- | ----- | -----------
url | string | Image URL
width | integer | Image width
height | integer | Image height

#### GalleryImage
Field | Type  | Description
----- | ----- | -----------
id | number | Internal gallery image ID
alt | string |  Image description, displayed in the image tag's *alt* attribute
url | string |  Image URL
thumbnail | string  | Image thumbnail URL resized to fit 46x42px box
width | number |  Image width
height |  number |  Image height
orderby |  number |  The sort weight of the image in the gallery images list. The less the number, the closer the image to the beginning of the gallery

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-types) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value
show | string | Defines where to display the product attribute value:. Supported values: `NOTSHOW`, `DESCR`, `PRICE` . 

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
sku | string  | Combination SKU. Omitted if the combination inherits the base product's SKU
smallThumbnailUrl | string  | URL of the combination thumbnail resized to fit 80x80 px box. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
thumbnailUrl |  string  | URL of the combination thumbnail displayed on the product list pages if the combination is default one. Thumbnails size is defined in the store settings and the same as the product thumbnail size. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Combination HD thumbnail URL resized to fit 650x650px
imageUrl |  string  | URL of the combination image resized to fit 500x500. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
originalImageUrl | string | URL of the original not resized combination image. Omitted if the combination inherits the base product's image.
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.

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
404 | Product is not found or is disabled in a store
500 | Cannot get the product because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

# Public categories

Get public information of **enabled** categories in a store. For full management of categories in a store, please see [this section](#categories)

## Get categories

### Request

> Request example

```http
GET /api/v3/4870020/categories?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
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
productIds | boolean | Set to `true` if you want the results to contain a list of product IDs assigned to category. `false` is default

<aside class="note">
Ecwid will return only <strong>enabled categories</strong> in a store.
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
            "thumbnailUrl": "https://app.ecwid.com/default-store/fruit-230-sq.jpg",
            "originalImageUrl": "https://app.ecwid.com/default-store/fruit-230-sq.jpg",
            "originalImage": {
                "url": "https://app.ecwid.com/default-store/fruit-230-sq.jpg",
                "width": 123,
                "height": 456
            },            
            "name": "Fruit",
            "url": "http://app.ecwid.com/store/4870020#!/Fruit/c/9691094",
            "productCount": 6,
            "description": "",
        },
        {
            "id": 9691095,
            "orderBy": 20,
            "hdThumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/397690772.jpg",
            "thumbnailUrl": "https://app.ecwid.com/default-store/vegetables-230-sq.jpg",
            "originalImageUrl": "https://app.ecwid.com/default-store/vegetables-230-sq.jpg",
            "originalImage": {
                "url": "https://app.ecwid.com/default-store/vegetables-230-sq.jpg",
                "width": 123,
                "height": 456
            },
            "name": "Vegetables",
            "url": "http://app.ecwid.com/store/4870020#!/Vegetables/c/9691095",
            "productCount": 4,
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
hdThumbnailUrl | string  | Category HD thumbnail URL resized to fit 650x650px
thumbnailUrl | string  | Category thumbnail URL. The thumbnail size is specified in the store settings
originalImageUrl | string  | Link to the original (not resized) category image
originalImage | \<ImageDetails\> | Details of the category image
name | string | Category name
url | string | Category page URL in the store
productCount | number | Number of products in the category and its subcategories
description | string  | The category description in HTML
productIds | Array\<number\>  | IDs of the products assigned to the category

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

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
500 | Cannot retrieve the categories info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Get public category

Get public information of **enabled** category in a store.

> Request example

```http
GET /api/v3/4870020/categories/10861116?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
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

<aside class="note">
Ecwid will return details of <strong>enabled categories</strong> only.
</aside>

### Response

> Response example

```json
{
    "id": 10861116,
    "parentId": 9691094,
    "orderBy": 20,
    "hdThumbnailUrl": "https://dpbfm6h358sh7.cloudfront.net/images/1003/397690771.jpg",
    "thumbnailUrl": "http://images-cdn.ecwid.com/images/4870020/244778352.jpg",
    "originalImageUrl": "http://images-cdn.ecwid.com/images/4870020/244778351.jpg",
    "originalImage": {
        "url": "http://images-cdn.ecwid.com/images/4870020/244778351.jpg",
        "width": 123,
        "height": 456
    },
    "name": "Subfruit2",
    "url": "http://app.ecwid.com/store/4870020#!/Subfruit2/c/10861116",
    "productCount": 0,
    "description": "<p>arf34</p>",
}
```

A JSON object of type 'Category' with the following fields:

#### Category

Field | Type | Description
----- | ---- | -----------
id | number | Internal category ID
parentId | number  | ID of the parent category, if any
orderBy | number | Sort order of the category in the parent category subcategories list
hdThumbnailUrl | string  | Category HD thumbnail URL resized to fit 650x650px
thumbnailUrl | string  | Category thumbnail URL. The thumbnail size is specified in the store settings
originalImageUrl | string  | Link to the original (not resized) category image
originalImage | \<ImageDetails\> | Details of the category image
name | string | Category name
url | string | Category page URL in the store
productCount | number | Number of products in the category and its subcategories
description | string  | The category description in HTML
productIds | Array\<number\>  | IDs of the products assigned to the category

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

HTTP Status | Meaning
------------|--------
400 | Malformed request parameters
404 | Category is not found or is disabled in a store
500 | Server error

# Public product combinations

Get public combinations details of **enabled** products in a store. For full management of combinations in a store, please see [this section](#product-combinations)

## Get all public product combinations

> Request example

```http
GET /api/v3/4870020/products/8383892837/combinations?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token
**productId** | number | Internal product ID

<aside class="note">
Ecwid will return combinations of <strong>enabled products</strong> only.
</aside>

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
hdThumbnailUrl | string  | Combination HD thumbnail URL resized to fit 650x650px
thumbnailUrl |  string  | URL of the combination thumbnail displayed on the product list pages if the combination is default one. Thumbnails size is defined in the store settings and the same as the product thumbnail size. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the combination image resized to fit 500x500. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
originalImageUrl | string | URL of the original not resized combination image. Omitted if the combination inherits the base product's image.
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.

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


## Get a public product combination

Get public combination details of **enabled** products in a store.

> Request example

```http
GET /api/v3/4870020/products/8383892837/combinations/772828388?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token
**productId** | number | Internal product ID
**combinationId** | number | Internal combination ID

<aside class="note">
Ecwid will return combinations of <strong>enabled products</strong> only.
</aside>

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
hdThumbnailUrl | string  | Combination HD thumbnail URL resized to fit 650x650px
thumbnailUrl |  string  | URL of the combination thumbnail displayed on the product list pages if the combination is default one. Thumbnails size is defined in the store settings and the same as the product thumbnail size. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the combination image resized to fit 500x500. Omitted if the combination inherits the base product's image. *The original uploaded combination image is available in the `originalImageUrl` field.*
originalImageUrl | string | URL of the original not resized combination image. Omitted if the combination inherits the base product's image.
quantity | number | Amount of the combination items in stock. Omitted if the combination inherits the base product's quantity.
unlimited | boolean | `true` if the combination has unlimited stock (that is, never runs out)
price | number | Combination price. Omitted if the combination inherits the base product's price.
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of the combination's wholesale price tiers (quantity limit and price). Omitted if the combination inherits the base product's tiered price settings. 
weight | number | Combination weight in the units defined in store settings. Omitted if the combination inherits the base product's weight.

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
404 | The combination is not found or product is disabled in a store
500 | Cannot get the combination because of an error on the server


# Public product types

Product types (or product classes) are groups of products which share the same attributes. Product attributes contain additional product information, e.g. ISBN, UPC, Brand, which is displayed in storefront and included in product feeds when exporting to marketplaces like Google Shopping, eBay, Amazon ads etc. Learn more about product types and attributes in [Ecwid Help center](http://help.ecwid.com/customer/portal/articles/1167365-product-types).

For full management of product types in a store, please see [this section](#product-types)

## Get public product types

### Request

> Request example

```http
GET /api/v3/4870020/classes?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/classes?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token

<aside class="note">
Ecwid will return product types with "show" value <strong>other than "NOTSHOW"</strong> in a store.
</aside>

### Response

> Response example (JSON)

```json
[
  {
    "id":0,
    "attributes":[
      {
        "id":5029057,
        "name":"Brand",
        "type":"BRAND",
        "show":"DESCR"
      },
      {
        "id":5029058,
        "name":"UPC",
        "type":"UPC",
        "show":"DESCR"
      }
    ]
  },
  {
    "id":4208002,
    "name":"Body Weight Scale Accessories",
    "googleTaxonomy":"Health & Beauty > Health Care > Biometric Monitor Accessories > Body Weight Scale Accessories",
    "attributes":[
      {
        "id":5508061,
        "name":"UPC",
        "type":"UPC",
        "show":"DESCR"
      },
      {
        "id":5508062,
        "name":"Brand",
        "type":"BRAND",
        "show":"DESCR"
      }
    ]
  }
]
```

A JSON array of objects of type 'ProductClass' with the following fields:

#### ProductClass
Field | Type | Description
----- | ---- | -----------
id | number | Product type (class) internal unique ID. Class with ID `0` is the default 'General' type assigned to all products by default
name | string | Product type name. Empty for the "General" type
googleTaxonomy | string | Google taxonomy associated with this type
attributes | Array\<*Attribute*\> | Product type attributes

#### Attribute
Field | Type  | Description
----- | ----- | -----------
id | number | Attribute internal unique ID
name |  string | Attribute title
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit' attributes. The 'type' field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines how and where to display the product attribute value: `DESCR`, `PRICE`


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
500 | Cannot retrieve the product type info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message





## Get public product type

### Request

> Request example

```http
GET /api/v3/4870020/classes/4208002?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/classes/{classId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token
**classId** | number | Product type internal ID

<aside class="note">
Ecwid will return product types with "show" value <strong>other than "NOTSHOW"</strong> in a store.
</aside>

### Response

> Response example (JSON)

```json
  {
    "id":4208002,
    "name":"Body Weight Scale Accessories",
    "googleTaxonomy":"Health & Beauty > Health Care > Biometric Monitor Accessories > Body Weight Scale Accessories",
    "attributes":[
      {
        "id":5508061,
        "name":"UPC",
        "type":"UPC",
        "show":"DESCR"
      },
      {
        "id":5508062,
        "name":"Brand",
        "type":"BRAND",
        "show":"DESCR"
      }
    ]
  }
```

A JSON object of type 'ProductClass' with the following fields:

#### ProductClass
Field | Type | Description
----- | ---- | -----------
id | number | Product class internal unique ID. Class with ID `0` is the default 'General' type assigned to all products by default
name | string | Product type name. Empty for the "General" type
googleTaxonomy | string | Google taxonomy associated with this type
attributes | Array\<*Attribute*\> | Product type attributes

#### Attribute
Field | Type  | Description
----- | ----- | -----------
id | number | Attribute internal unique ID
name |  string | Attribute title
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit' attributes. The 'type' field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines how and where to display the product attribute value: `DESCR`, `PRICE`


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
400 | Request parameters are malformed
404 | Product type is not found
500 | Cannot retrieve the product type info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

# Get deleted items statistics

Using this group of API methods, you can get a list of products, orders, customers and coupons that have been deleted from the store. In combination with the update statistics, you can use these endpoints to check whether something was changed in an Ecwid store and keep data in your application synchronized with the Ecwid store you're working with. 

> Get deleted products stats

```http
GET /api/v3/4870020/products/deleted?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

There is one endpoint available: deleted products.

* `GET https://app.ecwid.com/api/v3/{storeId}/products/deleted?from_date={from_date}&to_date={to_date}&offset={offset}&limit={limit}&token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token
from_date | string | Item deletion date lower bound. Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul> Examples: <ul><li>`1447804800`</li> <li>`2015-04-22 18:48:38 -0500`</li> <li>`2015-04-22` (this is 2015-04-22 00:00:00 UTC)</li></ul>
to_date | string | Item deletion date upper bound. Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Default value: `100`


### Response

> Response example (JSON). Removed order stats

```json
{
    "total": 1,
    "count": 1,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            "id": 9,
            "date": "2014-10-20 18:07:24 +0400"
        }
    ]
}
```

> Response example (JSON). Removed coupons stats

```json
{
    "total": 2,
    "count": 2,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            "code": "UNN9GN6XAW2M",
            "date": "2014-10-20 18:00:54 +0400"
        },
        {
            "code": "MOXQ3YCWXRXA",
            "date": "2014-10-20 18:00:57 +0400"
        }
    ]
}
```

Each endpoint returns the information about the batch, the removed items IDs and the deletion dates in JSON object of type DeletedItemsStats

#### DeletedItemsStats
Field | Type | Description
----- | ---- | -----------
total | number | The total number of found items (might be more than the number of returned items)
count | number | The total number of the items returned in this batch
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
items | Array<RemovedItem> | The removed items list with IDs and dates

#### Removed item
Field | Type | Description
----- | ---- | -----------
id | number | Item ID. Depending on the request, that is products ID, customer ID, order number or coupon code. In case of coupons the field is called `code` instead if `id` and has string format. 
date | string | Item deletion date

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | There was an internal server error while processing the request
400 | Request parameters are malformed

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

# Calculate public order details

This method will calculate and return shipping rates and taxes for the order sent in a request. You can use it for your custom checkout process prior to creating an actual order in the store. Requests to this endpoint don't create any new orders in the actual store. See [Shipping](https://help.ecwid.com/customer/en/portal/articles/1161340-shipping) and [Taxes](https://help.ecwid.com/customer/en/portal/articles/1182159-taxes) help articles to find out more information.

For full version of this method in Ecwid REST API, check out [this section](#calculate-order-details).

> Request example

```http
POST /api/v3/4870020/order/calculate?token=public_123abcddasjkhdsaldsahashdu1h4asmn HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
        "items": [
            {
                "price": 15,
                "weight": 0.32,
                "sku": "00004",
                "quantity": 2,
                "isShippingRequired": false,
                "name": "Cherry"
            },
            {
                "price": 4.22,
                "weight": 0.12,
                "sku": "00014",
                "quantity": 2,
                "isShippingRequired": true,
                "name": "Apple"
            }
        ],
        "billingPerson": {
            "name": "Peter Doe",
            "companyName": "Awesome store inc.",
            "street": "My Personal Street",
            "city": "San Diego",
            "countryCode": "US",
            "postalCode": "90002",
            "stateOrProvinceCode": "CA",
            "phone": "123141321"
        },
        "shippingPerson": {
            "name": "Mary Watson",
            "companyName": "Best Brownies Inc.",
            "street": "The other street",
            "city": "San Diego",
            "countryCode": "US",
            "postalCode": "90001",
            "stateOrProvinceCode": "CA",
            "phone": "123141321"
        }
}
```

`POST https://app.ecwid.com/api/v3/{storeId}/order/calculate?token={token}`

Field | Type | Description
----- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | Public oAuth token

### Request body

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
email | string  | Customer email address
ipAddress | string  | Customer IP
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
**items** | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information. If no `shippingPerson` provided, the values are taken from `billingPerson`

#### OrderItem
Field | Type |  Description
--------- | -----------| -----------
id | number | Order item ID. Can be used to address the item in the order, e.g. to manage ordered items.
productId | number | Store product ID
categoryId |  number  | ID of the category this product belongs to. If the product belongs to many categories, categoryID will return the ID of the default product category. If the product doesn't belong to any category, `0` is returned
**price** | number | Price of ordered item in the cart
productPrice | number | Basic product price without options markups, wholesale discounts etc.
weight |  number | Product weight
sku | string | Product SKU. If the chosen options match a combination, this will be a combination SKU.
**quantity** |  number | Amount purchased
shortDescription | string | Product description truncated to 120 characters
tax | number | Tax amount applied to the item
shipping | number| Order item shipping cost 
quantityInStock | number | The number of products in stock in the store
name |  string | Product name
isShippingRequired | boolean | `true`/`false`: shows whether the item requires shipping. If not specified, the value is considered `true`
trackQuantity | boolean | `true`/`false`: shows whether the store admin set to track the quantity of this product and get low stock notifications
fixedShippingRateOnly | boolean | `true`/`false`: shows whether the fixed shipping rate is set for the product
imageUrl | string | Product image URL
fixedShippingRate | number| Fixed shipping rate for the product
digital | boolean | `true`/`false`: shows whether the item has downloadable files attached
productAvailable | boolean | `true`/`false`: shows whether the product is available in the store
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer

#### OrderItemTax
Field | Type |  Description
--------- | -----------| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
taxOnDiscountedSubtotal | number |  Tax on item subtotal (after applying discounts)
taxOnShipping | number | Tax on item shipping

#### OrderItemOption
Field | Type |  Description
--------- | -----------| -----------
name |  string | Option name
type |  string | Option type. One of: <ul><li>`CHOICE` (dropdown or radio button)</li><li>`CHOICES` (checkboxes)</li><li>`TEXT` (text input and text area)</li><li>`DATE` (date/time)</li><li>`FILES` (upload file option)</li></ul>
value | string | Selected/entered option value(s) as a string. For the `CHOICES` type, provides a string with all chosen values (comma-separated). You can use this to simply print out all selected values.
valuesArray | Array | Selected option values as an array. For the `CHOICES` type, provides an array with the chosen values so you can iterate through them in your app.
files | Array\<*OrderItemOptionFile*\> | Attached files (if the option type is `FILES`)

#### OrderItemOptionFile
Field | Type |  Description
--------- | -----------| -----------
id | number | File ID
name |  string | File name
size |  number | File size in bytes
url |   string | File URL

#### PersonInfo
Field | Type |  Description
--------- | -----------| -----------
name |  string  | Full name
companyName |   string  | Company name
street |    string  | Address
city |  string  | City
countryCode | string  | Two-letter country code
countryName | string | Country name
postalCode | string  | Postal/ZIP code
stateOrProvinceCode |   string  | State code, e.g. `NY`
stateOrProvinceName | string | State/province name, e.g. `New York`
phone | string  | Phone number

#### DiscountCouponInfo
Field | Type  | Description
----- | ----- | -----------
name |  string | Coupon title in store control panel
code |  string | Coupon code
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP`
discount | number | Discount amount
launchDate | string | The date of coupon launch, e.g. `2014-06-06 08:00:00 +0000`
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0000`
totalLimit | number| The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE`
repeatCustomerOnly | boolean | Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | Products and categories the coupon can be applied to

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
  "subtotal": 38.44,
  "total": 3093.93,
  "tax": 3.87,
  "couponDiscount": 0,
  "volumeDiscount": 15,
  "membershipBasedDiscount": 0.77,
  "totalAndMembershipBasedDiscount": 0,
  "discount": 20,
  "usdTotal": 0,
  "items": [
    {
      "id": 938012012,
      "productId": 123456789,
      "price": 15,
      "productPrice": 0,
      "sku": "00004",
      "quantity": 2,
      "tax": 0,
      "shipping": 0,
      "quantityInStock": 0,
      "name": "Cherry",
      "isShippingRequired": true,
      "weight": 0.32,
      "trackQuantity": false,
      "fixedShippingRateOnly": false,
      "fixedShippingRate": 0,
      "digital": false,
      "productAvailable": true,
      "couponApplied": false
    },
    {
      "id": 1023921938,
      "productId": 123456788,
      "price": 4.22,
      "productPrice": 0,
      "sku": "00014",
      "quantity": 2,
      "tax": 0,
      "shipping": 0,
      "quantityInStock": 0,
      "name": "Apple",
      "isShippingRequired": true,
      "weight": 0.12,
      "trackQuantity": false,
      "fixedShippingRateOnly": false,
      "fixedShippingRate": 0,
      "digital": false,
      "productAvailable": true,
      "couponApplied": false
    }
  ],
  "billingPerson": {
    "name": "Peter Doe",
    "companyName": "Awesome store inc.",
    "street": "My Personal Street",
    "city": "San Diego",
    "countryCode": "US",
    "countryName": "United States",
    "postalCode": "90002",
    "stateOrProvinceCode": "CA",
    "stateOrProvinceName": "California",
    "phone": "123141321"
  },
  "shippingPerson": {
    "name": "Mary Watson",
    "companyName": "Best Brownies Inc.",
    "street": "The other street",
    "city": "San Diego",
    "countryCode": "US",
    "countryName": "United States",
    "postalCode": "90001",
    "stateOrProvinceCode": "CA",
    "stateOrProvinceName": "California",
    "phone": "123141321"
  },
  "shippingOption": {
    "shippingCarrierName": "U.S.P.S.",
    "shippingMethodName": "U.S.P.S. Priority Mail Express 2-Day™",
    "shippingRate": 3071.62,
    "estimatedTransitTime": "1"
  },
  "handlingFee": {
    "name": "Handling Fee",
    "value": 0,
    "description": ""
  },
  "discountInfo": [
    {
      "value": 15,
      "type": "ABS",
      "base": "ON_TOTAL",
      "orderTotal": 1
    },
    {
      "value": 2,
      "type": "PERCENT",
      "base": "ON_MEMBERSHIP"
    },
    {
      "value": 11,
      "type": "PERCENT",
      "base": "ON_TOTAL_AND_MEMBERSHIP",
      "orderTotal": 1
    }
  ]
}
```

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
subtotal |  number | Order subtotal
total | number | Order total cost
email | string  | Customer email address
tax | number | Tax total
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
usdTotal | number | Order total in USD
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)

#### OrderItem
Field | Type |  Description
--------- | -----------| -----------
id | number | Order item ID. Can be used to address the item in the order, e.g. to manage ordered items.
productId | number | Store product ID
categoryId |  number  | ID of the category this product belongs to. If the product belongs to many categories, categoryID will return the ID of the default product category. If the product doesn't belong to any category, `0` is returned
price | number | Price of ordered item in the cart
productPrice | number | Basic product price without options markups, wholesale discounts etc.
weight |  number | Product weight
sku | string | Product SKU. If the chosen options match a combination, this will be a combination SKU.
quantity |  number | Amount purchased
shortDescription | string | Product description truncated to 120 characters
tax | number | Tax amount applied to the item
shipping | number| Order item shipping cost 
quantityInStock | number | The number of products in stock in the store
name |  string | Product name
isShippingRequired | boolean | `true`/`false`: shows whether the item requires shipping
trackQuantity | boolean | `true`/`false`: shows whether the store admin set to track the quantity of this product and get low stock notifications
fixedShippingRateOnly | boolean | `true`/`false`: shows whether the fixed shipping rate is set for the product
imageUrl | string | Product image URL
fixedShippingRate | number| Fixed shipping rate for the product
digital | boolean | `true`/`false`: shows whether the item has downloadable files attached
productAvailable | boolean | `true`/`false`: shows whether the product is available in the store
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer

#### OrderItemOption
Field | Type |  Description
--------- | -----------| -----------
name |  string | Option name
type |  string | Option type. One of: <ul><li>`CHOICE` (dropdown or radio button)</li><li>`CHOICES` (checkboxes)</li><li>`TEXT` (text input and text area)</li><li>`DATE` (date/time)</li><li>`FILES` (upload file option)</li></ul>
value | string | Selected/entered option value(s) as a string. For the `CHOICES` type, provides a string with all chosen values (comma-separated). You can use this to simply print out all selected values.
valuesArray | Array | Selected option values as an array. For the `CHOICES` type, provides an array with the chosen values so you can iterate through them in your app.
files | Array\<*OrderItemOptionFile*\> | Attached files (if the option type is `FILES`)

#### OrderItemOptionFile
Field | Type |  Description
--------- | -----------| -----------
id | number | File ID
name |  string | File name
size |  number | File size in bytes
url |   string | File URL

#### PersonInfo
Field | Type |  Description
--------- | -----------| -----------
name |  string  | Full name
companyName |   string  | Company name
street |    string  | Address
city |  string  | City
countryCode | string  | Two-letter country code
countryName | string | Country name
postalCode | string  | Postal/ZIP code
stateOrProvinceCode |   string  | State code, e.g. `NY`
stateOrProvinceName | string | State/province name, e.g. `New York`
phone | string  | Phone number

#### DiscountCouponInfo
Field | Type  | Description
----- | ----- | -----------
name |  string | Coupon title in store control panel
code |  string | Coupon code
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP`
discount | number | Discount amount
launchDate | string | The date of coupon launch, e.g. `2014-06-06 08:00:00 +0000`
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0000`
totalLimit | number| The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE`
repeatCustomerOnly | boolean | Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | Products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

#### ShippingOptionInfo
Field | Type | Description
----- | ---- | -----------
shippingCarrierName | string | Shipping carrier name, e.g. `USPS`
shippingMethodName | string | Shipping option name
shippingRate | number | Rate
estimatedTransitTime | string | Delivery time estimation. Possible formats: number "5", several days estimate "4-9"

#### HandlingFeeInfo
Field | Type | Description
----- | ---- | -----------
name | string | Handling fee name set by store admin. E.g. `Wrapping`
value | number | Handling fee value
description | string | Handling fee description for customer

#### DiscountInfo
Field | Type | Description
----- | ---- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`
order_total | number | Minimum order subtotal the discount applies to

### Errors

> Error response example

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
422 | No order items or incorrect items details sent
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


