# Customize storefront

# Overview

The Ecwid API platform allows you to customize storefront in various ways: 

- apply custom styles to the store elements (buttons, fonts, pictures, etc.)
- add extra widgets, e.g. customer reviews, comments, image magnifier
- modify the look and behavior of Ecwid widgets
- add tracking pixels or any other third-party conversion-tracking scripts to the checkout page
- and more

Check out the sections below to find out what other features are available and how to use them.

# Add Ecwid to the site

## Add storefront to the site

> Add Ecwid storefront to your website

```html
<div id="my-store-1003"></div><div> <script type="text/javascript" src="https://app.ecwid.com/script.js?1003" charset="utf-8"></script><script type="text/javascript"> xProductBrowser("categoriesPerRow=3","views=grid(3,3) list(10) table(20)","categoryView=grid","searchView=list","id=my-store-1003");</script></div>
```

This is the most important Ecwid widget. It shows and includes a full-featured shopping cart with products, categories, catalog, checkout pages, etc.

To add an Ecwid store to the website, use the code example on the right. The `1003` is the ID of an Ecwid store. Make sure to specify your desired store ID in order to show your store!

## Set main storefront URL for widgets

When you add store widgets, like search, minicart, categories menu, to a site page separate from the storefront widget, the storefront will open in a pop-up. This pop-up contains the whole store and works just fine. But in some cases it is more convenient to open the store on another website page instead of the pop-up.
 
You can do this with the help of the `ecwid_ProductBrowserURL` option.

> 'Tell' Ecwid widgets that storefront is located at "http://www.example.com/store.html"

```html
<script>var ecwid_ProductBrowserURL = "http://www.example.com/store.html";</script>
```

**How Ecwid works by default**

- if a visitor uses the "minicart/vertical categories/search box" widget and a storefront is added on this page, all actions (go to cart, to category, etc.) will be made in that storefront widget
- but if a storefront widget doesn't exist on the same page page, **a pop-up window with the storefront will be created on this page** to complete the action

**How Ecwid works with this option**

When a merchant uses a storefront widget placed on another page of a website, you should use the `ecwid_ProductBrowserURL` option. If it's set up, Ecwid works in such way: 

- if a visitor uses the "minicart/vertical categories/search box" widget and the storefront widget isn't added on this page, **they will be redirected to the URL speicified in the code and then the necessary actions will be performed there**

For example, if a customer searches for a product on the page without a storefront, they will be redirected to a URL specified in `ecwid_ProductBrowserURL` variable and then Ecwid will show the search results on that page.

**How to set up this option**

In order to use it, you need to add the following JavaScript code to all pages, where Ecwid widgets are installed:

`<script>var ecwid_ProductBrowserURL = "PB_URL";</script>`

where `PB_URL` is the full URL of the page where your product browser widget (i.e. your Ecwid store) is installed.

For example: 

`<script>var ecwid_ProductBrowserURL = "http://www.example.com/store.html";</script>`

## Embed or remove storefront on demand

> Dynamic embedding of Ecwid storefront widget

```html
<div id="my-store-1003"></div>
<script>
window.ecwid_script_defer = true;
window.ecwid_dynamic_widgets = true;

   if (typeof Ecwid != 'undefined') Ecwid.destroy(); 
   window._xnext_initialization_scripts = [{
        widgetType: 'ProductBrowser',
        id: 'my-store-1003',
        arg: ["categoriesPerRow=3","views=grid(3,3) list(10) table(20)","categoryView=grid","searchView=list"]
      }];

  if (!document.getElementById('ecwid-script')) {
      var script = document.createElement('script');
      script.charset = 'utf-8';
      script.type = 'text/javascript';
      script.src = 'https://app.ecwid.com/script.js?1003';
      script.id = 'ecwid-script'
      document.body.appendChild(script);
    } else {
      ecwid_onBodyDone();
    }
</script>
```

In some cases it is necessary to dynamically create and destroy Ecwid widget within the HTML page. This is useful for dynamic sitebuilders that switch to online store page without actually reloading page (e.g. making it visible). 

<aside class="note">
This method of embedding Ecwid is slower than direct embedding of Ecwid widget. Please use it only when you need the storefront embedded on the fly. 
</aside>

You should use `window.ecwid_dynamic_widgets` variable to enable dynamic widget creating in Ecwid. See the example on the right that shows how to create and destroy Ecwid widget through the JavaScript functions. 

