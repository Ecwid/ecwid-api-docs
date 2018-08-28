# Add Payment Method

> Your custom payment integration in payment settings of the Ecwid Control Panel

> ![Custom payment integration example](https://don16obqbay2c.cloudfront.net/wp-content/uploads/custom-payment-1534763893.png)

With the Custom Payment API you can integrate a new payment method for customers of Ecwid stores to choose from. This functionality will work in the form of an application that users install from the Ecwid App Market.

<aside class="notice">
Access scope required: <strong>add_payment_method, read_orders, update_orders</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

**Table of contents**: 

- [How it works](https://developers.ecwid.com/api-documentation/how-payment-method-works)
- [Set up payment URL](https://developers.ecwid.com/api-documentation/set-up-payment-method)
- [Saving merchant settings](https://developers.ecwid.com/api-documentation/merchant-settings-for-payment-method)
- [Payment request from Ecwid – Details](https://developers.ecwid.com/api-documentation/processing-payment-request)
- [Payment integration template](https://github.com/Ecwid/payment-gateway-template)

You can find existing payment processor integrations on this page: [Payment Gateways](https://www.ecwid.com/apps/paymentgateways)

## How payment method works

### Payment overview

> Checkout flow for payment apps
> 
> ![Checkout flow for payment apps](https://don16obqbay2c.cloudfront.net/wp-content/uploads/Payment-Method-Diagram-1511788692.png)

#### 1. User configures app settings in settings tab

After the installation, user would need a page where they can configure it. We recommend using [Native apps feature](https://developers.ecwid.com/api-documentation/embedded-apps) to provide this functionality. 

In any case, your payment method will be displayed in the payment settings of the store control panel to edit the title, description, position at checkout, and availability.

#### 2. Ecwid sends order data to app payment URL

After registering your app, contact us and provide a payment URL where Ecwid will send **POST request** with order data as well as the [Merchant app settings](https://developers.ecwid.com/api-documentation/merchant-settings-for-payment-method) when customer is at checkout stage. 

Your app will need to get order details from the request, decrypt it and the information to a payment processor in a correct format, where customer can pay for that order. 

#### 3. Customer finishes the checkout

In a storefront, customer will see a new payment method to choose from. If they choose this new method to pay for their order, they will be directed to payment URL of your app to finish the checkout process.

Upon finishing the payment, your app must [update the order status](https://developers.ecwid.com/api-documentation/processing-payment-request#updating-order-status) and [return customer to the storefront](https://developers.ecwid.com/api-documentation/processing-payment-request#returning-customer-to-storefront).

### Payment installation process

There are two sources where merchants can add new payment methods: 

1. Payment settings in the Ecwid Control Panel
2. Search in the Ecwid App Market

#### Payment settings in the Ecwid Control Panel

Payment settings are located in *Ecwid Control Panel > Settings > Payment*. On that page merchants can edit existing payment methods and add new ones.  

If your payment application has been published in the Ecwid App Market and it's publicly available, merchants can find it in: *Add new payment methods > Other ways to get paid > Choose payment processor*.

Merchant will see your payment method in the drop down list depending on availability in their country. Make sure to specify the countries when submitting your app for review. 

The payment method name and logo will be used from your existing app settings. 

**Installation process in payment settings**

When a merchant clicks on your payment method from the available list, Ecwid will automatically install your application into that store. 

Then, the payment method of your application will appear in the list of added payment methods in the payment settings of that store.

If your application [works in the Ecwid Control Panel](https://developers.ecwid.com/api-documentation/native-applications), Ecwid will open your app interface in a popup. If your application [works externally](https://developers.ecwid.com/api-documentation/external-applications), Ecwid will redirect merchant to a new tab to continue the set up.

#### Search in the Ecwid App Market

Merchants can search for your payment method in the Ecwid App Market. If your app is published and is publicly available, it will appear in the search results.

Next, a merchant will click on your application and will be directed to your app details page. It will describe your integration in more details with screenshots. 

**Installation process in Ecwid App Market**

To add a payment method from app details page, merchants will click the 'Install' button and the installation will be the same as any other application from the Ecwid App Market. 

If your applicaiton [works in the Ecwid Control Panel](https://developers.ecwid.com/api-documentation/native-applications), user will be directed to your application tab and set up the connection. If your app [works externally](https://developers.ecwid.com/api-documentation/external-applications), the installation will continue on your website. 

As a result, a new payment method will appear among existing ones in the payment settings of merchant's Ecwid Control Panel.

### Payment availability

The payment method added by your app works just like any other payment method added by a merchant. 

It is possible to quickly enable/disable the payment method in a storefront right in the payment settings of the Ecwid Control Panel: *find your payment method > switch the toggle to the disabled state*

If you need to remove the payment method completely, go to: *Ecwid Control Panel > Apps > My Apps* and remove the app from the list.

## Set up payment method

After you [registered a new application](/register) for Ecwid, [send your payment URL](/contact) to the Ecwid team.

Ecwid will be sending order details requests to payment URL endpoint and expect the order status to be changed after the payment is complete.

Next, start working on payment processing page according to the documentation: [https://developers.ecwid.com/api-documentation/processing-payment-request](https://developers.ecwid.com/api-documentation/processing-payment-request)

You can get started quicker with our [payment integration template](https://github.com/Ecwid/payment-gateway-template).

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

> ![Merchant settings example](https://don16obqbay2c.cloudfront.net/wp-content/uploads/custom-payment-settings-1534763988.png)

Set up a new tab in Ecwid Control Panel, which will serve as a settings page for your users. This tab will load a page from your server in an iframe in a separate tab of Ecwid Control Panel. See [Native Applications](#native-applications) for more information.

<div class="notice">
Get started quicker with our <a href="https://github.com/Ecwid/payment-gateway-template" target="_BLANK">payment integration template</a>.
</div>

When merchant is in the settings tab of your app, your code can create and modify the merchant settings using the **Application storage** feature. It's a simple `key:value` storage, which can serve you as an app database. For your convenience, you can access it [via Javascript](https://developers.ecwid.com/api-documentation/storage-in-ecwid-api#javascript-storage-api) (client-side) or [Ecwid REST API](https://developers.ecwid.com/api-documentation/storage-in-ecwid-api#rest-storage-api) (server-side).

### Edit payment method

> Editing custom payment gateway example

> ![Edit custom gateway example](https://don16obqbay2c.cloudfront.net/wp-content/uploads/custom-payment-edit-1534764167.png)

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
<input type='hidden' name='data' value="2sO2fxaXOZz9aZMxwuwvEpFSBFKd6yPTeyk3R3V6ieXIDc9romK5vpTv9kTCoMZVmxJd8kabS-bEJy7ON6vzpG4X8QMb3CYhl_WF2T7VAX883FfMverEo4jY9VlEUS9ZSJuuBVeUPnG5FE-bLTPNrMbWFXltyzt3cz5mHu8ha50oPXPDJ7mcihSH-H-i32LLTvwkfgqJa7CNd3n4XM985B1smvJEw4o6acsy1RO62eJVWJZ7k1_UTfa5S3AhrmcY0OxZuTQ3pTVVG0uKsnB73eZMhijPkPlx5Rehu_9IaMylrwOy2E15hzTnmAYActRgFVGSH8NevL9lGnTifYTp6thUqJn1HhiFjIvNQveEHIPIq5n-uOn9wkodVoDeEsuin_96t_eP5uJUFjHEpnXsDdtJpE9BVzCJrDEU2wprzYCGdc_p28f9J6jEyH4Ld2FrF0kRv_GacJwIKghGQixu_BlTlrBO1DZ1B3AhBS2IHmZVCj_U2Ux9s20-5ZTgJRgsMyeObj1I8byMPc29GGXXSJVbYfcWjk8PGtcToQu9tSFV-zYur9VB7T8ewdxst-_kTnnAPmIdKFfxHpaRIu3GYVpubytnvOMhsIuoeN3lmZ0scX_leX6w4ZE1iduEKQT3GWopZoHBTKF1-441PDY4MemDDSfUTHqkvr9LrOC4gxvXlB2EddTqUfpFFX1QZX_jpvxttQp0RXwyIi5WDVJ0pU6b-De69-QHkzeaaLbbTzJvpzkKDOmQq_TNgZnPY7sdCLpQ8ECgadWxdIxmRGCY8s-HP_sOB8Pbp5T_BHHnr8Dzk6qFcay_MHy8IVn8kP0T3USxTBkdr44KTgq-5jZa77SojW9CwvaA-GqdIRxJwjabvWPGhYsiZBxJu8pyiYDHCadc9WeXAzk5yILd9peyp0ADZAHS5l4f7jnMJOfqElTqfSg_Xq16OwX74IQBx_Isu4swQkQX4whAcaJTDo0AsE2AkQJKXjkdqxfFUtDPyMwW6wmcK7DD81xmybcOVpxU0ZiA12L-Gg8Pczgr7GeUYUVm3aLQu5IbP_o2B1woLivNaT-fvs7ssw1LiRzGkFfsBdWhJkJe7g9OWW3QKs1T91IxWGRNLMA55UIxNXVYKLsOOSKQRUKH0KUe6llAR4ptYJoLl0fULwuGZjXdTePTu791tZ-0tPZrychVMHsvqCIvdFlMVQ9C50q8deRkozQ8YHbKu0ih6nKB2xDQ6dDhS-GPz5cQn-2xZFASZHkniqvzJvA6Iy-NMU-IYpnTXzleQixN4fyQMSAkQTOfnMmg54vvFL0esX4LTBXgRx58hqzxXDM3PffUznq2bbhS_ey8ytRM9gfXcNKLTSwdHtX89Uh_pez7mgAqTnp2RiaLyN-hsJryO3xnlPj-HoTgA_jZz_yxB9jaNBO5fLbDuWdftIVPJ9wTjrSgwvUYM0esD-QZvtrAx__drHeovLgsSZwPsZAdbQdY912NH30xTAYsuZISGB2stEiY3-jZMKTPbuWHAzyiWCrgOwEVuAAW9HgptffCkD28dAkqjPnJUaRMLzjzG1lrtAcolhr1TNzI3-qoKNMawvoN73-Mu1hshd3W81MyQSVVN4A1Mq46RA0QgGby2L26YcJ2zgJvbAwWFCk4Z4yLEOzwwRJvnmJ213ncBYAMKAlOgRijKHWaEWOBxztPQxP6WMjXrbAXIJhB_V8uLHkPYI3HO_5ntpLjHv8iPtRpRInZKqXLDinkA2Oez3EAj0gctLtsRCAPxFnC2dFABP_nIbogsaAkoWwXLdPVcp-CT3mtsuFo9Lrnbjp2T9aGAF9Ni0EAYDn1JI1hKmSLa_9jPnFzg5TDxbIBCS6UVfgD0oahf0fVOTChwaig8L6n92qJDhzpZ2YkBGIMgmp15F65OzNrpblRk0GFxdxKVpvetvWqqD4bT5BDoima5qqwnqvxiGp3kn_Y-oj5W3m98pT7DfjUALQzSwwmmHPsowFqdpBxrCcCqeoS2KgdvCn02w5vaYeOjCTr64hUHYzvMKhTS77U2JOp6lYU5009CSssK9mdQM134qW53d3uy_pnjD980rcvv0vbrjTN1qQYQFYOzggkgHLMqyELVZz8NPqoBO7NKyFXlevhkhm3vOMQdaILCVv-QVrLsAA3jwSLeWIeG9a2dL8xJyLj6qRoMzZ6TAEw4jfXAdNi3u6ap6AmyN8H2i0UTce6HrtEY92iPWXi-fEe9eLRkFZKbaNQK_1oEFLs6W_Owd3LTlXUIDqTxYXpxPOEMz0XjjYKa3BZhWES1xMVXsk9IP03GI3DNzKhJOBNJrsBn1Wn-qFB-bmnAvdUVVQeR9tdOYkcfqRiyOYAlBN4fijuO2Spz1s2jmZ0qF7XFjb_5YovtBnCnyQmz-HSNLvfo3Db_WEw1Yq56mozW4ztsRExQK4Mnq6IoTlDYJHE0DMGYeRO1tIV-aMqnlnBkeu8Rgs5zA3pPTuZvrzLd_2fNyI9HkOzr5cQ_ebaXrFHzs4NVdYVcZbogPysMwD1INj6_gAQdsvvgWt22XUTIOCNX8ZtTx-kCk8__zMTeEr0foXEO9JTupdCletliNAVJ7zbGQBj_q8o9Pc_7uOZo3rhv37Tc1plVcmyQDCWKh3iAsoOgZTFW0R7AWhigqtueH14m8IruynORJ6l1Y3ubr1JmCN1DzqHWxS7k04Sdp7zjfpU-_aQgslGTp7M7hJksm3v7lrvy8cr26zzAiSqtbab6pIpXJfbGCRBJb4Y7i_gc3ljJvmwLPmFY_mQ6AsCwKmzE1LEKrtOUm8BbJlecpwjH0ig-ynGX9nHs_j8_-jQilsHbstJ_a9O5HuuxJfQPPSTfkN0R7NUwoFZH9kxer1Y_QNC37yFalfZIB-z2i1h9ughCE4bYsoorFb2bTVb_R0DH42ZvwES9AAWLifa8v1p-uDVyPtFjk1k6wbaTp8tWdrU3T8MVY0fPVaxE1vdiBdHHcXwnEz-XplQsAd3FHVlowoqGeNHs9FKAv779MZGKa1O49IQOY2TLadd6Hcqwzm10DXB81Re92-pPROQSNGb7Vt876s0IO71k8j0aTD2GaqQlmoZ6GJJFzpK6yMEE-n4SFAsIIj9aEipeVwblroAYYN_ma2-5Gr02-fX9vKHby6TgDq71Z2stYQheUWvUVvcPiyWijrkdWhrYc3SeloJdJeVvhEMYsJ3IgZ3sSBtpRX-5vccpjN03LBih_PCQXQVWdQEAv8bTpMU6uDL804f09-86WFXu54v4p2pqPT-s214Hvnjm6jLZYJRksOOPxAduMSrDTz8JGidezE3n7-F3jZnFSNb6Juv5HXfRhxxJ2WGtgm2SUIvsJSVPWHKlE8PkrGOISjg7m0EUfD-ypzz3Fnv7sWnkfGg7HNwr69gWb--9dadxSUbWy82twCnaMm3OKpsW3ZQ__HE1_NPCpfyc-vW90hkj_hYa7Bpcq3jlY-5tTG6dqe8UNGdH7sKqVnYM98-VRuEBMj3ihQW_hfujErqWCo5JD748IV0Orz1yNo0NSt7kBqKJK9gSGkPDc5M9qS_EkIwIH7PLVpWItSuPvgvE8jso4qf-ywHzevlbcM7uBFBIaMhiRxYh33U6u02zUgT42ZFMPe3dV4SkIIFxqufnsmWn-iw2EszIoJrhNN_bllGikZ-sodTS66D5ygscvI-NqOTmoqjaV3jKMmppj42Rxe6rMTx2u9MiifRxJlg2iLHE273wJmLu5_RWKRNJpoWgeqFRQ9iuWrAyV72BwNeYbk2AI5tL74iiI567CgpdSmHwC7VUAV9F8AztFTq0kt1uTpIvf129ytt6l5hlvlDPFvvksG7dUFEMcryevPoJNo4rghBxY5Ru7MNb1FME3RGgk2pXI4nRPjOOFfEG8WyOK6TfseJSRXNhBH0irQnukRROCB1NXR9dq35TlXGT-L05oSYUaoiSomOC-J9qC3FkWglzRs3ZuDXPP8uab0v_rN3CSJYX8_nnlbu-VUo6A3BCEIVUsiZfi6Vrpb3-BpvCkbA_fXTo1kIzERK82OD3UYfcragvSFH8TrtbmuusnY_NTUOYuiylYStUjCT0ohXyxf8fStd60fNqFLR4Oa-g7IZf3cf8JdJelFiMF99XNdD3TrApnMpAq8QE01_bCAL77PZGWEsPQRcqGR6zFRhJ3a_H0KPhrlSwvcZ6XtZUvbPskj-PjwbDtLcvuCOSaKe-JEbN4Ljk_8G6sZgTJy_tSGCqVcgHCjbio7oN_Q0609cZ6nAOIwjKrmzKrdc086ALFWhebI3dIjV80YJays7clppo9rDv-WrmOH1-IOh7cneew8MhiQAzE_GA32oYimXRShBahMrbfSSC24WuJYDpixr38OUa8CQZdDmMD2j9pHziAtY5tEiVenmZM7OPiHy3Ywb_gHQk0Re_lo-LT1kocMShMJxCaS949oDkO6TM1kbkmAbVSqlrLzJzIG2lYqixmTH3JSmPZZsUrtzggp8uhGttozGyEgsvpvvcCCZ453YBrN1KylPY116f1VhI02FEOXBb3zUHuYyGPuO9ux7en50jMWWPu9npljgOL37b28JY0WX_Md5TOEnQa0xxEEygIQ0OZUpThOX8ySEr6ArFfrmd5T4ZYzlNiyK0W6z6JTg6_YDAYo7Xesg7AJvhow36vffbeyVN9QgJULywade_7xmJGSsqaOX_mrmqe_J5jSdXvn2eqWHbeMAx7J4tW7dzhuAbOK4k8AJcty1C0UJ5xQqlTGlvNRnqfSCG__w__lg7yF3e35DBSmJjAqVjkuVnYOaL4YMnDTEjWsaVrPsYM51Zl4nBAmW5IRwthwPCONovaw4ME_oR1xWp01iPF5ntntijBg1A1PgDVy_hPnb4boJMb2CAXElqpT-2AtgTok8hJtbM6mhZ6hZHlsZwHW16CJgqGbnuBI0UWvstrQu02WrgJHykbTLG2nkk1m59ytCh1cPmxFWAehikumciywABuwWN51r9HzMmgjdoVQh5ht68Qp9tyrgNc0HAEQmlkjqeopw8MF60vd5CMEStcOZwYkJOVSta26oKb0wkHz1M5PVDy1s922xha0I71N06VykH270dMa1NPdU-zC-OCJFDMCURiiIjFBWkigfKZ5M9rJKOJFNwtNEJZi3BI2VIwmlEjSPc6cDz3ayDmluaVToe6vsko4awBrD7OYrVRwxaZGsdxEzQ_wg3uaxPZkcBMChwoFBMpx_LLtCXv_H04urW8Ej6Dh3-fWZWwwtR7HDYNvrWCZZyunpAW3R5UTh18qBul5Xn5eW_A5vDHR2P64-Ve5GoI_Ey5cZ4KLGsD84AqjsayotHlucUtfNuMTG_Cb_27ja3SzMxGOpCRpQ-lVXutOVhy6yItmXSWuxNGmoQTaFlP2rH5VbAuO-4o4flCGDd6GW7Q2g58UTVggn7PqcSlbMSgnPvjApEqzTzHjlLct5kJk7w_-VuLO-JSmgascIWUWJKMBYz0kTyjs7N4_MeDiR52myYExUQt8g-d5UAT0xxlRhcYfMvHFRE2TQ-gGEuOIDCCdKJaEo-NTsb8IQ_dPz-VSog33o1y1UUQ24D_wsG8m5dhbMc2HVXZOgRb1jBYxiW8WTpMjaV6zHM7xFmyLV-EuWivHZmME1Mf1b4vayfV0TE9xDEZix8Omixln-Tg8G4iPqvlBwVcLVqjhvOi-iiolasL65VuO2w8JoA7XdAezLgkmmZoeiQUbqVyWEBeepF8lH-zEvABtivbSurZqk_aj2IQo-cyOZmd3n9s_q3jv-uYU6j-VNXAH8w41bmUWTGJeHrc-Xv_94eMCwxNNTyxS72FspNIqHVtoPklFyLi_Yp_Q7451NiZixX5ixAmdLaQEdcs6cYuVzp4zpnA3xB0GX1uueGY04llv50z0G8u3zjy4g5bF9vFUrZZHmdWZnFXlPOHsMX_deIOXow3rSdH7RD096nq5NSX5D8c99QdnQU6QMKrp6T-llbAdD5h9_e-8Px6N8ofEBxn50ukKYzblv51_q5e1wxo4qqNjmSDGttmQk3OJyQgvKRg6z1t343jjcx-LwKZilHV87__pkfpzXiACYlwPQUtU5FMOU-P2Au7zLbdof-7PZVSGk8JG3yifoZasz66cDEdgSgZw0yno8Z-bR9ukecfOFhRSCoz-dOeislyl8_Hr_emOinGLu2pNAuNrFonKrcek7arqNUgtyWZtrncbkGGEUG2OasoWkHxgyylwqNcEhcBCqQDX9XGXFTNretk4VfSjasMcPMS_N4o3jJhFnxMQMV0AXjksK3OpWVIljGDYjlGJqrb_Ils7tkFd5T-F8fcwKoTMSp7a8AOt4gRMrmS9mUeSbgzRJTjYlTr0a-0aI7mhejw47aO8eGI1GPP5zyUykgOmfnFny1uT5QrZ6mEHogLfP-0aecdOu5beutfTQM-VZYxDeq9KXKvSI5LljDM67D3W5UPdWDBEfJFZaKvBIxfgXzAFFrR7lXzzseEyYU_VkZb1yQy9O7YsKgageFg9P6UC0MkgDkguldH8BWKWq7dSqpNiIXzJ1tgpMz5xQKSkQHS-WSIIby10rgUvTpclxfqbyv-q1SPY558VSf-vwM5KZzvpKNxMP6YP-zYn_P3uqLPV7sEECDRVjs4Ti3xWjGJJ7IfvMm7T5dTii9YdSBVJ1dOJ19NpLZG6IeWczt1j2tbalO9D1ouhkyG2SZOfPtYZmIjgxwpB5eCOixPGA4rhh7ejRLQBLZ3BHhoPAq3e9bPmSUWjioYefytDGGlf7DvgP3y8bGTVyrtT7iEY7ut0xHMFa4rcYAlOW8WmMvo0PDbQjptz6fzvYawrSgUaJ6zF7VhecDhEuvs6tM1JtB4BD64uugpsJQfvQXM_nkrZArApYvFhHYTLqKaOVN7bUa17eHukScPTqcSJqpWGTecL9MBvroHMr8LJckXH9UKjZruoItLpGTjiIQVYTn03zqu54XclIPh2q5q5rCnltj7MAgxOyVGACj4PDOt-Q5riiTk-A-zmm9qYeMLx40UI1psE9cre8wrGURve9pOtjbKU5s6jRfgqrbU5CEm4aBnJZUiku8cBQkP_4HqqltzlCiAD40tsR--eDo5M0F1nqneMOOTL5V8kDn1V38Qh96Pwv50esv5JWxLA-gBI5YWBfiQGuoVSfXi1coRtPkquXmvJL6o-UhcO0KYOBwzkNrx9IZBWbKLLrxoMf6vSQwRk2sL8CgjCSX6nf9XaoQ17Ji0_TSICxIBo-tfz0pXq_x8Nq_lqgATk747QSBqWkj3KjGimAQYhWU_PZZDt4FMG5ROVen3bSKJ7zQvF_V5cNRCmn0pfhGzKCIS0OcNcq6WlSXmUPluqY"/>
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
$client_secret = "payment-app-secret-key"; // This is a dummy value. Place your client_secret key here. You received it from Ecwid team in email when registering the app 

// The resulting JSON array will be in $result variable
$result = getEcwidPayload($client_secret, $ecwid_payload);
?>
```

When customer tries to pay with your payment method, Ecwid will send a POST request with a format as described on the right. 

The value of the `data` input is encoded with a **AES-128** mechanism, where the first 16 characters is the `client_secret` of your application, which serves as a key to the decoding process. To find out more on how to decode the value, see the example code in **Step #1** of [Server-side Native Apps](https://developers.ecwid.com/api-documentation/authentication-in-embedded-apps#enhanced-security-user-auth) section.

<div class="notice">
Get started quicker with our <a href="https://github.com/Ecwid/payment-gateway-template" target="_BLANK">payment integration template</a>.
</div>

**If you are using C# language**

For correct payload decryption, create additional padding to make the payload a multiple of 4: 

`base64 = base64.PadRight(base64.Length + (4 - (base64.Length % 4)), '=');`

> Decoded request from Ecwid example

```json
{  
   "storeId":1003,
   "returnUrl":"https://app.ecwid.com/custompaymentapps/1003?orderId=65306446&clientId=mollie-pg",
   "merchantAppSettings":{  
      "apiKey":"\"XXX\""
   },
   "cart":{  
      "currency":"USD",
      "order":{  
         "vendorOrderNumber":"65306446",
         "refundedAmount":0,
         "subtotal":1076.64,
         "total":2014.97,
         "email":"mscott@gmail.com",
         "paymentModule":"CUSTOM_PAYMENT_APP-mollie-pg",
         "paymentMethod":"Credit or debit card (Mollie)",
         "tax":488.48,
         "customerTaxExempt":false,
         "customerTaxId":"",
         "customerTaxIdValid":false,
         "reversedTaxApplied":false,
         "ipAddress":"195.151.247.241",
         "couponDiscount":22,
         "paymentStatus":"INCOMPLETE",
         "fulfillmentStatus":"AWAITING_PROCESSING",
         "orderNumber":65306446,
         "refererUrl":"https://mdemo.ecwid.com/",
         "orderComments":"555",
         "volumeDiscount":4,
         "customerId":40201284,
         "membershipBasedDiscount":0,
         "totalAndMembershipBasedDiscount":0,
         "customDiscount":[  

         ],
         "discount":4,
         "usdTotal":2014.97,
         "globalReferer":"https://my.ecwid.com/",
         "createDate":"2018-05-31 15:08:36 +0000",
         "createTimestamp":1527779316,
         "discountCoupon":{  
            "id":29567026,
            "name":"API Testing",
            "code":"APITESTING",
            "discountType":"ABS",
            "status":"ACTIVE",
            "discount":22,
            "launchDate":"2018-05-24 20:00:00 +0000",
            "usesLimit":"UNLIMITED",
            "repeatCustomerOnly":false,
            "creationDate":"2018-05-31 15:08:33 +0000",
            "updateDate":"2018-05-24 13:40:32 +0000",
            "orderCount":0
         },
         "items":[  
            {  
               "id":140273658,
               "productId":66722487,
               "categoryId":19563207,
               "price":1060,
               "productPrice":1000,
               "sku":"ABCA-IAC",
               "quantity":1,
               "shortDescription":"",
               "tax":331.01,
               "shipping":0,
               "quantityInStock":0,
               "name":"iMac",
               "isShippingRequired":true,
               "weight":0,
               "trackQuantity":false,
               "fixedShippingRateOnly":false,
               "imageUrl":"https://ecwid-images-ru.gcdn.co/images/5035009/391870914.jpg",
               "smallThumbnailUrl":"https://ecwid-images-ru.gcdn.co/images/5035009/650638292.jpg",
               "hdThumbnailUrl":"https://ecwid-images-ru.gcdn.co/images/5035009/650638293.jpg",
               "fixedShippingRate":0,
               "digital":false,
               "productAvailable":true,
               "couponApplied":true,
               "selectedOptions":[  
                  {  
                     "name":"Price-Optimizer",
                     "value":"6",
                     "valuesArray":[  
                        "6"
                     ],
                     "selections":[  
                        {  
                           "selectionTitle":"6",
                           "selectionModifier":6,
                           "selectionModifierType":"PERCENT"
                        }
                     ],
                     "type":"CHOICE"
                  }
               ],
               "taxes":[  
                  {  
                     "name":"New Tax 2",
                     "value":12,
                     "total":124.13,
                     "taxOnDiscountedSubtotal":124.13,
                     "taxOnShipping":0
                  },
                  {  
                     "name":"TVA",
                     "value":20,
                     "total":206.88,
                     "taxOnDiscountedSubtotal":206.88,
                     "taxOnShipping":0
                  }
               ],
               "dimensions":{  
                  "length":0,
                  "width":0,
                  "height":0
               },
               "couponAmount":21.66,
               "discounts":[  
                  {  
                     "discountInfo":{  
                        "value":4,
                        "type":"ABS",
                        "base":"ON_TOTAL",
                        "orderTotal":1
                     },
                     "total":3.94
                  }
               ]
            },
            {  
               "id":140273659,
               "productId":66821181,
               "categoryId":0,
               "price":16.64,
               "productPrice":16,
               "sku":"001001",
               "quantity":1,
               "shortDescription":"This sturdy white, glossy ceramic mug is an essential to your cupboard. This brawny version of ceramic mugs shows it’s ...",
               "tax":157.47,
               "shipping":471.85,
               "quantityInStock":0,
               "name":"Mug",
               "isShippingRequired":true,
               "weight":0.4,
               "trackQuantity":false,
               "fixedShippingRateOnly":false,
               "imageUrl":"https://ecwid-images-ru.gcdn.co/images/5035009/389900000.jpg",
               "smallThumbnailUrl":"https://ecwid-images-ru.gcdn.co/images/5035009/475772545.jpg",
               "hdThumbnailUrl":"https://ecwid-images-ru.gcdn.co/images/5035009/408631478.jpg",
               "fixedShippingRate":0,
               "digital":false,
               "productAvailable":true,
               "couponApplied":true,
               "selectedOptions":[  
                  {  
                     "name":"Color",
                     "value":"White",
                     "valuesArray":[  
                        "White"
                     ],
                     "selections":[  
                        {  
                           "selectionTitle":"White",
                           "selectionModifier":0,
                           "selectionModifierType":"ABSOLUTE"
                        }
                     ],
                     "type":"CHOICE"
                  },
                  {  
                     "name":"Size",
                     "value":"11oz",
                     "valuesArray":[  
                        "11oz"
                     ],
                     "selections":[  
                        {  
                           "selectionTitle":"11oz",
                           "selectionModifier":0,
                           "selectionModifierType":"ABSOLUTE"
                        }
                     ],
                     "type":"CHOICE"
                  },
                  {  
                     "name":"Price-Optimizer",
                     "value":"4",
                     "valuesArray":[  
                        "4"
                     ],
                     "selections":[  
                        {  
                           "selectionTitle":"4",
                           "selectionModifier":4,
                           "selectionModifierType":"PERCENT"
                        }
                     ],
                     "type":"CHOICE"
                  }
               ],
               "taxes":[  
                  {  
                     "name":"New Tax 2",
                     "value":12,
                     "total":59.05,
                     "taxOnDiscountedSubtotal":1.95,
                     "taxOnShipping":57.1
                  },
                  {  
                     "name":"TVA",
                     "value":20,
                     "total":98.42,
                     "taxOnDiscountedSubtotal":3.25,
                     "taxOnShipping":95.17
                  }
               ],
               "dimensions":{  
                  "length":0,
                  "width":0,
                  "height":0
               },
               "couponAmount":0.34,
               "discounts":[  
                  {  
                     "discountInfo":{  
                        "value":4,
                        "type":"ABS",
                        "base":"ON_TOTAL",
                        "orderTotal":1
                     },
                     "total":0.06
                  }
               ]
            }
         ],
         "refunds":[  

         ],
         "billingPerson":{  
            "name":"Michael Scott",
            "companyName":"",
            "street":"555 Lackawanna Ave",
            "city":"Scranton",
            "countryCode":"US",
            "countryName":"United States",
            "postalCode":"18508",
            "stateOrProvinceCode":"PA",
            "stateOrProvinceName":"Pennsylvania",
            "phone":""
         },
         "shippingPerson":{  
            "name":"Michael Scott",
            "companyName":"",
            "street":"555 Lackawanna Ave",
            "city":"Scranton",
            "countryCode":"US",
            "countryName":"United States",
            "postalCode":"18508",
            "stateOrProvinceCode":"PA",
            "stateOrProvinceName":"Pennsylvania",
            "phone":""
         },
         "shippingOption":{  
            "shippingCarrierName":"Shipping app the-printful",
            "shippingMethodName":"USPS Priority Mail",
            "shippingRate":471.85,
            "estimatedTransitTime":"1-3",
            "isPickup":false
         },
         "handlingFee":{  
            "name":"Handling Fee",
            "value":4,
            "description":""
         },
         "predictedPackage":[  
            {  
               "length":0,
               "width":0,
               "height":0,
               "weight":0.4,
               "declaredValue":1076.64
            }
         ],
         "additionalInfo":{  
            "google_customer_id":"2008512504.1526280224"
         },
         "paymentParams":{  

         },
         "extraFields":{  
            "lang":"en",
            "askHowYouFoundUsApp":"From a friend",
            "kliken_vid":"99aa74d7-75a4-4624-9ed6-87892f1c165e"
         },
         "discountInfo":[  
            {  
               "value":4,
               "type":"ABS",
               "base":"ON_TOTAL",
               "orderTotal":1
            }
         ],
         "hidden":false,
         "referenceTransactionId":"transaction_65306446",
         "taxesOnShipping":[  
            {  
               "name":"New Tax 2",
               "value":12,
               "total":57.1
            },
            {  
               "name":"TVA",
               "value":20,
               "total":95.17
            }
         ]
      }
   },
   "token":"abcdefghijklmnopqrstuv1234567890"
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
order | \<*OrderDetails*\> | Order details for this payment request

#### OrderDetails 

Name | Type    | Description
---- | ------- | --------------
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

- If the order is in `PAID` payment status, customer's cart will be cleared and they will see 'Thank you for your order' page
- If the order is in `INCOMPLETE` payment status, customer will see the cart page of Ecwid storefront with the same items
- If the order is in `CANCELLED` payment status, Ecwid will show the 'Payment error' page

<aside class="note">
Before returning customer to storefront, make sure to <a href="https://developers.ecwid.com/api-documentation/processing-payment-request#updating-order-status">update the order payment status</a> first. This way Ecwid will be able to show relevant content to customers at all times.    
</aside>




