# Add Shipping Method

Using the Custom Shipping API, you can integrate a new shipping carrier to provide real-time shipping methods with different rates for the Ecwid store customers. This functionality will work in the form of an application that users install from the Ecwid App Market.

<aside class="notice">
Access scope required: <strong>add_shipping_method</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

**Table of contents**: 

- [How it works](https://developers.ecwid.com/api-documentation/how-shipping-method-works)
- [Set up custom shipping](https://developers.ecwid.com/api-documentation/set-up-shipping-method)
- [Saving merchant settings](https://developers.ecwid.com/api-documentation/merchant-settings-for-shipping-method)
- [Shipping request from Ecwid – Details](https://developers.ecwid.com/api-documentation/shipping-request-and-response)
- [Troubleshooting](https://developers.ecwid.com/api-documentation/troubleshooting-shipping)

## How shipping method works

> Checkout flow for shipping apps
> 
> ![Checkout flow for shipping apps](https://don16obqbay2c.cloudfront.net/wp-content/uploads/Shipping-Method-Diagram-n-1512473753.png)

#### 1. User configures app settings in settings tab

After the installation, user would need a page where they can configure it: provide their account details, set up dimensions, etc. We recommend using [Native apps feature](#embedded-apps) to provide this functionality. To manage and store those settings, see the [Merchant app settings](#merchant-settings-for-shipping-method) section.

#### 2. Ecwid sends order data to app request URL

The request to your app URL can be triggered by a customer in storefront or by an API request to order details calculation [endpoint](https://developers.ecwid.com/api-documentation/carts#calculate-order-details).

To show new shipping methods in storefront, Ecwid will send a **POST request** to your endpoint with order details: items that require shipping and have shipping set as [global or global + fixed rate per item](https://support.ecwid.com/hc/en-us/articles/115005899725-Setting-up-shipping-rates#Shippingfreightnbsp), customer address, merchant app settings, etc. That endpoint must respond to the request with the shipping rates for this configuration.

In the case of an API request for calculating order details, the products and cart information itself can be different from what the store has:  

for example, some other application can create a custom storefront where it requests order calculation with items that are not present in the store. For these cases, your application should also provide correct discount calculations for the amount of cart information available.

#### 3. Application returns the rates in a specific format

Ecwid will expect a response from your service within 10 second interval to display additional shipping methods for customers. In the response, provide shipping method name, rate and estimated delivery time. See the response format in the [Request and response](https://developers.ecwid.com/api-documentation/shipping-request-and-response) section.

#### 4. Ecwid displays the rates at checkout

Based on the response from your app, Ecwid will display the shipping methods for customers at the checkout. Customer can select them just like any other shipping method in that Ecwid store and it will be shown in the order details. Shipping methods from the application will be added to any currently enabled shipping methods a store has enabled.

### Custom shipping FAQ

#### Q: What happens, if my URL responds with an error? 

In case if your app responds with an error or in an incorrect format, then the shipping methods from your app will not be shown to customer at checkout stage. If your store has applicable shipping methods set up using the default means of creating shipping methods in Ecwid, then they will be shown to customer.

#### Q: Does Ecwid cache the requests in any way? 

Yes. Ecwid will not make a request to your URL on these conditions: 

- previous request happened less than 5 minutes ago
- request details are exactly the same as in previous request

In other cases, Ecwid will make a new request to your endpoint.

For example, a new product was added to cart, shipping/payment method was changed, customer address was changed, etc. 

Any event that changes the order details will reset the cache and Ecwid will make a new request to your endpoint.

## Set up shipping method

After you [registered a new application](/register) for Ecwid, **send your shipping URL** to [Ecwid team](/contact). 

Ecwid will be sending order details requests to that endpoint and expect shipping rates in a specific format in response.

Next, start working on shipping calculations according to the documentation: [https://developers.ecwid.com/api-documentation/shipping-request-and-response](https://developers.ecwid.com/api-documentation/shipping-request-and-response)

<aside class='notice'>
If your application is for a public use, the request URL must be working <strong>via HTTPS</strong>. Also, the certificate can only be from <strong>trusted CA's and not self-signed</strong>.
</aside>

## Merchant settings for shipping method

Your application can require merchants to specify their shipping account details, package size and any other user preferences you may require. We recommend adding a new tab into the Ecwid Control Panel's Shipping settings for optimal experience - Native applications feature.

> Merchant settings example

> ![Merchant settings example](https://don16obqbay2c.cloudfront.net/wp-content/uploads/shippingSettings-1468407629.png)

**Settings**

First, set up a new tab in Ecwid Control Panel, which will serve as a settings page for your users. This tab will load a page from your server in an iframe in a separate tab of Ecwid Control Panel. See [Native Applications](#native-applications).

When merchant is in the settings tab of your app, your code can create and modify the merchant settings using the **Application storage** feature. It's a simple `key:value` storage, which can serve you as an app database. For your convenience, you can access it [via Javascript](https://developers.ecwid.com/api-documentation/storage-in-ecwid-api#javascript-storage-api) (client-side) or [Ecwid REST API](https://developers.ecwid.com/api-documentation/storage-in-ecwid-api#rest-storage-api) (server-side).

**Request**

Once the settings are saved there, Ecwid will send them in a **POST request** to your application alongside order details when customer is at checkout stage. The request will contain **all data** from your application storage, including public and other keys that were specified. Use it to idetify the store and a user for the shipping rates.

You can also use the `public` key of the application storage to save data for accessing in the storefront. More details on how to handle such data: [Public application config](#public-application-config).

Please make sure **not to pass any sensitive user data in the public application config**, as this information will be available via Ecwid Javascript API to any 3-rd party. To save and get that kind of information, use any other key names in your application storage. They will be provided in a request to your application as well as public information, but not accessible in the storefront.

**Response**

After you get a request from Ecwid, your application endpoint should get its components and return correct rates back to the customer in a response.

## Shipping request and response

### Request

Ecwid will send order information in the **body** of a POST HTTP request in the following format. 

<aside class="note">
Ecwid can cache the requests to your endpoint. <a href="https://developers.ecwid.com/api-documentation/how-shipping-method-works#q-does-ecwid-cache-the-requests-in-any-way">Learn more</a>
</aside>

> Request from Ecwid example

```http
POST https://mycoolapp.com/integration HTTP/1.1
```
```json
{
    "storeId": 35002,
    "merchantAppSettings": {
        "public":"{color : \"red\", storeName : \"Cool Socks Ltd.\"}",
        "dimensions": "5x10x7",
        "userId": "12345"
    },
    "cart": {
        "subtotal": 7.08,
        "ipAddress": "127.0.0.1",
        "couponDiscount": 0,
        "paymentStatus": "INCOMPLETE",
        "fulfillmentStatus": "NEW",
        "refererUrl": "https://mdemo.ecwid.com/",
        "orderComments": "Leave at the front porch.",
        "volumeDiscount": 5,
        "membershipBasedDiscount": 0.14,
        "totalAndMembershipBasedDiscount": 1,
        "discount": 6.14,
        "customerGroupId": 123456,
        "customerGroup": "Gold members",
        "customerId": 23649002,
        "paymentMethod": "Purchase order",
        "email": "johnsmith@example.com",

        "discountCoupon": {
                "name": "Coupon # 3",
                "code": "5PERCENTOFF",
                "discountType": "PERCENT",
                "status": "ACTIVE",
                "discount": 5,
                "launchDate": "2014-06-06 00:00:00 +0000",
                "usesLimit": "UNLIMITED",
                "repeatCustomerOnly": false,
                "creationDate": "2014-09-20 19:58:49 +0000",
                "orderCount": 0
        },
        "discountInfo": [
            {
                "orderTotal": 1,
                "value": 5,
                "type": "ABS",
                "base": "ON_TOTAL",
                "membershipId": 0
            },
            {
                "orderTotal": 1,
                "value": 2,
                "type": "PERCENT",
                "base": "ON_MEMBERSHIP",
                "membershipId": 0
            },
            {
                "orderTotal": 1,
                "value": 1,
                "type": "ABS",
                "base": "ON_TOTAL_AND_MEMBERSHIP",
                "membershipId": 0
            }
        ],
        "items": [
            {  
                "weight":0.25,
                "price":18.95,
                "amount":1,
                "productId":85607123,
                "name":"All-Weather\u0026trade; Solid Black Mini Umbrella",
                "categoryId":-1,
                "sku":"GFUMLT",
                "selectedOptions":null,
                "dimensions":{  
                   "length":9.0,
                   "width":1.0,
                   "height":7.0
                },
                "productPrice":18.95,
                "categoryIds":[],
                "categories":[],
                "quantity":16658,
                "unlimited":false,
                "inStock":true,
                "priceInProductList":18.95,
                "isShippingRequired":true,
                "productClassId":0,
                "enabled":true,
                "warningLimit":0,
                "fixedShippingRateOnly":false,
                "fixedShippingRate":0.0,
                "options":null,
                "wholesalePrices":null,
                "compareToPrice":null,
                "url":"https://mdemo.ecwid.com/#!/All-Weather\u0026trade-Solid-Black-Mini-Umbrella/p/85607123",
                "created":"2017-06-06 11:47:18 +0000",
                "updated":"2017-09-18 08:22:29 +0000",
                "createTimestamp":1496749638,
                "updateTimestamp":1505722949,
                "defaultCombinationId":0,
                "imageUrl":null,
                "thumbnailUrl":null,
                "smallThumbnailUrl":null,
                "hdThumbnailUrl":null,
                "originalImageUrl":null,
                "originalImage":null,
                "borderInfo":null,
                "galleryImages":[],
                "defaultCategoryId":0,
                "seoTitle":"All-Weather\u0026trade; Solid Black Mini Umbrella",
                "seoDescription":"This 40\u0026quot; super mini umbrella folds up to a compact 9\u0026quot; x 1-3/4\u0026quot; x 1-1/4\u0026quot; to carry in your purse or briefcase. The uni-chrome ribs, plastic handle and self tips combine to make this a sturdy and functional accessory. Comes in a clear, plastic polypropylene case.",
                "favorites":{  
                   "count":0,
                   "displayedCount":"0"
                },
                "attributes":[],
                "relatedProducts":{  
                   "productIds":[],
                   "relatedCategory":{  
                      "enabled":false,
                      "categoryId":0,
                      "productCount":1
                   }
                },
                "combinations":[],
                "showOnFrontpage":1
             }
        ],
        "predictedPackages":[  
            {  
                "length":12.5,
                "width":6,
                "height":3.5,
                "weight":1.5,
                "declaredValue":7.08
            }
        ],
        "shippingAddress": {
            "street": "5th Avenue",
            "city": "New York",
            "countryCode": "US",
            "postalCode": "10002",
            "stateOrProvinceCode": "NY",
            "stateOrProvinceName": "New York"
        },
        "originAddress": {
            "street": "Columbus Street, 5",
            "city": "Idaho Falls",
            "countryCode": "US",
            "postalCode": "30135",
            "stateOrProvinceCode": "GA"
        },
        "weight": 1.5,
        "weightUnit": "lbs",
        "currency": "USD",
        "dimensionUnit" : "MM",
        "handlingFee": {
            "name": "Wrapping",
            "value": 2,
            "description": "Silk paper wrapping"
        },
        "extraFields": {
            "platform": {
                "value": "adobe_muse"
            },
            "AFF_ID": {
                "value": "fb-123"
            },
            "how_did_you_find_us": {
                "title": "How you found us?",
                "value": "TV show",
                "order_details_display_section": "order_comments"
            }
        }
    }
}
```

Name | Type    | Description
---- | ------- | --------------
storeId |  number | Ecwid store ID
merchantAppSettings | json | Merchant settings for your integration set up by your code. [More details](#merchant-settings-for-shipping-method)
cart | \<*CartDetails*\> | Offset from the beginning of the returned items list (for paging)

#### CartDetails

Name | Type    | Description
---- | ------- | --------------
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order
ipAddress | string  | Customer IP
paymentStatus | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li></ul>
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Order comments
couponDiscount | number | Discount applied to order using a coupon
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
customerGroupId | number | Customer group ID
customerGroup | string | The name of group (membership) the customer belongs to
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
items | Array\<*OrderItems*\> | Array of customer's order items with basic details. **Only includes items that that require shipping and have shipping set as [global or global + fixed rate per item](https://support.ecwid.com/hc/en-us/articles/115005899725-Setting-up-shipping-rates#Shippingfreightnbsp)**
weight | number | Total weight of the order
weightUnit | string | Active weight units in the store at the moment of the request. [Formats and units](https://developers.ecwid.com/api-documentation/shipping-request-and-response#weight-units)
currency | string | Active currency in the store at the moment of the request
predictedPackages | Array\<*PredictedPackage*\> | Predicted information about the packages to ship items in to customer
shippingAddress | \<*ShippingAddressInfo*\> | Shipping address details (destination)
originAddress | \<*OriginAdressInfo*\> | Origin address details (departure)
dimensionUnit | string | Active dimension units of a store at the moment of the request. Possible values: `IN`,`YD`,`CM`,`MM`
paymentMethod | string | Payment method used by customer
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
email | string  | Customer email address
extraFields | \<*ExtraFieldsInfo*\> | Additional optional information about order. Total storage of extra fields cannot exceed 8Kb. See [Order extra fields](#order-extra-fields)

#### OrderItems
Field | Type |  Description
--------- | -----------| -----------
productId | number | Store product ID
categoryId |  number  | ID of category this product was added to cart from. If the product was added to cart from API or Search page, `categoryID` will return `-1`
price | number | Price of ordered item in the cart
weight |  number | Product weight
sku | string | Product SKU. If the chosen options match a variation, this will be a variation SKU.
amount |  number | Amount purchased
name |  string | Product name
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer
dimensions | \<*OrderItemDimensions*\> | Product dimensions info
quantity |  number | Amount of product items in stock. *This field is omitted for the products with unlimited stock*
unlimited | boolean | `true` if the product has unlimited stock
inStock | boolean | `true` if the product or any of its variations is in stock (quantity is more than zero) or has unlimited quantity. `false` otherwise.
name |  string |  Product title
productPrice | number | Product price set by store owner
priceInProductList | number |  Product price displayed in a storefront. May differ from the *price* value when the product has options and variations and the default variation's price is different from the base product price. **Does not include taxes**
wholesalePrices | Array\<*WholesalePrice*\> |  Sorted array of wholesale price tiers (quantity limit and price pairs)
compareToPrice |  number | Product's sale price displayed strike-out in the customer frontend *Omitted if empty*
isShippingRequired | boolean | `true` if product requires shipping, `false` otherwise
url | string |  URL of the product's details page in the store. [Learn more](https://developers.ecwid.com/api-documentation/products#q-how-to-get-urls-for-products)
created | string | Date and time of the product creation. Example: `2014-07-30 10:32:37 +0000`
updated |  string | Product last update date/time
createTimestamp | number | The date of product creation in UNIX Timestamp format, e.g `1427268654`
updateTimestamp | number | Product last update date in UNIX Timestamp format, e.g `1427268654`
productClassId |  number | Id of the class (type) that this product belongs to. `0` value means the product is of the default 'General' class. See also: [Product types and attributes in Ecwid](http://help.ecwid.com/customer/portal/articles/1167365-product-types-and-attributes)
enabled | boolean | `true` if product is enabled, `false` otherwise. Disabled products are not displayed in the store front.
options | Array\<*ProductOption*\> | A list of the product options. Empty (`[]`) if no options are specified for the product. 
warningLimit | number | The minimum 'warning' amount of the product items in stock, if set. When the product quantity reaches this level, the store administrator gets an email notification.
fixedShippingRateOnly | boolean | `true` if shipping cost for this product is calculated as *'Fixed rate per item'* (managed under the "Tax and Shipping" section of the product management page in Ecwid Control panel). `false` otherwise. With this option on, the `fixedShippingRate` field specifies the shipping cost of the product
fixedShippingRate | number |  When `fixedShippingRateOnly` is `true`, this field sets the product fixed shipping cost per item. When `fixedShippingRateOnly` is `false`, the value in this field is treated as an extra shipping cost the product adds to the global calculated shipping
defaultCombinationId |  number |  Identifier of the default product variation, which is defined by the default values of product options.
thumbnailUrl |  string | URL of the product thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. Default size of the biggest dimension is 400px. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product image resized to fit 1500x1500px. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product thumbnail resized to fit 160x160px. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product HD thumbnail URL resized to fit 800x800px
originalImageUrl |  string  | URL of the original not resized product image
originalImage | \<*ImageDetails*\> | Details of the product image
galleryImages | Array\<*GalleryImage*\> |  List of the product gallery images
categoryIds | Array\<*number*\> | List of the categories, which the product belongs to. If no categories provided, product will be displayed on the store front page, see `showOnFrontpage` field
categories | Array\<*CategoriesInfo*\> | List of the categories, which the product belongs to, with brief details. If no categories provided, product belogs to store front page, see `showOnFrontpage` field
seoTitle | string | Page title to be displayed in search results on the web. Recommended length is under 55 characters
seoDescription | string | Page description to be displayed in search results on the web. Recommended length is under 160 characters
defaultCategoryId | number  | Identifier of the default category of the product
favorites | \<*FavoritesStats*\>  | Product favorites stats
attributes | Array\<*AttributeValue*\> | Product attributes and their values
relatedProducts | \<*RelatedProducts*\>  | Related or "You may also like" products of the product
combinations | Array\<*Variation*\> | List of the product variations
showOnFrontpage | number | A positive number indicates the position (index) of a product in the store front page – the smaller the number, the higher the product is displayed on a page. A negative value means the product is not shown in the store front page


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

#### OrderItemDimensions
Field | Type  | Description
-------------- | -------------- | --------------
length | number | Length of a product
width | number | Width of a product
height | number | Height of a product

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
url | string |  **Deprecated**. Original image URL. Equals `originalImageUrl`
thumbnail | string  | **Deprecated**. Image thumbnail URL resized to fit 160x160px. Equals `smallThumbnailUrl`
thumbnailUrl |  string | URL of the product thumbnail displayed on the product list pages. Thumbnails size is defined in the store settings. Default size of the biggest dimension is 400px. *The original uploaded product image is available in the `originalImageUrl` field.*
imageUrl |  string  | URL of the product image resized to fit 1500x1500px. *The original uploaded product image is available in the `originalImageUrl` field.*
smallThumbnailUrl | string  | URL of the product thumbnail resized to fit 160x160px. *The original uploaded product image is available in the `originalImageUrl` field.*
hdThumbnailUrl | string  | Product HD thumbnail URL resized to fit 800x800px
originalImageUrl |  string  | URL of the original not resized product image
width | number |  Image width
height |  number |  Image height
orderby |  number |  The sort weight of the image in the gallery images list. The less the number, the closer the image to the beginning of the gallery

#### CategoriesInfo
Field | Type  | Description
----- | ----- | -----------
id | number | Category ID
enabled | boolean | `true` if category is enabled, `false` otherwise

#### AttributeValue
Field | Type  | Description
-------------- | -------------- | --------------
id |  number |  Unique attribute ID. See [Product Classes](#product-types) for the information on attribute IDs
name |  string |  Attribute displayed name
value | string  | Attribute value
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit’ attributes. The 'type’ field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines where to display the product attribute value:. Supported values: `NOTSHOW`, `DESCR`, `PRICE` 

#### RelatedProducts
Field | Type  | Description
-------------- | -------------- | --------------
productIds | Array\<*number*\>  | IDs of the related products
relatedCategory | *RelatedCategory*  | Describes the "N random related products from a category" option

#### RelatedCategory
Field | Type  | Description
-------------- | -------------- | --------------
enabled | boolean | `true` if the "N random related products from a category" option is enabled. `false` otherwise
categoryId |  number |  Id of the related category. Zero value means "any category", that is, random products from the whole store.
productCount |  number |  Number of random products from the given category to be shown as related

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
warningLimit | number | The minimum 'warning' amount of the product items in stock for this variation, if set. When the variation in stock amount reaches this level, the store administrator gets an email notification. Omitted if the variation inherits the base product's settings.

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

#### DiscountInfo
Field | Type | Description
----- | ---- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`
order_total | number | Minimum order subtotal the discount applies to

#### ExtraFieldsInfo
Field | Type | Description
----- | ---- | -----------
YOUR_FIELD_NAME | string | Your custom name saved for the order extra field. The value length cannot exceed 255 characters

#### PredictedPackage
Name | Type    | Description
---- | ------- | --------------
height | number | Height of a predicted package
width | number | Width of a predicted package
length | number | Length of a predicted package
weight | number | Total weight of a predicted package
declaredValue | number | Declared value of a predicted package (subtotal of items in package)

#### ShippingAddressInfo

Name | Type    | Description
---- | ------- | --------------
street | string | Customer's street
city | string | Customer's city
countryCode | string | Customer's country code in Ecwid
countryName | string | Customer's country name in Ecwid
postalCode | string | Customer's postal code
stateOrProvinceCode | string | Customer's state or province code in Ecwid
stateOrProvinceName | string | Customer's state or province name in Ecwid

#### OriginAdressInfo

Name | Type    | Description
---- | ------- | --------------
street | string | Customer's street
city | string | Customer's city
countryCode | string | Customer's country code in Ecwid
postalCode | string | Customer's postal code
stateOrProvinceCode | string | Customer's state or province code in Ecwid

#### HandlingFeeInfo
Field | Type | Description
----- | ---- | -----------
name | string | Handling fee name set by store admin. E.g. `Wrapping`
value | number | Handling fee value
description | string | Handling fee description for customer

### Response

> Response to Ecwid example

```json
{
    "shippingOptions": [{
        "title": "SuperMail First Class",
        "rate": 10.31,
        "transitDays": "2-7"
    }, {
        "title": "SuperMail Regular Delivery",
        "rate": 5.01,
        "transitDays": "5"
    }]
}
```

An array of JSON data of type 'ShippingOptions' with the following fields:

#### ShippingOptions

Name | Type    | Description
---- | ------- | --------------
**title** | string | Shipping method name
**rate** | number | Shipping rate amount
**transitDays** | string | Estimated delivery time. Formats accepted: empty `""`, number `"5"`, several days estimate `"4-9"`

<aside class="notice">
Response parameters in <strong>bold</strong> are mandatory
</aside>

### Weight units

Ecwid supports several weigh units that can be passed in the request to your application to provide shipping rates. Below you can see all available units as well as their conversion values for calculation. 

Name | Code | Value
-----|------|------
Carat | carat | 0.2
Gram | gram | 1
Ounce | ounce | 28.35
Pound | lbs | 453.6
Kilogram | kg | 1000

Gram is the main weight unit, from which other units are converted. Merchants can change weight units in [Ecwid Control Panel](https://my.ecwid.com/cp/CP.html#formats-units).

## Troubleshooting shipping

#### A new shipping method at checkout is not appearing

You created an app and installed it on your test store, but a new shipping method, returned by your resource, is not appearing when you open your store. There are several possible reasons for this:

* **The application is not configured properly** to add a shipping method at checkout. E.g. during registration, you haven't provided a link for Ecwid to send the requests to when customer is at checkout or the URL is incorrect. See [How to set uo](https://developers.ecwid.com/api-documentation/set-up-shipping-method) for the details.
* **`add_shipping_method` access scope** is missing in the list of requested scopes while installing the app. While creating an oAuth URL or installing your app from an app details page, make sure it incudes the `add_to_cp` scope in the list of requested permissions. 
* **The response format from your resource is incorrect**. Ecwid accepts response from shipping applications in [a strict format](https://developers.ecwid.com/api-documentation/shipping-request-and-response), so please make sure your endpoint is responding correctly back to Ecwid with the correct shipping methods. 
* **The response from your resource has exceeded 10 second timeout**. When Ecwid sends a request for additional shipping methods to external resources, it expects to get a response within the 10 second timeout period. Make sure that your service is able to provide response in that time period.
* **You're testing it in an Ecwid store which is on Free plan**. Ecwid API functionality is available on paid Ecwid plans only. Please upgrade your account or [contact us](/contact).
