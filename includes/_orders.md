# Orders

## Search orders

> Request example

```http
GET /api/v3/4870020/orders?customer=johnsmith@example.com&paymentStatus=PAID,AWAITING_PAYMENT&token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/orders?keywords={keywords}&totalFrom={totalFrom}&totalTo={totalTo}&createdFrom={createdFrom}&createdTo={createdTo}&updatedFrom={updatedFrom}&updatedTo={updatedTo}&couponCode={couponCode}&orderNumber={orderNumber}&vendorOrderNumber={vendorOrderNumber}&customer={customer}&paymentMethod={paymentMethod}&shippingMethod={shippingMethod}&paymentStatus={paymentStatus}&fulfillmentStatus={fulfillmentStatus}&offset={offset}&limit={limit}&token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
keywords |  string | Search term. Ecwid will look for this term in order number, ordered items and customer details. 
couponCode | number | The code of coupon applied to order
totalFrom |  number | Minimum product price
totalTo | number | Maximum product price
orderNumber | number | Order number
vendorOrderNumber | string | Order number with prefix/suffix defined by admin
customer | string | Customer search term. Searches for customer details in order, **except for `customerId`**
createdFrom | string | Order placement date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul> Examples: <ul><li>`1447804800`</li> <li>`2015-04-22 18:48:38 -0500`</li> <li>`2015-04-22` (this is 2015-04-22 00:00:00 UTC)</li></ul>
createdTo | string | Order placement date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
updatedFrom | string | Order last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
updatedTo | string | Order last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
paymentMethod | string | Payment method used by customer
shippingMethod | string | Shipping method chosen by customer
paymentStatus | string | Comma separated list of order payment statuses to search. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string | Comma separated list of order fulfilment statuses to search. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li></ul>

<aside class="notice">
If no filters are set in the URL, API will return all orders <strong>except for unfinished orders</strong>. To get unfinished orders, use <i>INCOMPLETE</i> value for <strong>paymentStatus</strong> parameter.
</aside>

<aside class="notice">
Parameters in bold are mandatory
</aside>

### Response

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
            "vendorOrderNumber": "20",
            "orderNumber": 20,
            "tax": 1.79,
            "subtotal": 29.95,
            "total": 37.39,
            "usdTotal": 37.39,
            "paymentMethod": "Purchase order",
            "paymentStatus": "PAID",
            "fulfillmentStatus": "AWAITING_PROCESSING",
            // Additional information
            "refererUrl": "http://mysuperstore.ecwid.com/",
            "globalReferer": "",
            "createDate": "2014-09-20 19:59:43 +0000",
            "updateDate": "2014-09-21 00:00:12 +0000",
            "createTimestamp": 1427268654,
            "updateTimestamp": 1427272209,
            "hidden": false,
            "orderComments": "Test order comments",
            "privateAdminNotes": "Must be delivered till Sunday.",
            // Basic customer information
            "email": "johnsmith@example.com",
            "ipAddress": "83.217.8.241",
            "customerId": 15319410,
            "customerGroupId": 12345,
            "customerGroup": "Gold",
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
                    ]
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
                "estimatedTransitTime": "5"
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
items | Array\<*OrderEntry*\> | The items list

#### OrderEntry
Field | Type |  Description
------| -----| ------------
orderNumber | number | Unique order number without prefixes/suffixes, e.g. `34`
vendorOrderNumber |  string | Order number with prefix and suffix defined by admin, e.g. `ABC34-q`
subtotal |  number | Order subtotal
total | number | Order total cost
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total
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
privateAdminNotes | string | Private note about the order from store owner

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

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

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
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Get order details

> Request example

```http
GET /api/v3/4870020/orders/20?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**orderNumber** | number | Order number

<aside class="notice">
Parameters in bold are mandatory
</aside>

### Response

> Response example (JSON)

