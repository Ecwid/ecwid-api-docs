# Customize storefront

# Overview

Depending on the kind of application you integrate with Ecwid, you may want to customize the user storefront in some way. For example:

- apply custom styles to the store elements (buttons, fonts, pictures, etc.)
- add extra widgets, e.g. customer reviews, comments, image magnifier
- modify the look and behavior of Ecwid widgets
- add tracking pixels or any other third-party conversion-tracking scripts to the checkout page

The Ecwid API allows you to attach external JavaScript and CSS scripts, and load them automatically in a user storefront. 

> Check out guideline below for general guides

> [https://developers.ecwid.com/customizing-storefront](https://developers.ecwid.com/customizing-storefront)

**How It Works and How to Set It Up**

1. After your app is registered, [contact us](/contact) and provide the https URL of the `.css` and/or `.js` file you’d like to load in the user storefront.
2. When asking a user to install your app, Ecwid will request the `customize_storefront` permission from them. (*If your app is for a specific store, make sure to add `customize_storefront` scope in the OAuth process.*)
3. The next time the user storefront is loaded in any browser, the specified external JS/CSS files will be automatically loaded and executed on the page.

<aside class="notice">
Permission required: <strong>customize_storefront</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

Below, you will find more information on how to create custom JS/CSS. See the [Customizing Storefront guideline](/customizing-storefront) for more details about the app review process.

# Custom JavaScript

> Example of custom JavaScript to modify storefront

```js
/*
 * Show a promo 'Free shipping' message on the cart page. 
 * In the example below, users will see a message on the cart page 
 * informing them that they will qualify for free shipping if their 
 * order is more than $99
 */

var promoMessage = "Orders $99 and up ship free!";
var minSubtotal = 99;
var widgets;

// Calculating subtotal and displaying the message
var checkSubtotal = function(order) {
  if (order) {
    var subtotal = order.total - order.shipping;
    if (subtotal > minSubtotal) {
      alert(promoMessage);
    }  
  }
}

// Detecting whether we're on the cart page and get the cart info
Ecwid.OnPageLoaded.add(function(page) {
  widgets = Ecwid.getInitializedWidgets();

  // if storefront widget is present on this page
  for (i = 0; i < widgets.length ; i++) {
    if (widgets[i] == "ProductBrowser") {
      if ('CART' == page.type) {
        Ecwid.Cart.calculateTotal(function(order) {
          checkSubtotal(order);
        });
      }
    }
  }
});

// Get color value for the message and store it in color variable
var color = Ecwid.getAppPublicConfig(appId);

// your code here
// ...
```

As soon as your script is loaded on the page with Ecwid storefront, you can access the page DOM and do pretty much everything you want by means of native JavaScript or whatever framework you want. If you use a JS framework, please make sure it's already loaded on the page by the moment you start using it. 

In addition, Ecwid provides a [JavaScript API](#storefront-js-api) that you can use to retrieve store information and track Ecwid events on the page. For example:

* `Ecwid.getOwnerId()` returns the store ID. You may want to use it to identify which store your script is loaded in now
* `Ecwid.OnPageLoad()` and `Ecwid.OnPageLoaded()` help you to track store page switch and identify which page is opened
* `Ecwid.Cart` object and its methods allow to manage the customer cart
* `EcwidApp.getAppPublicConfig` will return store-specific data from application storage

More details: [Ecwid JavaScript API](#storefront-js-api)

### Q: How to know what Ecwid widgets are on a page?

If your applcation customizes storefront, your script will be loaded at **all times** when `app.ecwid.com/script.js?{store_id}` is on a page. It means that if a simple search widget is present on a page, your script will be executed. 

Ecwid widgets can be also embedded in many ways: 

- as a part of a website page
- in an iframe
- certain widgets are displayed only (like search and minicart, no storefront) 

So, it is important to know where exactly your application is loaded: if there is a storefront present on a page or if it's just a search widget. To check that, use `Ecwid.getInitializedWidgets()` function of [Ecwid Javascript API](#storefront-js-api). Using it, you can determine whether your app functionality needs to be initialized or not.

### Q: What is the best way to add new scripts from my code? 

> Add external script to the page example

```js
var s = document.createElement("script");
s.type = "text/javascript";
s.src = "https://example.com/example.js";
document.head.appendChild(s);
```

JavaScript is a very flexible programming language, that allows you to do the same thing in many different ways. For example, if you need to load additional external script on the page with your current code, you can do it in multiple ways too.

We recommend that you load external JS files using a standard function `createElement()`. See example code on the right. 
More details about this approach: [W3CSchools](http://www.w3schools.com/jsref/met_document_createelement.asp)

<aside class="note">Please make sure to avoid using the <em>document.write()</em> function as it works synchronously and will slow down the page.</aside>

## Store-specific custom JS

> Example of the script that dynamically loads store-specific code

```js
// Load external script with store
function loadConfig(ecwidStoreId, callback) {
  var script = document.createElement("script");
  script.setAttribute("src", '//example.com/myapp/store' + ecwidStoreId + '.js');
  script.charset = "utf-8";
  script.setAttribute("type", "text/javascript");
  script.onreadystatechange = script.onload = callback;
  document.body.appendChild(script);
}

// Application functionality
function doCoolStuff() {
  //...
}

// Get color value for the message and store it in color variable
var color = Ecwid.getAppPublicConfig(appId);

// Get Ecwid store ID and start working
loadConfig(Ecwid.getOwnerId(), doCoolStuff);
```

In most cases, your application behavior will vary depending on the store it is opened in. Although Ecwid API allows you to specify only one JS file per application, your application can detect the current Ecwid store ID and act correspondingly. Use the `Ecwid.getOwnerId()` method to detect the user store ID in your script. 

For example, let's say you need to dynamically add a store-specific configuration to your script when it's executed in some particular storefront. You can detect the store ID in your script and call a script on your server containing the store-specific code. 

To get information for that specific store you can use `Ecwid.getAppPublicConfig` function in [Ecwid Javascript API](#storefront-js-api). See more details on how to access that data in **Application data** section of documentation.

## Caching JavaScript file content

When your external JS file is loaded on a page, it is loaded like any other resource via a GET request by a web browser. If you'd like to use caching to increase the loading speeds of your script, we can suggest several options:

- invalidate cache for your JS file link when deploying a new version
- in your JS file, place a code that will get the latest JS assets file from your server dynamically and append it to a page (instead of the using an actual code of your app on that URL)
- don't use caching, so that your files for storefront are always up-to-date for any user

## Using jQuery on store pages

> Checking for jQuery on a page example

```js
// function to load jQuery

function loadjQuery(){
  var jq = document.createElement('script'); jq.type = 'text/javascript';
  jq.src = 'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js';
  document.getElementsByTagName('head')[0].appendChild(jq);
}

if (typeof jQuery == 'undefined') {  
  loadjQuery();
} else {}

// your app code
// ...
```

jQuery is a very popular JavaScript library that is used in many websites. The developers of themes for Wordpress CMS, for example, include this library right into the theme to have access to its functions and to operate various HTML elements on the page easily.

Ecwid storefronts can be customized in many ways, for example: you can set specific behaviour for each [page or each user](https://help.ecwid.com/customer/portal/articles/1085558) that visits those pages. Some developers prefer using native JavaScript functions to change the position or the display of certain elements. But many use different versions of popular jQuery library.

To ensure that your jQuery version code doesn't conflict with other applications that change Ecwid storefront, please see our guidelines: 

#### For simple modifications

When your modifications don't require specific versions of jQuery loaded on a page and are fairly simple, we suggest you follow this algorithm: 

First, check for jQuery object on current page. If no object is defined, load any jQuery version you prefer using the example code on the right. If there is already jQuery object available - skip the loading and use the curreny jQuery on this page.

#### For complex modifications

When you are using advanced jQuery's features such as jQuery UI, version specific functions, etc. it will be hard to work with, if a website already has varuous jQuery versions on a page. To avoid any version conflicts with current page jQuery version, we suggest you use jQuery noconflict feature. Check out how it works: [https://api.jquery.com/jquery.noconflict/](https://api.jquery.com/jquery.noconflict/)

<aside class='note'>
We recommend loading your jQuery from <a href='https://developers.google.com/speed/libraries/#jquery'>Google's CDN</a> to ensure it is available at all times.
</aside>

## Centering popups in iframe storefronts

> Example of centering popups

```html
<script src='https://d1e443hvef5jf2.cloudfront.net/static/iframeintegration.js'></script>
<script type='text/javascript'>

window.addEventListener('load',  function(e) {
  setPopupCentering('#myframe');
});

</script>
```

Ecwid can be embedded to a website in many ways. Sometimes a storefront can be inserted in an iframe container due to the limitations of a platform. To make sure that all popup windows such as customer account login popup are displayed in the center of an iframe, use the example code on the right **in a main frame of your page**.

`setPopupCentering()` function accepts one argument, which is the ID of an iframe element, where Ecwid storefront is loaded. In order to work, `setPopupCentering()` function needs to have `iframeintegration.js` file loaded for that frame. 

# Custom CSS

> Example of custom CSS to modify storefront

```css
/*
 * Change Ecwid buttons style 
 */
button.gwt-Button, #wrapper button.gwt-Button { 
  border-radius: 13px; 
  -moz-border-radius: 13px; 
  -webkit-border-radius: 13px; 
  height: 28px; 
  background-color: #e4e4e4; 
  padding: 0 15px; 
}  
 
button.gwt-Button:active,
#wrapper button.gwt-Button:active { 
  background-color: #999999; 
}  
 
div.ecwid-productBrowser-details-rightPanel div.ecwid-productBrowser-backgroundedPanel {
  min-width:185px; 
  width:auto; 
}
```

Ecwid provides a merchant with a built-in CSS customization tool in their control panel in the 'Design' section: Ecwid automatically loads the CSS code entered by user in their storefront. This allows merchants to customize their store look and feel flexibly. See also ["How to change Ecwid design"](http://help.ecwid.com/customer/portal/articles/1083332-how-to-change-ecwid-design) article in our knowledge base.

Ecwid API allows you to do the same in more convenient way: you simply specify the URL of file with your custom CSS code and Ecwid automatically loads that code in the user storefront. So you don't need to put the CSS on user site manually or ask them to do that. 

## Store-specific custom CSS

You may want to apply different CSS codes depending on the store your application is loaded. For example, if your application provides new design themes for merchant storefront, you may need to give a merchant ability to choose the theme they want to enable and change the applied CSS code according to their choice. 

In such cases, you will need to use custom JS files to dynamically detect merchant store ID and load different styles depending on the user store ID. See [Custom JavaScript](#custom-javascript) for details.

# Customization Tools

There are several storefront customization tools available at your disposal: you can set a category or product page to open by default, change storefront colors, take advantage of SEO-friendly URLs and many more. Check them out in more details below.

## SEO-friendly URLs

By default, Ecwid URLs address store pages in a hash part of the URL (after the # sign). Example: `https://www.mysite.com/store/#!/My-Product/p/123/category=0`

Search engines nowadays index such pages well. However, the general SEO recommendation is to keep URLs as simple as possible and address the site pages in the path part of the url, i.e. in the part before '?' or '#' signs. 

Ecwid allows to change the store URLs accordingly, so that they look like this:
`https://www.mysite.com/store/My-Product-p123`

Follow the steps below to enable SEO-friendly URLs in an Ecwid store.

### How to enable SEO-friendly URLs

**Step 1. Configure your server: add URL rewrite rules.**

In the SEO friendly URL scheme, Ecwid places a page title right after the store page path in the URL, so that it looks like a subdirectory: `www.mysite.com/store/My-Product-p123` . 

On the other hand, this is just a page on your server, not a directory. So if the server will try to find something inside the store page, it will fail and return 404 errors. To make your server properly process these URLs, you will need to configure URL rewrite rules. 

The general rule is to map everything after slash on the store page to the store page itself, i.e.:
`www.mysite.com/store/anything` → `www.mysite.com/store`

Specific configuration depends on your server: 

- **In Apache**, you will need to enable mod_rewrite and specify rewrite urls in `.htaccess` file See [https://httpd.apache.org/docs/2.4/rewrite/remapping.html](https://httpd.apache.org/docs/2.4/rewrite/remapping.html)
- **In Nginx**, you will need to add rewrite instruction to the config file. See [https://www.nginx.com/blog/creating-nginx-rewrite-rules/](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

<aside class='notice'>
<strong>Do not redirect</strong> the store/anything/ to /store . The rewrite rules should only map one URL to another, without redirection. If you redirect the visitor to the /store page instead, the store pages will not be properly indexed by Google. 
</aside>


**Step 2. Adjust Ecwid integration code: enable the SEO URLs.**

> Adjusting Ecwid integration code to enable SEO-friendly URLs

```html
<script>
  window.ec = window.ec || {};
  window.ec.config = window.ec.config || {};
  window.ec.config.storefrontUrls = window.ec.config.storefrontUrls || {};
   
  window.ec.config.storefrontUrls.cleanUrls = true;
  window.ec.config.baseUrl = '/store';
</script>
```

On the site page where you add Ecwid store, you will need to add a few lines of javascript code before the Ecwid integration code. It will tell Ecwid to enable SEO friednly URLs on this page. See example code on the right.

Make sure to replace the "/store" dummy value in the last line with the actual store page path. Ecwid will use this as a base for the clean URLs. E.g. if you specify this URL as your base URL: `/subfolder/shop` and your site is on `https://www.mysite.com`

Ecwid will address store pages like this:
- `https://www.mysite.com/subfolder/shop/My-product-p123`
- `https://www.mysite.com/subfolder/shop/cart`
- `https://www.mysite.com/subfolder/shop/checkout`

etc. 


<aside class='notice'>
We recommend using <strong>relative URLs</strong> in the <em>baseUrl</em> parameter. But you can also specify an absolute baseUrl if needed, e.g. "https://www.mysite.com/subfolder/shop/cart". <br/><br/>In such case, make sure it is under the same domain a visitor browses the site. E.g. if it includes 'www', do include 'www' in the base URL; if it contains https, do use https in the base URL. If the base URL domain or schema differ from the actual ones, the visitor browser will block URL rewrites and the SEO URls will not work. 
</aside>

### FAQ

#### Q: Will the regular hash-bang URLs continue working if I enable the new SEO URLs? 

Yes, the old URLs will be supported, so if someone clicks the old link anywhere on the web, they will get to the corresponding store page. Upon opening the page, Ecwid will automatically replace the hash part so that the visitor will see the new URLs in the browser address bar. 

## Change colors and fonts

Change any color and main font for user's storefront

### Apply colors and font automatically

> Set storefront colors and font automatically

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.font = 'auto';
window.ec.config.chameleon.colors = 'auto';
</script>
```

Ecwid can automatically detect the main colors and fonts of a surrounding website and match the storefront automatically. To apply the colors and font automatically for a store, use the code on the right.

### Change colors

> Set storefront colors automatically

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.colors = 'auto';
</script>
```

> Set storefront colors manually

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.colors = {
  'color-background':'#D3D3D3',
  'color-foreground':'#4EA3F0',
  'color-link':'#FF0606',
  'color-button':'#4EA3F0',
  'color-price':'#FF0606'
}
</script>
```

> Set storefront colors and font example

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon = {
  colors: {
    'color-button':'#4EA3F0',
    'color-price':'#FF0606'
  },
  font: {
    'font-family': 'arial,sans-serif'
  }
}
</script>
```

Using the global Ecwid config object you can control the colors in user's storefront. To pre-define colors for a storefront, add JavaScript code on the right in the same frame, where Ecwid integration code is added.

`auto` value allows Ecwid to detect the website colors around Ecwid storefront to adapt its colors automatically. Use the fields below to set colors manually.

**Fields:**

Name | Type | Description
---- | ----- | -----------
color-background | string (HEX and RGBA color) | Background color for storefront and  small buttons ("Clear bag", "Apply", etc.)
color-foreground | string (HEX and RGBA color) | Color of all texts in storefront
color-link | string (HEX and RGBA color) | Color for links in storefront (breadcrumbs, "Sign In", "Favourites", etc.)
color-button | string (HEX and RGBA color) | Color for main buttons in storefront ("Add to bag", "Checkout", etc.) and small buttons on mouse hover ("Clear bag", "Apply", etc.)
color-price | string (HEX and RGBA color) | Color for product prices in storefront

### Change font

> Set storefront font automatically

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.font = 'auto';
</script>
```

> Set storefront font manually

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.font = {
  'font-family': 'arial,sans-serif'
}
</script>
```

> Set storefront colors and font example

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon = {
  colors: 'auto',
  font: {
      'font-family': 'arial,sans-serif'
    }
  }
</script>
```

Using the global Ecwid config object you can control the fonts of all texts displayed in user's storefront. To pre-define a font for a storefront, add JavaScript code on the right in the same fram, where Ecwid integration code is added.

`auto` value allows Ecwid to detect the website's main font to adapt its font automatically. You can also set a specific font-family for a storefront manually.

## Storefront JavaScript API

Any Ecwid storefront has a JavaScript API available at all times to help users create various customizations for their storefronts. 

Its features include: 

- detect currently opened page details
- set shipping and billing address for customer
- get customer's cart details
- manage customer's cart
and others.

To find our more about it, plesae see the [Storefront JavsScript API Documentation](#storefront-js-api).

# Using REST API in storefront

When working on a custom storefront functionality, applications can require getting up-to-date catalog information from Ecwid store.

> Get 3 products from Ecwid REST API using JavaScript with public token

```js
var xhttp = new XMLHttpRequest();
var storeId = 1003;
var token = 'public_qKDUqKkNXzcj9DejkMUqEkYLq2E6BXM9';

var requestURL = 'https://app.ecwid.com/api/v3/'+storeId+'/products?limit=3&token='+token;

xhttp.open("GET", requestURL, true);
xhttp.send();

xhttp.onreadystatechange = function() {
  if (xhttp.readyState == 4 && xhttp.status == 200) {
    var apiResponse = xhttp.responseText;
    console.log(apiResponse); // prints response in format of Search Products request in Ecwid API
  }
};
```

We suggest using [public access token](#access-tokens) to get information about: store details, enabled products, enabled categories, combinations of enabled products, visible product types and deleted items statistics on demand with Ecwid REST API. Check out example on how to do this on the right.

With public access token you can safely make requests to Ecwid REST API without creating a buffer in a form of a server-side code, which requested information for your client-side code. You can make an Ajax request to Ecwid API with your JavaScript code and have a completely serverless application.

# Generate cart with products

Ecwid JavaScript API allows you to add items to customer's cart automatically. This can be useful when you are using custom storefront to provide any type of button to add products to cart.

Using the following steps you will be able to create links to your storefront with products in cart for your customers. So all they have to do is just clikc that promo link to go to your store and have products added to cart automatically.

Let's check out how this can be achieved: 

**Step 1: Add external files to your website**

> Script and CSS to load on your storefront page

```html
<link rel="stylesheet" type="text/css" href="https://s3.amazonaws.com/ecwid-addons/apps/ecwid-cart-app/cartapp.css">
<script src="https://s3.amazonaws.com/ecwid-addons/apps/ecwid-cart-app/cart.js"></script>
```

Add this code to the source code of the page, where your Ecwid storefront is displayed. Add the script **after** or below Ecwid integration code.

**Step 2: Generate your cart**

> Generate items in cart example

```js
var cartItems = [{
    "id": 66821181, // ID of the product in Ecwid
    "quantity": 3, // Quantity of products to add
    "options": { // Specify product options
        "Color": "White",
        "Size":"11oz"
    }
}, {
    "id": 66722581,
    "quantity": 5
}];

var cart = {
     "gotoCheckout": true, // go to next checkout step right away (after 'Cart' page)
     "products": cartItems, // products to add to cart
     "profile": {
         "address": { // Shipping address details
             "name": "john smith",
             "companyName": "general motors",
             "street": "5th Ave",
             "city": "New York",
             "countryCode": "US",
             "postalCode": "10002",
             "stateOrProvinceCode": "NY",
             "phone": "+1 234 235 22 12"
         },
         "billingAddress": { // Billing address details
             "countryCode": "US",
             "stateOrProvinceCode": "AL",
         },
         "email": "test@test.com", // Customer email
         "orderComments": "Comments!" // Order comments
     }
 }

cart = JSON.stringify(cart);
cart = encodeURIComponent(cart);

console.log(cart);

// prints the resulting string you need to use in step 3
//
// %7B%22gotoCheckout%22%3Atrue%2C%22products%22%3A%5B%7B%22id%22%3A66821181%2C%22quantity%22%3A3%2C%22options%22%3A%7B%22Color%22%3A%22White%22%2C%22Size%22%3A%2211oz%22%7D%7D%2C%7B%22id%22%3A66722581%2C%22quantity%22%3A5%7D%5D%2C%22profile%22%3A%7B%22address%22%3A%7B%22name%22%3A%22john%20smith%22%2C%22companyName%22%3A%22general%20motors%22%2C%22street%22%3A%225th%20Ave%22%2C%22city%22%3A%22New%20York%22%2C%22countryCode%22%3A%22US%22%2C%22postalCode%22%3A%2210002%22%2C%22stateOrProvinceCode%22%3A%22NY%22%2C%22phone%22%3A%22%2B1%20234%20235%2022%2012%22%7D%2C%22billingAddress%22%3A%7B%22countryCode%22%3A%22US%22%2C%22stateOrProvinceCode%22%3A%22AL%22%7D%2C%22email%22%3A%22test%40test.com%22%2C%22orderComments%22%3A%22Comments!%22%7D%7D
//
```

In order for the script to add items to cart, we need to 'tell' it what items to add. Check the example on the right on how to do this. The `cart` variable will have the generated cart content in it. 

The syntax is very similar to adding products to cart via [Ecwid JavaScript API](#ecwid-cart-addproduct).

**Step 3: Create a link for customers**

> Final link structure

```
http://example.com/store#!/~/cart/create={generatedCartCode}
```

> Final link to generated cart example

```
https://www.ecwid.com/demo#!/~/cart/create=%7B%22gotoCheckout%22%3Atrue%2C%22products%22%3A%5B%7B%22id%22%3A66821181%2C%22quantity%22%3A3%2C%22options%22%3A%7B%22Color%22%3A%22White%22%2C%22Size%22%3A%2211oz%22%7D%7D%2C%7B%22id%22%3A66722581%2C%22quantity%22%3A5%7D%5D%2C%22profile%22%3A%7B%22address%22%3A%7B%22name%22%3A%22john%20smith%22%2C%22companyName%22%3A%22general%20motors%22%2C%22street%22%3A%225th%20Ave%22%2C%22city%22%3A%22New%20York%22%2C%22countryCode%22%3A%22US%22%2C%22postalCode%22%3A%2210002%22%2C%22stateOrProvinceCode%22%3A%22NY%22%2C%22phone%22%3A%22%2B1%20234%20235%2022%2012%22%7D%2C%22billingAddress%22%3A%7B%22countryCode%22%3A%22US%22%2C%22stateOrProvinceCode%22%3A%22AL%22%7D%2C%22email%22%3A%22test%40test.com%22%2C%22orderComments%22%3A%22Comments!%22%7D%7D
```

Now we need to fill in customer's cart when your custom link is opened. 

First things first, let's determine where your Ecwid store is displayed and get a direct link to that page. For example, our demo store is located in: `https://www.ecwid.com/demo`
And to fill in customer's cart with items that you seleted earlier, we need to create a link to your storefront with the generated cart part. 

**Example link**

Generated link with products added automatically to cart for Ecwid demo store will be: 

`https://www.ecwid.com/demo#!/~/cart/create=%7B%22gotoCheckout%22%3Atrue%2C%22products%22%3A%5B%7B%22id%22%3A66821181%2C%22quantity%22%3A3%2C%22options%22%3A%7B%22Color%22%3A%22White%22%2C%22Size%22%3A%2211oz%22%7D%7D%2C%7B%22id%22%3A66722581%2C%22quantity%22%3A5%7D%5D%2C%22profile%22%3A%7B%22address%22%3A%7B%22name%22%3A%22john%20smith%22%2C%22companyName%22%3A%22general%20motors%22%2C%22street%22%3A%225th%20Ave%22%2C%22city%22%3A%22New%20York%22%2C%22countryCode%22%3A%22US%22%2C%22postalCode%22%3A%2210002%22%2C%22stateOrProvinceCode%22%3A%22NY%22%2C%22phone%22%3A%22%2B1%20234%20235%2022%2012%22%7D%2C%22billingAddress%22%3A%7B%22countryCode%22%3A%22US%22%2C%22stateOrProvinceCode%22%3A%22AL%22%7D%2C%22email%22%3A%22test%40test.com%22%2C%22orderComments%22%3A%22Comments!%22%7D%7D`

<aside class='note'>
Please note that this is an example link and it will not work in Ecwid's demo store. Please test this feature in your own website.
</aside>

# Access users’ data in storefront

Applications can work and look differently in various stores, so you can provide tailored user experience to the store owner and a website they have in your application.

App public config allows apps to access user specific data in storefront as soon as it starts to load, so no calls to external servers are required and result is available right away. It’s recommended that you use app storage to access public user data in storefront area of your application.

**How it works:**

1. App saves public data in public storage in backend
2. App gets public data from public storage in storefront
3. App tailors user experience to merchant settings

For example, using this method you can pass store ID to a storefront, category or product ID where the changes need to apply or whether your widget needs to be shown in storefront. 

To find out more about this process, please check [Public App Config](#public-application-config) section.
