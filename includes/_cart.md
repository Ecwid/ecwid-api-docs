## Carts

When a customer leaves an online store without making a purchase it is recorded as an abandoned cart in Ecwid. The methods below describe how you can search for abandoned carts, place them as completed orders or update its contents. Learn more about abandoned carts in the [Ecwid Help Center](https://support.ecwid.com/hc/en-us/articles/207806235-Orders#Unfinishedsales).

You can also calculate the order total and available shipping methods using the [special endpoint](https://developers.ecwid.com/api-documentation/carts#calculate-order-details) in the Ecwid API. It is useful for storefronts with a custom checkout process.

### Search abandoned carts

Search for abandoned carts in an Ecwid stores and filter the results by create/update date, customer, order total.

> Request example

```http
GET /api/v3/4870020/carts?customer=johnsmith@example.com&token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/carts?showHidden={showHidden}&totalFrom={totalFrom}&totalTo={totalTo}&createdFrom={createdFrom}&createdTo={createdTo}&updatedFrom={updatedFrom}&updatedTo={updatedTo}&couponCode={couponCode}&customer={customer}&offset={offset}&limit={limit}&token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
showHidden | boolean | If `true` Ecwid will show all abandoned carts found, even if store owner deleted them in the Ecwid Control Panel. If `false`, the response will provide abandoned carts with `hidden` field as `false` only. If not specified, the filter is set to `true`
totalFrom |  number | Minimum product price
totalTo | number | Maximum product price
createdFrom | string | Order placement date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul> Examples: <ul><li>`1447804800`</li> <li>`2015-04-22 18:48:38 -0500`</li> <li>`2015-04-22` (this is 2015-04-22 00:00:00 UTC)</li></ul>
createdTo | string | Order placement date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
updatedFrom | string | Order last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
updatedTo | string | Order last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
couponCode | number | The code of coupon applied to order
customer | string | Customer search term. Searches for customer details in order, **except for `customerId`**
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `100`

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (JSON)

```json
{
    "total": 1,
    "count": 1,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            // Basic information
            "cartId": "6626E60A-A6F9-4CD5-8230-43D5F162E0CD",
            "tax": 1.79,
            "subtotal": 29.95,
            "total": 37.39,
            "usdTotal": 37.39,
            "paymentMethod": "Purchase order",

            // Additional information
            "refererUrl": "http://mysuperstore.ecwid.com/",
            "globalReferer": "",
            "createDate": "2014-09-20 19:59:43 +0000",
            "updateDate": "2014-09-21 00:00:12 +0000",
            "createTimestamp": 1427268654,
            "updateTimestamp": 1427272209,
            "hidden": false,
            "orderComments": "Test order comments",

            // Basic customer information
            "email": "johnsmith@example.com",
            "ipAddress": "83.217.8.241",
            "customerId": 15319410,
            "customerGroupId": 12345,
            "customerGroup": "Gold",
            "customerTaxExempt": false,
            "customerTaxId": "",
            "customerTaxIdValid": false,
            "reversedTaxApplied": false,

            // Discounts in order
            "membershipBasedDiscount": 0,
            "totalAndMembershipBasedDiscount": 2.85,
            "couponDiscount": 1.5,
            "discount": 2.85,
            "volumeDiscount": 0,
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
                    "value": 10,
                    "type": "PERCENT",
                    "base": "ON_TOTAL_AND_MEMBERSHIP",
                    "orderTotal": 15
                },
                {
                    "value": 2,
                    "type": "ABSOLUTE",
                    "base": "CUSTOM",
                    "description": "Buy more than 3 cherries and get $2 off!"
                }
            ],

            // Order items details
            "items": [
                {
                    "id": 40989227,
                    "productId": 37208342,
                    "categoryId": 9691094,
                    "price": 5.99,
                    "productPrice": 5.99,
                    "weight": 0.32,
                    "sku": "00004",
                    "quantity": 5,
                    "shortDescription": "Cherry\nThe word cherry refers to a fleshy fruit (drupe) that contains a single stony seed. The cherry belongs to the fa...",
                    "tax": 1.79,
                    "shipping": 10,
                    "quantityInStock": 1981,
                    "name": "Cherry",
                    "isShippingRequired": true,
                    "trackQuantity": true,
                    "fixedShippingRateOnly": false,
                    "imageUrl": "http://app.ecwid.com/default-store/00006-sq.jpg",
                    "fixedShippingRate": 1,
                    "digital": true,
                    "productAvailable": true,
                    "couponApplied": false,
                    "files": [
                        {
                            "productFileId": 7215101,
                            "maxDownloads": 0,
                            "remainingDownloads": 0,
                            "expire": "2014-10-26 20:34:34 +0000",
                            "name": "myfile.jpg",
                            "description": "Sunflower",
                            "size": 54492,
                            "adminUrl": "https://app.ecwid.com/api/v3/4870020/products/37208340/files/7215101?token=123123123",
                            "customerUrl": "http://mysuperstore.ecwid.com/download/4870020/a2678e7d1d1c557c804c37e4/myfile.jpg"
                        }
                    ],
                    "selectedOptions": [
                        {
                            "name": "Size",
                            "value": "Big",
                            "valuesArray" : [
                              "Big"
                            ],
                            "type": "CHOICE"
                        },
                        {
                            "name": "Attach a file",
                            "type": "FILES",
                            "files": [
                                {
                                    "id": 5973037,
                                    "name": "makfruit_ava_sunnyflower_200_200.jpg",
                                    "size": 54492,
                                    "url": "https://app.ecwid.com/orderfile/4870020/5973037/54492/makfruit_ava_sunnyflower_200_200.jpg"
                                }
                            ]
                        },
                        {
                            "name": "Choose date",
                            "value": "2014-09-10",
                            "type": "DATE"
                        },
                        {
                            "name": "Any text",
                            "value": "Test text",
                            "type": "TEXT"
                        }
                    ],
                    "taxes": [
                        {
                            "name": "Tax X",
                            "value": 7,
                            "total": 1.79,
                            "taxOnDiscountedSubtotal": 1.79,
                            "taxOnShipping": 0
                        }
                    ],
                    "productDimensions": {
                        "length": 34,
                        "width": 3,
                        "height": 22
                    }
                }
            ],

            // Customer addresses
            "billingPerson": {
                "name": "John Smith",
                "companyName": "Unreal Company",
                "street": "W 3d st",
                "city": "New York",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "10001",
                "stateOrProvinceCode": "NY",
                "stateOrProvinceName": "New York",
                "phone": "+1234567890"
            },
            "shippingPerson": {
                "name": "John Smith",
                "companyName": "Unreal Company",
                "street": "W 3d st",
                "city": "New York",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "10001",
                "stateOrProvinceCode": "NY",
                "stateOrProvinceName": "New York",
                "phone": "+1234567890"
            },

            // Shipping information
            "shippingOption": {
                "shippingMethodName": "2nd day delivery",
                "shippingRate": 10,
                "estimatedTransitTime": "5",
                "isPickup": false,
                "pickupInstruction": ""
            },
            "handlingFee": {
                "name": "Wrapping",
                "value": 2,
                "description": "Silk paper wrapping"
            },

            // Other information
            "additionalInfo": {},
            "paymentParams": {
                "Company name": "Unreal Company",
                "Job position": "Manager",
                "PO number": "123abcd",
                "Buyer's full name": "John Smith"
            },
            "recovered_order_id": 1231245,
            "recovery_email_sent_timestamp": "1427272209"
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
items | Array\<*OrderEntry*\> | The items list

#### OrderEntry
Field | Type |  Description
------| -----| ------------
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order
total | number | Order total cost. Includes shipping, taxes, discounts, etc.
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total
customerTaxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)
customerTaxId | string | Customer tax ID
customerTaxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
reversedTaxApplied | boolean | `true` if order tax was set to 0 because customer specified their valid tax ID in checkout process. `false` otherwise
ipAddress | string  | Customer IP
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Order comments
couponDiscount | number | Discount applied to order using a coupon
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unsfinished orders only.
usdTotal | number | Order total in USD
globalReferer | string | URL that the customer came to the store from
createDate | date | The date/time of order placement, e.g `2014-06-06 18:57:19 +0000`
updateDate | date | The date/time of the last order change, e.g `2014-06-06 18:57:19 +0000`
createTimestamp | number | The date of order placement in UNIX Timestamp format, e.g `1427268654`
updateTimestamp | number | The date of the last order change in UNIX Timestamp format, e.g `1427268654`
customerGroupId | number | Customer group ID
customerGroup | string | The name of group (membership) the customer belongs to
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<string,string\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
recovered_order_id | string | Internal order ID. Is present if the abandoned cart was successfully recovered
recovery_email_sent_timestamp | string | UNIX timestamp of a date when abandoned cart recovery email was sent to customer

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
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item
files | Array\<*OrderItemProductFile*\> | Files attached to the order item
productDimensions | \<ProductDimensions\> | Product dimensions info