Please note that **this method allows to embed the storefront widget only**. If you need to embed other widgets dynamically, please, use the code for [deferred widget initialization](#delayed-widget-initialization)

## Delayed widget initialization 

> Delayed widget initialization

```html
<div id="my-store-1003"></div>
<div id="productBrowser"></div>
<script>
window.ecwid_script_defer = true;
var script = document.createElement('script');
script.charset = 'utf-8';
script.type = 'text/javascript';
script.src = 'https://app.ecwid.com/script.js?1003';
document.getElementById('my-store-1003').appendChild(script);
window._xnext_initialization_scripts = [
    { widgetType: 'ProductBrowser', id: 'productBrowser', arg: [
        '"categoriesPerRow=3","views=grid(4,4) list(10) table(20)","categoryView=grid","searchView=list","style=","responsive=yes","id=productBrowser"'
    ] }
];</script>
```

Sometimes it is necessary to delay widget initialization while the website page finishes the initialization procedures. This is useful when the website is built dynamically using libraries such as **ReactJs**.

See the example of delayed initialization of Ecwid widget on the right.

## Increase widget loading speed

> DNS Prefetching of Ecwid's resources

```html
<link rel="dns-prefetch" href="//images-cdn.ecwid.com/">
<link rel="dns-prefetch" href="//images.ecwid.com/">
<link rel="dns-prefetch" href="//app.ecwid.com/">
```

To increase the loading speed of an Ecwid store, you can start loading Ecwid resources even before a customer opens the store page by the means of so-called prefetching (prerendering) browser feature. Not all browsers support this at this moment though.

> Prefetching and prerendering of storefront page

```html
<link rel="prefetch" href="http://www.example.com/store/"/>
<link rel="prerender" href="http://www.example.com/store/"/>
```

You can also add your storefront page to preload and prerender in the background for further increase in loading speed for browsers that have this feature.

Feel free to learn more about these techniques in [https://css-tricks.com/prefetching-preloading-prebrowsing/](https://css-tricks.com/prefetching-preloading-prebrowsing/)

# Look and feel

## Change the store layout

Ecwid integration code provides a number of modifications to the store layout: number of products per row, default view mode and more. See the details below. 

### Change the number of products/categories on a page

>Add Ecwid storefront to your website

```html
<div id="my-store-1003"></div><div> <script type="text/javascript" src="https://app.ecwid.com/script.js?1003" charset="utf-8"></script><script type="text/javascript"> xProductBrowser("categoriesPerRow=3","views=grid(3,3) list(10) table(20)","categoryView=grid","searchView=list","id=my-store-1003");</script></div>
```

Find the following code in the widget code on the right:

`"categoriesPerRow=3","views=grid(3,3) list(10) table(20)","categoryView=grid","searchView=list"`

This code defines products and categories listing styles. Let's see its parts:

- `"categoriesPerRow=3"` -- defines number of categories per row
- `"views=grid(3,3) list(10) table(20)"` - defines the available views and their settings. If you want to remove any view, just remove the `grid(3,3)` or `list(10)` or `table(20)` text from the settings.

The number in braces is number of products in the particular view. I.e.:

- `grid(K,L)` - `K` is a number of products in a column, `L` - number of products in a row in grid view
- `list(M)` - `M` is a number of products on one page in list view
- `table(N)` - `N` is a number of products on one page in table view

Default view mode settings:

- `"categoryView=grid"` - defines the default view for products in categories. Possible values: `list`, `grid`, `table`
- `"searchView=list"` - defines the default view for search results. Possible values: `list`, `grid`, `table`.

Feel free to change the code to achieve the layout you need!

### Set default product or category page

> Open "Surfboards" category of Ecwid Demo store by default

```html
<div> <script type='text/javascript' src='https://app.ecwid.com/script.js?1003'></script> <script type='text/javascript'> xProductBrowser("categoriesPerRow=3","views=grid(3,3) list(10) table(20)","categoryView=grid","searchView=list","style=","defaultCategoryId=20671017"); </script> </div>
```

> Open "PYZEL Amigo 6'2 Surfboard" product in Ecwid Demo store by default

```html
<div> <script type='text/javascript' src='https://app.ecwid.com/script.js?1003'></script> <script type='text/javascript'> xProductBrowser("categoriesPerRow=3","views=grid(3,3) list(10) table(20)","categoryView=grid","searchView=list","style=","defaultProductId=70178249"); </script> </div>
```

By default, the storefront widget opens with a list of root category. But you can configure it to show a different category or a particular product when user opens it for the first time.

You can do it by adding either option to the Product Browser integration code:

`"defaultCategoryId=%categoryId%"`

or

`"defaultProductId=%productId%"`

The example codes are available on the right.

## Change storefront labels

> Format example

```html
<script type="text/javascript">
  ecwidMessages = { 
    "LABEL_NAME":"LABEL_VALUE", 
    "LABEL_NAME-2":"LABEL_VALUE-2", 
    "OTHER_LABEL_NAME":"OTHER_LABEL_VALUE" 
  }; 
</script>
```

Every text in Ecwid (that is not added by a merchant) is a label. You can change these labels yourself to translate your storefront or to make the wording more fitting a merchant's individual Ecwid setup.

This translation includes adding of a special JavaScript code with the updated labels **before** the Ecwid integration code. 

> Replace "Shopping Bag" with "Shopping Cart"

```html
<script type="text/javascript">
  ecwidMessages = {
    "BreadCrumbs.your_bag" : "Your shopping cart",
    "Minicart.shopping_bag" : "Shopping Cart",
    "ShoppingCartView.shopping_bag" : "Your Shopping Cart",
    "EmptyShoppingCartPanel.empty" : "Your Shopping Cart is empty"
  };
</script>
```

The format of this code is presented on the right. `LABEL_NAME` is the internal name of the text label. `LABEL_VALUE` is the text that is shown in the storefront. You'll find the label names and the default values in the [attached document](https://support.ecwid.com/hc/en-us/article_attachments/115000049609/Ecwid_storefront_labels.html).

For example: 
to replace the "Shopping Bag" text with "Shopping Cart" in the bag widget and "your bag" page, we need to add the following code before this line: 
`<script type='text/javascript' src='http://app.ecwid.com/script.js?store_id'></script>` 

on your web page. See the example one the right!

**Important notes**: 

- each line must be ended with comma except the last line
- do not leave empty lines in the Javascript code
- the special chars should be escaped. For example if the value of a label has double quote(") it should be escaped with the slash(\")
- some buttons are actually images, so you cannot translate them using JavaScript. Use CSS code to replace the images
- keep HTML tags in the label values, they're necessary for correct Ecwid work
- mind the spaces after the label values. Some labels have it.  So make sure that they won't be loaded during the translation.
- UTF-8 charset is strongly recommended
- if you don't like to insert many JavaScript lines to your store page, you can move them to the external JavaScript file: www.ecwid.com/forums/showthread.php?p=2337#15

If you're not familiar with JavaScript, but want to translate a store or change its labels, you can use [Storefront Label Editor](https://www.ecwid.com/apps/tools/storefront-label-editor) or the [Ecwid Translate Tool](http://www.ecwid.com/playground/translate-tool/). It will make the translation much easier.

For more details on this functionality, please see [this article](https://support.ecwid.com/hc/en-us/articles/207808835-Custom-Translations) in our help center.

## Apply custom CSS

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

**Add custom CSS in Ecwid Control Panel (for a single store)**

Ecwid provides a merchant with a built-in CSS customization tool in their control panel in the 'Design' section: Ecwid automatically loads the CSS code entered by user in their storefront. This allows merchants to customize their store look and feel flexibly. See also ["How to change Ecwid design"](http://help.ecwid.com/customer/portal/articles/1083332-how-to-change-ecwid-design) article in our knowledge base.

**Add custom CSS in an app (for multiple stores at once)**

Ecwid API allows you to do the same in more convenient way: you simply specify the URL of file with your custom CSS code and Ecwid automatically loads that code in the user storefront. So you don't need to put the CSS on user site manually or ask a merchant to do that. 

In more details: 

1. After your app is registered, [contact us](/contact) and provide the https URL of the `.css` and/or `.js` file you’d like to load in the user storefront.
2. When asking a user to install your app, Ecwid will request the `customize_storefront` permission from them. (*If your app is for a specific store, make sure to add `customize_storefront` scope in the OAuth process.*)
3. The next time the user storefront is loaded in any browser or website, the specified external JS/CSS files will be automatically appended, loaded, and executed on that page.

<aside class="notice">
Permission required: <strong>customize_storefront</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

### Q: Can I create new themes as apps?

Yes, you can! Please check out this page for more details: [How to create a theme](/how-to-create-a-theme-for-an-ecwid-store)

### Store-specific custom CSS

You may want to apply different CSS codes depending on the store your application is loaded. For example, if your application provides new design themes for merchant storefront, you may need to give a merchant ability to choose the theme they want to enable and change the applied CSS code according to their choice. 

In such cases, you will need to use custom JS files to dynamically detect merchant store ID and load different styles depending on the user store ID. See [Custom JavaScript](#add-custom-javascript-code) for details.

## Change default colors and fonts

Change any color and main font for user's storefront

### Apply colors and fonts to storefront automatically from a website

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

### Set storefront colors

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

Using the global Ecwid config object `window.ec.config` you can control the colors in user's storefront. To pre-define colors for a storefront, add JavaScript code on the right in the same frame, where Ecwid integration code is added.

`auto` value allows Ecwid to detect the website colors around Ecwid storefront to adapt its colors automatically. Use the fields below to set colors manually.

**Fields:**

Name | Type | Description
---- | ----- | -----------
color-background | string (HEX and RGBA color) | Background color for storefront and  small buttons ("Clear bag", "Apply", etc.)
color-foreground | string (HEX and RGBA color) | Color of all texts in storefront
color-link | string (HEX and RGBA color) | Color for links in storefront (breadcrumbs, "Sign In", "Favourites", etc.)
color-button | string (HEX and RGBA color) | Color for main buttons in storefront ("Add to bag", "Checkout", etc.) and small buttons on mouse hover ("Clear bag", "Apply", etc.)
color-price | string (HEX and RGBA color) | Color for product prices in storefront

### Set storefront font

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

Using the global Ecwid config object `window.ec.config` you can control the fonts of all texts displayed in user's storefront. To pre-define a font for a storefront, add JavaScript code on the right in the same fram, where Ecwid integration code is added.

`auto` value allows Ecwid to detect the website's main font to adapt its font automatically. You can also set a specific font-family for a storefront manually.


# SEO 

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

#### Q: Can I get SEO-friendly URLs in Ecwid REST API?

Yes, when getting information about products and categories in an Ecwid store, you can choose the URL format you wish to receive in response. For more details, see the links below: 

- [How to get URLs for products](#q-how-to-get-urls-for-products)
- [How to get URLs for categories](#q-how-to-get-urls-for-categories)

# Add or modify features in storefront

## Add custom JavaScript code

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

### How to load custom JavaScript anytime storefront is loaded

Ecwid API allows you to do the same in more convenient way: you simply specify the URL of file with your custom JavaScript code and Ecwid automatically loads that code in the user storefront. So you don't need to put the JS on merchant site manually or ask them to do that. 

In more details: 

1. After your app is registered, [contact us](/contact) and provide the https URL of the `.css` and/or `.js` file you’d like to load in the user storefront.
2. When asking a user to install your app, Ecwid will request the `customize_storefront` permission from them. (*If your app is for a specific store, make sure to add `customize_storefront` scope in the OAuth process.*)
3. The next time the user storefront is loaded in any browser or website, the specified external JS/CSS files will be automatically appended, loaded, and executed on that page.

<aside class="notice">
Permission required: <strong>customize_storefront</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

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

### Store-specific custom JavaScript

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

## Caching JavaScript file contents

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

### For simple modifications

When your modifications don't require specific versions of jQuery loaded on a page and are fairly simple, we suggest you follow this algorithm: 

First, check for jQuery object on current page. If no object is defined, load any jQuery version you prefer using the example code on the right. If there is already jQuery object available - skip the loading and use the curreny jQuery on this page.

### For complex modifications

When you are using advanced jQuery's features such as jQuery UI, version specific functions, etc. it will be hard to work with, if a website already has varuous jQuery versions on a page. To avoid any version conflicts with current page jQuery version, we suggest you use jQuery noconflict feature. Check out how it works: [https://api.jquery.com/jquery.noconflict/](https://api.jquery.com/jquery.noconflict/)

<aside class='note'>
We recommend loading your jQuery from <a href='https://developers.google.com/speed/libraries/#jquery'>Google's CDN</a> to ensure it is available at all times.
</aside>

## Center popups in iframe storefronts

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


## Storefront JavaScript API

Any Ecwid storefront has a JavaScript API available at all times to help users create various customizations for their storefronts. 

Its features include: 

- detect currently opened page details
- set shipping and billing address for customer
- get customer's cart details
- manage customer's cart
and others.

To find our more about it, plesae see the [Storefront JavsScript API Documentation](#storefront-js-api).

## Using REST API in storefront

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

## Access merchant in-app preferences

Applications can work and look differently in various stores, so you can provide tailored user experience to the store owner and a website they have in your application.

App public config allows apps to access user specific data in storefront as soon as it starts to load, so no calls to external servers are required and result is available right away. It’s recommended that you use app storage to access public user data in storefront area of your application.

**How it works:**

1. App saves public data in public storage in backend
2. App gets public data from public storage in storefront
3. App tailors user experience to merchant settings

For example, using this method you can pass store ID to a storefront, category or product ID where the changes need to apply or whether your widget needs to be shown in storefront. 

To find out more about this process, please check [Public App Config](#public-application-config) section.
