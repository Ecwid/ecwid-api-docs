## Orders

The endpoints below allow you to search/create/update/delete orders in an Ecwid store. Learn more about orders in Ecwid in our [Help Center](https://support.ecwid.com/hc/en-us/articles/207806235-Orders).

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

### Search orders

Search or filter orders in an Ecwid store. The response provides full details of the found orders.

> Request example

```http
GET /api/v3/4870020/orders?customer=mscott@gmail.com&paymentStatus=PAID,AWAITING_PAYMENT&token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
Accept-Encoding: gzip
```

`GET https://app.ecwid.com/api/v3/{storeId}/orders?keywords={keywords}&totalFrom={totalFrom}&totalTo={totalTo}&createdFrom={createdFrom}&createdTo={createdTo}&updatedFrom={updatedFrom}&updatedTo={updatedTo}&couponCode={couponCode}&orderNumber={orderNumber}&vendorOrderNumber={vendorOrderNumber}&email={email}&customerId={customerId}&paymentMethod={paymentMethod}&shippingMethod={shippingMethod}&paymentStatus={paymentStatus}&fulfillmentStatus={fulfillmentStatus}&acceptMarketing={acceptMarketing}&refererId={refererId}&productId={productId}&offset={offset}&limit={limit}&token={token}`

#### Q: How to get info about abandoned sales? 

To get abandoned sale information, specify `INCOMPLETE` status for `paymentStatus` filter when searching for orders.

Also, check the [carts endpoint](#carts) methods for accessing abandoned carts

#### Q: How to get all orders from a store?

Use the `offset` parameter to get all orders in a store. 

For example, if a store has 452 orders, you would need to call the `/orders` endpoint 5 times with different `offset` parameter values: 

- `offset=0` // 0-100 orders
- `offset=100` // 100-200 orders
- `offset=200` // 200-300 orders
- `offset=300` // 300-400 orders
- `offset=400` // 400-452 orders

As a result, you will get all 452 orders after these 5 consecutive requests.

#### Q: How to find customer emails I can use for promotions?

Use the `acceptMarketing` filter when searching for orders / customers or when getting order / customer details. 

If it is set to `true` or `null`, you can use their email for promotions in your app or customization. 

If it's set to `false` – you **cannot send promotional emails** to that person. 

<aside class="notice">
If no filters are set in the URL, API will return all orders <strong>except for unfinished orders</strong>. To get unfinished orders, use <i>INCOMPLETE</i> value for <strong>paymentStatus</strong> parameter.
</aside>

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `100`
keywords |  string | Search term. Ecwid will look for this term in:  order number, external transaction id, vendor order number, customer billing and shipping address, customer email, shipping tracking code, item SKUs, item names, item selected options, affiliate Id, private admin notes. If your keywords contain special characters, it may make sense to URL encode them before making a request
couponCode | string | The code of coupon applied to order
totalFrom |  number | Minimum product price
totalTo | number | Maximum product price
orderNumber | number | Order number(s) separated by a comma. If this field is not empty, other filters are ignored
vendorOrderNumber | string | Order number with prefix/suffix defined by admin
email | string | Customer email used to place the order
customerId | number | Customer ID used to place the order
createdFrom | number | Order placement date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul> Examples: <ul><li>`1447804800`</li></ul>
createdTo | number | Order placement date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedFrom | number | Order last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedTo | number | Order last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
paymentMethod | string | Payment method used by customer
shippingMethod | string | Shipping method chosen by customer
paymentStatus | string | Comma separated list of order payment statuses to search. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`PARTIALLY_REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string | Comma separated list of order fulfilment statuses to search. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li><li>`READY_FOR_PICKUP`</li></ul>
acceptMarketing | boolean | `true` if customer has accepted email marketing. `false` otherwise
refererId | string | Filter order results by `refererId` field in order details
productId | number | Comma-separated list of productIds. Use it to find orders with specific products by their IDs

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
            "vendorOrderNumber": "lala1028001",
            "orderNumber": 1028,
            "subtotal": 1076.64,
            "total": 2014.97,
            "usdTotal": 2014.97,
            "tax": 488.48,
            "paymentMethod": "Credit or debit card (Mollie)",
            "paymentStatus": "PARTIALLY_REFUNDED",
            "fulfillmentStatus": "DELIVERED",
            
            // Additional information
            "refererUrl": "https://mdemo.ecwid.com/",
            "globalReferer": "https://my.ecwid.com/",
            "createDate": "2018-05-31 15:08:36 +0000",
            "updateDate": "2018-05-31 15:09:35 +0000",
            "createTimestamp": 1527779316,
            "updateTimestamp": 1527779375,
            "hidden": false,
            "orderComments": "Test order comments",
            "privateAdminNotes": "Must be delivered till Sunday.",

            // Basic customer information
            "email": "mscott@gmail.com",
            "ipAddress": "123.431.234.243",
            "customerId": 40201284,
            "customerGroupId": 12345,
            "customerGroup": "Gold",
            "customerTaxExempt": false,
            "customerTaxId": "",
            "customerTaxIdValid": false,
            "reversedTaxApplied": false,

            // Discounts in order
            "discount": 4,
            "couponDiscount": 22,
            "volumeDiscount": 4,
            "membershipBasedDiscount": 0,
            "totalAndMembershipBasedDiscount": 0,
            "customDiscount": [],
            "discountCoupon": {
                "id": 29567026,
                "name": "API Testing",
                "code": "APITESTING",
                "discountType": "ABS",
                "status": "ACTIVE",
                "discount": 22,
                "launchDate": "2018-05-24 20:00:00 +0000",
                "usesLimit": "UNLIMITED",
                "applicationLimit": "UNLIMITED",
                "creationDate": "2018-05-31 15:08:33 +0000",
                "updateDate": "2018-05-24 13:40:32 +0000",
                "orderCount": 0
            },
            "discountInfo": [
                {
                    "value": 4,
                    "type": "ABS",
                    "base": "ON_TOTAL",
                    "orderTotal": 1
                }
            ],
            
            // Order items details
            "items": [
                {
                    "id": 140273658,
                    "productId": 66722487,
                    "categoryId": 19563207,
                    "price": 1060,
                    "productPrice": 1000,
                    "sku": "ABCA-IAC",
                    "quantity": 1,
                    "shortDescription": "",
                    "tax": 331.01,
                    "shipping": 0,
                    "quantityInStock": 0,
                    "name": "iMac",
                    "isShippingRequired": true,
                    "weight": 0,
                    "trackQuantity": false,
                    "fixedShippingRateOnly": false,
                    "imageUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/391870914.jpg",
                    "smallThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/650638292.jpg",
                    "hdThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/650638293.jpg",
                    "fixedShippingRate": 0,
                    "digital": false,
                    "couponApplied": true,
                    "selectedOptions": [
                        {
                            "name": "Price-Optimizer",
                            "value": "6",
                            "valuesArray": [
                                "6"
                            ],
                            "selections": [
                                {
                                    "selectionTitle": "6",
                                    "selectionModifier": 6,
                                    "selectionModifierType": "PERCENT"
                                }
                            ],
                            "type": "CHOICE"
                        }
                    ],
                    "taxes": [
                        {
                            "name": "State tax",
                            "value": 12,
                            "total": 124.13,
                            "taxOnDiscountedSubtotal": 124.13,
                            "taxOnShipping": 0,
                            "includeInPrice": false
                        },
                        {
                            "name": "TVA",
                            "value": 20,
                            "total": 206.88,
                            "taxOnDiscountedSubtotal": 206.88,
                            "taxOnShipping": 0,
                            "includeInPrice": true
                        }
                    ],
                    "dimensions": {
                        "length": 0,
                        "width": 0,
                        "height": 0
                    },
                    "couponAmount": 21.66,
                    "discounts": [
                        {
                            "discountInfo": {
                                "value": 4,
                                "type": "ABS",
                                "base": "ON_TOTAL",
                                "orderTotal": 1
                            },
                            "total": 3.94
                        }
                    ]
                },
                {
                    "id": 140273659,
                    "productId": 66821181,
                    "categoryId": 0,
                    "price": 16.64,
                    "productPrice": 16,
                    "sku": "001001",
                    "quantity": 1,
                    "shortDescription": "This sturdy white, glossy ceramic mug is an essential to your cupboard. This brawny version of ceramic mugs shows it’s ...",
                    "tax": 157.47,
                    "shipping": 471.85,
                    "quantityInStock": 0,
                    "name": "Mug",
                    "isShippingRequired": true,
                    "weight": 0.4,
                    "trackQuantity": false,
                    "fixedShippingRateOnly": false,
                    "imageUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/389900000.jpg",
                    "smallThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/475772545.jpg",
                    "hdThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/408631478.jpg",
                    "fixedShippingRate": 0,
                    "digital": false,
                    "couponApplied": true,
                    "selectedOptions": [
                        {
                            "name": "Color",
                            "value": "White",
                            "valuesArray": [
                                "White"
                            ],
                            "selections": [
                                {
                                    "selectionTitle": "White",
                                    "selectionModifier": 0,
                                    "selectionModifierType": "ABSOLUTE"
                                }
                            ],
                            "type": "CHOICE"
                        },
                        {
                            "name": "Size",
                            "value": "11oz",
                            "valuesArray": [
                                "11oz"
                            ],
                            "selections": [
                                {
                                    "selectionTitle": "11oz",
                                    "selectionModifier": 0,
                                    "selectionModifierType": "ABSOLUTE"
                                }
                            ],
                            "type": "CHOICE"
                        },
                        {
                            "name": "Price-Optimizer",
                            "value": "4",
                            "valuesArray": [
                                "4"
                            ],
                            "selections": [
                                {
                                    "selectionTitle": "4",
                                    "selectionModifier": 4,
                                    "selectionModifierType": "PERCENT"
                                }
                            ],
                            "type": "CHOICE"
                        }
                    ],
                    "taxes": [
                        {
                            "name": "State tax",
                            "value": 12,
                            "total": 59.05,
                            "taxOnDiscountedSubtotal": 1.95,
                            "taxOnShipping": 57.1,
                            "includeInPrice": false
                        },
                        {
                            "name": "TVA",
                            "value": 20,
                            "total": 98.42,
                            "taxOnDiscountedSubtotal": 3.25,
                            "taxOnShipping": 95.17,
                            "includeInPrice": true
                        }
                    ],
                    "dimensions": {
                        "length": 0,
                        "width": 0,
                        "height": 0
                    },
                    "couponAmount": 0.34,
                    "discounts": [
                        {
                            "discountInfo": {
                                "value": 4,
                                "type": "ABS",
                                "base": "ON_TOTAL",
                                "orderTotal": 1
                            },
                            "total": 0.06
                        }
                    ]
                }
            ],

            // Refund information
            "refundedAmount": 3.5,
            "refunds": [
                {
                    "date": "2017-09-12 10:12:56 +0000",
                    "source": "CP",
                    "reason": "Testing!",
                    "amount": 3.5
                }
            ],

            // Customer addresses
            "billingPerson": {
                "name": "Michael Scott",
                "companyName": "",
                "street": "555 Lackawanna Ave",
                "city": "Scranton",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "18508",
                "stateOrProvinceCode": "PA",
                "stateOrProvinceName": "Pennsylvania",
                "phone": ""
            },
            "shippingPerson": {
                "name": "Michael Scott",
                "companyName": "",
                "street": "555 Lackawanna Ave",
                "city": "Scranton",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "18508",
                "stateOrProvinceCode": "PA",
                "stateOrProvinceName": "Pennsylvania",
                "phone": ""
            },

            // Shipping information    
            "shippingOption": {
                "shippingCarrierName": "Shipping app the-printful",
                "shippingMethodName": "USPS Priority Mail",
                "shippingRate": 471.85,
                "estimatedTransitTime": "1-3",
                "isPickup": false
            },
            "handlingFee": {
                "name": "Handling Fee",
                "value": 4,
                "description": ""
            },
            "predictedPackage": [
                {
                    "length": 0,
                    "width": 0,
                    "height": 0,
                    "weight": 0.4,
                    "declaredValue": 1076.64
                }
            ],
            "taxesOnShipping": [
                {
                    "name": "State tax",
                    "value": 12,
                    "total": 57.1
                },
                {
                    "name": "TVA",
                    "value": 20,
                    "total": 95.17
                }
            ],    

            // Other information
            "paymentModule": "CUSTOM_PAYMENT_APP-mollie-pg",
            "additionalInfo": {
                "google_customer_id": "2008512504.1526280224"
            },
            "paymentParams": {},
            "extraFields": {
                "lang": "en",
                "askHowYouFoundUsApp": "From a friend"
            },
            "acceptMarketing": true,
            "refererId": "Amazon",
            "disableAllCustomerNotifications": false
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
orderNumber | number | Unique order number without prefixes/suffixes, e.g. `34`. Use `vendorOrderNumber` whenever you need to show order number in your interface
vendorOrderNumber |  string | Order number with prefix and suffix defined by admin, e.g. `ABC34-q`
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order (item's `price` x `quantity`)
total | number | Order total cost. Includes shipping, taxes, discounts, etc.
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total for order. Sum of all tax in order items, unless they were modified after order was placed
customerTaxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)
customerTaxId | string | Customer tax ID
customerTaxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
reversedTaxApplied | boolean | `true` if order tax was set to 0 because customer specified their valid tax ID in checkout process. `false` otherwise
ipAddress | string  | Customer IP
paymentStatus | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`PARTIALLY_REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li><li>`READY_FOR_PICKUP`</li></ul>
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Customer's order comments, specified at checkout
couponDiscount | number | Discount applied to order using a coupon
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values. Total order discount is a sum of all discounts applied to items (both regular discount and discount coupons) unless they were modified after order was placed
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
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer. Can be omitted, if this form is disabled by merchant in *Ecwid Control Panel > Settings > General > Cart > Ask for a billing address during checkout*
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
predictedPackages | \<*PredictedPackage*\> | Predicted information about the package to ship items in to customer
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<*string,string*\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any. It is present and visible in order details only if order status is not paid. When order becomes paid, `paymentMessage` is cleared
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
privateAdminNotes | string | Private note about the order from store owner
extraFields | \<*ExtraFieldsInfo*\> | Additional optional information about order. Total storage of extra fields cannot exceed 8Kb. See [Order extra fields](#order-extra-fields)
refundedAmount | number | A sum of all refunds made to order (for [Ecwid Payments only](https://support.ecwid.com/hc/en-us/articles/211954289-Ecwid-Payments-US-Canada-and-UK-))
refunds | Array\<*RefundsInfo*\> | Description of all refunds made to order (for [Ecwid Payments only](https://support.ecwid.com/hc/en-us/articles/211954289-Ecwid-Payments-US-Canada-and-UK-))
pickupTime | string | Order pickup time in the store date format, e.g.: `"2017-10-17 05:00:00 +0000"`
taxesOnShipping | Array\<*TaxOnShipping*\> | Taxes applied to shipping 'as is'. `null` for old orders, `[]` for orders with taxes applied to subtotal only. Are not recalculated if order is updated later manually. Is calculated like: `(shippingRate + handlingFee)*(taxValue/100)`
acceptMarketing | boolean | `true` if customer has accepted email marketing and you can use their email for promotions (`null` too). If value is `false`, you can't use this email for promotions
refererId | string | Referer identifier. Can be set in storefront via JS or by creating / updating an order with REST API
disableAllCustomerNotifications | boolean | `true` if no email notifications are sent to customer. If `false` or empty, then email notifications are sent to customer according to store mail notification settings. This field does not influence admin email notifications.

#### OrderItem
Field | Type |  Description
--------- | -----------| -----------
id | number | Order item ID. Can be used to address the item in the order, e.g. to manage ordered items.
productId | number | Store product ID
categoryId |  number  | ID of the category this product belongs to. If the product belongs to many categories, categoryID will return the ID of the default product category. If the product doesn't belong to any category, `0` is returned
price | number | Price of ordered item in the cart
productPrice | number | Basic product price without options markups, wholesale discounts etc.
weight |  number | Product weight
sku | string | Product SKU. If the chosen options match a variation, this will be a variation SKU.
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
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item
files | Array\<*OrderItemProductFile*\> | Files attached to the order item
dimensions | \<*ProductDimensions*\> | Product dimensions info
couponAmount | number | Coupon discount amount applied to item. Provided if discount applied to order. Is not recalculated if order is updated later manually
discounts | Array\<*OrderItemDiscounts*\> | Discounts applied to order item 'as is'. Provided if discounts are applied to order (not including discount coupons) and are not recalculated if order is updated later manually

#### OrderItemTax
Field | Type | Description
----- | -----| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
taxOnDiscountedSubtotal | number |  Tax on item subtotal (after applying discounts)
taxOnShipping | number | Tax on item shipping
includeInPrice | boolean | `true` if the tax rate is included in product prices. More details: [Taxes in Ecwid](http://help.ecwid.com/customer/portal/articles/1182159-taxes)

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
selections | Array\<*SelectionInfo*\> | Details of selected product options. If sent in update order request, other fields will be regenerated based on information in this field

#### OrderItemOptionFile
Field | Type |  Description
--------- | -----------| -----------
id | number | File ID
name |  string | File name
size |  number | File size in bytes
url |   string | File URL

#### SelectionInfo
Field | Type |  Description
--------- | -----------| -----------
selectionTitle | string | Selection title, as set by merchant
selectionModifier | number | Selection price modifier amount. Value is negative for negative modifiers
selectionModifierType | string | Price modifier type: `"PERCENT"` or `"ABSOLUTE"`

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

#### OrderItemDiscounts
Field | Type  | Description
----- | ----- | -----------
discountInfo | \<*OrderItemDiscountInfo*\> | Info about discounts applied to item
total | number | Total order item discount value for this discount

#### OrderItemDiscountInfo
Field | Type  | Description
----- | ----- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
orderTotal | number | Minimum order subtotal the discount applies to

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
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | Products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<*number*\> | The list of product IDs the coupon can be applied to
categories | Array\<*number*\> | The list of category IDs the coupon can be applied to

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

#### PredictedPackage
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
orderTotal | number | Minimum order subtotal the discount applies to
description | string | Description of a discount (for discounts with base == `CUSTOM`)

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

#### ExtraFieldsInfo
Field | Type | Description
----- | ---- | -----------
YOUR_FIELD_NAME | string | Your custom name saved for the order extra field. The value length cannot exceed 255 characters

#### RefundsInfo
Field | Type | Description
----- | ---- | -----------
date | date | The date/time of a refund, e.g `2014-06-06 18:57:19 +0000`
source | string | What action triggered refund. Possible values: `"CP"` - changed my merchant in Ecwid CP, `"API"` - changed by another app, `"External"` - refund made from payment processor website
reason | string | A text reason for a refund. 256 characters max
amount | number | Amount of this specific refund (not total amount refunded for order. see `redundedAmount` field)

#### TaxOnShipping
Field | Type | Description
----- | ---- | -----------
name | string | Tax name
value | number | Tax value in store settings, applied to destination zone
total | number | Tax total applied to shipping

#### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
------------|--------
400 | Request parameters are malformed
403 | Access token doesn't have `read_orders` scope 
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Get order details

Get all available information about an order referring to its ID. The order details include: customer email, payment/shipping method, items purchased and more.

#### Q: How can I request details of several orders at once?

When you know the exact order numbers for orders you need, you can get those order details in one request (batch request). To do that, use the [Search orders](https://developers.ecwid.com/api-documentation/orders#search-orders) method: provide the order numbers you have in the `orderNumber` parameter separating them with a comma. 

This way your app will save some time as you will be performing less requests to the Ecwid API and they will be much more efficient.

#### Q: How to get info about abandoned sales? 

To get abandoned sale information, specify `INCOMPLETE` status for `paymentStatus` filter when searching for orders.

Also, check the [carts endpoint](#carts) methods for accessing abandoned carts

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
**orderNumber** | number | Order number. Use the `orderNumber` value here only, not the `vendorOrderNumber` or anything else

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (JSON)

```json
{
    // Basic information
    "vendorOrderNumber": "lala1028001",
    "orderNumber": 1028,
    "subtotal": 1076.64,
    "total": 2014.97,
    "usdTotal": 2014.97,
    "tax": 488.48,
    "paymentMethod": "Credit or debit card (Mollie)",
    "paymentStatus": "PARTIALLY_REFUNDED",
    "fulfillmentStatus": "DELIVERED",
    
    // Additional information
    "refererUrl": "https://mdemo.ecwid.com/",
    "globalReferer": "https://my.ecwid.com/",
    "createDate": "2018-05-31 15:08:36 +0000",
    "updateDate": "2018-05-31 15:09:35 +0000",
    "createTimestamp": 1527779316,
    "updateTimestamp": 1527779375,
    "hidden": false,
    "orderComments": "Test order comments",
    "privateAdminNotes": "Must be delivered till Sunday.",

    // Basic customer information
    "email": "mscott@gmail.com",
    "ipAddress": "123.431.234.243",
    "customerId": 40201284,
    "customerGroupId": 12345,
    "customerGroup": "Gold",
    "customerTaxExempt": false,
    "customerTaxId": "",
    "customerTaxIdValid": false,
    "reversedTaxApplied": false,

    // Discounts in order
    "discount": 4,
    "couponDiscount": 22,
    "volumeDiscount": 4,
    "membershipBasedDiscount": 0,
    "totalAndMembershipBasedDiscount": 0,
    "customDiscount": [],
    "discountCoupon": {
        "id": 29567026,
        "name": "API Testing",
        "code": "APITESTING",
        "discountType": "ABS",
        "status": "ACTIVE",
        "discount": 22,
        "launchDate": "2018-05-24 20:00:00 +0000",
        "usesLimit": "UNLIMITED",
        "applicationLimit": "NEW_CUSTOMER_ONLY",
        "creationDate": "2018-05-31 15:08:33 +0000",
        "updateDate": "2018-05-24 13:40:32 +0000",
        "orderCount": 0
    },
    "discountInfo": [
        {
            "value": 4,
            "type": "ABS",
            "base": "ON_TOTAL",
            "orderTotal": 1
        }
    ],
    
    // Order items details
    "items": [
        {
            "id": 140273658,
            "productId": 66722487,
            "categoryId": 19563207,
            "price": 1060,
            "productPrice": 1000,
            "sku": "ABCA-IAC",
            "quantity": 1,
            "shortDescription": "",
            "tax": 331.01,
            "shipping": 0,
            "quantityInStock": 0,
            "name": "iMac",
            "isShippingRequired": true,
            "weight": 0,
            "trackQuantity": false,
            "fixedShippingRateOnly": false,
            "imageUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/391870914.jpg",
            "smallThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/650638292.jpg",
            "hdThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/650638293.jpg",
            "fixedShippingRate": 0,
            "digital": false,
            "couponApplied": true,
            "selectedOptions": [
                {
                    "name": "Price-Optimizer",
                    "value": "6",
                    "valuesArray": [
                        "6"
                    ],
                    "selections": [
                        {
                            "selectionTitle": "6",
                            "selectionModifier": 6,
                            "selectionModifierType": "PERCENT"
                        }
                    ],
                    "type": "CHOICE"
                }
            ],
            "taxes": [
                {
                    "name": "State tax",
                    "value": 12,
                    "total": 124.13,
                    "taxOnDiscountedSubtotal": 124.13,
                    "taxOnShipping": 0,
                    "includeInPrice": false
                },
                {
                    "name": "TVA",
                    "value": 20,
                    "total": 206.88,
                    "taxOnDiscountedSubtotal": 206.88,
                    "taxOnShipping": 0,
                    "includeInPrice": true
                }
            ],
            "dimensions": {
                "length": 0,
                "width": 0,
                "height": 0
            },
            "couponAmount": 21.66,
            "discounts": [
                {
                    "discountInfo": {
                        "value": 4,
                        "type": "ABS",
                        "base": "ON_TOTAL",
                        "orderTotal": 1
                    },
                    "total": 3.94
                }
            ]
        },
        {
            "id": 140273659,
            "productId": 66821181,
            "categoryId": 0,
            "price": 16.64,
            "productPrice": 16,
            "sku": "001001",
            "quantity": 1,
            "shortDescription": "This sturdy white, glossy ceramic mug is an essential to your cupboard. This brawny version of ceramic mugs shows it’s ...",
            "tax": 157.47,
            "shipping": 471.85,
            "quantityInStock": 0,
            "name": "Mug",
            "isShippingRequired": true,
            "weight": 0.4,
            "trackQuantity": false,
            "fixedShippingRateOnly": false,
            "imageUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/389900000.jpg",
            "smallThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/475772545.jpg",
            "hdThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/408631478.jpg",
            "fixedShippingRate": 0,
            "digital": false,
            "couponApplied": true,
            "selectedOptions": [
                {
                    "name": "Color",
                    "value": "White",
                    "valuesArray": [
                        "White"
                    ],
                    "selections": [
                        {
                            "selectionTitle": "White",
                            "selectionModifier": 0,
                            "selectionModifierType": "ABSOLUTE"
                        }
                    ],
                    "type": "CHOICE"
                },
                {
                    "name": "Size",
                    "value": "11oz",
                    "valuesArray": [
                        "11oz"
                    ],
                    "selections": [
                        {
                            "selectionTitle": "11oz",
                            "selectionModifier": 0,
                            "selectionModifierType": "ABSOLUTE"
                        }
                    ],
                    "type": "CHOICE"
                },
                {
                    "name": "Price-Optimizer",
                    "value": "4",
                    "valuesArray": [
                        "4"
                    ],
                    "selections": [
                        {
                            "selectionTitle": "4",
                            "selectionModifier": 4,
                            "selectionModifierType": "PERCENT"
                        }
                    ],
                    "type": "CHOICE"
                }
            ],
            "taxes": [
                {
                    "name": "State tax",
                    "value": 12,
                    "total": 59.05,
                    "taxOnDiscountedSubtotal": 1.95,
                    "taxOnShipping": 57.1,
                    "includeInPrice": false
                },
                {
                    "name": "TVA",
                    "value": 20,
                    "total": 98.42,
                    "taxOnDiscountedSubtotal": 3.25,
                    "taxOnShipping": 95.17,
                    "includeInPrice": true
                }
            ],
            "dimensions": {
                "length": 0,
                "width": 0,
                "height": 0
            },
            "couponAmount": 0.34,
            "discounts": [
                {
                    "discountInfo": {
                        "value": 4,
                        "type": "ABS",
                        "base": "ON_TOTAL",
                        "orderTotal": 1
                    },
                    "total": 0.06
                }
            ]
        }
    ],

    // Refund information
    "refundedAmount": 3.5,
    "refunds": [
        {
            "date": "2017-09-12 10:12:56 +0000",
            "source": "CP",
            "reason": "Testing!",
            "amount": 3.5
        }
    ],

    // Customer addresses
    "billingPerson": {
        "name": "Michael Scott",
        "companyName": "",
        "street": "555 Lackawanna Ave",
        "city": "Scranton",
        "countryCode": "US",
        "countryName": "United States",
        "postalCode": "18508",
        "stateOrProvinceCode": "PA",
        "stateOrProvinceName": "Pennsylvania",
        "phone": ""
    },
    "shippingPerson": {
        "name": "Michael Scott",
        "companyName": "",
        "street": "555 Lackawanna Ave",
        "city": "Scranton",
        "countryCode": "US",
        "countryName": "United States",
        "postalCode": "18508",
        "stateOrProvinceCode": "PA",
        "stateOrProvinceName": "Pennsylvania",
        "phone": ""
    },

    // Shipping information    
    "shippingOption": {
        "shippingCarrierName": "Shipping app the-printful",
        "shippingMethodName": "USPS Priority Mail",
        "shippingRate": 471.85,
        "estimatedTransitTime": "1-3",
        "isPickup": false
    },
    "handlingFee": {
        "name": "Handling Fee",
        "value": 4,
        "description": ""
    },
    "predictedPackage": [
        {
            "length": 0,
            "width": 0,
            "height": 0,
            "weight": 0.4,
            "declaredValue": 1076.64
        }
    ],
    "taxesOnShipping": [
        {
            "name": "State tax",
            "value": 12,
            "total": 57.1
        },
        {
            "name": "TVA",
            "value": 20,
            "total": 95.17
        }
    ],    

    // Other information
    "paymentModule": "CUSTOM_PAYMENT_APP-mollie-pg",
    "additionalInfo": {
        "google_customer_id": "2008512504.1526280224"
    },
    "paymentParams": {},
    "extraFields": {
        "lang": "en",
        "askHowYouFoundUsApp": "From a friend",
    },
    "acceptMarketing": true,
    "refererId": "Amazon",
    "disableAllCustomerNotifications": false
}
```

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
orderNumber | number | Unique order number without prefixes/suffixes, e.g. `34`. Use `vendorOrderNumber` whenever you need to show order number in your interface
vendorOrderNumber |  string | Order number with prefix and suffix defined by admin, e.g. `ABC34-q`
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order (item's `price` x `quantity`)
total | number | Order total cost. Includes shipping, taxes, discounts, etc.
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total for order. Sum of all tax in order items, unless they were modified after order was placed
customerTaxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)
customerTaxId | string | Customer tax ID
customerTaxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
reversedTaxApplied | boolean | `true` if order tax was set to 0 because customer specified their valid tax ID in checkout process. `false` otherwise
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
paymentStatus | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`PARTIALLY_REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li><li>`READY_FOR_PICKUP`</li></ul>
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Customer's order comments, specified at checkout
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values. Total order discount is a sum of all discounts applied to items (both regular discount and discount coupons) unless they were modified after order was placed
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
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer. Can be omitted, if this form is disabled by merchant in *Ecwid Control Panel > Settings > General > Cart > Ask for a billing address during checkout*
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
predictedPackages | \<*PredictedPackage*\> | Predicted information about the package to ship items in to customer
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<*string,string*\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any. It is present and visible in order details only if order status is not paid. When order becomes paid, `paymentMessage` is cleared
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
privateAdminNotes | string | Private note about the order from store owner
extraFields | \<*ExtraFieldsInfo*\> | Additional optional information about order. Total storage of extra fields cannot exceed 8Kb. See [Order extra fields](#order-extra-fields)
refundedAmount | number | A sum of all refunds made to order (for [Ecwid Payments only](https://support.ecwid.com/hc/en-us/articles/211954289-Ecwid-Payments-US-Canada-and-UK-))
refunds | Array\<*RefundsInfo*\> | Description of all refunds made to order (for [Ecwid Payments only](https://support.ecwid.com/hc/en-us/articles/211954289-Ecwid-Payments-US-Canada-and-UK-))
pickupTime | string | Order pickup time in the store date format, e.g.: `"2017-10-17 05:00:00 +0000"`
acceptMarketing | boolean | `true` if customer has accepted email marketing and you can use their email for promotions (`null` too). If value is `false`, you can't use this email for promotions
refererId | string | Referer identifier. Can be set in storefront via JS or by creating / updating an order with REST API
disableAllCustomerNotifications | boolean | `true` if no email notifications are sent to customer. If `false` or empty, then email notifications are sent to customer according to store mail notification settings. This field does not influence admin email notifications.

#### OrderItem
Field | Type |  Description
--------- | -----------| -----------
id | number | Order item ID. Can be used to address the item in the order, e.g. to manage ordered items.
productId | number | Store product ID
categoryId |  number  | ID of the category this product belongs to. If the product belongs to many categories, categoryID will return the ID of the default product category. If the product doesn't belong to any category, `0` is returned
price | number | Price of ordered item in the cart
productPrice | number | Basic product price without options markups, wholesale discounts etc.
weight |  number | Product weight
sku | string | Product SKU. If the chosen options match a variation, this will be a variation SKU.
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
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item
files | Array\<*OrderItemProductFile*\> | Files attached to the order item
dimensions | \<*ProductDimensions*\> | Product dimensions info
couponAmount | number | Coupon discount amount applied to item. Provided if discount applied to order. Is not recalculated if order is updated later manually
discounts | Array\<*OrderItemDiscounts*\> | Discounts applied to order item 'as is'. Provided if discounts are applied to order (not including discount coupons) and are not recalculated if order is updated later manually
taxesOnShipping | Array\<*TaxOnShipping*\> | Taxes applied to shipping. `null` for old orders, `[]` for orders with taxes applied to subtotal only. Are not recalculated if order is updated later manually. Is calculated like: `(shippingRate + handlingFee)*(taxValue/100)`

#### OrderItemTax
Field | Type |  Description
--------- | -----------| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
taxOnDiscountedSubtotal | number |  Tax on item subtotal (after applying discounts)
taxOnShipping | number | Tax on item shipping
includeInPrice | boolean | `true` if the tax rate is included in product prices. More details: [Taxes in Ecwid](http://help.ecwid.com/customer/portal/articles/1182159-taxes)

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
selections | Array\<*SelectionInfo*\> | Details of selected product options. If sent in update order request, other fields will be regenerated based on information in this field

#### OrderItemOptionFile
Field | Type |  Description
--------- | -----------| -----------
id | number | File ID
name |  string | File name
size |  number | File size in bytes
url |   string | File URL

#### SelectionInfo
Field | Type |  Description
--------- | -----------| -----------
selectionTitle | string | Selection title, as set by merchant
selectionModifier | number | Selection price modifier amount. Value is negative for negative modifiers
selectionModifierType | string | Price modifier type: `"PERCENT"` or `"ABSOLUTE"`

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
stateOrProvinceName | string | State/province name, e.g. `New York`
phone | string  | Phone number

#### OrderItemDiscounts
Field | Type  | Description
----- | ----- | -----------
discountInfo | \<*OrderItemDiscountInfo*\> | Info about discounts applied to item
total | number | Total order item discount value for this discount

#### OrderItemDiscountInfo
Field | Type  | Description
----- | ----- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
orderTotal | number | Minimum order subtotal the discount applies to

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
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | Products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<*number*\> | The list of product IDs the coupon can be applied to
categories | Array\<*number*\> | The list of category IDs the coupon can be applied to

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

#### PredictedPackage
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
orderTotal | number | Minimum order subtotal the discount applies to
description | string | Description of a discount (for discounts with base == `CUSTOM`)

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system.
cvvMessage | string  | Credit card verification status returned by the payment system.

#### ExtraFieldsInfo
Field | Type | Description
----- | ---- | -----------
YOUR_FIELD_NAME | string | Your custom name saved for the order extra field. The value length cannot exceed 255 characters

#### RefundsInfo
Field | Type | Description
----- | ---- | -----------
date | date | The date/time of a refund, e.g `2014-06-06 18:57:19 +0000`
source | string | What action triggered refund. Possible values: `"CP"` - changed my merchant in Ecwid CP, `"API"` - changed by another app, `"External"` - refund made from payment processor website
reason | string | A text reason for a refund. 256 characters max
amount | number | Amount of this specific refund (not total amount refunded for order. see `redundedAmount` field)

#### TaxOnShipping
Field | Type | Description
----- | ---- | -----------
name | string | Tax name
value | number | Tax value in store settings, applied to destination zone
total | number | Tax total applied to shipping

#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
------------|--------
400 | Request parameters are malformed
403 | Access token doesn't have `read_orders` scope 
404 | The order is not found
405 | Method not allowed. Can occur when using `POST` instead of `PUT` HTTP request method
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Get order invoice

Get HTML code of an invoice for a specific order placed in an Ecwid store.

> Request example

```http
GET /api/v3/4870020/orders/20/invoice?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}/invoice?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**orderNumber** | number | Order number. Make sure to use the `orderNumber` value here and not the `vendorOrderNumber`

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (HTML)

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Order - #ab645ad</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=Edge">
        <script>
        function printFrame() {
            window.document.close();
            window.focus();
            window.print();
            setTimeout(function(){window.close();}, 1);
        }
    </script>
    </head>
    <body onload="printFrame()">
        <!-- Invoice styles -->
        <style type="text/css">
html,body {
    min-height:50%;
    margin:0;
    padding:0;
}

body {
    background-color:#fff;
    width:210mm;
    -moz-box-sizing:border-box;
    box-sizing:border-box;
    padding:20mm 10mm 15mm;
}

body > div {
    height:100%;
}

body p:empty {
    display:none;
}

body a {
    text-decoration:none;
    color:#000;
}

body table {
    border-collapse:collapse;
    border-spacing:0;
}

body .body {
    height:100%;
    width:130mm;
    margin:0 auto;
}

body .body td {
    vertical-align:top;
    height:0;
    font-family:Arial, Helvetica, "Sans Serif";
    font-size:8pt;
    line-height:10pt;
    color:#000;
    padding:0;
}

body .body td.footer {
    height:100%;
    text-align:center;
    vertical-align:bottom;
}

body .body td.footer div {
    border-bottom:1px solid #000;
    padding:10mm 0 5pt;
}

body .body .invoice td td {
    vertical-align:top;
    padding:0;
}

body .body .invoice p {
    margin:2pt 0;
}

body .body .invoice .store-info {
    width:100%;
    margin-bottom:16pt;
}

body .body .invoice .store-info .invoice-logo {
    max-height:25mm;
    max-width:100%;
    width:auto;
    display:block;
    margin:0 0 5mm;
}

body .body .invoice .store-info .store-title {
    font-size:10pt;
    line-height:12pt;
    font-weight:600;
    padding:5pt 0;
}

body .body .invoice .store-info .store-main-info {
    width:89mm;
    padding-right:5mm;
}

body .body .invoice .store-info .store-main-info .title-detached {
    margin-top:5pt;
    margin-bottom:5pt;
}


body .body .invoice .store-info .store-customer-info {
    vertical-align:top;
}

body .body .invoice .store-info .store-customer-info p {
    max-width:60mm;
    word-wrap:break-word;
}

body .body .invoice td.invoice-row,body .body .invoice .invoice-row > tr:first-child > td {
    border-top:1px solid #000;
}

body .body .invoice .order-comment {
    padding-bottom: 12pt;
}

body .body .invoice .order-info {
    margin-bottom:11pt;
    padding:0;
}

body .body .invoice .order-items {
    margin-bottom:16pt;
    padding:0;
}

body .body .invoice .order-info td,body .body .invoice .order-items td {
    width:42mm;
    padding-right:5mm;
}

body .body .invoice .order-info td:last-child,body .body .invoice .order-items td:last-child {
    width:auto;
}

body .body .invoice .order-info .title,body .body .invoice .order-items .title {
    font-weight:600;
    padding-top:5pt;
    padding-bottom:12pt;
    margin:0;
}

body .body .invoice .order-info td:last-child {
    padding-right:0;
}

body .body .invoice .order-info p {
    max-width:42mm;
    word-wrap:break-word;
}

body .body .invoice .order-info .order-comment p {
    max-width: 100%;
    word-break: break-all;
    white-space: pre-wrap;
}

body .body .invoice .order-comment-title {
    margin-bottom: 4pt; 
}

body .body .invoice .order-items table tbody tr:last-child td {
    padding-bottom:3pt;
}

body .body .invoice .order-items .title {
    padding-bottom:5pt;
}

body .body .invoice .order-items .order-item-desc {
    width:87mm;
}

body .body .invoice .order-items .order-item-count {
    width:11mm;
}

body .body .invoice .order-items .order-item-count,body .body .invoice .order-items .order-item-total {
    text-align:right;
    padding-right:1mm;
}

body .body .invoice .order-items .order-item-total {
    padding-right:2mm;
    white-space: nowrap;
}

body .body .invoice .order-items .order-total td {
    padding-top:1pt;
    padding-bottom:1pt;
}

body .body .invoice,body .body .invoice .order-items table {
    width:100%;
}

body .body .invoice td,body .body .invoice .order-items table td {
    padding:0 1mm;
}

body .body .invoice .text-uppercase,body .body .invoice .order-info .title::first-letter,body .body .invoice .order-items .title::first-letter {
    text-transform:uppercase;
}

body .body .invoice .text-bold,body .body .invoice .order-items .order-item-title {
    font-weight:600;
}

body .body .invoice .store-info .store-main-info .url,body .body .invoice .order-info .title-detached,body .body .invoice .order-items .title-detached {
    margin-bottom:5pt;
}

body .body .invoice .store-info .store-main-info p,body .body .invoice .order-items .order-item-desc p {
    max-width:87mm;
    word-wrap:break-word;
}

body .body .invoice .order-items .order-item-desc .order-discount-coupon {
    max-width: 64mm;
}

body .body .invoice .order-items table tbody tr td,body .body .invoice .order-items .order-total tr:first-child td {
    padding-top:7pt;
}

@media print{
    * {
        -webkit-print-color-adjust:exact;
    }

    html,body {
        height:100%;
    }

    body {
        padding:0;
    }

    @page {
        margin:20mm 0 15mm;
    }
}
</style>
        <!-- Invoice body markup -->
        <table class="body">
            <tbody>
                <tr>
                    <td>
                        <table class="invoice">
                            <tbody>
                                <!-- Your company information -->
                                <tr>
                                    <td>
                                        <table class="store-info">
                                            <thead>
                                                <!-- Invoice logo -->
                                                <tr>
                                                    <td colspan="2" class="store-title">
                                                        <!-- Store name -->
                                                        <span>Awesome store</span>
                                                    </td>
                                                </tr>
                                            </thead>
                                            <tbody>
                                                <tr>
                                                    <!-- Company information: store URL, tax ID, company name, address and contact information -->
                                                    <td class="store-main-info">
                                                        <p class="url">    superstore.com
</p>
                                                        <p class="title-detached">Tax registration ID: 1234567</p>
                                                        <p>Cool slippers for dogs</p>
                                                        <p>5th Avenue</p>
                                                        <p>New York, NY 10002</p>
                                                        <p>United States</p>
                                                    </td>
                                                    <td class="store-customer-info">
                                                        <p>
                                                            <b>Customer service</b>
                                                        </p>
                                                        <p></p>
                                                        <p>info@superstore.com</p>
                                                    </td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </td>
                                </tr>
                                <!-- Shipping and billing information -->
                                <tr>
                                    <td class="invoice-row">
                                        <table class="order-info">
                                            <thead>
                                                <tr>
                                                    <!-- Order date -->
                                                    <td colspan="2" class="title">
                                                        <span>Fri, Jun 3, '16</span>
                                                    </td>
                                                </tr>
                                            </thead>
                                            <tbody>
                                                <tr>
                                                    <!-- Customer shipping address -->
                                                    <!-- Customer billing address -->
                                                    <td>
                                                        <p class="text-uppercase">s</p>
                                                        <p></p>
                                                        <p>Mark Cool</p>
                                                        <p>New York, New York, NY</p>
                                                        <p>United States</p>
                                                        <p></p>
                                                        <p>customer@gmail.com</p>
                                                    </td>
                                                    <td>
                                                        <!-- Shipping option chosen to deliver the order -->
                                                        <p>Shipped via Ground Delivery</p>
                                                        <!-- Payment option that customer used to pay for the order -->
                                                        <p>Payment method PayPal</p>
                                                    </td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </td>
                                </tr>
                                <!-- Order comments -->
                                <!-- Order information -->
                                <tr>
                                    <td class="order-items">
                                        <table>
                                            <tbody class="invoice-row">
                                                <tr>
                                                    <td colspan="3" class="title">
                                                        <!-- Order number -->
                                                        <span>Order #ab645ad</span>
                                                    </td>
                                                </tr>
                                                <!-- Ordered items list -->
                                                <tr>
                                                    <td class="order-item-desc">
                                                        <p class="order-item-title">Galaxy SII</p>
                                                        <p>SKU : 000026</p>
                                                    </td>
                                                    <td class="order-item-count">
                                                        <p>1</p>
                                                    </td>
                                                    <td class="order-item-total text-bold">
                                                        <p>$100</p>
                                                    </td>
                                                </tr>
                                            </tbody>
                                            <!-- Order totals -->
                                            <tbody class="invoice-row order-total">
                                                <!-- Order subtotal -->
                                                <tr>
                                                    <td colspan="2" class="order-item-count">
                                                        <p>Items</p>
                                                    </td>
                                                    <td class="order-item-total">
                                                        <p>$100</p>
                                                    </td>
                                                </tr>
                                                <!-- Order shipping cost -->
                                                <tr>
                                                    <td colspan="2" class="order-item-count">
                                                        <p>Shipping</p>
                                                    </td>
                                                    <td class="order-item-total">
                                                        <p>$10</p>
                                                    </td>
                                                </tr>
                                                <!-- Order handling fee cost -->
                                                <!-- Order tax -->
                                                <!-- Order discounts total -->
                                                <tr>
                                                    <td colspan="2" class="order-item-count">
                                                        <p>Discount</p>
                                                    </td>
                                                    <td class="order-item-total">
                                                        <p>-$14</p>
                                                    </td>
                                                </tr>
                                                <!-- Order total cost -->
                                                <tr>
                                                    <td class="order-item-desc"></td>
                                                    <td class="order-item-count text-bold">
                                                        <p>Total</p>
                                                    </td>
                                                    <td class="order-item-total text-bold">
                                                        <p>$96</p>
                                                    </td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </td>
                </tr>
                <!-- Invoice footer -->
                <tr>
                    <td class="footer">
                        <div>Thank you for your order!</div>
                    </td>
                </tr>
            </tbody>
        </table>
    </body>
</html>
```

HTML code of an invoice for the requested order number.

#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
------------|--------
400 | Request parameters are malformed
403 | Access token doesn't have `read_orders` scope 
404 | The order is not found
405 | Method not allowed. Can occur when using `PUT/POST/DELETE` HTTP request methods instead of `GET`
415 | Unsupported content-type: expected `application/json` or `text/json`
422 | Cannot generate an invoice for unfinished order
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Update order

This request allows you to update existing orders in an Ecwid store. When updating order information, you can omit unchanged fields – they will be ignored so the resulting order will keep the corresponding information unchanged. 

However, please mind that if you want to update the ordered items, you should submit all the items in the request. The omitted items will be removed. This is done this way to let you remove some purchased items from the order. 

<aside class='note'>Access scopes required: <strong>read_orders</strong> and <strong>update_orders</strong></aside>

> Request example

```http
PUT /api/v3/4870020/orders/20?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

```json
{
        "subtotal": 35,
        "total": 45,
        "email": "newemail@example.com",
        "paymentMethod": "New payment method",
        "tax": 0,
        "paymentStatus": "CANCELLED",
        "fulfillmentStatus": "PROCESSING",
        "customerTaxExempt": false,
        "customerTaxId": "",
        "customerTaxIdValid": false,
        "reversedTaxApplied": false,
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
                "selectedOptions": [
                    {
                        "name": "Size",
                        "value": "Big",
                        "selections": [
                            {
                                "selectionTitle": "Big",
                                "selectionModifier": 1,
                                "selectionModifierType": "PERCENT"
                            }
                        ],
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
                        "total": 4.79,
                        "includeInPrice": false
                    }
                ],
                "dimensions": {
                    "length": 34,
                    "width": 3,
                    "height": 22
                }
            }
        ],
        "hidden": false,
        "privateAdminNotes": "Must be delivered till Sunday.",
        "pickupTime": "2017-10-17 05:00:00 +0000",
        "taxesOnShipping": [
            {
                "name": "Tax X",
                "value": 20,
                "total": 2.86
            }
        ],
        "acceptMarketing": false,
        "refererId": "Amazon",
        "disableAllCustomerNotifications": true
    }
```

`PUT https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**orderNumber** | number | Order number. Make sure to use the `orderNumber` value here and not the `vendorOrderNumber`

#### Request body

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order (item's `price` x `quantity`)
total | number | Order total cost. Includes shipping, taxes, discounts, etc.
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total for order. Sum of all tax in order items, unless they were modified after order was placed
customerTaxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)
customerTaxId | string | Customer tax ID
customerTaxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
reversedTaxApplied | boolean | `true` if order tax was set to 0 because customer specified their valid tax ID in checkout process. `false` otherwise
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
paymentStatus | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`PARTIALLY_REFUNDED`</li></ul>
fulfillmentStatus | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li><li>`READY_FOR_PICKUP`</li></ul>
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Customer's order comments, specified at checkout
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values. Total order discount is a sum of all discounts applied to items (both regular discount and discount coupons) unless they were modified after order was placed
globalReferer | string | URL that the customer came to the store from
customerGroup | string | The name of group (membership) the customer belongs to
discountCoupon | \<*DiscountCouponInfo*\> | Information about applied coupon
items | Array\<*OrderItem*\> | Order items
billingPerson | \<*PersonInfo*\> | Name and billing address of the customer
shippingPerson | \<*PersonInfo*\> | Name and address of the person entered in shipping information
shippingOption | \<*ShippingOptionInfo*\> | Information about selected shipping option
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
additionalInfo | Map\<*string,string*\> | Additional order information if any (*reserved for future use*)
paymentParams | Map\<*string,string*\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any. It is present and visible in order details only if order status is not paid. When order becomes paid, `paymentMessage` is cleared
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
privateAdminNotes | string | Private note about the order from store owner
pickupTime | string | Order pickup time in the store date format, e.g.: `"2017-10-17 05:00:00 +0000"`
taxesOnShipping | Array\<*TaxOnShipping*\> | Taxes applied to shipping. `null` for old orders, `[]` for orders with taxes applied to subtotal only. Are not recalculated if order is updated later manually. Is calculated like: `(shippingRate + handlingFee)*(taxValue/100)`
acceptMarketing | boolean | `true` if customer has accepted email marketing and you can use their email for promotions. If value is `false`, you can't use this email for promotions
refererId | string | Referer identifier. Can be set in storefront via JS or by creating / updating an order with REST API
disableAllCustomerNotifications | boolean | `true` if no email notifications are sent to customer. If `false` or empty, then email notifications are sent to customer according to store mail notification settings. This field does not influence admin email notifications.

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
sku |   string | Product/variation SKU
shortDescription | string | Product description truncated to 120 characters
tax | number | Tax amount applied to the item
shipping | number| Order item shipping cost 
quantityInStock | number | The number of products in stock in the store
isShippingRequired | boolean | `true`/`false`: shows whether the item requires shipping
trackQuantity | boolean | `true`/`false`: shows whether the store admin set to track the quantity of this product and get low stock notifications
fixedShippingRateOnly | boolean | `true`/`false`: shows whether the fixed shipping rate is set for the product
fixedShippingRate | number| Fixed shipping rate for the product
digital | boolean | `true`/`false`: shows whether the item has downloadable files attached
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item

#### OrderItemTax
Field | Type |  Description
--------- | -----------| -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
includeInPrice | boolean | `true` if the tax rate is included in product prices. More details: [Taxes in Ecwid](http://help.ecwid.com/customer/portal/articles/1182159-taxes)

#### OrderItemOption
Field | Type |  Description
--------- | -----------| -----------
**name** |  string | Option name
type |  string | Option type. One of: <ul><li>`CHOICE` (dropdown or radio button)</li><li>`CHOICES` (checkboxes)</li><li>`TEXT` (text input and text area)</li><li>`DATE` (date/time)</li><li>`FILES` (upload file option)</li></ul>
**value** | string | Selected/entered value by customer. Multiple values separated by comma in a single string
files | Array\<*OrderItemOptionFile*\> | Attached files (if the option type is `FILES`)
selections | Array\<*SelectionInfo*\> | Details of selected product options. If sent in update order request, other fields will be regenerated based on information in this field

#### SelectionInfo
Field | Type |  Description
--------- | -----------| -----------
selectionTitle | string | Selection title, as set by merchant
selectionModifier | number | Selection price modifier amount. Value is negative for negative modifiers
selectionModifierType | string | Price modifier type: `"PERCENT"` or `"ABSOLUTE"`

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

#### OrderItemDiscounts
Field | Type  | Description
----- | ----- | -----------
discountInfo | \<*OrderItemDiscountInfo*\> | Info about discounts applied to item
total | number | Total order item discount value for this discount

#### OrderItemDiscountInfo
Field | Type  | Description
----- | ----- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
orderTotal | number | Minimum order subtotal the discount applies to

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
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | Products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<*number*\> | The list of product IDs the coupon can be applied to
categories | Array\<*number*\> | The list of category IDs the coupon can be applied to

#### ShippingOptionInfo
Field | Type | Description
----- | ---- | -----------
shippingCarrierName | string | Optional. Is present for orders made with carriers, e.g. `USPS` or shipping applications.
shippingMethodName | string | Shipping option name
shippingRate | number | Rate
estimatedTransitTime | string | Delivery time estimation. Formats accepted: number "5", several days estimate "4-9"
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
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`
orderTotal | number | Minimum order subtotal the discount applies to

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system
cvvMessage | string  | Credit card verification status returned by the payment system

#### TaxOnShipping
Field | Type | Description
----- | ---- | -----------
name | string | Tax name
value | number | Tax value in store settings, applied to destination zone
total | number | Tax total applied to shipping

#### Response


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

#### Errors

> Error response example

```http
HTTP/1.1 400 Status QUEUED is deprecated, use AWAITING_PAYMENT instead
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
------------|--------
400 | Request parameters are invalid
403 | Access token doesn't have `update_orders` scope 
404 | The order is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot update the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Delete order

Delete a specific order in an Ecwid store

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
**token** |  string |  oAuth token
**orderNumber** | number | Order number. Make sure to use the `orderNumber` value here and not the `vendorOrderNumber`

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
deleteCount | number | The number of deleted orders (`1` or `0` depending on whether the request was successful)


#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description.

#### HTTP codes

HTTP Status | Description
-------------- | --------------
400 | Request parameters are malformed
403 | Access token doesn't have `update_orders` scope 
404 | The order with given number is not found
500 | The delete request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Create order

> Request example

```http
POST /api/v3/4870020/orders?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

```json
{
        "subtotal": 30,
        "total": 40,
        "email": "example@example.com",
        "paymentMethod": "Phone order",
        "tax": 0,
        "paymentStatus": "PAID",
        "customerTaxExempt": false,
        "customerTaxId": "",
        "customerTaxIdValid": false,
        "reversedTaxApplied": false,
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
        "privateAdminNotes": "Must be delivered till Sunday.",
        "acceptMarketing": false,
        "disableAllCustomerNotifications": true
    }
```

`POST https://app.ecwid.com/api/v3/{storeId}/orders?token={token}`

Field | Type | Description
----- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

Create a new order in an Ecwid store. This can be useful for storefronts with a custom checkout process or manually creating orders for sales made earlier.

<aside class="notice">
You can create only one order per API request. To create several orders, send several separate requests for each coupon.
</aside>

<aside class="notice">
    When making a request with <a href='#access-tokens'>public access token</a>, there are a few differences applied: 
    <ul><li>The <strong>paymentStatus</strong> can only be "AWAITING_PAYMENT" or "INCOMPLETE". Any other value is ignored.</li>
        <li>The <strong>fulfillmentStatus</strong> can only be "AWAITING_PROCESSING". Any other value is ignored.</li>
        <li><strong>privateAdminNotes</strong> field is always ignored.</li></ul>
</aside>

#### Request body

A JSON object of type 'Order' with the following fields:

#### Order
Field | Type |  Description
------| -----| ------------
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order (item's `price` x `quantity`)
total | number | Order total cost. Includes shipping, taxes, discounts, etc.
email | string  | Customer email address
paymentMethod | string |  Payment method name
paymentModule | string | Payment processor name
tax | number | Tax total for order. Sum of all tax in order items, unless they were modified after order was placed
customerTaxExempt | boolean | `true` if customer is tax exempt, `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)
customerTaxId | string | Customer tax ID
customerTaxIdValid | boolean | `true` if customer tax ID is valid, `false` otherwise
reversedTaxApplied | boolean | `true` if order tax was set to 0 because customer specified their valid tax ID in checkout process. `false` otherwise
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
**paymentStatus** | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`PARTIALLY_REFUNDED`</li> <li>`INCOMPLETE`</li></ul>. Ignored when creating orders with [public token](#access-tokens)
**fulfillmentStatus** | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li> <li>`READY_FOR_PICKUP`</li> </ul>. Ignored when creating orders with [public token](#access-tokens)
refererUrl | string | URL of the page when order was placed (without hash (#) part)
orderComments | string  | Customer's order comments, specified at checkout
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
customerId | number  | Unique customer internal ID (if the order is placed by a registered user)
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unfinished orders only.
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field 
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values. Total order discount is a sum of all discounts applied to items (both regular discount and discount coupons) unless they were modified after order was placed
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
paymentParams | Map\<*string,string*\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
discountInfo | Array\<*DiscountInfo*\> | Information about applied discounts (coupons are not included)
trackingNumber |  string | Shipping tracking code
paymentMessage | string | Message from the payment processor if any. It is present and visible in order details only if order status is not paid. When order becomes paid, `paymentMessage` is cleared
externalTransactionId | string | Transaction ID / invoice number of the order in the payment system (e.g. PayPal transaction ID)
affiliateId |   string  | Affiliate ID
creditCardStatus | \<*CreditCardStatus*\> | The status of credit card payment
privateAdminNotes | string | Private note about the order from store owner. Ignored when creating orders with [public token](#access-tokens)
pickupTime | string | Order pickup time in the store date format, e.g.: `"2017-10-17 05:00:00 +0000"`
acceptMarketing | boolean | `true` if customer has accepted email marketing and you can use their email for promotions. If value is `false`, you can't use this email for promotions
disableAllCustomerNotifications | boolean | `true` if no email notifications are sent to customer. If `false` or empty, then email notifications are sent to customer according to store mail notification settings. This field does not influence admin email notifications.

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
couponApplied | boolean | `true`/`false`: shows whether a discount coupon is applied for this item
selectedOptions | Array\<*OrderItemOption*\> | Product options values selected by the customer
taxes |  Array\<*OrderItemTax*\> | Taxes applied to this order item
dimensions | \<*ProductDimensions*\> | Product dimensions info

#### OrderItemTax
Field | Type | Description
----- | ---- | -----------
name |  string | Tax name
value | number | Tax value in percent
total | number | Tax amount for the item
includeInPrice | boolean | `true` if the tax rate is included in product prices. More details: [Taxes in Ecwid](http://help.ecwid.com/customer/portal/articles/1182159-taxes)

#### OrderItemOption
Field | Type | Description
----- | ---- | -----------
**name** |  string | Option name
type |  string | Option type. One of: <ul><li>`CHOICE` (dropdown or radio button)</li><li>`CHOICES` (checkboxes)</li><li>`TEXT` (text input and text area)</li><li>`DATE` (date/time)</li><li>`FILES` (upload file option)</li></ul>
**value** | string | Selected/entered value by customer. Multiple values separated by comma in a single string
files | Array\<*OrderItemOptionFile*\> | Attached files (if the option type is `FILES`)
selections | Array\<*SelectionInfo*\> | Details of selected product options. If sent in update order request, other fields will be regenerated based on information in this field

#### SelectionInfo
Field | Type |  Description
--------- | -----------| -----------
selectionTitle | string | Selection title, as set by merchant
selectionModifier | number | Selection price modifier amount. Value is negative for negative modifiers
selectionModifierType | string | Price modifier type: `"PERCENT"` or `"ABSOLUTE"`

#### ProductDimensions
Field | Type  | Description
-------------- | -------------- | --------------
length | number | Length of a product
width | number | Width of a product
height | number | Height of a product

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
applicationLimit | string | Application limit for discount coupons. Possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`
creationDate |  string | Coupon creation date
orderCount | number | Number of uses
catalogLimit |  \<*DiscountCouponCatalogLimit*\> | Products and categories the coupon can be applied to

#### DiscountCouponCatalogLimit
Field | Type | Description
----- | ---- | -----------
products | Array\<*number*\> | The list of product IDs the coupon can be applied to
categories | Array\<*number*\> | The list of category IDs the coupon can be applied to

#### ShippingOptionInfo
Field | Type | Description
----- | ---- | -----------
shippingCarrierName | string | Optional. Is present for orders made with carriers, e.g. `USPS` or shipping applications.
shippingMethodName | string | Shipping option name
shippingRate | number | Rate
estimatedTransitTime | string | Delivery time estimation. Formats accepted: number "5", several days estimate "4-9"
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
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`
orderTotal | number | Minimum order subtotal the discount applies to

#### CreditCardStatus
Field | Type | Description
----- | ---- | -----------
avsMessage | string  | Address verification status returned by the payment system
cvvMessage | string  | Credit card verification status returned by the payment system

#### Response


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

#### Errors

> Error response example

```http
HTTP/1.1 400 Field OrderItem.quantity is absent
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
------------|--------
400 | Request parameters are invalid
403 | Access token doesn't have `create_orders` scope 
404 | The customer or any other linked object is not found in the store
500 | Cannot create an order because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Upload item option file

Using this method, you can attach a file to an order item (implying that the order item has a 'file upload' option). Request parameters specify which order, item and option should be updated. Request body is the file itself (binary data). Maximum allowed file size is 100Mb.

> Request example

```http
POST /api/v3/4870020/orders/20/items/987653/options/Attach+your+image?fileName=my_photo.jpg&token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
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

`POST https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}/items/{itemId}/options/{optionName}?fileName={fileName}&token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**orderNumber** | number | Order number. Make sure to use the `orderNumber` value here and not the `vendorOrderNumber`
**itemId** | number | Order item ID
**optionName** | string | Item product option name, e.g. `Upload your photo`
**fileName** |  string |  Uploaded file name
**token** |  string |  oAuth token
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading an item option file, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description | Code (optional)
--------- | -----------| -----------
400 | Request parameters are malformed | 
402 | The functionality/method is not available on the merchant plan | 
403 | Access token doesn't have `create_orders` scope | 
404 | Order, order item or item option is not found | 
409 | Product option type is not `FILES` | `OPTIONS_IS_NOT_FILES_TYPE`
413 | The file is too large.  Maximum allowed file size is 100Mb. | 
415 | Unsupported content-type: expected `application/octet-stream` | 
500 | Uploading of the file failed or there was an internal server error while processing a file | 

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Delete item option file

Customers can attach files to product options when adding products to cart. Using this method, you can remove that file from an order.

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
**orderNumber** | number | Order number. Make sure to use the `orderNumber` value here and not the `vendorOrderNumber`
**itemId** | number | Order item ID
**optionName** | string | Item product option name, e.g. `Upload your photo`
**fileId** | number | Option file's internal ID
**token** |  string |  oAuth token

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
deleteCount | number | The number of deleted files (`1` or `0` depending on whether the request was successful)

#### Errors

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
403 | Access token doesn't have `update_orders` scope 
404 | Order, order item or item option is not found
409 | Product option type is not `FILES` | `OPTIONS_IS_NOT_FILES_TYPE`
500 | Request failed -- there was an internal server error

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
errorCode | string | Error code


### Delete all item option's files

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
**orderNumber** | number | Order number. Make sure to use the `orderNumber` value here and not the `vendorOrderNumber`
**itemId** | number | Order item ID
**optionName** | string | Item product option name, e.g. `Upload your photo`
**token** |  string |  oAuth token

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
402 | The functionality/method is not available on the merchant plan
403 | Access token doesn't have `update_orders` scope 
404 | Order, order item or item option is not found
500 | Request failed – there was an internal server error

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
errorCode | string | Error code