#### OrderItemTax
Field | Type | Description
----- | -----| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
taxOnDiscountedSubtotal | number |  Tax on item subtotal (after applying discounts)
taxOnShipping | number | Tax on item shipping

#### OrderItemProductFile
Field | Type | Description
----- | -----| -----------
productFileId | number | Internal unique file ID
maxDownloads | number | Max allowed number of file downloads. See [E-goods article](http://help.ecwid.com/customer/portal/articles/1163931?q=egoods) in Ecwid Help center for the details
remainingDownloads | number | Remaining number of download attempts
expire | string | Date/time of the customer download link expiration
name |  string |  File name
description | string |  File description defined by the store administrator
size |  number |  File size, bytes (64-bit integer)
adminUrl | string | Link to the file. Be careful: the link contains the API access token. Make sure you do not display the link as is in your application and not give it to a customer.
customerUrl | string | File download link that is sent to the customer when the order is paid

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

#### ProductDimensions
Field | Type  | Description
-------------- | -------------- | --------------
length | number | Length of a product
width | number | Width of a product
height | number | Height of a product

#### PersonInfo
Field | Type |  Description
--------- | -----------| -----------
name |  string  | Full name
companyName |   string  | Company name
street |    string  | Address line 1 and address line 2, separated by '\n'
city |  string  | City
countryCode | string  | Two-letter country code
countryName | string | Country name
postalCode | string  | Postal/ZIP code
stateOrProvinceCode |   string  | State code, e.g. `NY`
stateOrProvinceName | string | State/province name
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
shippingCarrierName | string | Optional. Is present for orders made with carriers, e.g. `USPS` or shipping applications.
shippingMethodName | string | Shipping option name
shippingRate | number | Rate
estimatedTransitTime | string | Delivery time estimation. Possible formats: number "5", several days estimate "4-9"
isPickup | boolean | `true` if selected shipping option is local pickup. `false` otherwise
pickupInstruction | string | Instruction for customer on how to receive their products

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
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
order_total | number | Minimum order subtotal the discount applies to
description | string | Description of a discount (for discounts with base == `CUSTOM`)

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

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
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Get abandoned cart

Get details of an abandoned cart using its unique cart ID.

> Request example

```http
GET /api/v3/4870020/carts/6626E60A-A6F9-4CD5-8230-43D5F162E0CD?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/carts/{cartId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**cartId** | string | Unique abandoned cart identifier. To get a correct response, the value must completely match the record in Ecwid

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (JSON)

```json
{
    "total": 1,
    "count": 1,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            // Basic information
            "cartId": "6626E60A-A6F9-4CD5-8230-43D5F162E0CD",
            "tax": 1.79,
            "subtotal": 29.95,
            "total": 37.39,
            "usdTotal": 37.39,
            "paymentMethod": "Purchase order",

            // Additional information
            "refererUrl": "http://mysuperstore.ecwid.com/",
            "globalReferer": "",
            "createDate": "2014-09-20 19:59:43 +0000",
            "updateDate": "2014-09-21 00:00:12 +0000",
            "createTimestamp": 1427268654,
            "updateTimestamp": 1427272209,
            "hidden": false,
            "orderComments": "Test order comments",

            // Basic customer information
            "email": "johnsmith@example.com",
            "ipAddress": "83.217.8.241",
            "customerId": 15319410,
            "customerGroupId": 12345,
            "customerGroup": "Gold",
            "customerTaxExempt": false,
            "customerTaxId": "",
            "customerTaxIdValid": false,
            "reversedTaxApplied": false,

            // Discounts in order
            "membershipBasedDiscount": 0,
            "totalAndMembershipBasedDiscount": 2.85,
            "couponDiscount": 1.5,
            "discount": 2.85,
            "volumeDiscount": 0,
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
                    "value": 10,
                    "type": "PERCENT",
                    "base": "ON_TOTAL_AND_MEMBERSHIP",
                    "orderTotal": 15
                },
                {
                    "value": 2,
                    "type": "ABSOLUTE",
                    "base": "CUSTOM",
                    "description": "Buy more than 3 cherries and get $2 off!"
                }
            ],

            // Order items details
            "items": [
                {
                    "id": 40989227,
                    "productId": 37208342,
                    "categoryId": 9691094,
                    "price": 5.99,
                    "productPrice": 5.99,
                    "weight": 0.32,
                    "sku": "00004",
                    "quantity": 5,
                    "shortDescription": "Cherry\nThe word cherry refers to a fleshy fruit (drupe) that contains a single stony seed. The cherry belongs to the fa...",
                    "tax": 1.79,
                    "shipping": 10,
                    "quantityInStock": 1981,
                    "name": "Cherry",
                    "isShippingRequired": true,
                    "trackQuantity": true,
                    "fixedShippingRateOnly": false,
                    "imageUrl": "http://app.ecwid.com/default-store/00006-sq.jpg",
                    "fixedShippingRate": 1,
                    "digital": true,
                    "productAvailable": true,
                    "couponApplied": false,
                    "files": [
                        {
                            "productFileId": 7215101,
                            "maxDownloads": 0,
                            "remainingDownloads": 0,
                            "expire": "2014-10-26 20:34:34 +0000",
                            "name": "myfile.jpg",
                            "description": "Sunflower",
                            "size": 54492,
                            "adminUrl": "https://app.ecwid.com/api/v3/4870020/products/37208340/files/7215101?token=123123123",
                            "customerUrl": "http://mysuperstore.ecwid.com/download/4870020/a2678e7d1d1c557c804c37e4/myfile.jpg"
                        }
                    ],
                    "selectedOptions": [
                        {
                            "name": "Size",
                            "value": "Big",
                            "valuesArray" : [
                              "Big"
                            ],
                            "type": "CHOICE"
                        },
                        {
                            "name": "Attach a file",
                            "type": "FILES",
                            "files": [
                                {
                                    "id": 5973037,
                                    "name": "makfruit_ava_sunnyflower_200_200.jpg",
                                    "size": 54492,
                                    "url": "https://app.ecwid.com/orderfile/4870020/5973037/54492/makfruit_ava_sunnyflower_200_200.jpg"
                                }
                            ]
                        },
                        {
                            "name": "Choose date",
                            "value": "2014-09-10",
                            "type": "DATE"
                        },
                        {
                            "name": "Any text",
                            "value": "Test text",
                            "type": "TEXT"
                        }
                    ],
                    "taxes": [
                        {
                            "name": "Tax X",
                            "value": 7,
                            "total": 1.79,
                            "taxOnDiscountedSubtotal": 1.79,
                            "taxOnShipping": 0
                        }
                    ],
                    "productDimensions": {
                        "length": 34,
                        "width": 3,
                        "height": 22
                    }
                }
            ],

            // Customer addresses
            "billingPerson": {
                "name": "John Smith",
                "companyName": "Unreal Company",
                "street": "W 3d st",
                "city": "New York",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "10001",
                "stateOrProvinceCode": "NY",
                "stateOrProvinceName": "New York",
                "phone": "+1234567890"
            },
            "shippingPerson": {
                "name": "John Smith",
                "companyName": "Unreal Company",
                "street": "W 3d st",
                "city": "New York",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "10001",
                "stateOrProvinceCode": "NY",
                "stateOrProvinceName": "New York",
                "phone": "+1234567890"
            },

            // Shipping information
            "shippingOption": {
                "shippingMethodName": "2nd day delivery",
                "shippingRate": 10,
                "estimatedTransitTime": "5",
                "isPickup": false,
                "pickupInstruction": ""
            },
            "handlingFee": {
                "name": "Wrapping",
                "value": 2,
                "description": "Silk paper wrapping"
            },

            // Other information
            "additionalInfo": {},
            "paymentParams": {
                "Company name": "Unreal Company",
                "Job position": "Manager",
                "PO number": "123abcd",
                "Buyer's full name": "John Smith"
            },
            "recovered_order_id": 1231245,
            "recovery_email_sent_timestamp": "1427272209"
        }
    ]
}
```

A JSON object of type 'OrderEntry' with the following fields:

#### OrderEntry
Field | Type |  Description
------| -----| ------------
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order
total | number | Order total cost. Includes shipping, taxes, discounts, etc.
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total
customerTaxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)
customerTaxId | string | Customer tax ID
customerTaxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
reversedTaxApplied | boolean | `true` if order tax was set to 0 because customer specified their valid tax ID in checkout process. `false` otherwise
ipAddress | string  | Customer IP
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Order comments
couponDiscount | number | Discount applied to order using a coupon
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unsfinished orders only.
usdTotal | number | Order total in USD
globalReferer | string | URL that the customer came to the store from
createDate | date | The date/time of order placement, e.g `2014-06-06 18:57:19 +0000`
updateDate | date | The date/time of the last order change, e.g `2014-06-06 18:57:19 +0000`
createTimestamp | number | The date of order placement in UNIX Timestamp format, e.g `1427268654`
updateTimestamp | number | The date of the last order change in UNIX Timestamp format, e.g `1427268654`
customerGroupId | number | Customer group ID
customerGroup | string | The name of group (membership) the customer belongs to
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<string,string\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
recovered_order_id | string | Internal order ID. Is present if the abandoned cart was successfully recovered
recovery_email_sent_timestamp | string | UNIX timestamp of a date when abandoned cart recovery email was sent to customer

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
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item
files | Array\<*OrderItemProductFile*\> | Files attached to the order item
productDimensions | \<ProductDimensions\> | Product dimensions info

#### OrderItemTax
Field | Type | Description
----- | -----| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
taxOnDiscountedSubtotal | number |  Tax on item subtotal (after applying discounts)
taxOnShipping | number | Tax on item shipping

#### OrderItemProductFile
Field | Type | Description
----- | -----| -----------
productFileId | number | Internal unique file ID
maxDownloads | number | Max allowed number of file downloads. See [E-goods article](http://help.ecwid.com/customer/portal/articles/1163931?q=egoods) in Ecwid Help center for the details
remainingDownloads | number | Remaining number of download attempts
expire | string | Date/time of the customer download link expiration
name |  string |  File name
description | string |  File description defined by the store administrator
size |  number |  File size, bytes (64-bit integer)
adminUrl | string | Link to the file. Be careful: the link contains the API access token. Make sure you do not display the link as is in your application and not give it to a customer.
customerUrl | string | File download link that is sent to the customer when the order is paid

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

#### ProductDimensions
Field | Type  | Description
-------------- | -------------- | --------------
length | number | Length of a product
width | number | Width of a product
height | number | Height of a product

#### PersonInfo
Field | Type |  Description
--------- | -----------| -----------
name |  string  | Full name
companyName |   string  | Company name
street |    string  | Address line 1 and address line 2, separated by '\n'
city |  string  | City
countryCode | string  | Two-letter country code
countryName | string | Country name
postalCode | string  | Postal/ZIP code
stateOrProvinceCode |   string  | State code, e.g. `NY`
stateOrProvinceName | string | State/province name
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
shippingCarrierName | string | Optional. Is present for orders made with carriers, e.g. `USPS` or shipping applications.
shippingMethodName | string | Shipping option name
shippingRate | number | Rate
estimatedTransitTime | string | Delivery time estimation. Possible formats: number "5", several days estimate "4-9"
isPickup | boolean | `true` if selected shipping option is local pickup. `false` otherwise
pickupInstruction | string | Instruction for customer on how to receive their products

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
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
order_total | number | Minimum order subtotal the discount applies to
description | string | Description of a discount (for discounts with base == `CUSTOM`)

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

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
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Update abandoned cart

Update the details of specific abandoned cart using its unique cart ID.

> Request example

```http
PUT /api/v3/4870020/carts/6626E60A-A6F9-4CD5-8230-43D5F162E0CD?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`PUT https://app.ecwid.com/api/v3/{storeId}/carts/{cartId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**cartId** | string | Unique abandoned cart identifier

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (JSON)

```json
{
  "hidden": true
}
```

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.

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
400 | Request parameters are malformed
404 | The order is not found
405 | Method not allowed. Can occur when using `POST` instead of `PUT` HTTP request method
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Calculate order details

This method will calculate and return shipping rates and taxes for the order sent in a request. You can use it for your custom checkout process prior to creating an actual order in the store. Requests to this endpoint don't create any new orders in the actual store. See [Shipping](https://help.ecwid.com/customer/en/portal/articles/1161340-shipping) and [Taxes](https://help.ecwid.com/customer/en/portal/articles/1182159-taxes) help articles to find out more information.

> Request example

```http
POST /api/v3/4870020/order/calculate?token=1234567890qwqeertt HTTP/1.1
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
                "name": "Cherry", 
                "dimensions": {
                    "length": 5,
                    "width": 4,
                    "height": 6
                }
            },
            {
                "price": 4.22,
                "weight": 0.12,
                "sku": "00014",
                "quantity": 2,
                "isShippingRequired": true,
                "name": "Apple",
                "dimensions": {
                    "length": 9,
                    "width": 8,
                    "height": 10
                }
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
**token** |  string | oAuth token

#### Request body

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


#### Response

> Response example

```json
{
  "subtotal": 38.44,
  "total": 3093.93,
  "tax": 3.87,
  "couponDiscount": 0,
  "paymentStatus": "INCOMPLETE",
  "fulfillmentStatus": "AWAITING_PROCESSING",
  "volumeDiscount": 15,
  "membershipBasedDiscount": 0.77,
  "totalAndMembershipBasedDiscount": 0,
  "discount": 20,
  "usdTotal": 0,
  "createDate": "2016-02-25 15:12:14 +0000",
  "createTimestamp": 1456413134,
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
  "availableShippingOptions": [
    {
      "shippingCarrierName": "U.S.P.S.",
      "shippingMethodName": "U.S.P.S. Priority Mail Express 2-Day™",
      "shippingRate": 3071.62,
      "estimatedTransitTime": "1"
    },
    {
      "shippingCarrierName": "U.S.P.S.",
      "shippingMethodName": "U.S.P.S. Priority Mail Express 2-Day™ Hold For Pickup",
      "shippingRate": 3071.62,
      "estimatedTransitTime": "1"
    },
    {
      "shippingCarrierName": "U.S.P.S.",
      "shippingMethodName": "U.S.P.S. Library Mail Parcel",
      "shippingRate": 234.87,
      "estimatedTransitTime": "2-9"
    },
    {
      "shippingCarrierName": "U.S.P.S.",
      "shippingMethodName": "U.S.P.S. Media Mail Parcel",
      "shippingRate": 246.34,
      "estimatedTransitTime": "2-9"
    },
    {
      "shippingMethodName": "Fixed rate",
      "shippingRate": 10
    },
    {
      "shippingMethodName": "Local store pickup",
      "isPickup": true,
      "pickupInstruction": "Bring your receipt and order number."
    }
  ],
  "availableTaxes": [
    {
      "id": 1939536453,
      "name": "VAT",
      "enabled": true,
      "includeInPrice": true,
      "useShippingAddress": true,
      "taxShipping": false,
      "appliedByDefault": true,
      "defaultTax": 21,
      "rules": [
        {
          "zoneId": "3772-1438789680791",
          "tax": 21
        }
      ]
    },
    {
      "id": 1117939047,
      "name": "World tax",
      "enabled": false,
      "includeInPrice": false,
      "useShippingAddress": true,
      "taxShipping": false,
      "appliedByDefault": true,
      "defaultTax": 5,
      "rules": [
        {
          "zoneId": "7715-1423477610739",
          "tax": 5
        }
      ]
    }
  ],
  "handlingFee": {
    "name": "Handling Fee",
    "value": 0,
    "description": ""
  },
  "predictedPackage": [
    {
        "length": 17.779,
        "width": 25.4,
        "height": 10.16,
        "weight": 0.88,
        "declaredValue": 3.37
    }
  ],
  "additionalInfo": {},
  "paymentParams": {},
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
      "value": 10,
      "type": "ABSOLUTE",
      "base": "CUSTOM",
      "description": "Buy one get one free for T-shirts"
    }
  ],
  "hidden": false
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
paymentStatus | string |    Payment status, will always be returned as `INCOMPLETE`
fulfillmentStatus | string |    Fulfilment status, will always be returned as `AWAITING_PROCESSING`
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
usdTotal | number | Order total in USD
createDate | date |  The date/time of order placement, e.g `2014-06-06 18:57:19 +0000`
createTimestamp | number | The date of order placement in UNIX Timestamp format, e.g `1427268654`
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
availableShippingOptions | Array\<*ShippingOptionInfo*\> | All calculated shipping methods for this order
availableTaxes | Array\<*TaxInfo*\> | All calculated taxes for this order 
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
predictedPackage | Array\<*PredictedPackage*\> | Predicted information about the package to ship items in to customer
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<string,string\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
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
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item
dimensions | \<ProductDimensions\> | Product dimensions info

#### OrderItemTax
Field | Type | Description
----- | -----| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
taxOnDiscountedSubtotal | number |  Tax on item subtotal (after applying discounts)
taxOnShipping | number | Tax on item shipping

#### ProductDimensions
Field | Type  | Description
-------------- | -------------- | --------------
length | number | Length of a product
width | number | Width of a product
height | number | Height of a product

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
isPickup | boolean | `true` if selected shipping option is local pickup. `false` otherwise
pickupInstruction | string | Instruction for customer on how to receive their products

#### TaxInfo

Field | Type | Description
----- | ---- | -----------
id | number | Unique internal ID of the tax
name |  string | Displayed tax name
enabled | boolean | Whether tax is enabled `true` / `false` 
includeInPrice | boolean | `true` if the tax rate is included in product prices. More details: [Taxes in Ecwid](http://help.ecwid.com/customer/portal/articles/1182159-taxes)
useShippingAddress | boolean | `true` if the tax is calculated based on shipping address, `false` if billing address is used
taxShipping | boolean | `true` is the tax applies to subtotal+shipping cost . `false` if the tax is applied to subtotal only
appliedByDefault | boolean | `true` if the tax is applied to all products. `false` is the tax is only applied to thos product that have this tax enabled
defaultTax | number | Tax value, in %, when none of the destination zones match
rules | Array\<TaxRule\>  | Tax rates

#### TaxRule
Field | Type | Description
----- | ---- | -----------
zoneId | string | Destination zone ID
tax | number | Tax rate for this zone in %

#### HandlingFeeInfo
Field | Type | Description
----- | ---- | -----------
name | string | Handling fee name set by store admin. E.g. `Wrapping`
value | number | Handling fee value
description | string | Handling fee description for customer

### PredictedPackage
Name | Type    | Description
---- | ------- | --------------
height | number | Height of a predicted package
width | number | Width of a predicted package
length | number | Length of a predicted package
weight | number | Total weight of a predicted package
declaredValue | number | Declared value of a predicted package (subtotal of items in package)

#### DiscountInfo
Field | Type | Description
----- | ---- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
order_total | number | Minimum order subtotal the discount applies to
description | string | Description of a discount (for discounts with base == `CUSTOM`)

#### Errors

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
415 | Unsupported content-type: expected `application/json` or `text/json`
422 | No order items or incorrect items details sent
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Convert abandoned cart to order

> Request example

```http
POST /api/v3/4870020/carts/6626E60A-A6F9-4CD5-8230-43D5F162E0CD/place?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

```

Converts the abandoned cart into a completed order in an Ecwid store.

`POST https://app.ecwid.com/api/v3/{storeId}/carts/{cartID}/place?token={token}`

#### Request body

Request body is empty for this endpoint.

#### Response

> Response example (JSON)

```json
{
  "orderNumber": 829,
  "vendorOrderNumber": "000008292017"
}
```

A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus

Field | Type |  Description
-------------- | -------------- | --------------
orderNumber | number | Unique order number without prefixes/suffixes, e.g. `34`
vendorOrderNumber |  string | Order number with prefix and suffix defined by admin, e.g. `ABC34-q`

#### Errors

> Error response example

```http
HTTP/1.1 400 Field OrderItem.quantity is absent
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are invalid
404 | The customer or any other linked object is not found in the store
500 | Cannot create an order because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
