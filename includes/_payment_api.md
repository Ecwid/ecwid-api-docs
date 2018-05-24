# Add Payment Method

With the Custom Payment API you can integrate a new payment method for customers of Ecwid stores to choose from. This functionality will work in the form of an application that users install from the Ecwid App Market.

<aside class="notice">
Access scope required: <strong>add_payment_method, read_orders, update_orders</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

**Table of contents**: 

- [How it works](https://developers.ecwid.com/api-documentation/how-payment-method-works)
- [Set up payment URL](https://developers.ecwid.com/api-documentation/set-up-payment-method)
- [Saving merchant settings](https://developers.ecwid.com/api-documentation/merchant-settings-for-payment-method)
- [Payment request from Ecwid – Details](https://developers.ecwid.com/api-documentation/processing-payment-request)

You can find existing payment processor integrations on this page: [Payment Gateways](https://www.ecwid.com/apps/paymentgateways)

## How payment method works

### Payment overview

> Checkout flow for payment apps
> 
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

After you [registered a new application](/register) for Ecwid, **send your payment URL** to [Ecwid team](/contact). 

Ecwid will be sending order details requests to payment URL endpoint and expect the order status to be changed after the payment is complete.

Next, start working on payment processing page according to the documentation: [https://developers.ecwid.com/api-documentation/processing-payment-request](https://developers.ecwid.com/api-documentation/processing-payment-request)

### Show payment options as icons

> Add payment options icons below your payment method example

```js

// Set payment method title that matches merchant's payment method title set in Ecwid Control Panel. Use public access token to get it from payment settings in store profile
//
// Public token: https://developers.ecwid.com/api-documentation/access-tokens#public-access-token
//
// Store profile: https://developers.ecwid.com/api-documentation/store-information#get-store-profile

    var paymentMethodTitle = "PayPal";

// Custom styles for icons for our application

    var customStyleForPaymentIcons = document.createElement('style');
    customStyleForPaymentIcons.innerHTML = ".ecwid-PaymentMethodsBlockSvgCustom { display: inline-block; width: 40px; height: 26px; background-color: #fff !important; border: 1px solid #e2e2e2 !important;}";

    document.querySelector('body').appendChild(customStyleForPaymentIcons);

// Set your custom icons or use your own URLs to icons here

    var iconsSrcList = [
        'https://djqizrxa6f10j.cloudfront.net/apps/ecwid-api-docs/payment-icons-svg/paypal.svg',
        'https://djqizrxa6f10j.cloudfront.net/apps/ecwid-api-docs/payment-icons-svg/mastercard.svg',
        'https://djqizrxa6f10j.cloudfront.net/apps/ecwid-api-docs/payment-icons-svg/visa.svg',
        'https://djqizrxa6f10j.cloudfront.net/apps/ecwid-api-docs/payment-icons-svg/amex.svg'
    ];

// Function to process current payment in the list

    var getPaymentContainer = function(label) {
        var container = label.parentNode.getElementsByClassName('payment-methods');
        if (container.length === 0) {
            container = [document.createElement('div')];
            container[0].className += 'payment-methods';
            container[0].style.paddingLeft = '18px';
            label.parentNode.appendChild(container[0]);
        }
          return container[0];
    }

// Function to process the payment page

    var ecwidUpdatePaymentData = function() {
        var optionsContainers = document.getElementsByClassName('ecwid-Checkout')[0].getElementsByClassName('ecwid-PaymentMethodsBlock-PaymentOption');

        for (var i = 0; i < optionsContainers.length; i++) {
            var radioContainer = optionsContainers[i].getElementsByClassName('gwt-RadioButton')[0];
            var label = radioContainer.getElementsByTagName('label')[0];

// If current payment method title matches the one you need

            if (paymentMethodTitle && label.innerHTML.indexOf(paymentMethodTitle) !== -1) {
                var container = getPaymentContainer(label);
                if (
                    container
                    && container.getElementsByTagName('img').length === 0
                ) {
                    for (i=0; i<iconsSrcList.length; i++) {
                        var image = document.createElement('img');
                        image.setAttribute('src', iconsSrcList[i]);
                        image.setAttribute('class', 'ecwid-PaymentMethodsBlockSvgCustom');
                        if (container.children.length !== 0) {
                            image.style.marginLeft = '5px';
                        }
                        container.appendChild(image);
                    }
                }
            }
        }
    }


// Execute the code after the necessary page has loaded

    Ecwid.OnPageLoaded.add(function(page){
        if(page.type == "CHECKOUT_PAYMENT_DETAILS"){
            ecwidUpdatePaymentData();
        }
    });

```

To show various payment options, i.e. Paypal, MasterCard, Visa, etc. you can add a custom JavaScript code to add the icons below your payment method. 

Check the example of that code on the right. 

To add a new custom JS to the storefront, check [Customizing Storefront](https://developers.ecwid.com/api-documentation/customize-behaviour#add-custom-javascript-code)

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
<input type='hidden' name='data' value="u2D5lOdpstoEc9W9r5Jqdoth0bMMqqfo9eFouhpUVeSondZqlTVGrW13vFujYHjQlm5H8WU1E1Qu1Fr6-YehYtj4W9Jkt6se-KzJ1FEZHCsv_Qp_hfRoysvOlzJCh8kw5tmOdhesRJnXs8ITVEVP4PGMPteytrKrBDPdF0f4IPYd3lnyp-3SzH-dnixtDMlzszaXy29nYxAr7D2EkgL_DBT5vW-uWm0it_K1fCmGFjCvAHxYbwjNhyRRLP_W5LB0PGhSVYxpyGqLD757zahu__rAwqSSKpew1FSX383nF6Q5BffWdUoG_zmHLYlCxsyv0c_sSw4n5IwiPxwQatie3ibLiro-6GpN6LBNzywewptA8u_Smgup4KmI9qBsywxgGCY7BAeFR5gLoxY147ZXv1Hnganjk79xDIruEN3pqgca4-mfaEJjQIAcuDxmd6Twy9BPUTGFIgnEWUfbief44ZKgG41N5fkqSMmAScBN1LYMuMj9BOME0DH6iho-txy3YgRrCPQgqxSRe2-ytKC6chXfwvWdWs5rQz7Jctl9S6Iobp_whWk6hV8xY1QxpujJP2kOnX9ysMpcmt2aCP1ES6kXPzDr1Y9u4cxnepgagX2Jth9j41mO5e4C-AYdTRFndMMSBkp6r4RvpA64cULBMJLbAD6eI4A4zqmTQR4PJ4s7eyvvlmkPFSnN0PMOn2WO9qsFweLAztTp6neuiECUvsELWPj20UY49Z5idejOmXsq5SAPdv7iDnEBEjIxY2xrgAQ6R5baGo8UtCmA2rfgROCx1UiMViCU5XjIBibI09NqYJxKCTrXGShANsZpigwssc8i3DxxWQJY7D7Axo8AYIBRqFhtssAU00iF2Xqn5Sa3Mzcf5Z4onA0RdLf9bOkKNijik5bt4vVHRxk89O-mhp2vS2o78WYhw8cf4R3DiiTaSvKzJi6DtKgIeJAxuvL5Iw1UDSQkC8GfRXRdXfDvG2cCmmWrNJgIfY8pDZEedsSEBxJN-5-hcajkc3Naf-QCj6WioljyVt8Pm090SLdxUEyZhKUegF9zz78ZXEWBQfUdKvqDVopPxwba5nbrwjaMKompDELrqBAViALX_9_IFriq7Jt3tMIQX2JdVJFfyTWJ3LJefDTZUmadEVc96yBN2AawIhGXpUwp-86PZmHgrB-A_ehvhovsP6mdINPs_iV822ft86wlG7lOivKGfjhUmtL8CBJh1bC11QRkUxDJhm3oXFlBISzwv_hOlMYG3DAPasjPb80rEWR7hYy3fe4BzCQozER40OJ0-z2eXkXSAY0r2kyW62jtdiKnO-MwWRvZ7rqYjpSpJA7nLcRULpjiXEGcvs5GGdJcZ7KKrFWC6UTFvamWmz9KUG7Mr_ENmbCqUs9J5WBAj6DlpFPONU54omrOQTk3qhD6lyPzVlazIteoE3Yh-r0kPaWBZZPk1vhWkM6rX4w5z1UDNWYAGOPUSpfLlsV5_AqBjn3BqMkM0gjGn72hYWNX6xdb1fLMq_mH_imaaUrk0Xmpt9ZcqSPo2oHmP93RQZQ5CcKl-G3YC8H0s0Mt5pZarJLDJwrwaQC35V8zkaASB-AKBETcqh7f_7vGD8FasDpJBGD3Aoln615Nh31VzIhPy4hwW2EwDD8Gz2R2OLpuzlkV7gxnytmBWD08KTfdaiPKBXfWZS651ggBWB_ZLTEez1uFCuIDjJf3kJb2US77sU7ov8CJcRau4k92vgSFoY8QQGUPo9wjYRs1VOP05c0UXLLf6IRQ0q8b0uZUqF6hleLqC2FKpTw_8Yx3lHXUzg7zYUWpnBRktbDc64kSnmr3Qwq1MNuwz7iU2k3alOuD9WMxA8NRd4RIEWNFt333wqmPwKqEWtR0faVLldhUIwi2lBygqwFAIP-Bb5a0fOXL6ahiYVgli8CFU_3BnvCgH1C4zbO1tXhdNMhLyH4l97v7F5TPs8iIminifBgWVztDJsmtEUDjmc_AUUjaNbqw3KPGt6KYGYZB1m-JTdwQhm189OZx2KnnurMiEZHjaggxIeVX8H_8ZjK9xssuOrtQwV4XYyWSTjzllxVPyi15qzZvlMkwATi_BoFO_Uktd9DmkhX2M01tn_j3lipWegcgFcml-oODVQuqQQYkCuA9Se1vx0o8ocxpBpY220gy_E3Osp4cI-GX56ldB8wa3-keD930OXBTj9fWI4iVc7nMLmdIYmRZajc7UMcQq0C_BNSfEYpxxxasMgtIHGQ-Qp6r_KcwPJd2HungtBqsY4QdjFAwhugZfrRqBWs4vbmNUbAYArMwjQLGqRGStDIQ563IdfGmzyIf9cKSqFvJL2nHlgPs5_DRZH9vnK1L8lZImJoakq7mLD8AJJHlZ2lyS2Xaqi5q-b9uzN-DiZCZ15BQVEQ851q1ftwwVZdU2X6Cdl4B0uEweeigGFc1R6ArhGk8cu7FeNve25FonEbK_9Ts0qdJfWEQ0xvVneTYzQgM23hZPcCMNCp6LTswXFW-71ca3ReXLELxPoT0tiRm1iwjSWPg_QQ1kLZ3mKlhKTH--whU8fVtoVBRBccsQpWuyNW6i9usRDgHwGW2se73qV5PDhtvwA8Wshf4fiOxl-akOvAcfRK0l_Z52YiVfLcZ2yFszwy59jVol5In9W0iDzmgRse1LNubavQ4iCvdgLnjfn8niCCZO8ymUE2-1TondrOIfpWqq6yyezgZoMlXgquttgGValadzXbiR12cyvJpqDU6YkxDJlwUqGydfZ4PSuOuJyBXrzw3dRIrC3H8lEcGq0LCBRs_tFKBR8JgZiHcT43HhDL9fs2BA59QTi_gR75S3PRVNspXZWz56rUkPBnImzcYrTUAJh0xNpURNpv__RbpYJmE1chOsYoGTLHyljl2CTnN_MoJM8Aw8FWbJZJSc77LBIlj-SXOqvHfJCjRGwx3KptOnrZPSbIM45HRgX1HGhp4MYlZGIPchMJNlguwhGzW2_E_UnXMpNtHFhN5-_nYVRUP8Q8t9YY2cNvZolgy0tm0Gr87JOlGJjpu85Hi0vrYwiB47ot52W94Jn2TbziuODM7a-3kt0tF3utrdfOc6vewoTiImk9UK1TKxi7NAioyZhbxJu3Rp9U5dgK0Egsf2U8gTT9_F0CVpLxelye9DFArnDXpLmoya0uGxX10yZCHfeUa0M44BFFRJh046r_rL1MBVZ9cLcsrAFPXrHIO3vPgVe7gzM7HC-2aN9q-qsVym85tLrdK7fc5QkroeBd5JcXiuNikFD__7x7yKx7hPpqjBOHDkLUSZmBCOLlwiwu-whPI-gl3kG2lxyuvzKfj_lI_EwdEYfPRAFb6AGaD7iN3ytwi34MGRCp2WrUmSNzYOECGdZrECUgIwsqvB06Xk6URc7QMvlk5ewEdvoy2wxIFbu1RX-bNq7jvWvE5qLwmvBizybffIZhRSPxe1HkrqvC8lqaXl5TVMBFMfjuIeSsEEFxgaBEKLrH3CQc__vlEJF6AelXwJi1IYOFGratDV4gmY0LNqLc0XLMUuruE-4jtI7NCAtnZQ2LKG7YD3fMqcAcukjEaKSCo2Pknt1ldcanX5raCE4oYh--AD85Rbtwc52nX0CeX2rnRU6jx40eujusktUb7UXWURvttpoocIH-tqmbSuawNFNlIjagft0wVhzhXF1UewGzRnDHV1yJCo9X8Oe7ln_2unSsdh7ziblt4C5_5yFPvyF6WgMzP2YuCpW9JRXUq9oWYHLUIzGjqW9cx74nZxsJWaG880-8QzRVrSuC6ExYWcQiK3gppWToxgm1xTUumeqJm5dDa6hdytAQ6DC9BzNlRCLhhWMGoUOWFid1aNw__DRO2G_y9O3jdaaT1cD5FuQ"/>
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
                }],
                "discounts": [
                    {
                      "discountInfo": {
                        "value": 1.15,
                        "type": "ABS",
                        "base": "ON_TOTAL",
                        "orderTotal": 14
                      },
                    "total": 1.15
                    }],
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
            "hidden": false,
            "extraFields": {
                "referred_by": "Referrer is: Facebook Ads",
                "AFF_ID": "fb-123"
            }
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
hidden | boolean | Determines if the order is hidden (removed from the list). Applies to unsfinished orders only
extraFields | \<*ExtraFieldsInfo*\> | Additional optional information about order. Total storage of extra fields cannot exceed 8Kb. See [Order extra fields](#order-extra-fields)

#### OrderItem

Name | Type    | Description
---- | ------- | --------------
id | number | Order item ID. Can be used to address the item in the order, e.g. to manage ordered items.
productId | number | Store product ID
categoryId |  number  | ID of category this product was added to cart from. If the product was added to cart from API or Search page, `categoryID` will return `-1`
price | number | Price of ordered item in the cart including product options and variations. Excludes discounts, taxes
productPrice | number | Product price as set by merchant in Ecwid Control Panel including product variation pricing. Excludes product options markups, wholesale discounts etc. 
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

#### DiscountInfo
Field | Type | Description
----- | ---- | -----------
value | number | Discount value
type | string | Discount type: `ABS` or `PERCENT`
base | string | Discount base, one of `ON_TOTAL`, `ON_MEMBERSHIP`, `ON_TOTAL_AND_MEMBERSHIP`, `CUSTOM`
orderTotal | number | Minimum order subtotal the discount applies to
description | string | Description of a discount (for discounts with base == `CUSTOM`)

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

#### ExtraFieldsInfo
Field | Type | Description
----- | ---- | -----------
YOUR_FIELD_NAME | string | Your custom name saved for the order extra field. The value length cannot exceed 255 characters

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

> cURL request in PHP example

```php
$url = "https://app.ecwid.com/api/v3/4870020/orders/transaction_55885213?token=1234567890qwqeertt";
$data = array('paymentStatus'=>'PAID');
$data_json = json_encode($data);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: application/json','Content-Length: ' . strlen($data_json)));
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'PUT');
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_json);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response  = curl_exec($ch);
curl_close($ch);
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

<aside class="note">
Before returning customer to storefront, make sure to <a href="https://developers.ecwid.com/api-documentation/processing-payment-request#updating-order-status">update the order payment status</a> first. This way Ecwid will be able to show relevant content to customers at all times.    
</aside>