```json
{
    // Basic information
    "vendorOrderNumber": "20",
    "orderNumber": 20,
    "tax": 1.79,
    "subtotal": 29.95,
    "total": 37.39,
    "usdTotal": 37.39,
    "paymentMethod": "Purchase order",
    "paymentStatus": "PAID",
    "fulfillmentStatus": "AWAITING_PROCESSING",
    // Additional information
    "refererUrl": "http://mysuperstore.ecwid.com/",
    "globalReferer": "",
    "createDate": "2014-09-20 19:59:43 +0000",
    "updateDate": "2014-09-21 00:00:12 +0000",
    "createTimestamp": 1427268654,
    "updateTimestamp": 1427272209,
    "hidden": false,
    "orderComments": "Test order comments",
    "privateAdminNotes": "Must be delivered till Sunday.",
    // Basic customer information
    "email": "johnsmith@example.com",
    "ipAddress": "83.217.8.241",
    "customerId": 15319410,
    "customerGroupId": 12345,
    "customerGroup": "Gold",
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
            ]
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
        "estimatedTransitTime": "5"
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
    }
}
```

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
orderNumber | number | Unique order number without prefixes/suffixes, e.g. `34`
vendorOrderNumber |  string | Order number with prefix and suffix defined by admin, e.g. `ABC34-q`
subtotal |  number | Order subtotal
total | number | Order total cost
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
paymentStatus | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string |    Fulfilment status. Supported values: <nobr><ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li></ul></nobr>
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Order comments
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
usdTotal | number | Order total in USD
globalReferer | string | URL that the customer came to the store from
createDate | date |  The date/time of order placement, e.g `2014-06-06 18:57:19 +0000`
updateDate | date |  The date/time of the last order change, e.g `2014-06-06 18:57:19 +0000`
createTimestamp | number | The date of order placement in UNIX Timestamp format, e.g `1427268654`
updateTimestamp | number | The date of the last order change in UNIX Timestamp format, e.g `1427268654`
customerGroupId | number | Customer group ID
customerGroup | string | The name of group (membership) the customer belongs to
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<string,string\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
privateAdminNotes | string | Private note about the order from store owner

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

#### OrderItemTax
Field | Type |  Description
--------- | -----------| -----------
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

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

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
404 | The order is not found
405 | Method not allowed. Can occur when using `POST` instead of `PUT` HTTP request method
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

## Update order

This request allows you to update existing orders in the store. When updating order information, you can omit unchanged fields – they will be ignored so the resulting order will keep the corresponding information unchanged. However, please mind that if you want to update the ordered items, you should submit all the items in the request. The omitted items will be removed. This is done this way to let you remove some purchased items from the order. 

> Request example

```http
PUT /api/v3/4870020/orders/20?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
        "subtotal": 35,
        "total": 45,
        "email": "newemail@example.com",
        "paymentMethod": "New payment method",
        "tax": 0,
        "paymentStatus": "CANCELLED",
        "fulfillmentStatus": "PROCESSING",
        "billingPerson": {
            "name": "Eugene K",
            "companyName": "Hedgehog and Bucket",
            "street": "My Street",
            "city": "San Diego",
            "countryCode": "US",
            "postalCode": "90002",
            "stateOrProvinceCode": "CA",
            "phone": "123141321"
        },
        "shippingPerson": {
            "name": "Eugene K",
            "companyName": "Hedgehog and Bucket",
            "street": "My Street",
            "city": "San Diego",
            "countryCode": "US",
            "postalCode": "90002",
            "stateOrProvinceCode": "CA",
            "phone": "123141321"
        },
        "shippingOption": {
            "shippingMethodName": "Fast Delivery",
            "shippingRate": 10
        },
        "items": [
            {
                "id": 41056225,
                "productId": 37208346,
                "price": 7.99,
                "productPrice": 7.99,
                "weight": 0.31,
                "sku": "00001",
                "quantity": 5,
                "shortDescription": "New description",
                "tax": 1.79,
                "shipping": 10,
                "quantityInStock": 1981,
                "name": "New name",
                "isShippingRequired": true,
                "fixedShippingRateOnly": false,
                "productAvailable": true,
                "selectedOptions": [
                    {
                        "name": "Size",
                        "value": "Big",
                        "type": "CHOICE"
                    },                  
                    {
                        "name": "Any text",
                        "value": "Test text",
                        "type": "TEXT"
                    }
                ],
                "taxes": [
                    {
                        "name": "Tax Y",
                        "value": 7,
                        "total": 4.79
                    }
                ]
            }
        ],
        "hidden": false,
        "privateAdminNotes": "Must be delivered till Sunday."
    }
```

