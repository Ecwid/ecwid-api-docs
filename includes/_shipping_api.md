# Add Shipping Method

# Overview

> Added custom shipping methods in checkout process

> ![Custom shipping methods in checkout process](https://don16obqbay2c.cloudfront.net/wp-content/uploads/shippingMethod-1468407305.png)

Using Custom Shipping API you can integrate a new shipping carrier to provide real time shipping methods with different rates for customers of Ecwid stores. This functionality will work in a form of an application that users install from the Ecwid App Market.

<aside class="notice">
Access scope required: <strong>add_shipping_method</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

# How to set up

When [registering a new application](/register) for Ecwid, specify the request URL for your application. Ecwid will be sending order details requests to that endpoint and expect shipping rates in a specific format in response.

# How it works

### 1. User configures app settings in settings tab

After the installation, user would need a page where they can configure it: provide their account details, set up dimensions, etc. We recommend using [Native apps feature](#embedded-apps) to provide this functionality. To manage and store those settings, see the [Merchant app settings](#merchant-settings-for-shipping-method) section.

### 2. Ecwid sends order data to app request URL

To show new shipping methods in storefront, Ecwid will send a **POST request** to your endpoint with order details: items, customer address, merchant app settings, etc. That endpoint must respond to the request with the shipping rates for this configuration.

### 3. Application returns the rates in a specific format

Ecwid will expect a response from your service within 10 second interval to display additional shipping methods for customers. In the response, provide shipping method name, rate and estimated delivery time. See the response format in the [Request and response](#request-and-response) section.

### 4. Ecwid displays the rates at checkout

Based on the response from your app, Ecwid will display the shipping methods for customers at the checkout. Customer can select them just like any other shipping method in that Ecwid store and it will be shown in the order details. Shipping methods from the application will be added to any currently enabled shipping methods a store has enabled.

# Merchant settings for shipping method

Your application can require merchants to specify their shipping account details, package size and any other user preferences you may require. We recommend adding a new tab into the Ecwid Control Panel's Shipping settings for optimal experience - Native applications feature.

> Merchant settings example

> ![Merchant settings example](https://don16obqbay2c.cloudfront.net/wp-content/uploads/shippingSettings-1468407629.png)

**Settings**

First, set up a new tab in Ecwid Control Panel, which will serve as a settings page for your users. This tab will load a page from your server in an iframe in a separate tab of Ecwid Control Panel. See [Native Applications](#native-applications).

When merchant is in the settings tab of your app, your code can create and modify the merchant settings using the **Application storage** feature. It's a simple `key:value` storage, which can serve you as an app database. For your convenience, you can access it [via Javascript](#javascript-storage-api) (client-side) or [Ecwid REST API](#rest-storage-api) (server-side).

**Request**

Once the settings are saved there, Ecwid will send them in a **POST request** to your application alongside order details when customer is at checkout stage. The request will contain **all data** from your application storage, including public and other keys that were specified. Use it to idetify the store and a user for the shipping rates.

You can also use the `public` key of the application storage to save data for accessing in the storefront. More details on how to handle such data: [Public application config](#public-application-config).

Please make sure **not to pass any sensitive user data in the public application config**, as this information will be available via Ecwid Javascript API to any 3-rd party. To save and get that kind of information, use any other key names in your application storage. They will be provided in a request to your application as well as public information, but not accessible in the storefront.

**Response**

After you get a request from Ecwid, your application endpoint should get its components and return correct rates back to the customer in a response.

# Request and response

### Request

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
        "handlingFee": {
            "name": "Handling Fee",
            "value": 0,
            "description": ""
        },
        "items": [
            {
                "weight": 1.2,
                "price": 2,
                "amount": 1,
                "productId": 65955001,
                "name": "Apple",
                "categoryId": 19175294,
                "sku": "30022537",
                "selectedOptions": [
                    {
                        "name": "Box type",
                        "value": "Big",
                        "valuesArray": [
                            "Big"
                        ]
                    }
                ]
            },
            {
                "weight": 0.3,
                "price": 5.08,
                "amount": 1,
                "productId": 66568001,
                "name": "Orange",
                "categoryId": 19175294,
                "sku": "02266183",
                "selectedOptions": null
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
        "currency": "USD"
    }
}
```

Name | Type    | Description
---- | ------- | --------------
storeId |  number | Ecwid store ID
merchantAppSettings | json | Merchant settings for your integration set up by your code. [More details](#merchant-settings-for-shipping-method)
cart | \<*CartDetails*\> | Offset from the beginning of the returned items list (for paging)

### CartDetails

Name | Type    | Description
---- | ------- | --------------
subtotal |  number | Order subtotal
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
weightUnit | string | Active weight units in the store at the moment of the request
currency | string | Active currency in the store at the moment of the request
shippingAddress | \<*ShippingAddressInfo*\> | Shipping address details (destination)
originAddress | \<*OriginAdressInfo*\> | Origin address details (departure)

#### OrderItems
Field | Type |  Description
--------- | -----------| -----------
productId | number | Store product ID
categoryId |  number  | ID of the category this product belongs to. If the product belongs to many categories, categoryID will return the ID of the default product category. If the product doesn't belong to any category, `0` is returned
price | number | Price of ordered item in the cart
weight |  number | Product weight
sku | string | Product SKU. If the chosen options match a combination, this will be a combination SKU.
amount |  number | Amount purchased
name |  string | Product name
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

#### HandlingFeeInfo
Field | Type | Description
----- | ---- | -----------
name | string | Handling fee name set by store admin. E.g. `Wrapping`
value | number | Handling fee value
description | string | Handling fee description for customer

### ShippingAddressInfo

Name | Type    | Description
---- | ------- | --------------
street | string | Customer's street
city | string | Customer's city
countryCode | string | Customer's country code in Ecwid
countryName | string | Customer's country name in Ecwid
postalCode | string | Customer's postal code
stateOrProvinceCode | string | Customer's state or province code in Ecwid
stateOrProvinceName | string | Customer's state or province name in Ecwid

### OriginAdressInfo

Name | Type    | Description
---- | ------- | --------------
street | string | Customer's street
city | string | Customer's city
countryCode | string | Customer's country code in Ecwid
postalCode | string | Customer's postal code
stateOrProvinceCode | string | Customer's state or province code in Ecwid

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

### ShippingOptions

Name | Type    | Description
---- | ------- | --------------
**title** | string | Shipping method name
**rate** | number | Shipping rate amount
**transitDays** | string | Estimated delivery time. Formats accepted: empty `""`, number `"5"`, several days estimate `"4-9"`

<aside class="notice">
Response parameters in <strong>bold</strong> are mandatory
</aside>

## Weight units

Ecwid supports several weigh units that can be passed in the request to your application to provide shipping rates. Below you can see all available units as well as their conversion values for calculation. 

Name | Code | Value
-----|------|------
Carat | carat | 0.2
Gram | gram | 1
Ounce | ounce | 28.35
Pound | lbs | 453.6
Kilogram | kg | 1000

Gram is the main weight unit, from which other units are converted. Merchants can change weight units in [Ecwid Control Panel](https://my.ecwid.com/cp/CP.html#formats-units).

# Troubleshooting

### A new shipping method at checkout is not appearing

You created an app and installed it on your test store, but a new shipping method, returned by your resource, is not appearing when you open your store. There are several possible reasons for this:

* **The application is not configured properly** to add a shipping method at checkout. E.g. during registration, you haven't provided a link for Ecwid to send the requests to when customer is at checkout or the URL is incorrect. See [How to set uo](#how-to-set-up) for the details.
* **`add_shipping_method` access scope** is missing in the list of requested scopes while installing the app. While creating an oAuth URL or installing your app from an app details page, make sure it incudes the `add_to_cp` scope in the list of requested permissions. 
* **The response format from your resource is incorrect**. Ecwid accepts response from shipping applications in [a strict format](#request-and-response), so please make sure your endpoint is responding correctly back to Ecwid with the correct shipping methods. 
* **The response from your resource has exceeded 10 second timeout**. When Ecwid sends a request for additional shipping methods to external resources, it expects to get a response within the 10 second timeout period. Make sure that your service is able to provide response in that time period.
* **You're testing it in an Ecwid store which is on Free plan**. Ecwid API functionality is available on paid Ecwid plans only. Please upgrade your account or [contact us](/contact).
