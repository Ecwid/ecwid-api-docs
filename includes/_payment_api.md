# Add Payment Method

With the Custom Payment API you can integrate a new payment method for customers of Ecwid stores to choose from. This functionality will work in the form of an application that users install from the Ecwid App Market.

<aside class="notice">
Access scope required: <strong>add_payment_method, read_orders, update_orders</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

**Table of contents**: 

- [How it works](https://developers.ecwid.com/api-documentation/how-payment-method-works)
- [Set up your application](https://developers.ecwid.com/api-documentation/set-up-payment-method)
- [Saving merchant settings](https://developers.ecwid.com/api-documentation/merchant-settings-for-payment-method)
- [Payment request from Ecwid – Details](https://developers.ecwid.com/api-documentation/processing-payment-request)

You can find existing payment processor integrations on this page: [Payment Gateways](https://www.ecwid.com/apps/paymentgateways)

## How payment method works

### Payment overview

> Checkout flow for payment apps

> ![Checkout flow for payment apps](https://don16obqbay2c.cloudfront.net/wp-content/uploads/Payment-Method-Diagram-1511788692.png)

#### 1. User configures app settings in settings tab

After the installation, user would need a page where they can configure it. We recommend using [Native apps feature](#embedded-apps) to provide this functionality. 

In any case, your payment method will be displayed in the payment settings of the store control panel to edit the title, description, position at checkout, and availability.

#### 2. Ecwid sends order data to app payment URL

After registering your app, contact us and provide a payment URL where Ecwid will send **POST request** with order data as well as the [Merchant app settings](#merchant-settings-for-payment-method) when customer is at checkout stage. 

Your app will need to get order details from the request, decrypt it and the information to a payment processor in a correct format, where customer can pay for that order. 

#### 3. Customer finishes the checkout

In a storefront, customer will see a new payment method to choose from. If they choose this new method to pay for their order, they will be directed to payment URL of your app to finish the checkout process.

Upon finishing the payment, your app must [update the order status](https://developers.ecwid.com/api-documentation/orders#update-order) and [return customer to the storefront](https://developers.ecwid.com/api-documentation/processing-payment-request#returning-customer-to-storefront).

### Payment installation process

There are two sources where merchants can add new payment methods: 

1. Payment settings in the Ecwid Control Panel
2. A search in the Ecwid App Market

Let's check them in more details: 

#### Payment settings in the Ecwid Control Panel

**Find new payment method**

Payment settings are located in *Ecwid Control Panel > Settings > Payment*. On that page merchants can edit existing payment methods and add new ones.  

If your payment application has been published in the Ecwid App Market and it's publicly available, merchants can find it in: *Add new payment methods > Other ways to get paid > Choose payment processor*.

Merchant will see your payment method in the drop down list depending on availability in their country. Make sure to specify the countries when submitting your app for review. 

The payment method name and logo will be used from your existing app settings. 

**Add new payment method**

When a merchant clicks on your payment method from the available list, Ecwid will automatically install your application into that store. 

Then, the payment method of your application will appear in the list of added payment methods in the payment settings of that store.

If your application [works in the Ecwid Control Panel](https://developers.ecwid.com/api-documentation/native-applications), Ecwid will open your app interface in a popup. If your application [works externally](https://developers.ecwid.com/api-documentation/external-applications), Ecwid will redirect merchant to a new tab to continue the set up.

#### A search in the Ecwid App Market

**Find new payment method**

Merchants can search for your payment method in the Ecwid App Market. If your app is published and is publicly available, it will appear in the search results.

Next, a merchant will click on your application and will be directed to your app details page. It will describe your integration in more details with screenshots. 

**Add new payment method**

To add a payment method from app details page, merchants will click the 'Install' button and the installation will be the same as any other application from the Ecwid App Market. 

If your applicaiton [works in the Ecwid Control Panel](https://developers.ecwid.com/api-documentation/native-applications), user will be directed to your application tab and set up the connection. If your app [works externally](https://developers.ecwid.com/api-documentation/external-applications), the installation will continue on your website. 

As a result, a new payment method will appear among existing ones in the payment settings of merchant's Ecwid Control Panel.

### Payment availability

The payment method added by your app works just like any other payment method added by a merchant. 

It is possible to quickly enable/disable the payment method in a storefront right in the payment settings of the Ecwid Control Panel: find your payment method > switch the toggle to the disabled state.

If you need to remove the payment method completely, go to Ecwid Control Panel > Apps > My Apps and remove the app from the list.

## Set up payment method

After you [registered a new application](/register) for Ecwid, send the payment URL for that new payment method by [contacting us](/contact). Ecwid will be sending order details requests to payment URL endpoint and expect the order status to be changed after the payment is complete.

## Merchant settings for payment method

Your application can require merchants to specify their account details in your system and any other user preferences you may require.

### Merchant account settings

> Merchant settings example

> ![Merchant settings example](https://don16obqbay2c.cloudfront.net/wp-content/uploads/paymentSettings-1468408208.png)

Set up a new tab in Ecwid Control Panel, which will serve as a settings page for your users. This tab will load a page from your server in an iframe in a separate tab of Ecwid Control Panel. See [Native Applications](#native-applications) for more information.

When merchant is in the settings tab of your app, your code can create and modify the merchant settings using the **Application storage** feature. It's a simple `key:value` storage, which can serve you as an app database. For your convenience, you can access it [via Javascript](https://developers.ecwid.com/api-documentation/storage-in-ecwid-api#javascript-storage-api) (client-side) or [Ecwid REST API](https://developers.ecwid.com/api-documentation/storage-in-ecwid-api#rest-storage-api) (server-side).

### Edit payment method

When your app is installed in merchant's store, merchants are able to change its name, position and description just like any other payment method. 

**Change title and description**

You can edit payment method added by your app just like any other payment method in *Ecwid Control Panel > Settings > Payment > Select payment method > Actions > Edit*.

A merchant will see a standard interface for editing payment method: edit payment method name, description, instruction title and text.

**Sorting**

Using the Actions button merchant can also sort payment methods in the way they prefer – move it up or down the list of available methods. 

**Account settings**

If a merchant clicks on Account Settings, then Ecwid will open embedded interface of your application in a popup on the same page.

Important: Account Settings button will only be visible if your app is [Native](https://developers.ecwid.com/api-documentation/native-applications).

**Availability**

It is possible to quickly enable/disable the payment method in a storefront right in the payment settings of the Ecwid Control Panel: *find your payment method > switch the toggle to the disabled state*.

If a merchant needs to remove a payment method completely, go to *Ecwid Control Panel > Apps > My Apps > remove the app from the list*.

### Request

Once the settings are saved there, Ecwid will send them in an HTML form with a **POST request** to your payment URL with order details when customer chooses to pay with the new payment method. The request will contain **all data** from your application storage, including public and other keys that were specified.

You can use the `public` key of the application storage to save data for accessing in the storefront. More details on how to handle such data: [Public application config](#public-application-config).

Please make sure **not to pass any sensitive user data in the public application config**, as this information will be available via Ecwid Javascript API to any 3-rd party. To save and get that kind of information, use any other key names in your application storage. They will be provided in a request to your application as well as public information, but not accessible in the storefront.

## Processing payment request

### Request details

> Encoded request from Ecwid example

```http
POST https://mycoolapp.com/integration HTTP/1.1
```

```html
<form action="https://mycoolapp.com/integration" method="POST" accept-charset='utf-8'>
<input type='hidden' name='data' value="XwYHGNpzBfQesQl4JkZclImcnQXkGhClU2c4-O-8WXZR3N7jx4xkmzbOALXi99ZiV4rpIOOQx6ua_8MC14Emwh4gqOrPqbYctZ54xkbUVmkArtt5TcJd9JngD8C2-DEKJ96Egfi-1RJEgXDau3cUWpelUBXecdAd5g01MW_PZEJzwn8c9WawRw8jLC_kKJX68TU-_jET9GVDaP3rjxmssYPyoSWrMxUNKwaEyzZbdVNkHgQZq6dScNqL1JjP2bnKR7PlK0FhkvNNlC7G0PjnAfSAvw41zdIu2UjYutiba2gwQf5iYH-4J6J8sM4iRmTlFMEJwAFClffXRjacntxnV3PsLNvbOp8VFC2hzU2fsOQBSM8YmhsDgTRDdjQK1u7j2kiUuJbTEAijsQjoxicDlQJJ_tYh-6pfsyop9plL9ifhOylrUUe4VOsAVNi7iQ1UCVfuutsbm8aMYMQY4NmFKVK2K6Q_tguI5-NN9r9aG16MB3lFAPPI9SJgw0W9ucBSF8mWKQ6DVV9w7iFnq5Clw4TGCYw3QA_pb02TeCDZS8P-cFau21BPUas-K_JuebUD3BhGct8gHqpP_nqwwBj97bN94muerSxveT4m_jifR_bsbRZgDxtoDP8jiOK9O7ACWXsZhuTUhMxXYaQ8F3e0AIHp2FgQmL6pc0lTMZaiyqPr14WdYyqKHeG0Z8OrRVN3_zvfysb-SahJs18DElm8pBFqOrAiopEfFoAEll5cX5ZC09w9wy6SkheZOvv0l2KoH_lS6ovPgw6AArZw8lvNEQPOoDy4VQIofSOUYezCYwYDqQ1Im0yR3tLKvuONFPNUdQ5qkWCvyRhZyGqIs2VIiRJV6ErWmOYS_06Ri12Q3236Q3W2bKuEAMca6d9_PL51Q_GtBuxk5bWTTvTqDA3uL2Cy7WdE-VPesRmgmngozlbjjWQfXrS8slhFZpQvy0Pjqp0c7iNKhG7dkTLlpqFFGLCGpQ6l8yMhzluG0ZTOTy7iuNxcSWfBUhy1ZnOS-M8LzLY5UZYxvsK5PHyEdQjOxqxUfO0ORkNEFmhM33f4XLB6EuPyT2x4yt8F3DmctsHXdfuWYyzEHHe86IjqWOuFAzLMo0EhDVSAxtVvWt3FQlWBw-vRGAgG2XtXcNSbb00wj2T3uISHOlhnjPKs1nYMrXTKSxQ5X98YHpmp-09eOu_zkidYquhoJo3ioldzMYBI08wI9WFUzEc7ALr0YPr3XoVRqKvolVNExcPgCHEe4BcIcgsoPzLm5UolBST6_fB4sGJGxiKAfgAUb9Bv_PT7GYSbIfcfn17UPaO_1nXuQcXAB3b1jGbZoCaJbXhBJVGc1vZAzdAq69eyIuiQygWSuSLCK9DEffqtqfBq_YwDcQIbSv9ZTcVyRXYQYGNCyAdER5wE9B2dv0qe7wxSFElt3t6cGPSBZnA8-pcNzsUhBChaYJuPj2JJRGSkvdAjnl3NREbxogqayPGbqVf_lYKp9Q192seMKw99dswCB9EIDThdOwFu_sXeo57RolqFqOknzYI0QedcBb6sEv_g_OwMNZZW3oxCEIB76k-6zgUBiAQhK0s95NzlFrJ9cCO3Kvk-6NS-du_Ow3uSiRmCSbgA5djAnjx--XbJZpCo4vQzOwsV0CHjtato3KiZjyTRw5d4aJZt4Vp-fAwOS9PvXAordsVPL7Ns0IiaKb81EF0bwCDLZRE-oR4nndiiyVhK-JlRbSERm32VPPu7271PaXGvxO8sSGyTLhX1ABHKeteboX1TzXM-TPq7-uXeDQw5bJe6lLMn6438NhxsxHa7bygktYvSs6hoZY7qq6oPTs5u_N_8qVKW7T2UpHpFKyjVCPYpThIGG1Onx3B6jZ74QAnHhYT5DVgg4j2pJoy3pGWLZ6hn8BRto0-eUT4fUT2e5-37SVfRvCY56jm69nEKbbq3e98I6wa6PvIGUILbGjN0LpuT4h0Q2b4pJDyNqPV8sb4dLjXYwRwotpc6lhxEHQylIxIWsfso4DSZ4xGVA8u9sLaNSNO3yMgME1jZ7z5GGo7m4jXIMLW-LYmB9razHuavXZl8KmUsNF8PcM6LwK0sHUyPcTwgwWqOaADPP08PXFMcMtlywg28Kt56gpJaeH_FLloEU3DYPEEfawEzoqxbu18tCr3Gwc6LqkxsyNCJvuQ63DWee5Ff5FB-NDrGm9T4sd69y0OttyY1I96GTIHj-aPxDfd9r_ZJdcBQOpYao_DZtS8aT4p_hPedaj978cvYGJSjpZZFctsx5SyYOGWL48Jce2rAKswRy0fkx15PUIgH3Yp5Kf7lEl--lr3IpNI8FbmPCC0KyfqymffFT1vtUG64B4CQxeYavOoszOHgAAbqKBXKSmwfYfalAnXaFQWmMyAiGJHx7-7xzxJ6IXvBkwaKXsaeBnFZ1sC0rp4MM3FT48q6o9hIrUV731Wg_t_EkH6aS71jD8xxxHwq3AbNEWIcUfPve8b55Re0_ttOky6VV7yOGQyTATk4gCchy_2HAH4P9O_rrVL2ir9rwX8Jk2xD5In5lWt-KYaY2Uyanhs7ac5X2SOdJJXqlg0fbhiHJAbsCkG_jwOC38K-JL0p6RRoKsV44GO0Qcqh6vzDWVhDPdYq4JAK_odBlDOdfHWfbATPftrmEahKeyNctlBP7g_oYtBmkUn-2uSVv670L3GPzLS8daxEERKv9DF1BoIXI3Ms4EXrUV5HwNafceMt-hS05AEpUl13srE_fL0xMJGmI3tgg-t6S-H_NMYqs85sgjclTrZkluHRY_2qGD8KgxTm-CV5IZ7gihuH68sjYXS-sCGTrHCTGVG1KkBrbkmAjut3ZTIgIoL-6p-begqeq9SLSQ0MDcBXIDa-DPtHnctAEv-vQmvOOq76YRcAC5FYwQZT5rj6ns0bqTWC24zLtQqxQFC9an802zVuIczLJoSD"/>
</form>
```

> Decoding the request to get order details

```php
<?php
function getEcwidPayload($app_secret_key, $data) {
  // Get the encryption key (16 first bytes of the app's client_secret key)
  $encryption_key = substr($app_secret_key, 0, 16);

  // Decrypt payload
  $json_data = aes_128_decrypt($encryption_key, $data);

  // Decode json
  $json_decoded = json_decode($json_data, true);
  return $json_decoded;
}

function aes_128_decrypt($key, $data) {
  // Ecwid sends data in url-safe base64. Convert the raw data to the original base64 first
  $base64_original = str_replace(array('-', '_'), array('+', '/'), $data);

  // Get binary data
  $decoded = base64_decode($base64_original);

  // Initialization vector is the first 16 bytes of the received data
  $iv = substr($decoded, 0, 16);

  // The payload itself is is the rest of the received data
  $payload = substr($decoded, 16);

  // Decrypt raw binary payload
  $json = openssl_decrypt($payload, "aes-128-cbc", $key, OPENSSL_RAW_DATA, $iv);
  //$json = mcrypt_decrypt(MCRYPT_RIJNDAEL_128, $key, $payload, MCRYPT_MODE_CBC, $iv); // You can use this instead of openssl_decrupt, if mcrypt is enabled in your system

  return $json;
}

// Get payload from the POST and process it
$ecwid_payload = $_POST['data'];
$client_secret = "payment-app-secret-key"; // this is a dummy value. Please place your app secret key here

// The resulting JSON array will be in $result variable
$result = getEcwidPayload($client_secret, $ecwid_payload);
?>
```

When customer tries to pay with your payment method, Ecwid will send a POST request with a format as described on the right. 

The value of the `data` input is encoded with a **AES-128** mechanism, where the first 16 characters is the `client_secret` of your application, which serves as a key to the decoding process. To find out more on how to decode the value, see the example code in **Step #1** of [Server-side Native Apps](https://developers.ecwid.com/api-documentation/authentication-in-embedded-apps#enhanced-security-user-auth) section.

> Decoded request from Ecwid example

```json
{
    "storeId": 41002,
    "returnUrl": "https://mdemo.ecwid.com/?orderId=106002&clientId=payment-integration",
    "merchantAppSettings": {
        "public":"{color : \"red\", storeName : \"Cool Mittens Ltd.\"}",
        "id": "1234567890",
        "username": "mittensstore"
    },
    "cart": {
        "currency": "USD",
        "order": {
            "referenceTransactionId":"transaction_55885213",
            "subtotal": 1.15,
            "total": 14,
            "email": "john@example.com",
            "paymentModule": "CUSTOM_PAYMENT_APP-payment-integration",
            "paymentMethod": "Cool payment",
            "tax": 0,
            "ipAddress": "127.0.0.1",
            "couponDiscount": 0,
            "paymentStatus": "INCOMPLETE",
            "fulfillmentStatus": "AWAITING_PROCESSING",
            "refererUrl": "https://mdemo.ecwid.com",
            "volumeDiscount": 4,
            "membershipBasedDiscount": 0,
            "totalAndMembershipBasedDiscount": 0,
            "discount": 1.15,
            "usdTotal": 14,
            "globalReferer": "https://mdemo.ecwid.com",
            "createDate": "2016-04-26 09:14:51 +0000",
            "createTimestamp": 1461662091,
            "items": [{
                "id": 111001,
                "productId": 61003,
                "categoryId": 48003,
                "price": 1.15,
                "productPrice": 1.15,
                "sku": "00007",
                "quantity": 1,
                "shortDescription": "Radish \n The radish (Raphanus sativus) is an edible root vegetable of the Brassicaceae family that was domesticated in ...",
                "tax": 0,
                "shipping": 1,
                "quantityInStock": 0,
                "name": "Radish",
                "isShippingRequired": true,
                "weight": 0.31,
                "trackQuantity": false,
                "fixedShippingRateOnly": false,
                "imageUrl": "https://images.ecwid.com/store/default-store/00007-sq.jpg",
                "smallThumbnailUrl": "https://images.ecwid.com/store/default-store/00007-80-sq.jpg",
                "fixedShippingRate": 0,
                "digital": false,
                "productAvailable": true,
                "couponApplied": false,
                "selectedOptions": [{
                    "name": "Color",
                    "value": "Blue",
                    "valuesArray": ["Blue"],
                    "type": "CHOICE"
                }]
            }],
            "billingPerson": {
                "name": "John Doe",
                "companyName": "Some Company",
                "street": "5th Avenue",
                "city": "New York",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "10002",
                "stateOrProvinceCode": "NY",
                "stateOrProvinceName": "New York",
                "phone": ""
            },
            "shippingPerson": {
                "name": "John Doe",
                "companyName": "Some Company",
                "street": "5th Avenue",
                "city": "New York",
                "countryCode": "US",
                "countryName": "United States",
                "postalCode": "10002",
                "stateOrProvinceCode": "NY",
                "stateOrProvinceName": "New York",
                "phone": ""
            },
            "shippingOption": {
                "shippingMethodName": "U.S.P.S. First Class",
                "shippingRate": 10,
                "estimatedTransitTime": "2"
            },
            "handlingFee": {
                "value": 0
            },
            "additionalInfo": {
                "google_customer_id": "123123.12312312"
            },
            "paymentParams": {},
            "hidden": false
        }
    },
    "token": "abcdefghijklmnopqrstuv1234567890"
}
```

After you decode the payload, you will get a JSON formatted string with the store and order details to allow customer pay for the order. Fields include:

Name | Type    | Description
---- | ------- | --------------
storeId |  number | Ecwid store ID
returnurl | string | A URL to send customer to after the payment. [More details](https://developers.ecwid.com/api-documentation/processing-payment-request#returning-customer-to-storefront)
merchantAppSettings | json | Merchant settings for your integration set up by your code. [More details](#merchant-settings-for-payment-method)
cart | \<*CartDetails*\> | Offset from the beginning of the returned items list (for paging)
token | string | Access token of the Ecwid store. Use it to update order status after the payment

#### CartDetails

Name | Type    | Description
---- | ------- | --------------
currency | string | Code of the currency currently enabled in the store
subtotal |  number | Order subtotal. Includes the sum of all products' cost in the order
referenceTransactionId | string | Unique transaction identification. Used to update order status after payment is processed. See [Updating order status](https://developers.ecwid.com/api-documentation/processing-payment-request#updating-order-status)
total | number | Order total cost. Includes shipping, taxes, discounts, etc.
email | string  | Customer email address
paymentMethod | string | Payment method name as specified when registering the app
paymentModule | string | Payment processor name in Ecwid
tax | number | Tax total
ipAddress | string  | Customer IP
couponDiscount | number | Discount applied to order using a coupon
paymentStatus | string |    Payment status. Supported values: <ul><li>`AWAITING_PAYMENT`</li> <li>`PAID`</li> <li>`CANCELLED`</li> <li>`REFUNDED`</li> <li>`INCOMPLETE`</li></ul>
fulfillmentStatus | string |    Fulfilment status. Supported values: <ul><li>`AWAITING_PROCESSING`</li> <li>`PROCESSING`</li> <li>`SHIPPED`</li> <li>`DELIVERED`</li> <li>`WILL_NOT_DELIVER`</li> <li>`RETURNED`</li></ul>
refererUrl | string | URL of the page when order was placed (without hash (#) part)
volumeDiscount | number | Sum of discounts based on subtotal. Is included into the `discount` field
membershipBasedDiscount | number | Sum of discounts based on customer group. Is included into the `discount` field
totalAndMembershipBasedDiscount | number | The sum of discount based on subtotal AND customer group. Is included into the `discount` field
discount | number | The sum of all applied discounts **except for the coupon discount**. To get the total order discount, take the sum of `couponDiscount` and `discount` field values
usdTotal | number | Order total in USD
globalReferer | string | URL that the customer came to the store from
createDate | date | The date/time of order placement, e.g `2014-06-06 18:57:19 +0000`
createTimestamp | number | The date of order placement in UNIX Timestamp format, e.g `1427268654`
items | Array\<*OrderItem*\> | Array of customer's order items
shippingPerson | \<*AddressDetails*\> |  Shipping address details of a customer. Can be missing if no products in cart require shipping
billingPerson | \<*AddressDetails*\> | Billing address of the customer. Can be missing if merchant disabled it in Ecwid Control Panel > Settings > General > Cart.
shippingOption | \<*ShippingOptionInfo*\> | Details of the shipping method selected
handlingFee | \<*HandlingFeeInfo*\> | Handling fee details
additionalInfo | Map\<*string,string*\> | Additional order information if any
paymentParams | Map\<*string,string*\> |  Additional payment parameters entered by customer on checkout, e.g. `PO number` in "Purchase order" payments
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unsfinished orders only.


#### OrderItem

Name | Type    | Description
---- | ------- | --------------
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

#### AddressDetails

Name | Type    | Description
---- | ------- | --------------
street | string | Customer's street
city | string | Customer's city
companyName | string | Customer's company name
countryCode | string | Customer's country code in Ecwid
countryName | string | Customer's country name in Ecwid
postalCode | string | Customer's postal code
stateOrProvinceCode | string | Customer's state or province code in Ecwid
stateOrProvinceName | string | Customer's state or province name in Ecwid
phone | string | Customer's phone number

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

### Updating order status

> Update order status example

```http
PUT /api/v3/4870020/orders/transaction_55885213?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
    "paymentStatus": "PAID"
}
```

For Ecwid to find out the result of the payment, your application must update the order status before returning them back to the storefront. 

To update order status, you will need these details: **reference transaction id**, **store ID** and **access token**. 

All of these details are provided in a request to your application's payment URL in corresponding fields: 

- `referenceTransactionId` field in the `cart` object of a request body
- `storeId` field in the request body
- `token` field in the request body

Once the order is updated with correct status, your app should return the customer back to the store – see below. 

### Returning customer to storefront

When a customer is finished making their payment for an order, your app needs to return them back to the storefront.

`returnurl` is a field provided in a request from Ecwid. It's value is a destination, where your app should return the customer to after the payment process is complete. 

After user is directed to that page, Ecwid will check that order and depending on its status, the action will be different: 

- If the order is in `PAID` or `QUEUED` payment status, customer's cart will be cleared and they will see 'Thank you for your order' page
- If the order is in `INCOMPLETE` payment status, customer will see the cart page of Ecwid storefront with the same items
- If the order is in `CANCELLED` payment status, Ecwid will show the 'Payment error' page







