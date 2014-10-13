# Customize storefront

## Load external JS/CSS files

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
/*
 * Show a promo 'Free shipping' message on the cart page. 
 * In the example below, users will see a message on the cart page 
 * informing them that they will qualify for free shipping if their 
 * order is more than $99
 */
var promoMessage = "Orders $99 and up ship free!";
var minSubtotal = 99;

// Calculating subtotal and displaying the message
var checkSubtotal = function(order) {
  if (order) {
    var subtotal = order.total - order.shipping;
    if (subtotal < minSubtotal) {
      alert(promoMessage);
    }  
  }
}

// Detecting whether we're on the cart page and get the cart info
Ecwid.OnPageLoaded.add(function(page) {
  if ('CART' == page.type) {
    Ecwid.Cart.calculateTotal(function(order) {
      checkSubtotal(order);
    });
  }
});
```

As soon as your script is loaded on the page with Ecwid storefront, you can access the page DOM and do pretty much everything you want by means of native JavaScript or whatever framework you want. If you use a JS framework, please make sure it's already loaded on the page by the moment you start using it. 

In addition, Ecwid provides a [JavaScript API](http://kb.ecwid.com/w/page/41188517/JavaScript%20API) that you can use to retrieve store information and track Ecwid events on the page. For example:

* `Ecwid.getOwnerId()` returns the store ID. You may want to use it to identify which store your script is loaded in now
* `Ecwid.OnPageLoad()` and `Ecwid.OnPageLoaded()` help you to track store page switch and identify which page is opened
* `Ecwid.Cart` object and its methods allow to manage the customer cart

More details: [Ecwid JavaScript API](http://kb.ecwid.com/w/page/41188517/JavaScript%20API)

## Custom CSS

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