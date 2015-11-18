# Embedded apps

# Overview

Ecwid API allows your application to be embedded right into user Control panel and work like it is built into Ecwid. Although this is not necessary and you can use Ecwid API without embedding an application into Control panel, we highly recommend this approach. Being integrated with Ecwid this way, your app will get a way more user engagement as it will be a part of a merchant store backend.

At high level, it works this way:

- When registering an app with us, you specify an URL of an iframe application page on your server
- When installing the app, user authorizes your app and allows it to appear in their Control panel
- As soon as the access is granted, an additional tab with your application will appear in the user Control panel
- On this new tab, your application will appear in an iframe and the user will interact with it as it was a part of their Ecwid backend

Below, you'll find more details on how to build an app.

# Building an embedded app

## Set up your application
To make your application displayed in Ecwid Control panel, you will need to register your application with us, if you haven't yet. During the registration, you will need to provide us with additional details about your application that are necessary to properly show your app in Ecwid control panel:

Parameter | Meaning
--------- | -------
iframe URL | This is a URL of the application page hosted on your server, which will be loaded in Ecwid Control panel. Requirements: <ul><li>It must load over HTTPS</li> <li>The page must not contain header/footer, i.e. you will need to design this page as an embeddable, not as a standalone application.</li><li>It should not contain the word 'Ecwid', so we can offer your app to our partners</li></ul>
App page title | This will be the title of the tab in Ecwid control panel where your application resides. Please keep it short as it will reside in a row of native Ecwid tabs and other applications 
Control panel section | The section of Ecwid control panel where you want your application to be added. Supported sections: <ul><li>*Sales* – choose this if your application works with orders or customers</li> <li>*Products* – choose this if your application works with products, combinations, product images etc. </li> <li>*Promotions* – this section is for the applications working with discounts, coupons, loyalty programs and other promotion features</li> <li>*Settings* – you can choose this section if you need to place your application settings at the same level as the store settings</li> <li>*Design* – this section is for the applications that customize storefront look and feel</li></ul>
Server-side or Client-side app | The way Ecwid passes the store information to your app will depend on whether your application will work on client side only or it's a server-side application.

You'll be asked for those details on the app registration page. If you already have a registered app and want to make it embeddable, you can [contact us](http://developers.ecwid.com/contact) to adjust your app settings.

