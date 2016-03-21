# Custom Shipping API
# Overview

Using Custom Shipping API you can integrate a new shipping carrier to provide real time shipping methods with different rates for customers of Ecwid stores. This functionality will work in a form of an application that users install from Ecwid App Market.

<aside class="notice">
Access scope required: <strong>add_shipping_method</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

# How to set up

When [registering a new application](/register) for Ecwid, specify the request URL for your application. Ecwid will be sending order details requests to that endpoint and expect shipping rates in a specific format in response.

# How it works

### 1. User configures app settings in settings tab

After the installation, user would need a page where they can configure it. We recommend using [Native apps feature](#embedded-apps) to provide this functionality.

### 2. Ecwid sends order data to app request URL

When registering your app, provide a request URL where Ecwid will send **POST request** with order data as well as the [Merchant app settings](#merchant-settings) when customer is at checkout stage.

### 3. Application returns the rates in a specific format

Ecwid will expect a response from your service within 10 second interval to display additional shipping methods for customers. Ecwid will expect shipping method name, rate and estimated delivery time in that response. See the format in the [Request and response](#request-and-response) section.

### 4. Ecwid displays the rates at checkout

Based on the response from your app, Ecwid will display the shipping methods for customers at the checkout. Customer can select them just like any other shipping method in that Ecwid store and it will display in the order details.

# Merchant settings

Your application can require merchants to specify their shipping account details, package size and any other user preferences you may require.

First, set up a new tab in Ecwid Control Panel, which will serve as a settings page for your users. This tab will load a page from your server in an iframe in a separate tab of Ecwid Control Panel. See [Native Applications](#native-applications).

When merchant is in the settings tab of your app, your code can create and modify the merchant settings using the **Application storage** feature. It's a simple `key:value` storage, which can serve you as an app database. For your convenience, you can access it [via Javascript](#javascript-storage-api) (client-side) or [Ecwid REST API](#rest-storage-api) (server-side).

Once the settings are saved there, Ecwid will send them in a **POST request** to your application alongside order details when customer is at checkout stage. The request will contain **all data** from your application storage, including public and other keys that were specified.

You can use the `public` key of the application storage to save data for accessing in the storefront. More details on how to handle such data: [Public application config](#public-application-config).

Please make sure **not to pass any sensitive user data in the public application config**, as this information will be available via Ecwid Javascript API to any 3-rd party. To save and get that kind of information, use any other key names in your application storage. They will be provided in a request to your application as well as public information, but not accessible in the storefront.

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
        "weightUnit": "lbs",
        "currency": "USD"
    }
}
```

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
weightUnit | string | Active weight units in the store at the moment of the request
currency | string | Active currency in the store at the moment of the request

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
