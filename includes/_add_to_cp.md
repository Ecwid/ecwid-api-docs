# Embedded apps

> Native app interface example

> ![Edit orders interface](https://don16obqbay2c.cloudfront.net/wp-content/uploads/native-apps-1551262705.png)

The Ecwid API allows your application to be embedded right into the user Control Panel and work like it is built into Ecwid. 

Overall, it works this way:

- The user installs the application and allows it to add a new tab to the Ecwid Control Panel.
- After installation, a new tab shows the page content of your specified IFRAME URL.
- Ecwid provides a [REST API](#rest-api-reference) and a [JS SDK](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#ecwid-javascript-sdk) for authentication and operations inside of the Ecwid Control Panel.

Although this approach is not necessary and you can use the Ecwid API without embedding an application into the Control Panel, we highly recommend it. Being integrated with Ecwid this way, your app will get way more user engagement, as it will be a part of a merchant store backend. 

**Links to get started quickly**: 

- [Set your app to embed into Ecwid Control Panel](https://developers.ecwid.com/api-documentation/building-a-native-app#set-up-your-application)
- [Build your app interface from a template](https://developers.ecwid.com/api-documentation/building-a-native-app#app-page-template)
- [Available user authentication types](https://developers.ecwid.com/api-documentation/authentication-in-native-apps#user-authentication)
- [Ecwid CSS Framework for Native Apps](https://developers.ecwid.com/api-documentation/ecwid-css-framework)
- [Ecwid JavaScript SDK for Native Apps](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk)
- [Troubleshooting issues with Native apps](https://developers.ecwid.com/api-documentation/troubleshooting)

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

## Building a native app

### Set up your application

After the app registration you will need to provide us with additional details about your app interface in the Ecwid Control Panel. 

These details are necessary to properly show your page, please see the required details below:

Parameter | Meaning
--------- | -------
Iframe URL | This is a <strong>HTTPS URL</strong> of the application page hosted on your server, which will be loaded in Ecwid Control panel. Requirements: <ul><li>It must load over <strong>HTTPS</strong></li> <li>The page must not contain header/footer, i.e. you will need to design this page as an embeddable, not as a standalone application.</li><li>The page must be mobile-ready for cases when store owners go to Ecwid Control Panel on mobile devices.</li><li>The page content should not contain the word 'Ecwid', so we can offer your app to our partners</li><li>Its interface must use [Ecwid CSS Framework](#ecwid-css-framework)</li><li>The page must <a href="https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#init">initialize the app</a> using Ecwid Javascript SDK to be displayed</li></ul>
App page title | This will be the title of the tab in Ecwid control panel where your application resides. Please keep it short as it will reside in a row of native Ecwid tabs and other applications 
Control panel section | The section of Ecwid control panel where you want your application to be added. Supported sections: <ul><li>*Sales* – choose this if your application works with orders or customers</li> <li>*Catalog* – choose this if your application works with products, variations, product images etc. </li> <li>*Discounts* – this section is for the applications working with discounts, coupons, loyalty programs and other promotion features</li> <li>*Settings* – you can choose this section if you need to place your application settings at the same level as the store settings</li><li>*Reports* - choose this section if your app shows store statistics</li><li>*Sales Channels* - choose this section if your app adds another sales channel for a store</li></ul>

<aside class="notice">
Access scope required: <strong>add_to_cp</strong> 
</aside>

If you already have a registered app and want to make it native, you can [contact us](/contact) to adjust your app settings.

### App page template

> Native app source code example: [https://github.com/Ecwid/sample-native-app](https://github.com/Ecwid/sample-native-app)

> Skeleton of an application embedded into Ecwid Control panel

```html
<!doctype html>
<html>

<head>
  <!-- Include Ecwid JS SDK -->
  <script src="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.2.5/ecwid-app.js"></script>

  <script>
    // Initialize the application
    EcwidApp.init({
      app_id: "my-super-app", // your client_id
      autoloadedflag: true, 
      autoheight: true
    });

    // Get the store ID and access token
    var storeData = EcwidApp.getPayload();
    var storeId = storeData.store_id;
    var accessToken = storeData.access_token;
    var language = storeData.lang;

    if (storeData.public_token !== undefined){
      var publicToken = storeData.public_token;
    }
    
    if (storeData.app_state !== undefined){
      var appState = storeData.app_state;
    }

    // do something...
  </script>

  <!-- Include Ecwid CSS Framework -->
  <link rel="stylesheet" href="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.6/ecwid-app-ui.css"/>  
</head>

<body class='normalized'>
  <div>Show something</div>

<!-- JS for CSS Framework components -->
  <script src="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.6/ecwid-app-ui.min.js"></script>
</body>

</html>
```

Here you can find a starter template that you can use as a skeleton of your own application.

#### Notes

* The `EcwidApp.init()` method initializes the application within Ecwid Control panel and allows it to show the page content
* The `EcwidApp.getPayload()` method allows you to get the Store ID and REST API access token. See details in the further sections
* Ecwid will load your app's *Iframe URL* with a payload to help identify the merchant's store. To find out more about the store authentication process in the app tab, see the [Authentication section](#authentication-in-embedded-apps).
* See the detailed description of the `init()` and `getPayload()` functions here: [Ecwid JS SDK](#ecwid-javascript-sdk).

A sample native application with examples of how to access Ecwid REST API, save data to application storage and how to design the app is available here: [https://github.com/Ecwid/sample-native-app](https://github.com/Ecwid/sample-native-app)

<aside class="note">
If your app for public use, it is required to use <a href='https://github.com/Ecwid/sample-native-app'>Sample Native App</a> interface for its user interface. 
</aside>

Also, see the [set up your application](https://developers.ecwid.com/api-documentation/building-a-native-app#set-up-your-application) and [Native Apps Guideline](/native-applications) to find out how this page should work.


#### Q: What is my app_id?

When user opens the application tab, the browser's address bar URL will have a format like this: 

`https://my.ecwid.com/cp/#app:name=my-cool-app&parent-menu=sales&app_state=orderId%3A%2012`

Where the `my-cool-app` is your **app_id** which you will need to use in this code template to initiale the application on the page. The `sales` part is the Ecwid Control Panel section where the app is embedded into.

#### Q: Do I need to use Ecwid styles for my app? 

Native apps become a part of an Ecwid Control Panel, hence they need to look like a part of it. You can use Ecwid CSS Framework to achieve it in your app. 

Ecwid CSS Framework allows you to create interface faster and easier. If your application is a custom solution for your own store, we still recommend using the Ecwid styles, but it's not mandatory. 

### Native interface translations

You are able to translate native application interface based on the language of Ecwid Control Panel. 

The current language of Ecwid Control Panel is provided in the authentication payload your application receives.

[Authentication Payload](https://developers.ecwid.com/api-documentation/authentication-in-native-apps#app-authentication) can be sent in two modes: 

- client-side (via hash `#`)
- server-side (URL query string: `?` and `&`)

**Use these steps to translate your Native app**: 

1. Find out how your app gets the payload (check your app iframe URL in browser's dev tools)
2. Get `lang` field value from the payload
3. Load the interface based on that value

If the current language is not supported, use the fallback labels. For example, load English texts if user is Spanish and you don't have Spanish translations yet. 

More info on getting and parsing authentication payload: [https://developers.ecwid.com/api-documentation/authentication-in-native-apps#app-authentication](https://developers.ecwid.com/api-documentation/authentication-in-native-apps#app-authentication)

## Deep linking

Native apps in Ecwid Control Panel support deep linking, which means that they can receive information prior to being loaded and opened. This can provide your app with a new level of interactivity with a user by reacting to the context, sent to your app.

This functionality is achieved by passing a URL-encoded value - `app_state` to your application prior to loading and opening it for a user. 

### Sending app state

A typical native application URL looks like this: `https://my.ecwid.com/cp/CP.html#app:name=my-cool-app&parent-menu=sales`

In case of your app being called using deep linking, that URL will also have a new parameter - `app_state` :

`https://my.ecwid.com/cp/#app:name=my-cool-app&app_state=orderId%3A%2012`

The `app_state` parameter value is a URL encoded string with a specific application state your app can understand and process. 

### Receiving app state

Receiving and processing the externally called app state depends on the type of user authentication you are using. See the details below. 

**Default user authentication**

> Default user authentication

```
GET https://www.example.com/my-app-iframe-page#53035362c226163636573735f746f6b656e223a22776d6
```

```json
{
  "store_id": 1003,
  "lang": "en",
  "access_token":"xxxxxxxxxxxxxxxx",
  "app_state":"orderId%3A%2012",
  "public_token":"public_QnWFi7GpGemRUvz3SJ18crtJ5ru8yjfy"
}
```

> Get app state with Ecwid JS SDK

```js
  var storeData = EcwidApp.getPayload();
  var storeId = storeData.store_id;
  var accessToken = storeData.access_token;
  var language = storeData.lang;

  if (storeData.public_token !== undefined){
    var publicToken = storeData.public_token;
  }

  if (storeData.app_state !== undefined){
    var appState = storeData.app_state;
  }
```

When using default user authentication, the app state will be delivered through the Ecwid JavaScript SDK in the `EcwidApp.getPayload()` method.

Once it's called, you can save the user details and app state into your client-side variables. See example on the right.

Learn more about [Default User Authentication](https://developers.ecwid.com/api-documentation/authentication-in-native-apps#default-user-auth)

**Enhanced security user authentication**

> Enhanced security user authentication

```
GET https://www.example.com/my-app-iframe-page?payload=353035362c226163636573735f746f6b656e223a22776d6&app_state=orderId%3A%2012&cache-killer=13532
```

> Get app state from GET parameter

```php
  // URL Encoded App state passed to the app
  if (isset($_GET['app_state'])){
    $app_state = $_GET['app_state'];
  }
```

When using enhanced security user authentication, the app state will be delivered as a GET parameter `app_state` of your iframe URL alongside the standard parameters.

You can retrieve it just like any other value of a GET parameter on a server-side and then save and use it in your app code. See example on the right.

Learn more about [Enhanced Security User Authentication](https://developers.ecwid.com/api-documentation/authentication-in-native-apps#enhanced-security-user-auth)

## Authentication in native apps

### User authentication

Native apps can help you show store data, like orders or products. To do that, your application will need to know:

* Store ID of a merchant
* Access token for the Ecwid REST API
* Language of the Ecwid Control Panel
* Application state to initialize the app with (optional)

Ecwid will pass this data to your application as soon as it is opened in Ecwid Control panel. 

See the available user authentication methods for native applications below.

#### Default User Authentication

In the default user auth process, Ecwid will call your iframe URL like this: 

`https://www.example.com/my-app-iframe-page#53035362c226163636573735f746f6b656e223a22776d6`

Where the `#53035362c226163636573735f746f6b656e223a22776d6` is the payload, containing store information described above. 

This process allows for simple user authentication in your app using the **Ecwid JS SDK**.

[Continue with default authentication](https://developers.ecwid.com/api-documentation/authentication-in-native-apps#default-user-auth)

#### Enhanced Security User Authentication

In the enhanced security auth process, Ecwid will call your iframe URL like this:

`https://www.example.com/my-app-iframe-page?payload=353035362c226163636573735f746f6b656e223a22776d6&app_state=orderId%3A%2012&cache-killer=13532`

Where the `?payload=353035362c226163636573735f746f6b656e223a22776d6&app_state=orderId%3A%2012` is the payload and app state, containing store information described above. 

We recommend using this type of authentication for complex applications that can modify parts of a store and require additional security measures on server-side. 

[Continue with enhanced security authentication](https://developers.ecwid.com/api-documentation/authentication-in-native-apps#enhanced-security-user-auth)

<aside class="notice">By default, Ecwid uses <strong>Default User Authentication</strong> so you can start working on your application's tab right away. If you need your app to be switched to <strong>Enhanced Security User Authentication</strong>, please <a href='/contact'>contact us</a> and we will update your app.</aside>

### App authentication

After your app authorized a merchant, it will need an **access token** for the Ecwid REST API to read and modify Ecwid store orders, products and other information. 

The process of getting an access token is different for each user authentication type. Both authentication types are described below, so please check them out for more info on getting access tokens.

### Default User Auth

> Example of the iframe URL call in client-side apps

```
https://www.example.com/my-app-iframe-page#53035362c226163636573735f746f6b656e223a22776d6
```

> Workflow of client-side applications

```js
//
//  Get store details
//
    var storeData = EcwidApp.getPayload();
    var storeId = storeData.store_id;
    var accessToken = storeData.access_token;
    var language = storeData.lang;
    var viewMode = storeData.view_mode;
    
    if (storeData.public_token !== undefined){
      var publicToken = storeData.public_token;
    }
    
    if (storeData.app_state !== undefined){
      var appState = storeData.app_state;
    }

//
//  Get store specific data
//

    var backgroundColor;
    EcwidApp.getAppStorage('color', function(value) {
      if (value !== null) {
        // set user color from storage
        backgroundColor = value;
      } else {
        // set default color
        backgrountColor = 'black';
      } 
    });

//
//  Start the flow of your application
//  ...
```

Ecwid allows your application to fully reside on client side and not use server side at all. 

You can authenticate the user, get store ID and access token, and manage the store data via Ecwid API right inside your app in Control panel without calling your server scripts. 

By default, all applications are registered with this authentication type.

The workflow can be described into the following steps: 

1. Get store preferences and data
2. Initialize your application functionality

#### 1. Get store preferences and data

For convenience, we provide a simple [Javascript SDK](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk) to authenticate the user and get access to the API. 

As soon as the JS SDK script is used, you can call the provided `EcwidApp.getPayload()` method to retrieve the user's store ID and access token as shown in example code on the right. See also [.getPayload()](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#getpayload) method specification. 

[Application Storage](https://developers.ecwid.com/api-documentation/application-storage) can help you store and access user preferences without saving this information on your server. Ecwid JavaScript SDK can help you do that easy with built-in methods.

See functions `EcwidApp.getAppStorage`, `EcwidApp.setAppStorage` and `EcwidApp.getAppPublicConfig`, `EcwidApp.setAppPublicConfig` in [Ecwid Javascript SDK](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk) for more details.

#### 2. Initialize your app functionality

After completing the first step, you know the store you are working with and you have the settings and other data specific to that store. 

Use use that information in your native application to start its standard workflow: load user preferences, render the user interface and so on. 

### Enhanced Security User Auth

In some cases you may need additional security for authenticating users: server-side app, integrations with 3rd party login systems and so on. 

That's when you should enable the Enhanced Security User Auth type for your Native app.

<aside class="notice">All Native applications are registered with the <a href = "https://developers.ecwid.com/api-documentation/authentication-in-native-apps#default-user-auth">Default User Auth</a>, so you can start working on your application's tab right away without using server side. <br/><br/>

If you need your app to be switched to Enhanced Security User Auth (server-side), please <a href = "/contact">contact us</a> and we will update your app.
</aside>

The workflow of such applications can be divided into several steps: 

1. Decrypt the payload from Ecwid 
2. Get store specific data
3. Initialize your application functionality

#### 1. Decrypt the payload from the Ecwid Control Panel

Let's say, you process user input and prepare the data to display in your app on your server and then pass this information to your application UI to be displayed in the user Control panel. 

In this case, you will need to authenticate user on server side of your application. 

When using Enhanced Security Auth, Ecwid sends an auth data to your app in a payload while requesting your iframe URL as follows: 

`https://www.example.com/my-app-iframe-page?payload={payload}&app_state={app_state}&cache-killer={cache-killer}`

> Example of the iframe URL call in server-side apps

```
https://www.example.com/my-app-iframe-page?payload=353035362c226163636573735f746f6b656e223a22776d6&app_state=orderId%3A%2012&cache-killer=13532
```


> Workflow of server-side applications (PHP)

```php
<?php
//
//  Get and decrypt the payload from Ecwid
//

// authenticate user in iframe page
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
$client_secret = "0123abcd4567efgh1234567890"; // This is a dummy value. Place your client_secret key here. You received it from Ecwid team in email when registering the app 

$result = getEcwidPayload($client_secret, $ecwid_payload);

// Get store info from the payload
$token = $result['access_token'];
$storeId = $result['store_id'];
$lang = $result['lang'];
$viewMode = $result['view_mode'];

if (isset($result['public_token'])){
  $public_token = $result['public_token'];
}

// URL Encoded App state passed to the app
if (isset($_GET['app_state'])){
  $app_state = $_GET['app_state'];
}

//
//  Get store specific data
//

// Get store specific data from storage endpoint
$key = 'color';

$url = 'https://app.ecwid.com/api/v3/' .$storeId. '/storage/' .$key. '?token=' .$token;

$ch = curl_init();

curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_URL,$url);

$curlResult = curl_exec($ch);
curl_close($ch);

$curlResult = (json_decode($curlResult));
$color = $curlResult -> {'value'};

  if ($color !== null ) {
    // set color from storage
    } else {
    // set default colors
  }

//
//  Start the flow of your application
//  ...
?>
```

> Example of decrypted payload

```json
{
  "store_id": 1003,
  "lang": "en",
  "access_token":"xxxxxxxxxxxxxxxx",
  "view_mode":"PAGE",
  "public_token":"public_ASDlkDASmasdaslkdASkndasANJKLsAf"
}
```

Name | Type | Description
---- | ---- | -----------
payload | string | Encrypted JSON string containing the authentication information (see the details below)
cache-killer | string | Random string preventing caching on your server
app_state | string | Optional **URL Encoded** app state passed to the app in the URL

#### Payload

The payload parameter is encrypted JSON string, which, when decrypted, has the following format:

Name | Type | Description
---- | ---- | -----------
store_id | number | Ecwid store ID
lang | string | User language (which is currently set in their Control Panel). Use this parameter to translate your application UI to the user language.
access_token | string | Secure oAuth token for Ecwid REST API
public_token | string | Public oAuth token for Ecwid REST API
view_mode | string | Mode used to display the application interface: `POPUP`, `PAGE` or `INLINE`. `PAGE` is a default mode when app is displayed in a separate tab in Ecwid Control Panel, `POPUP` is used when the app UI is displayed in a popup on any page of Ecwid CP, `INLINE` type is used for displaying the interface inside an element on a page where other elements are also present

#### Decryption of payload on your server

Ecwid uses *AES-128* to encrypt the payload. The key is the first 16 symbols (128 bit) of your application secret key (**client_secret**). It is provided when you register an app with us. See a PHP example of decryption to get better idea on how to receive and decrypt the payload.

**If you are using C# language**

For correct payload decryption, create additional padding to make the payload a multiple of 4: 

`base64 = base64.PadRight(base64.Length + (4 - (base64.Length % 4)), '=');`

#### 2. Get store preferences and data

After completing the first step, you have store details and access to it using Ecwid REST API. 

Now you can get store-specific information from the storage endpoint or from your local database. 

The result of the payload decryption will be provided in an array `$result`, which you can use to get store ID, access token and language information about a store, that opened your Native app.

[Application Storage](https://developers.ecwid.com/api-documentation/application-storage) can help you store and access user preferences with the Ecwid REST API. 

To save and get store-specific data in the storage endpoint, you can use cURL functionality in PHP or any other way that you prefer to access REST API endpoints. 

See how to retrieve the value of `'color'` key in the application storage using cURL and process the result based on a response from API on the right.

#### 3. Initialize your application functionality

Once you got all details that you need, you can start the standard planned workflow for your app and operate with Ecwid API using the details you got earlier.

## Ecwid CSS Framework

[Go to the CSS framework documentation →](/ecwid-css-framework)

We provide a set of ready UI components in a form of CSS framework to help you easily design your application embedded into Ecwid Control Panel. The framework includes buttons, links, messages, forms in a nice and consistent design. 

#### How to use it?

```html
<head>
  <link rel="stylesheet" href="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.6/ecwid-app-ui.css"/>
</head>

<body>
  
  <div>Some content</div>

  <script type="text/javascript" src="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.6/ecwid-app-ui.min.js"></script>
</body>
```

1) Add this CSS file to your native app: 
`https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.6/ecwid-app-ui.css`

2) Add this JS file at before closing the BODY tag: 
`https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.6/ecwid-app-ui.min.js`

3) Use this guide to find the elements and CSS classes you need: [http://developers.ecwid.com/ecwid-css-framework/](/ecwid-css-framework)

<img src="https://dj925myfyz5v.cloudfront.net/wp-content/uploads/ecwid-buttons.png"></img>


## Ecwid Javascript SDK

```html
<!-- Include Ecwid JS SDK -->
<script src="https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.2.5/ecwid-app.js"></script>
```

Ecwid Javascript SDK is a simple JS framework with a set of basic JS functions that will help you to embed your app to Ecwid Control Panel and interact with Ecwid from within your application.

To use the SDK, include this file into your app: `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.2.5/ecwid-app.js` .

### init

> Example of using EcwidApp.init() method

```js
// Initialize the application
EcwidApp.init({
  app_id: "my-super-app", // use your application client_id
  autoloadedflag: true, 
  autoheight: true
});
```

The `EcwidApp.init()` method initializes your app inside Ecwid Control Panel. Call it once at the beginning of executable code in your app.

You can get the `app_id` using the URL in your browser when you open your application tab. Here's a typical link that you can get when you open your app: `https://my.ecwid.com/cp/CP.html#app:name=my-super-app&parent-menu=promotions`

The `app_id` here is the `my-super-app` part. Make sure to use this to initialize your application with Ecwid JS SDK.

#### Parameters

The only parameter is a JS object with the following fields:

Name | Type | Description
---- | ---- | -----------
**app_id** | string | Namespace of your application (as set in the application settings)
autoloadedflag | boolean | Define how Ecwid should detect when your app is loaded. Set as `true`, if you want Ecwid to automatically detect the fact that you your app is loaded. Ecwid uses the window.onload event of your application document. If you want to contol when Ecwid should start displaying your app and inform it of your app's ready state, you should set this flag as `false` and use the [`EcwidApp.ready()`](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#ready) method. As soon as the app is loaded, Ecwid hides the 'Loading' animation and shows the app content.
autoheight | boolean | Set as `true` if you want Ecwid to dynamically adjust your app iframe height depending on your app content. If you want to control the iframe size yourself, set this flag as `false` and use the [`EcwidApp.setSize()`](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#setsize) method.


### getPayload

> Retrieving store ID and access token using Ecwid JavaScript SDK 

```js
    var storeData = EcwidApp.getPayload();
    var storeId = storeData.store_id;
    var accessToken = storeData.access_token;
    var language = storeData.lang;
    var viewMode = storeData.view_mode;

    if (storeData.public_token !== undefined){
      var publicToken = storeData.public_token;
    }
    
    if (storeData.app_state !== undefined){
      var appState = storeData.app_state;
    }

    // now you know the user you interact with and can access Ecwid API on their behalf
```

`EcwidApp.getPayload()`

Above, we explained how your app can be a client-side HTML/JS application and still access Ecwid API right from Ecwid Control Panel (see [Authentication in native apps](#authentication-in-native-apps) section). There, we used the `EcwidApp.getPayload()` method to get the store ID and API access token. 

The payload is a JSON with the following fields:

Name | Type | Description
---- | ---- | -----------
**store_id** | number | Ecwid store ID
**lang** | string | User language (which is currently set in their Control Panel). Use this parameter to translate your application UI to the user language.
**access_token** | string | Secure oAuth token
app_state | string | URL Encoded application state
public_token | string | Public oAuth token for Ecwid REST API
view_mode | string | Mode used to display the application interface: `POPUP`, `PAGE` or `INLINE`. `PAGE` is a default mode when app is displayed in a separate tab in Ecwid Control Panel, `POPUP` is used when the app UI is displayed in a popup on any page of Ecwid CP, `INLINE` type is used for displaying the interface inside an element on a page where other elements are also present

<aside class="note">Fields marked in <strong>bold</strong> are always sent in the payload. Others are sent depending on conditions.</aside>

### openPage

> Open some page inside Control Panel from your application

```js
EcwidApp.openPage('products');
```

The `EcwidApp.openPage()` method allows you to direct the user to some particular page in the Control Panel

To open a page in Ecwid CP, you will need to get its identification after the hash (#) part in the URL. 

#### Parameters
Name | Type | Description
---- | ---- | -----------
page | string | Hash part of of the page URL in the Control Panel. Examples: `billing` will open the Billing page, `products` will open the Catalog page.

**Examples**: 

1) Open Catalog page 

Target URL: `https://my.ecwid.com/store/1003#products`
What we need use: `products`

The result: `EcwidApp.openPage('products');`

2) Open specific order details page

Target URL: `https://my.ecwid.com/store/5035009#order:id=938&return=orders`
What we need to use: `order:id=938&return=orders`

The result: `EcwidApp.openPage('order:id=938&return=orders');`

### ready

> Show app UI when your app is ready to show it

```js
// Initialize the application
EcwidApp.init({
  app_id: "my-super-app", // use your application client_id
  autoloadedflag: false, 
  autoheight: true
});

// Show app UI
EcwidApp.ready();
```

You can use the `EcwidApp.ready()` method in your application to inform Ecwid of ready state of your application. 

For example, you may need to make a few API calls or load some additional assets before your app UI should be displayed to the user. In this way, pass `false` in the `autoloadedflag` parameter in the [`EcwidApp.init()`](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#init) method and call the `.ready()` function when you are ready. 


### setSize

> Set your app iframe size from within the app

```js
EcwidApp.setSize({height: 800});
```

We recommend using the `autoheight` parameter set as `true` in [`EcwidApp.init()`](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#init) function to let Ecwid dynamically adjust your app iframe size depending on your application content size. But if you want to control the iframe size yourself, set that flag as `false` and use this `EcwidApp.setSize()` method.


#### Parameters

The only parameter is a JSON object with the `height` field:

Name | Type | Description
---- | ---- | -----------
height | number | The iframe height in pixels

### setAppStorage

> Save multiple 'key' : 'value' data to app storage

```js
var data = { 
  color : 'red',
  size : 'big',
  page_id : '123456'
};

EcwidApp.setAppStorage(data, callback);
```

#### Parameters

`EcwidApp.setAppStorage` accepts two parameters: 

Name | Type | Description
---- | ---- | -----------
**data** | Object | Place object you want to add in storage
callback | Function | Specify your callback function if needed

Your object data as specified in example will be stored as corresponding keys in your application storage in `'key' : 'value'` format.

This method accepts only string type values in your data object, so make sure all values in your object, such as 'red', are of type `string`.

Learn more about application storage: [https://developers.ecwid.com/api-documentation/application-storage](https://developers.ecwid.com/api-documentation/application-storage)

### setAppPublicConfig

> Save a string to 'public' key in application storage

```js
var data = '{ "color" : "red", "page_id" : "123456" }';

EcwidApp.setAppPublicConfig(data, callback);
```

> Get app public config in your native app

```js
var publicConfig;

EcwidApp.getAppStorage('public', function(value){
  console.log(value);
  // '{ "color" : "red", "page_id" : "123456" }'

  publicConfig = value;
})

publicConfig = JSON.parse(publicConfig);

console.log(publicConfig.color);
// 'red'
```

#### Parameters

`EcwidApp.setAppPublicConfig` accepts two parameters: 

Name | Type | Description
---- | ---- | -----------
**data** | String | Place json string you want to add in public storage
callback | Function | Specify your callback function if needed

**The string that you provide in `data` variable will be specified for `public` key in application storage.** [Learn more](https://developers.ecwid.com/api-documentation/public-application-config)

You will be able to retrieve it using [Ecwid Javascript API](https://developers.ecwid.com/api-documentation/get-storefront-details#ecwid-getapppublicconfig) in storefront. 

If you need to pass more than one value, you can specify your data in a json string and parse that string in Ecwid storefront. 

<aside class="note">
This method accepts <strong>string type values only</strong> in your data object, so make sure all values in your object, such as 'red', are of type <strong>'string'</strong>.
</aside>

<aside class="notice">
Data that you save for public access cannot exceed <strong>64Kb</strong>
</aside>

### getAppStorage

> Get all data from application storage

```js
EcwidApp.getAppStorage(callback);
```

> Example

```js
EcwidApp.getAppStorage(function(allKeys) {
  // returns an array of objects, containing all keys and their values in your app storage
  console.log(allKeys);
});
```

> Get data from application storage by key

```js
EcwidApp.getAppStorage(key, callback);
```

> Example

```js
EcwidApp.getAppStorage('color', function(value){
  //prints 'red' 
  console.log(value);
});
```

#### Parameters

`EcwidApp.getAppStorage` accepts two parameters: 

Name | Type | Description
---- | ---- | -----------
key | string | Specify key that you need to get value from. If no key specified, all data will be returned
**callback** | Function | Specify your callback function

Using this method you can retrieve either all keys and their values that are located in your application storage or get the value of a specific key.

Learn more about application storage: [https://developers.ecwid.com/api-documentation/application-storage](https://developers.ecwid.com/api-documentation/application-storage)

### getAppPublicConfig

> Get app public config value function usage

```js
EcwidApp.getAppPublicConfig(callback);
```

> Get app public config value example

```js
EcwidApp.getAppPublicConfig(function(publicData) {
  // returns value of 'public' key in application storage
  console.log(publicData);
});
```


#### Parameters

`EcwidApp.getAppPublicConfig` accepts two parameters: 

Name | Type | Description
---- | ---- | -----------
**callback** | Function | Specify your callback function

Using this method you can retrieve the value of public application config stored in your application storage. Learn more about public app config: [https://developers.ecwid.com/api-documentation/public-application-config](https://developers.ecwid.com/api-documentation/public-application-config)

### closeAppPopup

```js
EcwidApp.closeAppPopup();
```

When a native application is opened in a popup window in Ecwid Control Panel, executing this function will close that popup for the user.

In case when application is opened in a tab, nothing will happen. 

### sendUserToUpgrade

> Send user to upgrade usage

```
EcwidApp.sendUserToUpgrade(features);
```

> Send user to upgrade examples

```js
// Send user to upgrade to get Product Variations feature
EcwidApp.sendUserToUpgrade(["COMBINATIONS"]);

// Send user to upgrade to the minimal plan where Order Editor and Marketplaces features are available
EcwidApp.sendUserToUpgrade(["ORDER_EDITOR", "MARKETPLACES"]);

```

Ecwid JS SDK allows your app interface to initiate the upgrade process for a user. The upgrade process is launched using the `EcwidApp.sendUserToUpgrade()` function, where you should provide the feature to upgrade to.

When you pass `features` array as argument, Ecwid will determine the minimum required plan for merchant to use them and initiate upgrade process

Features list is available in the [Store profile endpoint](https://developers.ecwid.com/api-documentation/store-information#get-store-profile) of Ecwid REST API. [Contact Ecwid team](/contact) to find out the feature name if it's missing in your store.

## Troubleshooting

### "Something went wrong" error

You created an app and installed it on your test store. The new tab appears in your Control Panel. 

But the new tab content is not loaded and displaing the "Something went wrong" error message instead? Possible reasons:

##### The application in the tab is not initialized

To display your app, it must first be initialized via Ecwid JS SDK with a correct namespace.

*How to check*

Make sure you initialize the app in your code with the proper namespace using the `.init()` method of [Ecwid JS SDK](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk). 

##### The iframe URL performs a redirect

When loading application in Ecwid Control Panel, Ecwid checks for the iframe page to have the same address as specified in the application. 

*How to check*

Open Development tools in your browser. Find your iframe URL in the Ecwid Control Panel DOM.

Open that URL to check if it does any redirects when opened. Make sure that your page initializes the app via JS SDK **before making a redirect**.

##### Browser cannot reach the iframe URL

Your URL is unavailable or because it has restricted access.

*How to check*

Open Development tools in your browser. Find your iframe URL in the Ecwid Control Panel DOM. 

Open that URL to check if it opens for you successfully. 

##### Browser blocks the document in iframe

The page loads content over HTTP while the Control Panel is working over HTTPS only - (the ["mixed content"](https://developer.mozilla.org/en-US/docs/Security/MixedContent) issue). 

*How to check*

When this happens, you should see a browser prompt at the top about some content being blocked on a page. 

To fix this, you can either explicitly allow it to load, or provide Ecwid team with a HTTPS iframe URL for your app. 

##### Ecwid Control Panel is blocked from loading your app in iframe

Your app server responds with the `"SAMEORIGIN"` value in [X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/X-Frame-Options) header. 

*How to check*

This usually is recorded in the browser's console in Developer tools. You can also open your iframe URL directly and check the headers it sends. 

### App tab does not appear

You created an app and installed it on your test store, but the new tab is not appearing when you open your store. There are several possible reasons:

##### The application is misconfigured to be displayed inside Control Panel

*How to check*

See ["Set up your application"](https://developers.ecwid.com/api-documentation/building-a-native-app#set-up-your-application) and make sure you've provided those details to the Ecwid team.

##### The app doesn't have the necessary access scope

While creating an oauth URL, make sure it incudes the `"add_to_cp"` scope in the list of requested permissions. 

*How to check*

Go to Ecwid Control Panel -> Apps -> My apps and check the permissions of your app provided by the store. If it misses the `add_to_cp` permission, contact Ecwid team to add it. 

##### You're testing it in an Ecwid store which is on Free plan 

Ecwid API functionality including embedding apps is available on paid Ecwid plans only. Please upgrade your account.

*How to check*

Go to Ecwid Control Panel -> My Profile -> Billing and Plans and check your current plan status. 

If you need a store for testing and development only, Ecwid can provide a free upgrade for you. [Contact Ecwid team](/contact) with Store ID and app details to get it.


