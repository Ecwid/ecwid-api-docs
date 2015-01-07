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
iframe URL | This is a URL of the application page hosted on your server, which will be loaded in Ecwid Control panel. Requirements: <ul><li>It must load over HTTPS</li> <li>The page must not contain header/footer, i.e. you will need to design this page as an embeddable, not as a standalone application.</li></ul>
App page title | This will be the title of the tab in Ecwid control panel where your application resides. Please keep it short as it will reside in a row of native Ecwid tabs and other applications 
Control panel section | The section of Ecwid control panel where you want your application to be added. Supported sections: <ul><li>*Sales* – choose this if your application works with orders or customers</li> <li>*Products* – choose this if your application works with products, combinations, product images etc. </li> <li>*Promotions* – this section is for the application working with discounts, coupons, loyalty programs and other promotion features</li></ul>
Server-side or Client-side app | The way Ecwid passes the store information to your app will depend on whether your application will work on client side only or it's a server-side application.

You'll be asked for those details on the app registration page. If you already have a registered app and want to make it embeddable, you can contact us to adjust your app settings.

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
  <script src="https://app.ecwid.com/ecwid-app.js"></script>

  <script>
    // Initialize the application
    EcwidApp.init({
      app_id: "my-super-app", // your application namespace
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
  <link rel="stylesheet" href="https://d3fi9i0jj23cau.cloudfront.net/gz/17.0-117-gf439c8d/ecwid-app-ui.css"/>
  
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


## Authentication in embedded apps
In your application, you will likely show some user-specific data, for example the store order list. To do that, your iframe application will need to know:

* The ID of the store using your application at the moment
* The token that allows to access the store data

Ecwid will pass this data to your application as soon as it is opened in Ecwid Control panel. The way data is passed to your application and the way you should decrypt the received data depends on whether you process it on a client or a server side of your application. Below, you will find how you can do that in either case.

### User authentication on server side
Let's say, you process user input and prepare the data to display in your app on your server and then pass this information to your application UI to be displayed in the user Control panel. In this case, you will need to authenticate user on server side of your application. Ecwid sends an auth data to your app in a payload while requesting your iframe URL as follows:
`https://www.example.com/my-app-iframe-page?payload={payload}&cache-killer={cache-killer}`

```
https://www.example.com/my-app-iframe-page?payload=353035362c226163636573735f746f6b656e223a22776d6&cache-killer=13532
```

Name | Type | Description
---- | ---- | -----------
payload | string | Encrypted JSON string containing the authentication information (see the details below)
cache-killer | string | Random string preventing caching on your server

#### Payload

> Example of decrypted payload

```json
{
  "store_id": 1003,
  "sign_date": 1336914487311,
  "app_id": "aff5016d29144d",
  "access_token":"xxxxxxxxxxxxxxxx",
  "permissions": ["add_to_cp", "read_store_profile", "read_orders"]
}
```

The payload parameter is encrypted JSON string, which, when decrypted, has the following format:

Name | Type | Description
---- | ---- | -----------
store_id | number | Ecwid store ID
sign_date | number | Payload generation date/time (UNIX timestamp)
app_id | string | Application ID
access_token | string | oAuth token
permissions | array of strings | List of permissions (API access levels) given to the app, separated by space

#### Decryption of payload on your server

> Example of payload decryption (PHP)

```php
// An example will be added soon
```

Ecwid uses *AES-128* to encrypt the payload. The key is the first 16 symbols (128 bit) of your application secret key (**client_secret**). It is provided when you register an app with us. See a PHP example of decryption to get better idea on how to receive and decrypt the payload.


### User authentication on client side

> Retrieving of the store ID and access token using Ecwid JavaScript SDK

```js
...
    var storeData = EcwidApp.getPayload();
    var storeId = storeData.store_id;
    var accessToken = storeData.access_token;
    // now you know the user you interact with and can access Ecwid API on their behalf
...
```

Ecwid allows your application to fully reside on client side and not use server side at all, i.e. you can authenticate the user, get store ID and access token and manage the store data via Ecwid API right inside your app in Control panel without calling your server scripts. For convenience, we provide a simple Javascript SDK that you can use in your application to authenticate the user and get access to the API. As soon as the JS SDK script is included, you can use the provided `EcwidApp.getPayload()` method to retrieve the user's store ID and access token as shown in example.

So, in your application code, you will need to include Ecwid JS SDK script and use provided methods to authenticate the user as shown in the example. See also: [Ecwid JS SDK](#js-css-sdk) .

# JS/CSS SDK

<aside class="notice">
Documentation is in progress...
</aside>