`PUT https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**orderNumber** | number | Order number

### Request body

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
subtotal |  number | Order subtotal
total | number | Order total cost
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
paymentStatus | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li></ul>
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Order comments
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
globalReferer | string | URL that the customer came to the store from
customerGroup | string | The name of group (membership) the customer belongs to
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<string,string\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
privateAdminNotes | string | Private note about the order from store owner

#### OrderItem
Field | Type |  Description
--------- | -----------| -----------
**id** | number | Order item ID. 
quantity |  number | Amount purchased
name |  string | Product name
productId | number | Store product ID
categoryId |  number  | ID of the category this product belongs to. If the product belongs to many categories, categoryID will return the ID of the default product category. If the product doesn't belong to any category, `0` is returned
price | number | Price of ordered item in the cart
productPrice | number | Basic product price without options markups, wholesale discounts etc.
weight |  number | Product weight
sku |   string | Product/Combination SKU
shortDescription | string | Product description truncated to 120 characters
tax | number | Tax amount applied to the item
shipping | number| Order item shipping cost 
quantityInStock | number | The number of products in stock in the store
isShippingRequired | boolean | `true`/`false`: shows whether the item requires shipping
trackQuantity | boolean | `true`/`false`: shows whether the store admin set to track the quantity of this product and get low stock notifications
fixedShippingRateOnly | boolean | `true`/`false`: shows whether the fixed shipping rate is set for the product
fixedShippingRate | number| Fixed shipping rate for the product
digital | boolean | `true`/`false`: shows whether the item has downloadable files attached
productAvailable | boolean | `true`/`false`: shows whether the product is available in the store
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item

#### OrderItemTax
Field | Type |  Description
--------- | -----------| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item

#### OrderItemOption
Field | Type |  Description
--------- | -----------| -----------
**name** |  string | Option name
type |  string | Option type. One of: <ul><li>`CHOICE` (dropdown or radio button)</li><li>`CHOICES` (checkboxes)</li><li>`TEXT` (text input and text area)</li><li>`DATE` (date/time)</li><li>`FILES` (upload file option)</li></ul>
**value** | string | Selected/entered value by customer. Multiple values separated by comma in a single string
files | Array\<*OrderItemOptionFile*\> | Attached files (if the option type is `FILES`)

#### PersonInfo
Field | Type |  Description
--------- | -----------| -----------
name |  string  | Full name
companyName |   string  | Company name
street |    string  | Address line 1 and address line 2, separated by '\n'
city |  string  | City
countryCode | string  | Two-letter country code
postalCode | string  | Postal/ZIP code
stateOrProvinceCode |   string  | State code, e.g. `NY`
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
estimatedTransitTime | string | Delivery time estimation. Formats accepted: number "5", several days estimate "4-9"

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

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

### Response


> Response example (JSON)

```json
{
    "updateCount": 1
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus

Field | Type |  Description
-------------- | -------------- | --------------
updateCount | number | The number of updated orders (`1` or `0` depending on whether the update was successful)

### Errors

> Error response example

```http
HTTP/1.1 400 Status QUEUED is deprecated, use AWAITING_PAYMENT instead
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are invalid
404 | The order is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot update the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Delete order

> Request example

```http
DELETE /api/v3/4870020/orders/20?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**orderNumber** | number | Order number
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
deleteCount | number | The number of deleted orders (`1` or `0` depending on whether the request was successful)


### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description.

#### HTTP codes

HTTP Status | Meaning
-------------- | --------------
400 | Request parameters are malformed
404 | The order with given number is not found
500 | The delete request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

## Create order

> Request example

```http
POST /api/v3/4870020/orders?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
        "subtotal": 30,
        "total": 40,
        "email": "example@example.com",
        "paymentMethod": "Phone order",
        "tax": 0,
        "paymentStatus": "PAID",
        "fulfillmentStatus": "AWAITING_PROCESSING",
        "createDate": "2015-09-20 19:59:43 +0000",
        "items": [
            {
                "price": 15,
                "weight": 0.32,
                "sku": "00004",
                "quantity": 2,
                "name": "Cherry"
            }
        ],
        "billingPerson": {
            "name": "Eugene K",
            "companyName": "Hedgehog and Bucket",
            "street": "My Street",
            "city": "San Diego",
            "countryCode": "US",
            "postalCode": "90002",
            "stateOrProvinceCode": "CA",
            "phone": "123141321"
        },
        "shippingPerson": {
            "name": "Eugene K",
            "companyName": "Hedgehog and Bucket",
            "street": "My Street",
            "city": "San Diego",
            "countryCode": "US",
            "postalCode": "90002",
            "stateOrProvinceCode": "CA",
            "phone": "123141321"
        },
        "shippingOption": {
            "shippingMethodName": "Fast Delivery",
            "shippingRate": 10
        },
        "hidden": false,
        "privateAdminNotes": "Must be delivered till Sunday."
    }
```

`POST https://app.ecwid.com/api/v3/{storeId}/orders?token={token}`

Field | Type | Description
----- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

### Request body

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
subtotal |  number | Order subtotal
total | number | Order total cost
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
**paymentStatus** | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`INCOMPLETE`</li></ul>. Ignored when creating orders with [public token](#access-tokens)
**fulfillmentStatus** | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li></ul>. Ignored when creating orders with [public token](#access-tokens)
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Order comments
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of applied discounts without coupon discount. 
globalReferer | string | URL that the customer came to the store from
createDate | date |  The date/time of order placement, e.g `2014-06-06 18:57:19 +0000`
customerGroup | string | The name of group (membership) the customer belongs to
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<string,string\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
privateAdminNotes | string | Private note about the order from store owner. Ignored when creating orders with [public token](#access-tokens)

#### OrderItem
Field | Type | Description
----- | ---- | -----------
**name** | string | Product name
quantity | number | Amount purchased
productId | number | Store product ID
categoryId |  number  | ID of the category this product belongs to. If the product belongs to many categories, categoryID will return the ID of the default product category. If the product doesn't belong to any category, `0` is returned
price | number | Price of ordered item in the cart
productPrice | number | Basic product price without options markups, wholesale discounts etc.
weight |  number | Product weight
sku |   string | Product SKU
shortDescription | string | Product description truncated to 120 characters
tax | number | Tax amount applied to the item
shipping | number| Order item shipping cost 
quantityInStock | number | The number of products in stock in the store
isShippingRequired | boolean | `true`/`false`: shows whether the item requires shipping
trackQuantity | boolean | `true`/`false`: shows whether the store admin set to track the quantity of this product and get low stock notifications
fixedShippingRateOnly | boolean | `true`/`false`: shows whether the fixed shipping rate is set for the product
fixedShippingRate | number| Fixed shipping rate for the product
digital | boolean | `true`/`false`: shows whether the item has downloadable files attached
productAvailable | boolean | `true`/`false`: shows whether the product is available in the store
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item

#### OrderItemTax
Field | Type | Description
----- | ---- | -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item

#### OrderItemOption
Field | Type | Description
----- | ---- | -----------
**name** |  string | Option name
type |  string | Option type. One of: <ul><li>`CHOICE` (dropdown or radio button)</li><li>`CHOICES` (checkboxes)</li><li>`TEXT` (text input and text area)</li><li>`DATE` (date/time)</li><li>`FILES` (upload file option)</li></ul>
**value** | string | Selected/entered value by customer. Multiple values separated by comma in a single string
files | Array\<*OrderItemOptionFile*\> | Attached files (if the option type is `FILES`)

#### PersonInfo
Field | Type | Description
----- | ---- | -----------
name |  string  | Full name
companyName |   string  | Company name
street |    string  | Address line 1 and address line 2, separated by '\n'
city |  string  | City
countryCode | string  | Two-letter country code
postalCode | string  | Postal/ZIP code
stateOrProvinceCode |   string  | State code, e.g. `NY`
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
estimatedTransitTime | string | Delivery time estimation. Formats accepted: number "5", several days estimate "4-9"

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

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

### Response


> Response example (JSON)

```json
{
    "id": 20
}
```

A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus

Field | Type |  Description
-------------- | -------------- | --------------
id | number | The number of created order in the store

### Errors

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


## Upload item option file

Using this method, you can attach a file to an order item (implying that the order item has a 'file upload' option). Request parameters specify which order, item and option should be updated. Request body is the file itself (binary data). Maximum allowed file size is 100Mb.

> Request example

```http
POST /api/v3/4870020/orders/20/items/987653/options/Attach+your+image?fileName=my_photo.jpg&token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/4870020/orders/20/items/987653/options/Attach+your+image?fileName=my_photo.jpg&token=123456789abcd';

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

request_url = "https://app.ecwid.com/api/v3/4870020/orders/20/items/987653/options/Attach+your+image?fileName=my_photo.jpg&token=123456789abcd"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}/items/{itemId}/options/{optionName}?fileName={fileName}token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**orderNumber** | number | Order number
**itemId** | number | Order item ID
**optionName** | string | Item product option name, e.g. `Upload your photo`
**fileName** |  string |  Uploaded file name
**token** |  string |  oAuth token

When uploading an item option file, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

### Response

> Response example

```json
{
    "id": 6005003
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
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan
404 | Order, order item or item option is not found
413 | The file is too large.  Maximum allowed file size is 100Mb.
415 | Unsupported content-type: expected `application/octet-stream`
500 | Uploading of the file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Delete item option file

Using this method, you can remove a file attached to an order item option by customer.

> Request example

```http
DELETE /api/v3/4870020/orders/20/items/987653/options/Attach+your+image/files/1838839292388?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}/items/{itemId}/options/{optionName}/files/{fileId}?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**orderNumber** | number | Order number
**itemId** | number | Order item ID
**optionName** | string | Item product option name, e.g. `Upload your photo`
**fileId** | number | Option file's internal ID
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
deleteCount | number | The number of deleted files (`1` or `0` depending on whether the request was successful)

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
500 | Request failed -- there was an internal server error
404 | Order, order item or item option is not found
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Delete all item option's files

Using this method, you can remove all files attached to an order item option by customer.

> Request example

```http
DELETE /api/v3/4870020/orders/20/items/987653/options/Attach+your+image/files?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}/items/{itemId}/options/{optionName}/files?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**orderNumber** | number | Order number
**itemId** | number | Order item ID
**optionName** | string | Item product option name, e.g. `Upload your photo`
**token** |  string |  oAuth token

### Response

> Response example

```json
{
    "deleteCount": 2
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus
Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted files

### Errors

> Error response example 

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Request failed – there was an internal server error
404 | Order, order item or item option is not found
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
