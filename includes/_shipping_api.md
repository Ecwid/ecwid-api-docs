# Custom Shipping API
# Overview

<aside class="notice">
<strong>Note</strong>: this feature is currently in the development and will be available soon.
</aside>

Custom Shipping API allows you to integrate a new shipping method to provide real time shipping rates for customers of Ecwid stores. This functionality will work in a form of an application that users install from Ecwid App Market.

To provide the ability to display shipping rates from a carrier that is not currently supported by Ecwid, application would need to request the `add_shipping_method` access scope from the Ecwid merchants. Also, the application would need to provide a URL where Ecwid will send order data to get the shipping rates from in a response.

<aside class="notice">
Access scope required: <strong>add_shipping_method</strong>
</aside>

# How it works

The main workflow of the application happens when customer is in checkout and order is formed. Steps include:

### 1. Ecwid sends order data to app request URL

When registering your app, provide a request URL where Ecwid will send order data as well as the [merchant app settings](#merchant-settings) that your integration requires. These settings are kept in the Application storage of your app.

### 2. Application returns the rates in a specific format

Ecwid will expect a response from your service within 10 second interval to display the shipping rates for the customer. In the response from your application there should be several values present: the rate itself, the name of the method and a delivery estimation. See the format in the Request and response section.

### 3. Ecwid displays the rates in checkout process for customer

Based on the response from your service, Ecwid will display the shipping rates for customer in the checkout process. Customer can select them just like any other shipping method in that Ecwid store and it will display in the order details.

## Merchant settings

To provide customers with correct shipping rates your application can require merchants to specify their shipping account details, package size and any other user preferences you may require.

To get and set those preferences, use [Application storage](#application-storage) feature of Ecwid API. Once specified for the store, Ecwid will send this data in each request to your server to calculate the shipping rates correctly. 

# Request and response

> Ecwid request example

```http
POST https://mycoolapp.com/integration HTTP/1.1

{
    "storeId": 35002,
    "merchantAppSettings": {
    	"public":"{color : \"red\", storeName : \"Cool Socks Ltd.\"}",
        "dimensions": "5x10x7",
        "userId": "12345"
    },
    "cart": {
        "items": [{
            "weight": 0.32,
            "price": 11.98,
            "amount": 1
        }, {
            "weight": 0.90,
            "price": 9.98,
            "amount": 1
        }],
        "shippingAddress": {
            "street": "5th Avenue",
            "city": "New York",
            "countryCode": "US",
            "countryName": "United States",
            "postalCode": "10002",
            "stateOrProvinceCode": "NY",
            "stateOrProvinceName": "New York"
        },
        "originAddress": {
            "street": "30 Mortensen Avenue",
            "city": "Salinas",
            "countryCode": "US",
            "postalCode": "93905",
            "stateOrProvinceCode": "CA"
        },
        "weight": 1.22,
        "weightUnit": "lb",
        "currency": "USD"
    }
}
```

This is the details of the step where Ecwid sends order data to app request URL. 

Name | Type    | Description
---- | ------- | --------------
storeId |  number | Ecwid store ID
merchantAppSettings | json | Merchant settings for your integration set up by your code
cart | \<*CartDetails*\> | Offset from the beginning of the returned items list (for paging)

### CartDetails

Name | Type    | Description
---- | ------- | --------------
items | Array\<*OrderItems*\> | Array of customer's order items with basic details
shippingAddress | \<*ShippingAddressInfo*\> |  Shipping address details of a customer (ship to)
originAddress | \<*OriginAdressInfo*\> | Origin address of the store (ship from)
weight | number | Total weight of the order
weightUnit | string | Weight units
currency | string | Currency code for the order

### OrderItems

Name | Type    | Description
---- | ------- | --------------
weight | number | Order item weight
price | number | Order item price (including tax)
amount | number | Quantity of product in the cart

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

> Response example

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
title | string | Shipping method name
rate | number | Shipping rate amount
transitDays | string | Estimated delivery time

<aside class="notice">
Parameters in bold are mandatory
</aside>

