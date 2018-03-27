# Add Custom Discount

With the Custom Discount API you can apply custom discounts to the order total when the customer is at the checkout.

<aside class="notice">
Access scope required: <strong>customize_cart_calculation</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

**Table of contents**: 

- [How it works](https://developers.ecwid.com/api-documentation/how-custom-discount-works)
- [Set up custom discounts](https://developers.ecwid.com/api-documentation/set-up-custom-discount)
- [Saving merchant settings](https://developers.ecwid.com/api-documentation/user-interface-for-discount-rules)
- [Discount request from Ecwid – Details](https://developers.ecwid.com/api-documentation/request-for-discount-and-response)

#### Discount types examples

The Custom Discount API allows you to apply an absolute or percent discount to an order. While this sounds simple enough, it provides many possibilities and different ways to use it in a store.

Examples:

- **Customer loyalty**: when a customer from a VIP customer group is at the checkout, apply 5% discount.
- **Limited-time offers**: enable or disable discounts based on a current date.
- **Apply discount to select products**: when a customer adds a product from the sale category, apply 3% discount.
- **Local customer discount**: if the customer’s location is in the same city as the store, apply a local discount of $5.
- **Buy one, get one free (BOGOF)**: when the customer puts a specific product in their cart, the app adds a new free product with the JS API. The app applies an absolute discount for the cost amount of that free product.
- And many more!


## How custom discount works

> Checkout flow for discount apps

> ![Checkout flow for discount apps](https://don16obqbay2c.cloudfront.net/wp-content/uploads/Custom-Discount-Diagram-1515505409.png)

#### 1. Ecwid sends cart data to app request URL

The request to your app URL can be triggered by a customer in storefront or by an API request to order details calculation [endpoint](https://developers.ecwid.com/api-documentation/carts#calculate-order-details). 

To apply new discounts for a cart in storefront, Ecwid will send a **POST request** to your endpoint with cart details: items, customer address, merchant app settings, etc.

For all of the cases below, your application should provide correct discount calculations for the total of cart information available to the application.

**Customer in storefront**

The request will be sent when the customer's cart changes - a new product is added to cart, shipping address is changed, cart initialized, a product was added to a cart, a product removed from cart, the product’s options changed on cart page, cart was cleared, a discount coupon applied, selected shipping method changed, cart contents are synced, if there are a few browser tabs with the store are opened, etc.

**Calculate order details API сall**

The products and cart information itself can be different from what the store actually has. For example, some other application can create a custom storefront where it requests order calculation with items that are not present in the store. 

#### 2. Application returns discounts in a specific format

Ecwid will expect **a response to its request** from your service within **5 second interval** to display additional discounts for an order. In the response, provide discount value, type of discount (percent or absolute) and discount description. See the response format in the [Request and response](https://developers.ecwid.com/api-documentation/request-for-discount-and-response) section.

<aside class="notice">
Please mind that the response isn't a separate request back to a specific URL in Ecwid. This needs to be a response to the request Ecwid makes to the application URL.
</aside>

#### 3. Ecwid displays discounts at checkout

Based on the response from your app, Ecwid will display the discounts for customers on the cart page. Customer can view the amount and the reason for a discount that your solution sent to Ecwid. All the discount details will be saved for that order and they will be displayed in related order information.

### Custom discounts FAQ

#### Q: In what order does Ecwid calculate the order total?

Ecwid calculates the order total in this order: 

1. Order subtotal
2. Coupon discount
3. Handling fee
4. Discounts (app discounts too)
5. Shipping
6. Taxes

Please keep this in mind when creating an app for Ecwid.

#### Q: What happens, if my URL responds with an error? 

In case if your app responds with an error or in an incorrect format, then the discounts from your app will not be shown to customer at checkout stage and they will be able to shop in the store as usual. If your store has applicable discounts methods set up using the default means of creating discounts in Ecwid, then they will be shown to customer and applied to order.

#### Q: Does Ecwid cache the requests in any way? 

Yes. Ecwid will not make a request to your URL on these conditions: 

- previous request happened less than 5 minutes ago
- request details are exactly the same as in previous request

In other cases, Ecwid will make a new request to your endpoint.

For example, a new product was added to cart, shipping/payment method was changed, customer address was changed, etc. 

Any event that changes the order details will reset the cache and Ecwid will make a new request to your endpoint.

#### Q: Can I create a user interface for user to select and set different duscount rules?

After the installation, your app can add a page where they can configure it: provide their account details, set up discount rules, enable/disable rules, etc. We recommend using **Native apps** feature and the **Application storage** feature to provide this functionality. To manage and store those settings, see the [Advanced setup](https://developers.ecwid.com/api-documentation/user-interface-for-discount-rules) section.

## Set up custom discount

After you [registered a new application](/register) for Ecwid, **send your custom discount URL** to [Ecwid team](/contact). 

Ecwid will be sending order details requests to that endpoint and expect discounts in a specific format in response.

Next, start working on discount calculations according to the documentation: [https://developers.ecwid.com/api-documentation/request-for-discount-and-response](https://developers.ecwid.com/api-documentation/request-for-discount-and-response)

<aside class='notice'>
If your application is for a public use, the request URL must be working <strong>via HTTPS</strong>. Also, the certificate can only be from <strong>trusted CA's and not self-signed</strong>.
</aside>

## Request for discount and response

Ecwid will send the cart details in a **body** of POST HTTP request in the following format. 

<aside class="note">
Ecwid can cache the requests to your endpoint. <a href="https://developers.ecwid.com/api-documentation/how-custom-discount-works#q-does-ecwid-cache-the-requests-in-any-way">Learn more</a>
</aside>

#### Request

> Request from Ecwid example

```http
POST https://mycoolapp.com/discounts HTTP/1.1
```
```json
{  
   "storeId":1003,
   "merchantAppSettings":{  
      "syncOrders":"tXXXe",
      "accountName":"cXXXs",
      "exists":"yXXXs"
   },
   "cart":{  
      "subtotal":24.15,
      "ipAddress":null,
      "couponDiscount":5.0,
      "paymentStatus":"INCOMPLETE",
      "fulfillmentStatus":"NEW",
      "refererUrl":"https://mdemo.ecwid.com/",
      "orderComments":null,
      "volumeDiscount":4.0,
      "membershipBasedDiscount":0.0,
      "totalAndMembershipBasedDiscount":0.0,
      "discount":4.0,
      "customerGroupId":null,
      "customerGroup":null,
      "customerId":40201284,
      "customerEmail":"riq363@gmail.com",
      "discountCoupon":{  
         "id":27718364,
         "name":"SALE",
         "code":"AB0708",
         "discountType":"ABS",
         "status":"ACTIVE",
         "discount":5.0,
         "launchDate":"Dec 12, 2017 8:00:00 PM",
         "expirationDate":"Sep 1, 2020 7:59:59 PM",
         "usesLimit":"UNLIMITED",
         "catalogLimit":null,
         "repeatCustomerOnly":false,
         "creationDate":"Feb 16, 2018 2:27:01 PM",
         "orderCount":0,
         "updateDate":"Dec 13, 2017 1:35:31 PM"
      },
      "discountInfo":[  
         {  
            "orderTotal":1.0,
            "value":4.0,
            "type":"ABS",
            "base":"ON_TOTAL",
            "membershipId":0,
            "description":null
         }
      ],
      "handlingFee":{  
         "name":"Handling Fee",
         "value":4.0,
         "description":""
      },
      "hidden":false,
      "items":[  
         {  
            "weight":0.0,
            "price":0.0,
            "amount":1,
            "productId":66722516,
            "name":"SCH-R850",
            "categoryId":-1,
            "sku":"00034",
            "selectedOptions":[  
               {  
                  "name":"Size",
                  "value":"M",
                  "valuesArray":[  
                     "M"
                  ],
                  "type":"CHOICE",
               }
            ],
            "dimensions":{  
               "length":0.0,
               "width":0.0,
               "height":0.0
            },
            "productPrice":0.0,
            "categoryIds":[],
            "categories":[],
            "quantity":null,
            "unlimited":true,
            "inStock":true,
            "priceInProductList":0.0,
            "isShippingRequired":true,
            "productClassId":0,
            "enabled":true,
            "warningLimit":0,
            "fixedShippingRateOnly":false,
            "fixedShippingRate":0.0,
            "options":[  
               {  
                  "type":"RADIO",
                  "name":"Size",
                  "defaultChoice":0,
                  "required":false,
                  "choices":[  
                     {  
                        "text":"S",
                        "priceModifier":0.0,
                        "priceModifierType":"PERCENT"
                     },
                     {  
                        "text":"M",
                        "priceModifier":1.0,
                        "priceModifierType":"PERCENT"
                     },
                     {  
                        "text":"L",
                        "priceModifier":2.0,
                        "priceModifierType":"PERCENT"
                     },
                     {  
                        "text":"XL",
                        "priceModifier":3.0,
                        "priceModifierType":"PERCENT"
                     }
                  ]
               }
            ],
            "wholesalePrices":null,
            "compareToPrice":null,
            "url":"https://mdemo.ecwid.com/#!/SCH-R850/p/66722516",
            "created":"2016-05-30 06:13:17 +0000",
            "updated":"2018-02-15 14:33:39 +0000",
            "createTimestamp":1464588797,
            "updateTimestamp":1518705219,
            "defaultCombinationId":0,
            "imageUrl":"https://dqzrr9k4bjpzk.cloudfront.net/images/1003/389587699.jpg",
            "thumbnailUrl":"https://dqzrr9k4bjpzk.cloudfront.net/images/1003/469445162.jpg",
            "smallThumbnailUrl":"https://dqzrr9k4bjpzk.cloudfront.net/images/1003/469445166.jpg",
            "hdThumbnailUrl":"https://dqzrr9k4bjpzk.cloudfront.net/images/1003/469445168.jpg",
            "originalImageUrl":"https://dqzrr9k4bjpzk.cloudfront.net/images/1003/389587700.jpg",
            "originalImage":{  
               "url":"https://dqzrr9k4bjpzk.cloudfront.net/images/1003/389587700.jpg",
               "width":257,
               "height":599
            },
            "galleryImages":[],
            "defaultCategoryId":0,
            "seoTitle":"",
            "seoDescription":"",
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
            "showOnFrontpage":4
         },
         {  
            "weight":0.0,
            "price":27.049999999999997,
            "amount":1,
            "productId":97548001,
            "name":"Testing",
            "categoryId":-1,
            "sku":"CP007C",
            "selectedOptions":[  
               {  
                  "name":"Size",
                  "value":"M",
                  "valuesArray":[  
                     "M"
                  ],
                  "type":"CHOICE",
                  "files":null
               }
            ],
            "dimensions":{  
               "length":0.0,
               "width":0.0,
               "height":0.0
            },
            "productPrice":23.0,
            "categoryIds":[],
            "categories":[],
            "quantity":null,
            "unlimited":true,
            "inStock":true,
            "priceInProductList":23.0,
            "isShippingRequired":true,
            "productClassId":0,
            "enabled":true,
            "warningLimit":0,
            "fixedShippingRateOnly":false,
            "fixedShippingRate":0.0,
            "options":[  
               {  
                  "type":"RADIO",
                  "name":"Size",
                  "defaultChoice":0,
                  "required":false,
                  "choices":[  
                     {  
                        "text":"S",
                        "priceModifier":0.0,
                        "priceModifierType":"PERCENT"
                     },
                     {  
                        "text":"M",
                        "priceModifier":1.0,
                        "priceModifierType":"PERCENT"
                     },
                     {  
                        "text":"L",
                        "priceModifier":2.0,
                        "priceModifierType":"PERCENT"
                     },
                     {  
                        "text":"XL",
                        "priceModifier":3.0,
                        "priceModifierType":"PERCENT"
                     }
                  ]
               }
            ],
            "wholesalePrices":null,
            "compareToPrice":null,
            "url":"https://mdemo.ecwid.com/#!/Testing/p/97548001",
            "created":"2017-12-07 06:12:20 +0000",
            "updated":"2018-02-15 14:33:39 +0000",
            "createTimestamp":1512627140,
            "updateTimestamp":1518705219,
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
            "seoTitle":"",
            "seoDescription":"",
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
            "showOnFrontpage":3
         }
      ],
      "shippingAddress":{  
         "street":"324",
         "city":"423",
         "countryCode":"RU",
         "postalCode":"324",
         "stateOrProvinceCode":"73",
         "stateOrProvinceName":"Ulyanovskaya oblast"
      },
      "originAddress":{  
         "street":"av 127",
         "city":"Bogota",
         "countryCode":"FI",
         "postalCode":"21293",
         "stateOrProvinceCode":"09"
      },
      "weight":0.0,
      "weightUnit":"kg",
      "dimensionUnit":"CM",
      "currency":"AUD",
      "predictedPackages":null,
      "paymentMethod":null,
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
merchantAppSettings | json | Merchant settings for your integration set up by your code. [More details](https://developers.ecwid.com/api-documentation/user-interface-for-discount-rules)
cart | \<*CartDetails*\> | Offset from the beginning of the returned items list (for paging)

#### CartDetails

Name | Type    | Description
---- | ------- | --------------
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order without taxes, but including product variations and options
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
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
items | Array\<*OrderItems*\> | Array of customer's order items with basic details
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
price | number | Price of ordered item in the cart. **Includes taxes in product price and product variations/options**
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
id | number | Internal unique coupon ID
name |  string | Coupon title in store control panel
code |  string | Coupon code
discountType | string | Discount type: `ABS`, `PERCENT`, `SHIPPING`, `ABS_AND_SHIPPING`, `PERCENT_AND_SHIPPING`
status | string | Discount coupon state: `ACTIVE`, `PAUSED`, `EXPIRED` or `USEDUP`
discount | number | Discount amount
launchDate | string | The date of coupon launch, e.g. `2014-06-06 08:00:00 +0000`
expirationDate | string | Coupon expiration date, e.g. `2014-06-06 08:00:00 +0000`
creationDate |  string | Coupon creation date. Format example: `2016-06-29 11:36:55 +0000`
updateDate | string | Coupon update date. Format example: `2016-06-29 11:36:55 +0000`
totalLimit | number| The minimum order subtotal the coupon applies to
usesLimit | string | Number of uses limitation: `UNLIMITED`, `ONCEPERCUSTOMER`, `SINGLE`
repeatCustomerOnly | boolean | Coupon usage limitation flag identifying whether the coupon works for all customers or only repeat customers
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | Products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<number\> | The list of product IDs the coupon can be applied to
categories | Array\<number\> | The list of category IDs the coupon can be applied to

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

#### Response

> Response to Ecwid example

```json
{
    "discounts": [
        {
          "value": 20,
          "type": "ABSOLUTE",
          "description": "T-shirt for free for orders over $100"
        },
        {
          "value": 10,
          "type": "PERCENT",
          "description": "10% off. Thanks for liking us on Facebook!"
        }
    ]
}
```

An array of JSON data of type 'CustomDiscounts' with the following fields:

#### CustomDiscounts

Name | Type    | Description
---- | ------- | --------------
**value** | number | Discount amount
type | number | Discount type: `ABSOLUTE` or `PERCENT`. Default is `ABSOLUTE`
description | string | Discount description

<aside class="notice">
Response parameters in <strong>bold</strong> are mandatory
</aside>

## User interface for discount rules

You can create a user interface for merchants to specify their own promo rule variations or any other user preferences you may require for your app. 

We recommend adding a new tab into the Ecwid Control Panel's promotion section for optimal experience using the: [Native applications](#native-applications) feature and [Application storage](#application-storage) features.

**Settings and User interface**

First, set up a new tab in Ecwid Control Panel, which will serve as a settings page for your users. This tab will load a page from your server in an iframe in a separate tab of Ecwid Control Panel. See [Native Applications](#native-applications) for more info.

**Request**

Once the settings are saved there, Ecwid will send them in a **POST request** to your application alongside cart details when customer is at checkout stage. The request will contain **all data** from your application storage, including public and other keys that were saved by the app. Use it to idetify the store and apply discounts accordingly. See [Application storage](#application-storage) for more info.

**Response**

After you get a request from Ecwid, your application endpoint should get its components and return discounts back to the customer in a response.
