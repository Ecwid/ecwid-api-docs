# Application data

# Overview

Applications work with Ecwid stores and each store is unique in its own way. So each of those stores can have different settings and data for your application. Using [Application Storage](#application-storage) you can save and get data for a specific store for your application's needs.

The methods vary depending on the type of your application, so check out the following details on how to do that in your application.

# Accessing data in embedded apps

Embedded applications are adding a new tab in Ecwid control panel of a store, to provide quick settings for an application itself as well as containing application functionality in that tab. To find out more about such applications and how they work, see [Embedded apps](#embedded-apps) section.

To use the following methods you just need to be sure that your application is set as client-side. By default, all new applications are registered as such, so in case if you didn't explicitly asked to change your app to server-side, it means that your app is working as client-side application.

## Save storage data 

> Save multiple 'key' : 'value' data to app storage

```js
var data = { 
  color : 'red',
  size : 'big',
  page_id : '123456'
};

EcwidApp.setAppStorage(data, callback);
```

To save data to application storage, use `EcwidApp.setAppStorage` function of Ecwid Javascript SDK. It allows you to pass an object of elements that you wish to save in the storage and then use later on for that specific store, no server-side functionality needed. Check the example on the right how to use that function in your app.

See `EcwidApp.setAppStorage` function details in Ecwid [Javascript SDK](#setappstorage) documentation.

> Save a string to 'public' key in application storage

```js
var data = '{ "color" : "red", "page_id" : "123456" }';

EcwidApp.setAppPublicConfig(data, callback);
```

In some cases you may want to save this data to storage and make it accessible in storefront area for your application's scripts. This is also possible, using another function, called `EcwidApp.setAppPublicConfig`. 

In the first parameter of that function you will need to pass a string with the data you wish to be accessble in a storefront. That string can be a simple value like `blue` if you wish to save a color, but if you need several keys and values passed there, you can store a JSON string of this information and then parse that string in a storefront, to get access to that data. See [Get public data](#get-public-data) in storefront apps for more details.

## Get storage data

> Get all data from application storage

```js
EcwidApp.getAppStorage(function(allKeys) {
  console.log(allKeys);
});
```

To get all data from an application storage, use `EcwidApp.getAppStorage` with a callback function to get the results of this function. It will return an array of objects, containing all keys and their values in your app storage, so you can easily access all the data you saved previously without storing the data on your server.

> Get data from application storage by key

```js
EcwidApp.getAppStorage('color', function(value){
  //prints 'red' 
  console.log(value);
})
```

There are two ways to get data from your application storage: get all keys and their values at once, or get value of a specific key. To use any of these methods, you will need to use `EcwidApp.getAppStorage` function of Ecwid Javascript SDK

If you need to know the value of a specific key in your application storage, pass the key name as a first parameter of `EcwidApp.getAppStorage` function and you will receive the value for that key in a callback function. 

# Accessing data in storefront apps

Using Ecwid Javascript API, applcations can change storefront of Ecwid stores and add completely new functionality to it, using the power of Javascript. See [Customize Storefront](#customize-storefront) section for more details on how it works. 

## Save public data

There are two ways on how you can save data to application storage: using `setAppPublicConfig` in Ecwid [Javascript SDK](#setapppublicconfig) (for embedded client-side applications) or through [Storage endpoint](#application-storage) of Ecwid API. The one you choose depends on how exactly your application operates, so feel free to explore all the options and find out which one suits you more.

## Get public data

> Example values from app storage

```
  {
    "key": "public",
    "value": "{ \"color\" : \"black\", \"pageId\" : 12345 }"
  }
```

> Example of usage

```js
Ecwid.OnAPILoaded.add(function(page){
  var data = Ecwid.getAppPublicConfig(appId);
  data = JSON.parse(data);

  // prints 'black'
  console.log(data.color);
})
```

Applications that change storefront can be as simple as adding some custom Javascript code to a page to the ones that involve server side actions, like creating a discount coupon via [Coupons endpoint](#discount-coupons) and then passing that information to the script in the storefornt.

In case if your aplication is partly client-side, for example, it stores user data in [Storage endpoint](#application-storage) and you get that information in your storefront via Javascript code, then you can access that information using `EcwidApp.getAppPublicConfig` function of Ecwid Javascript API.

That function allows you to get the value of `public` key in storage, so, for example, you can either store a simple value like `blue` or `enabled` and get that information in storefront area. Alternatively, if you need access to more than one value, you can store a JSON string as a value of `public` key in your app storage and then parse it using JSON.parse function to get specific keys and values of that string in user's storefront. See example code on how to get public data for your application from storage on the right.

# Ecwid REST API

To access and modify data in application storage using Ecwid API, see [Application storage](#application-storage) endpoint for more details.

