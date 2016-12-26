# Cart

## Calculate order details

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
**token** |  string | oAuth token

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

#### DiscountInfo
Field | Type | Description
----- | ---- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
order_total | number | Minimum order subtotal the discount applies to
description | string | Description of a discount (for discounts with base == `CUSTOM`)

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
415 | Unsupported content-type: expected `application/json` or `text/json`
422 | No order items or incorrect items details sent
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

