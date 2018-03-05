# Application storage

> App storage data example

> ![App storage data example](https://don16obqbay2c.cloudfront.net/wp-content/uploads/appStorage-1468406476.png)

The Ecwid API offers simple key-value storage for apps. You can use it to save private user preferences and public storefront settings, or as a database for your app.

This storage allows you to create complex client-side (JavaScript) apps and skip using a database on a server (like MySQL, etc.).

[Learn more about Application Storage benefits](https://developers.ecwid.com/api-documentation/benefits-of-application-storage)

Each data entry is stored as a pair: a key and its value. You can get a specific value right away by checking a corresponding key. It is available for both embeddable and storefront apps. 

[Using application storage in your app](https://developers.ecwid.com/api-documentation/storage-in-ecwid-api)

<aside class="note">
  <strong>Important</strong>: the storage of each app is isolated from other apps, so user data of your application is not available to other apps and vice versa.
</aside>

You can also retrieve public app configuration (user preferences, etc.) saved in the application storage when your app works in storefront. [Learn more](https://developers.ecwid.com/api-documentation/public-application-config)

## Benefits of application storage

### For client-side applications

Many Ecwid applications are client-side, which are created completely in Javascript and don’t require any backend interface or server-side functionality for a developer. 

These apps don’t require their own database on a dedicated server to manage user specific data and access it when it’s needed. To access that data, apps make calls to Ecwid API, where this data is stored.

Ecwid API has got you covered on two main aspects of developing client-side applications: private data can be stored securely on Ecwid servers and all the hosting for that data is provided by Ecwid as well. This allows for a secure and fast storage for your application’s needs.

Your application can also utilize storage as a primary key-value database using the Ecwid API. Ecwid provides all the tools to operate with that storage – you can manage storage either through REST API or using simple functions provided by the Javascript SDK. 

### For server-side applications

Sometimes server-side applications need to store user’s data as well as the client-side ones. Usually developers create mySQL or PostgreSQL databases or store data locally on a file system. 

Application storage offers an alternative - store data on Ecwid servers and access it using REST API. Why it can be helpful: now there is no need for mySQL databases, your code becomes simpler and you don’t have to worry about storing the private merchant data on your servers. 

## Storage in Ecwid API

Below you can find information about two different ways you can access your application storage depending on the type of an application you have.

### Javascript Storage API

This JavaScript storage API allows to set and update values in the app storage right from your JavaScript code. You should use it if your app is client-side, works mainly in Javascript and you don’t plan to use a server.

Use the code examples below with the help of Ecwid JS SDK to manage user details in your application's tab in Ecwid Control Panel.

<aside class="notice">Your application must be a native <a href="https://developers.ecwid.com/api-documentation/authentication-in-embedded-apps#default-user-auth">client-side</a> application to access JavaScript Storage API.
</aside>

#### Save data to storage

> Save data to application storage

```js
var data = { 
  color : 'red',
  size : 'big',
  page_id : '123456'
};

EcwidApp.setAppStorage(data, function(){
  console.log('Data saved!');
});
```

To save data to storage, create a Javascript object with all fields and values for them specified there. Once it’s ready, send it to app storage using `EcwidApp.setAppStorage` function to store them for future use. 

This function supports object as an incoming data type only, so make sure that you send object as a first parameter of that function. The example code is available on the right.

<aside class="notice">The data you wish save to app storage must be stored in an object and values must be string type at all times.
</aside>

#### Get data from storage

> Get all data from application storage 

```js
EcwidApp.getAppStorage(function(allKeys) {
  // prints an array of key : value objects from app storage
  console.log(allKeys);
});
```

To retrieve all data from storage, use `EcwidApp.setAppStorage` with a callback function as a parameter. It will return an array of objects, containing all keys and their values in your app storage. You can save that response in a variable and access it in your app later.

> Get data from application storage by key

```js
EcwidApp.getAppStorage('color', function(value){
  //prints 'red' 
  console.log(value);
})
```

To retrieve data for a specific key, use `EcwidApp.setAppStorage` and specify the key as first parameter and callback function as the second one. It will return a value of the key you requested in a form of a string.

> Get public app config

```js
EcwidApp.getAppStorage('public', function(value){
  //prints '1234' 
  console.log(value);
})
```

Public application config is a part of application storage: data that you save there is kept in `public` key of the storage. In case if you need to check the value you saved to public app config, use `EcwidApp.getAppStorage` and specify the `public` key as first parameter and a callback function as a second one. 

### REST Storage API

This REST API allows to set, update and delete values in the application storage. You should usually use it if your code is executed on a server, i.e. you mainly use PHP, Ruby, Python, Java, and any other server-side programming language.

#### Get all storage data

Retrieves all stored data for the given store ID. Public application config can be found in the `public` key of your application storage.

#### Request

> Request example

```http
GET /api/v3/1003/storage?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/storage?token={token}`

Name | Type | Description
---- | ---- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token


#### Response

> Response example (JSON)

```json
[
  {
    "key":"birth_date",
    "value":"18.11.1987"
  },
  {
    "key":"sex",
    "value":"male"
  }
]
```

A JSON array of the objects in the following format

#### StorageElement
Field | Type | Description
----- | ---- | -----------
key | string | The key you set for the stored data
value | string | The stored data

#### HTTP codes

HTTP Status | Meaning
------------|--------
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the data because of an error on the server

#### Get storage data by key

Retrieves the stored data for the given store ID by the given key. Public application config can be found in the `public` key of your application storage.

#### Request

> Request example

```http
GET /api/v3/1003/storage/birth_date?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

> Get public app config example

```http
GET /api/v3/1003/storage/public?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) of the stored data
**token** |  string | oAuth token


#### Response

> Response example (JSON)

```json
{
  "key":"birth_date",
  "value":"18.11.1987"
}
```

A JSON object of the following format

#### StorageElement
Field | Type | Description
----- | ---- | -----------
key | string | The key you set for the stored data
value | string | The stored data


#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8

{
    "error": "OTHER",
    "errorMessage": "Key not found"
}
```

In case of error, Ecwid responds with an error HTTP status code and JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
404 | The key is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the data because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
error | string | Type of the error
errorMessage | string | Error message


#### Add data to storage

Use this method to put a new data into the storage. If the key you specify in the request already exists, the corresponding value in the storage will be replaced with the submitted one. You can also create app public config data by using `public` as storage key in your request.

#### Request

> Request example

```http
POST /api/v3/1003/storage/my_key?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

Value of the new stored parameter
```

`POST https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) for the stored data (Limit: 1024 symbols)
**token** |  string | oAuth token


A key for the new key-value pair will be taken from the request URL. The data (value) is the request body (**1MB** max)


#### Response

> Response example (JSON)

```json
{
  "updateCount": 1
}
```

A JSON object with the status of operation

#### Status
Field | Type | Description
----- | ---- | -----------
success | boolean | `true` if the data has been added to the storage, `false` otherwise

#### HTTP codes

HTTP Status | Meaning
------------|--------
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the data because of an error on the server


#### Edit storage data

Use this method to update data in the storage. If the key you specify in the request doesn't yet exist, the corresponding value in the storage will be created. You can also update public application config by accessing `public` key in your request.

#### Request

> Request example

```http
PUT /api/v3/1003/storage/my_key?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

Updated value of the new stored parameter
```

`PUT https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) for the stored data (Limit: 1024 symbols)
**token** |  string | oAuth token


A key for the new key-value pair will be taken from the request URL. The data (value) is the request body (**1MB** max)


#### Response

> Response example (JSON)

```json
{
  "updateCount": 1
}
```

A JSON object with the status of operation

#### Status
Field | Type | Description
----- | ---- | -----------
success | boolean | `true` if the data has been changed in the storage, `false` otherwise

#### HTTP codes

HTTP Status | Meaning
------------|--------
404 | The key is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the data because of an error on the server


#### Delete storage data

Use this method to delete data in the storage by key. 

#### Request

> Request example

```http
DELETE /api/v3/1003/storage/my_key?token=487487437834aasdfd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/storage/{key}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**key** |  string | Key (name) of the stored data
**token** |  string | oAuth token


#### Response

> Response example (JSON)

```json
{
  "deleteCount": 1
}
```

A JSON object with the status of operation

#### Status
Field | Type | Description
----- | ---- | -----------
success | boolean | `true` if the data has been removed from the storage, `false` otherwise. 

#### HTTP codes

HTTP Status | Meaning
------------|--------
404 | The key is not found
500 | Cannot retrieve the data because of an error on the server

## Public application config

Application Storage allows to save public app config and easily access it in a storefront. 

You can use this feature if your app changes the look, appearance or logic of storefronts – [Customize Storefront](#customize-storefront) feature.

Public application config represents value of the `public` key in application storage and it is **always of a string type**. Application storage can be accessed from both Ecwid Javascript SDK and REST API. 

This provides equal access to storage for native and external applications, which operate outside of the Ecwid Control Panel. To get the public config in storefront, use a simple Ecwid Javascript API function.

See sections below for more details on how to work with app public config.

**Use case example**

Let’s say that you plan to build an app, that customizes a storefront, by displaying custom text created by store owner on a cart page.

> Get public application config data in storefront example

```js
Ecwid.getAppPublicConfig('my-cool-app')
// "enabled"
```
> See also: [App utilizing the app public config source code example](https://github.com/Ecwid/custom-thank-you-page-app)

Or you create a fully customizable widget, where merchant can input custom text, change its size, color, background and do other customizations. 

To do all of the above, your app will need to request all these fields from a merchant first and then apply them in storefront pages. 

User preferences or unique data can be carried over to storefront using public app config. To do that, save necessary merchant settings in public configuration of your app and access them in storefront using Ecwid JS API.

### Save public data

> Client-side native app example:

```js
var categoryId = '12345';

EcwidApp.setAppPublicConfig(categoryId, function(){
  console.log('Public config saved!');
});
```

> Server-side or external app example: 

```http
POST /api/v3/1003/storage/public?token=4884asd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

'12345'
```

There are two ways to save user’s data to app public config: 

1. For native client-side applications, created in Javascript, use `setAppPublicConfig()` function of the Ecwid Javascript SDK

2. For external or server-side applications, use **REST API** for saving user data to `public` key of application storage 

<aside class="note">
Check out examples on how to save and get multiple fields in app public config: <a href="https://developers.ecwid.com/api-documentation/public-application-config#examples">https://developers.ecwid.com/api-documentation/public-application-config#examples</a>
</aside>

**Important:**

- App public config can accept any string with the size less than 64Kb. Please make sure to store the required data only.

- App public config can be accessed by any 3rd party in storefront, if they know your appId. 

If any other 3rd party obtains your appId, they will have access to public user data of your application in storefront. Please make sure not to store any user sensitive data in app public config.

Check out example on how to save data to app public config on the right. To find out more details on how to access app storage via REST API, please see [this page](#rest-storage-api). 

### Get public data

> Initialize app on a specific category page in storefront

```js
Ecwid.OnPageLoaded.add(function(page){
  // Get public config in storefront
  // 'my-cool-app' is your appId value
  var category_id = Ecwid.getAppPublicConfig('my-cool-app'); 

  category_id = parseInt(category_id);

  if (page.categoryId == category_id) {
    // initialize your app
  } else {
    return;
  }
})
```

> Get app public config in your native app

```js
// variable to store our public config
var publicConfig;

// get app public config in native app
EcwidApp.getAppStorage('public', function(value){
  console.log(value);
  // '{ "color" : "red", "page_id" : "123456" }'

  publicConfig = value;
})

// parse the string to a JS object
publicConfig = JSON.parse(publicConfig);

console.log(publicConfig.color);
// 'red'
```

If your app customizes Ecwid storefront, you can get public config of your app using Ecwid JS API. 

In your Javascript file, which is executed when a storefront is loaded, you should get config using this API call: `Ecwid.getAppPublicConfig('appId')`. It will return a string value that you saved previously.

To get the value, specify your `appId` (app `client_id`) as its parameter and store the result of this function in a variable. If you are not sure on what your appId is, please [contact us](http://developers.ecwid.com/contact).

App public config is available to your app as soon as storefront starts to load. 

<aside class="note">
Check out examples on how to save and get multiple fields in app public config: <a href="https://developers.ecwid.com/api-documentation/public-application-config#examples">https://developers.ecwid.com/api-documentation/public-application-config#examples</a>
</aside>

### Access single or multiple values

#### Access a single public value

> Check whether widget needs to be shown

```js
// Save data to app public config in native app (default authentication)

var widget_enabled = 'true';

EcwidApp.setAppPublicConfig(widget_enabled, function(){
  console.log('Public app config saved!');
});

// Get data from app public config in storefront 

Ecwid.OnPageLoaded.add(function(page){
  var widget_enabled = Ecwid.getAppPublicConfig('ecwid-example-app');

  if (widget_enabled == 'true') {
    // initialize your app
  } else {
    return;
  }
})
```

Using `EcwidApp.setAppPublicConfig` you can save a simple string to use in storefront. 

For example, you can store the status of your widget (enabled / disabled) based on user preferences in [native applications](#embedded-apps) and then access it in storefront using `Ecwid.getAppPublicConfig`.

#### Access multiple public user data

> Add custom text to category page and change its color

```js
//
// Save multiple public user data in native client-side app using JavaScript SDK
//

var data = '{ "color" : "red", "page_id" : "123456", "text" : "Get 10% off on checkout with this code: ABCDEFG" }';

EcwidApp.setAppPublicConfig(data, function(){
  console.log('Public app config saved!');
});
```

```js
//
// Get multiple public user data in storefront and display user text on specific category page using JavaScript API
// 

Ecwid.OnPageLoaded.add(function(page){
  var data = Ecwid.getAppPublicConfig('my-cool-app');
  data = JSON.parse(data);

  var color = data.color;
  var page_id = data.page_id;
  var text = data.text;
  var added;

  page_id = parseInt(page_id);

  if (page.categoryId == page_id && added !== true) {
    div = document.getElementsByClassName("ecwid-productBrowser")[0];
    div.innerHTML = text + div.innerHTML;
    
    // customize text style using color 
    // ...

    added = true;
  }
  else {
    return;
  }
})
```

Sometimes applications require more user information in storefront and it's possible to access it as well as a simple value.

To save more than one value and utilize it as a key : value storage within app public config, you can save your data in a JSON format in a single string using `EcwidApp.setAppPublicConfig`.

After that, you can access this data in storefront using `Ecwid.getAppPublicConfig` and then parse it via standard Javascript function JSON.parse, which will present your data in a JavaScript object. Check out example on the right to find out how it works.

<aside class='notice'>App public config can accept any string with the size <strong>less than 64Kb</strong>. Please make sure to store the required data only.</aside>
