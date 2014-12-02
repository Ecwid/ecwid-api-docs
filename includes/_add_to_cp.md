# Embedding in Ecwid Control panel

## Overview

Ecwid API allows you to embed your application right into user Control panel and work like it is built into Ecwid. Although this is not necessary and you can use Ecwid API without embedding an application to Control panel, we highly recommend using this feature. Being integrated with Ecwid this way, your app will get a way more user engagement as it will be a part of a merchant store backend.

At high level, it works this way:
- When registering an app with us, you specify an URL of an iframe application page on your server
- When installing the app, user authorizes your app and allows it to appear in their Control panel
- As soon as the access is granted, an additional tab with your application will appear in the user Control panel
- On this new tab, the user will interact with your application, which will appear in an iframe. 

Below, you'll find more details on how to build an app.

## Building an app

### Application settings
To make your application displayed in Ecwid Control panel, you will need to provide us with some additional details about your application.

---- | -----------
iframe URL | This is a URL of the application page hosted on your server, which will be loaded in Ecwid Control panel. Requirements: <ul><li>It must load over HTTPS</li> <li>The page must not contain header/footer, i.e. you will need to design this page as an embeddable, not as a standalone application.</li></ul>
App page title | This will be the title of the tab in Ecwid control panel where your application resides. Please keep it short as it will reside in a row of native Ecwid tabs and other applications 
Control panel section | The section of Ecwid control panel where you want your application to be added. Supported sections: <ul><li>*Sales* – choose this if your application works with orders or customers</li> <li>*Products* – choose this if your application works with products, combinations, product images etc. </li> <li>*Promotions* – this section is for the application working with discounts, coupons, loyalty programs and other promotion features</li></ul>

You'll be asked for those details on the app registration page. If you already have a registered app and want to make it embeddable, you can contact us to adjust your app settings.

### Authorization / installation**
The authorization flow is described in details here: [Authentication](#Authentication) . It works the same way for the applications that you want to embed in Ecwid Control panel. The only difference is the access scope you ask permission for. If you want user to allow your app appear in their Control panel, include the `add_to_cp` scope to the list of requested scopes. See also: [Access Scopes](#Access_scopes)

**Building the application**
Ecwid provides 



Starter template / skeleton 




When a user authorizes 
When an application is embedded into user Control panel, it will appear in a separate tab in one of the following sections:
- Sales
- Products
- Promotions


https://my.ecwid.com/ecwid-app.js -- SDK
https://my.ecwid.com/styles/ecwid-app-ui.css
https://d3so0jchpginli.cloudfront.net/gz/16.7-290-gf7d9ee9/ecwid-app-ui.css

Depending on the kind of application you integrate with Ecwid, you may want to customize user storefront in some way. For example:

* apply custom styles to the store elements (buttons, fonts, pictures etc.)
* add extra widgets, e.g. customer reviews, comments, image magnifier
* modify Ecwid widgets look and behavior
* add tracking pixels or any other third party conversion-tracking scripts at checkout page

Ecwid API allows you to attach external JavaScript and CSS scripts and load them automatically in a user storefront. Basically, it works as follows:

1. When [registering of your app in Ecwid](#register-your-app-in-ecwid), you specify URL of the .css or .js file (or both) you'd like to be loaded in user storefront. 
2. When asking user to authorize your application, you add `customize_storefront` scope in the request to let user know your application is going to change their storefront. 
3. If the user grants your application with access to their store, the next time their storefront is loaded in any browser, the specified external JS/CSS files will be automatically loaded and executed on the page. 

<aside class="notice">
Permission required: `customize_storefront` (see [Access scopes](#access-scopes))
</aside>

Below, you will find more information on how to create custom JS/CSS.

## Custom JavaScript

> Example of custom JavaScript to modify storefront

```js

```

As soon as your script is loaded on the page with Ecwid storefront, you can access the page DOM and do pretty much everything you want by means of native JavaScript or whatever framework you want. If you use a JS framework, please make sure it's already loaded on the page by the moment you start using it. 

In addition, Ecwid provides a [JavaScript API](http://kb.ecwid.com/w/page/41188517/JavaScript%20API) that you can use to retrieve store information and track Ecwid events on the page. For example:

* `Ecwid.getOwnerId()` returns the store ID. You may want to use it to identify which store your script is loaded in now
* `Ecwid.OnPageLoad()` and `Ecwid.OnPageLoaded()` help you to track store page switch and identify which page is opened
* `Ecwid.Cart` object and its methods allow to manage the customer cart