## Installation of the app
To access to Ecwid API on behalf of the merchant, your app should implement the oAuth flow described in details here: [Authentication](#authentication) . During the authorization, you will need to add `add_to_cp` scope to your request to let the merchant give your app permissions to appear in their control panel. If the `add_to_cp` access is granted and your app settings on our side allows the app to appear in the user Control panel, a new tab with your application will be added to the merchant's Ecwid backend. When the user opens the new tab, we will load your application's *iframe URL*. 

As the regular oAuth flow implies, you can get the access token right after the store owner grants your application access to their store data (see [Authentication](#authentication) for more details). You may want to use this if you're going to access the store data from your server in background, for example, to periodically synchronize user products or orders with the data stored on your servers. As to the part of your application working in the user Control panel in iframe, it requires additional authentication to let you know which store backend your app is opened from right now.


## Build your application

> Skeleton of an application embedded into Ecwid Control panel

```html
<!doctype html>
<html>

<head>
  <!-- Include Ecwid JS SDK -->
  <script src="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.0.1/ecwid-app.js"></script>

  <script>
    // Initialize the application
    EcwidApp.init({
      app_id: "my-super-app", // place your application namespace here (not clientID)
      autoloadedflag: true, 
      autoheight: true
    });

    // Get the store ID and access token
    var storeData = EcwidApp.getPayload();
    var storeId = storeData.store_id;
    var accessToken = storeData.access_token;

    // do something...
  </script>

  <!-- Include Ecwid CSS SDK -->
  <link rel="stylesheet" href="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.2.0/ecwid-app-ui.css"/>  
</head>

<body>
  <div>Show something</div>
</body>

</html>
```


Here you can find a starter template that you can use as a skeleton of your own application.

#### Notes
* `EcwidApp.init()` method initialize the application within Ecwid Control panel.
* `EcwidApp.getPayload()` method allows you to simply get the store ID and API access token. See details in the further sections

See the detailed description of the init() and getPayload() functions here: [Ecwid JS SDK](http://api.ecwid.com/#ecwid-javascript-sdk) .

## Authentication in embedded apps
In your application, you will likely show some user-specific data, for example the store order list. To do that, your iframe application will need to know:

* The ID of the store using your application at the moment
* The token that allows to access the store data

Ecwid will pass this data to your application as soon as it is opened in Ecwid Control panel. The way data is passed to your application and the way you should decrypt the received data depends on whether you process it on a client or a server side of your application. Below, you will find how you can do that in either case.

### User authentication on server side
Let's say, you process user input and prepare the data to display in your app on your server and then pass this information to your application UI to be displayed in the user Control panel. In this case, you will need to authenticate user on server side of your application. Ecwid sends an auth data to your app in a payload while requesting your iframe URL as follows:
`https://www.example.com/my-app-iframe-page?payload={payload}&cache-killer={cache-killer}`


> Example of the iframe URL call

```
https://www.example.com/my-app-iframe-page?payload=353035362c226163636573735f746f6b656e223a22776d6&cache-killer=13532
```


> Example of payload decryption (PHP)

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

// Get payload from the GET and process it
$ecwid_payload = $_GET['payload'];
$client_secret = "0123abcd4567efgh1234567890"; // this is a dummy value. Please place your app secret key here
$result = getEcwidPayload($client_secret, $ecwid_payload);
?>
```

> Example of decrypted payload

```json
{
  "store_id": 1003,
  "lang": "en_US",
  "access_token":"xxxxxxxxxxxxxxxx"
}
```

Name | Type | Description
---- | ---- | -----------
payload | string | Encrypted JSON string containing the authentication information (see the details below)
cache-killer | string | Random string preventing caching on your server

#### Payload

The payload parameter is encrypted JSON string, which, when decrypted, has the following format:

Name | Type | Description
---- | ---- | -----------
store_id | number | Ecwid store ID
lang | string | User language (which is currently set in their Control Panel). Use this parameter to translate your application UI to the user language.
access_token | string | oAuth token

#### Decryption of payload on your server

Ecwid uses *AES-128* to encrypt the payload. The key is the first 16 symbols (128 bit) of your application secret key (**client_secret**). It is provided when you register an app with us. See a PHP example of decryption to get better idea on how to receive and decrypt the payload.


### User authentication on client side

> Retrieving payload on client side using Ecwid JavaScript SDK 

```js
...
    var storeData = EcwidApp.getPayload();
    var storeId = storeData.store_id;
    var accessToken = storeData.access_token;
    // now you know the user you interact with and can access Ecwid API on their behalf
...
```

Ecwid allows your application to fully reside on client side and not use server side at all, i.e. you can authenticate the user, get store ID and access token and manage the store data via Ecwid API right inside your app in Control panel without calling your server scripts. For convenience, we provide a simple Javascript SDK that you can use in your application to authenticate the user and get access to the API. As soon as the JS SDK script is used, you can call the provided `EcwidApp.getPayload()` method to retrieve the user's store ID and access token as shown in example. See also [.getPayload()](#ecwidapp-getpayload) method specification.

So, in your application code, you will need to include Ecwid JS SDK script and use provided methods to authenticate the user as shown in the example. See also: [Ecwid Javascript SDK](#ecwid-javascript-sdk) .

## Troubleshooting

### A new tab inside Ecwid Control Panel is not appearing
You created an app and installed it on your test store, but the new tab is not appearing when you open your store. There are several possible reasons:

* **The application is not configured properly** to be displayed inside Control Panel. E.g. during registration, you forgot to mention that your app will embed itself into Control Panel, or did not choose exact section inside Control Panel where Ecwid needs to display your app. See ["Set up your application"](#set-up-your-application) for the details.
* **You didn't include the `add_to_cp` access scope** to the list of requested scopes while authorizing the app. While creating an oauth URL, make sure it incudes the "add_to_cp" scope in the list of requested permissions. 
* **You're testing it in an Ecwid store which is on Free plan**. Ecwid API functionality including embedding apps is available on paid Ecwid plans only. Please upgrade your account.

### The application tab appears in Ecwid Control Panel, but it doesn't load the app content
You created an app and installed it on your test store. The new tab appears in your Control Panel but the new tab content is not loaded and displaing the "Something went wrong" error message instead. Possible reasons:

* **Ecwid cannot reach the iframe URL** that you set up for your application either because it's unavailable or because it has restricted access.
* **Browser blocks the document in iframe** because it loads over HTTP while the Control Panel is working over HTTPS (the ["mixed content"](https://developer.mozilla.org/en-US/docs/Security/MixedContent) issue). Please make sure you set an HTTPS URL as the iframe URL in the app settings.
* **Ecwid Control Panel is restricted to load your app in iframe** because your app server responds with the "SAMEORIGIN" value in [X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/X-Frame-Options) header. 
* **The application in the tab is not initialized**. Make sure you initialized the app with the proper namespace using the .init() method of [Ecwid JS SDK](http://api.ecwid.com/#ecwid-javascript-sdk). 


# Ecwid CSS Framework

[Go to the CSS framework documentation →](http://api.ecwid.com/ecwid-css-framework/)

We provide a set of ready UI components in a form of CSS framework to help you easily design your application embedded into Ecwid Control Panel. The framework includes buttons, links, messages, forms in a nice and consistent design. 

### How to use it?

```html
<link rel="stylesheet" href="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.2.0/ecwid-app-ui.css"/>
```

1. Add this CSS file to your app embedded into Ecwid Control Panel: 
`https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.2.0/ecwid-app-ui.css`

2. Use this guide to find the elements and CSS classes you need: [http://api.ecwid.com/ecwid-css-framework/](http://api.ecwid.com/ecwid-css-framework/)

<img src="http://take.ms/uXxvy"></img>


# Ecwid Javascript SDK

```html
<!-- Include Ecwid JS SDK -->
<script src="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.0.2/ecwid-app.js"></script>
```

Ecwid Javascript SDK is a simple JS framework with a set of basic JS functions that will help you to embed your app to Ecwid Control Panel and interact with Ecwid from within your application.

To use the SDK, include this file into your app: `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.0.2/ecwid-app.js` .

## init

> Example of using EcwidApp.init() method

```js
// Initialize the application
EcwidApp.init({
  app_id: "my-super-app", // your application namespace (this is not clientId)
  autoloadedflag: true, 
  autoheight: true
});
```


The `EcwidApp.init()` method initializes your app inside Ecwid Control Panel. Call it once at the beginning of executable code in your app.

### Parameters

The only parameter is a JS object with the following fields:

Name | Type | Description
---- | ---- | -----------
**app_id** | string | Namespace of your application (as set in the application settings). This is not the same as clientId. 
autoloadedflag | boolean | Define how Ecwid should detect when your app is loaded. Set as `true`, if you want Ecwid to automatically detect the fact that you your app is loaded. Ecwid uses the window.onload event of your application document. If you want to contol when Ecwid should start displaying your app and inform it of your app's ready state, you should set this flag as `false` and use the [`EcwidApp.ready()`](#ready) method. As soon as the app is loaded, Ecwid hides the 'Loading' animation and shows the app content.
autoheight | boolean | Set as `true` if you want Ecwid to dynamically adjust your app iframe height depending on your app content. If you want to control the iframe size yourself, set this flag as `false` and use the [`EcwidApp.setSize()`](#setsize) method.


## getPayload

> Retrieving store ID and access token using Ecwid JavaScript SDK 

```js
...
    var storeData = EcwidApp.getPayload();
    var storeId = storeData.store_id;
    var accessToken = storeData.access_token;
    // now you know the user you interact with and can access Ecwid API on their behalf
...
```

`EcwidApp.getPayload()`

Above, we explained how your app can be a client-side HTML/JS application and still access Ecwid API right from Ecwid Control Panel (see [Authentication in embedded apps](#authentication-in-embedded-apps) section). There, we used the `EcwidApp.getPayload()` method to get the store ID and API access token. 


> Payload example

```json
{
  "store_id": 1003,
  "lang": "en_US",
  "access_token":"xxxxxxxxxxxxxxxx"
}
```


The payload is a JSON with the following fields:

Name | Type | Description
---- | ---- | -----------
store_id | number | Ecwid store ID
lang | string | User language (which is currently set in their Control Panel). Use this parameter to translate your application UI to the user language.
access_token | string | oAuth token



## openPage

> Open some page inside Control Panel from your application

```js
EcwidApp.openPage('products');
```

The `EcwidApp.openPage()` method allows you to direct the user to some particular page in the Control Panel

### Parameters
Name | Type | Description
---- | ---- | -----------
page | string | Hash part of of the page URL in the Control Panel. Examples: `billing` will open the Billing page, `products` will open the Catalog page.


## ready

You can use the `EcwidApp.ready()` method in your application to inform Ecwid of ready state of your application. For example, you may need to make a few API calls or load some additional assets before your app UI should be displayed to the user. In this way, pass `false` in the `autoloadedflag` parameter in the [`EcwidApp.init()`](#init) method and call the `.ready()` function when you are ready. 


## setSize

> Set your app iframe size from within the app

```js
EcwidApp.setSize({height: 800});
```

We recommend using the `autoheight` parameter set as `true` in [`EcwidApp.init()`](#init) function to let Ecwid dynamically adjust your app iframe size depending on your application content size. But if you want to control the iframe size yourself, set that flag as `false` and use this `EcwidApp.setSize()` method.


### Parameters

The only parameter is a JSON object with the `height` field:

Name | Type | Description
---- | ---- | -----------
height | number | The iframe height in pixels



