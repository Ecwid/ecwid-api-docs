# Storefront JS API

> Check out [Customize Storefront](#customize-storefront) section for general details on changing Ecwid's storefront.

> Access Page object to find the current page type

```javascript
Ecwid.OnPageLoad.add(function(page) {
  alert("My page load handler: " + page.type);
});
```

> See also: [App working in storefront source code example](https://github.com/Ecwid/custom-thank-you-page-app)

Storefront JS API allows your app to integrate with storefront on a deeper level. It provides access to events (page changed, cart changed), entities (product, cart, order) and others. 

Get started with this list of available use cases: 

- [Get basic storefront details (store ID, public token, public app config)](https://developers.ecwid.com/api-documentation/get-storefront-details)
- [Subscribe to store events (page changes, cart changes, user login)](https://developers.ecwid.com/api-documentation/subscribe-to-events)
- [Get details of a logged in customer (email, name, billing address)](https://developers.ecwid.com/api-documentation/get-customer-details)
- [Manage customer's cart (add product, remove product, clear cart)](https://developers.ecwid.com/api-documentation/manage-customer-cart)
- [Get cart details (get items in cart, get selected payment/shipping method, calculate order total)](https://developers.ecwid.com/api-documentation/get-cart-details)
- [JS API Examples](https://developers.ecwid.com/api-documentation/js-api-examples)

<aside class="notice">
Ecwid Storefront JavaScript API is available on any Ecwid plan.
</aside>


## Get storefront details

Get basic storefront information to use in your script.

### Ecwid.getStaticBaseUrl

> Get Static Base Url code example

```javascript
var StaticBaseUrl = Ecwid.getStaticBaseUrl();

console.log(StaticBaseUrl);

// prints
// "https://d3fi9i0jj23cau.cloudfront.net/gz/"
```

Returns the base URL for static Ecwid files, like images and CSS, with the ’/’ at the end.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.getOwnerId

> Get store ID code example

```javascript
var storeId = Ecwid.getOwnerId()

console.log(storeId);

// prints
// 1003
```

Returns the store ID.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.getInitializedWidgets

> Get all widgets to be displayed when page loads

```js
var widgets = Ecwid.getInitializedWidgets();

console.log(widgets);

// prints 
// ["Minicart", "SearchPanel", "ProductBrowser"]
```

Returns array containing widget types present on a page. There are four types available: 

* `Minicart` - Minicart widget
* `SearchPanel` - Search widget
* `ProductBrowser` - Storefront widget
* `Categories` - Categories widget
* `SingleProduct` - Embedded product (old version)
* `Product` - Embedded product (latest version)

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.formatCurrency

> Format a number using currency format of a store

```javascript
var currencyFormat = Ecwid.formatCurrency(12.99);

console.log(currencyFormat);

// prints
// "$12.99"
```

Converts the given currency value to a human-readable string according to the store settings. It accepts both string and integer as arguments.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.getStorefrontLang

```javascript
var lang = Ecwid.getStorefrontLang();

console.log(lang);

//prints
// "en"
```

Get current language of storefront. The function is available as soon as Ecwid store starts to load.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.getAppPublicConfig

> Get app public config function usage

```js
Ecwid.getAppPublicConfig(appId);
```

> Example values from app publc config

```json
  {
    "key": "public",
    "value": "12345"
  }
```

> Get app public config code example

```js
  var pageId = Ecwid.getAppPublicConfig("my-cool-app");

  console.log(pageId);
  // prints 
  // '12345'
```

Returns value for `public` key from Ecwid Application Storage endpoint.

`Ecwid.getAppPublicConfig()` receives one argument: 

Name | Type | Description
---- | ---- | -----------
**appId** | String | Namespace of your application (as set in the application settings).

<aside class="notice">
Data in public storage of your app must not exceed <strong>64Kb</strong>
</aside>

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.getAppPublicToken 

> Get app public config function usage

```js
Ecwid.getAppPublicToken(appId);
```

> Get app public token code example

```js
var publicToken = Ecwid.getAppPublicToken('my-cool-app');

console.log(publicToken);

// 'public_qKDUqKkNXzcj9DejkMUqEkYLq2E6BXM9'

```

Returns public applicaiton token for an Ecwid store. In order for this function to work, your app has to ask for `public_storefront` scope from a store. [Learn more](https://developers.ecwid.com/api-documentation/access-tokens)

`Ecwid.getAppPublicToken()` receives one argument: 

Name | Type | Description
---- | ---- | -----------
**appId** | String | Namespace of your application (as set in the application settings).

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.getFeatureToggles

```js
var ecwidFeatureTogglesInfo = Ecwid.getFeatureToggles();

console.log(ecwidFeatureTogglesInfo);

// {
//  newProductList: true, 
//  newDetailsPage: true,
//  customerLoginByLink: true,
//  newCartPage: true,
//  newCheckoutPage: true
// }
```

`Ecwid.getFeatureToggles()` function returns enabled or disabled storefront features we released over the years and just recently. 

**Returned fields:**

Name | Type | Description
---- | ---- | -----------
newProductList | boolean | `true` if [new product listing](https://developers.ecwid.com/api-documentation/look-and-design#new-product-listing) is enabled in a store. `false` otherwise
newDetailsPage | boolean | `true` if new product details page is enabled in a store. `false` otherwise. [Learn more](https://www.ecwid.com/blog/ecwid-features-digest-spring-2018.html#newstorefront)
customerLoginByLink | boolean | `true` if customers log in to store by link. `false` otherwise. [Learn more](https://support.ecwid.com/hc/en-us/articles/115003429065-Managing-customers#customerlogin)
newCartPage | boolean | `true` if latest version of the cart page is enabled for store. Is always `true` when `newCheckoutPage` is also `true`. `false` otherwise. [Learn more](https://www.ecwid.com/blog/whats-new-in-your-ecwid-store-summer-18.html#cp)
newCheckoutPage | boolean | `true` if latest version of the checkout process is enabled in a store. `false` otherwise

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Get opened page info

> Get page type after new page is loaded 

```js
Ecwid.OnPageLoaded.add(function(page){
  console.log("Current page is of type: " + page.type);
});

// prints
// Current page is of type: CATEGORY

```

Get basic information about a page user opened in storefront. 

How to do it: 

1. Subscribe to `Ecwid.OnPageLoad` or `Ecwid.OnPageLoaded` event
2. Extract page information from `page` object with your code
3. When event happens, your callback code is executed

<aside class='notice'>If you need more information about a product or category, use <a href="https://developers.ecwid.com/api-documentation/access-tokens">Ecwid REST API</a>. Public access token can be used in the client-side code, and private token on the server-side.</aside>

**Page object fields**:

Name | Type | Description
---- | ----- | -----------
name | string | For types: `CATEGORY`, `PRODUCT`. Name of the opened category or product 
type | string, one of the following: `ACCOUNT_SETTINGS`, `ADDRESS_BOOK`, `ORDERS`, `RESET_PASSWORD`, `CATEGORY`, `CART`, `CHECKOUT_ADDRESS_BOOK`, `CHECKOUT_PAYMENT_DETAILS`, `CHECKOUT_PLACE_ORDER`, `CHECKOUT_SHIPPING_ADDRESS`, `ORDER_CONFIRMATION`, `ORDER_FAILURE`, `CHECKOUT_RESULT`, `DOWNLOAD_ERROR`, `PRODUCT`, `SEARCH`, `FAVORITES`, `RESET_PASSWORD` | The type of the page. Some pages may have parameters, like: `productId` of the viewing product. Those parameters are described below.
keywords | string, optional | for type==’ORDERS’: the keywords that are used to find orders in the customer account page. for type==’SEARCH’: the keywords that are used to find products on the product search page.
from | integer timestamp, optional | for type==’ORDERS’: The timestamp of the start of the orders date range.
to | integer timestamp, optional | for type==’ORDERS’: The timestamp of the end of the orders date range.
offset | | for type==’ORDERS’: the position of the current order list page (starting from 0). for type==’CATEGORY’ and SEARCH’: the position of the current product list page (starting from 0).
categoryId | integer | for type==’CATEGORY’: the id of the showing product category or 0 if this is the starting page of the catalog and no categories are selected yet. for type==’PRODUCT’: the category internal id the current product has been navigated from. Zero (0) is the root category, −1 meaning that the category is unknown (e.g. a product opened from a search result).
mainCategoryId | integer | for type==’PRODUCT’ in the OnPageLoaded event: the internal id of category that is considered the default category of this product (in case if the product is assigned to a few different categories). If a product is assigned to a single category, mainCategoryId will be equeal to categoryId; if a product is not assigned to any category, its mainCategoryId is 0 (zero). for type==’PRODUCT’ in the OnPageLoad event: always 0 (zero);
sort | string, one of: ‘normal’, ‘addedTimeDesc’, ‘priceAsc’, ‘priceDesc’, ‘nameAsc’, ‘nameDesc’ | for type==’CATEGORY’ and ’SEARCH’: the order of the product list, as selected by the user in the ‘sort by’ drop-down. ‘Desc’ suffix stands for the descending order, ‘Asc’ suffix stands for the ascending order.
orderId | integer | for type==’CHECKOUT_RESULT’ and type==’ORDER_CONFIRMATION’: the internal id of the order (not to be confused with the store order number)
ticket | integer | for type==’CHECKOUT_RESULT’: the security random code that allows to retrieve information about the order
errorType | one of the following: ‘expired’, ‘invalid’, ‘limit’ | for type==’DOWNLOAD_ERROR’: the type of the error while downloading an e-good file.
key | integer, optional | for type==’DOWNLOAD_ERROR’: the downloading file internal id
productId | integer | for type==’PRODUCT’: the internal id of the displaying product (not to be confused with SKU).
orderNumber | integer | for type==’ORDER_CONFIRMATION’ the number of the order placed by customer(without prefix and suffix).
vendorOrderNumber | string | for type==’CHECKOUT_RESULT’ and type==’ORDER_CONFIRMATION’ the number of the order placed by customer(with prefix and suffix)
hasPrevious | boolean | `true` if customer visited some previous pages earlier and current page is not the entry page. `false` if current page is the first page of entry
variationId | number | Variation ID if specified in the URL. [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#open-specific-product-variation)
filterParams | \<*FilterParameters*\> | Filter parameters used in product search request. For `"CATEGORY"` and `"SEARCH"` pages only

#### FilterParameters

Name | Type | Description
---- | ----- | -----------
attributes | \<*SelectedAttributes*\> | Information about selected attributes and their values
categories | Array of string | List of category IDs where customer searches in
includeProductsFromSubcategories | boolean | `true` if the search needs to include products from subcategories. `false` otherwise
keyword | string | The keyword customer used when searching for products
options | \<*SelectedOptions*\> | Information about selected options and their values

#### SelectedAttributes

Name | Type | Description
---- | ----- | -----------
ATTRIBUTE_NAME | string | Attribute name is the field name and array of attribute value(s) is the value of this field

#### SelectedOptions

Name | Type | Description
---- | ----- | -----------
OPTION_NAME | string | Option name is the field name and array of option value(s) is the value of this field


### Ecwid.setStorefrontBaseUrl 

> Set storefront base URL to a custom URL after store is loaded

```js
Ecwid.setStorefrontBaseUrl('https://www.ecwid.com/demo/?ipad&lang=en');
```

Dynamically updates storefront base URL. 

Typically is used for [SEO URLs feature](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls) to preserve dynamically added query parameters in a page URL. Works similarly to `window.ec.config.baseUrl` method.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

## Open page in storefront

> Open cart page in storefront

```js
Ecwid.openPage('cart');
```

When customer browses the store, Ecwid makes the navigation easy and fast due to the Ajax technology without any page reloads. However, sometimes you may need to open a specific page in storefront automatically or after a user clicks on a link. This is where `Ecwid.openPage` method can be useful. 

The pages in storefront can be of two types: 

* general page like `cart` and shipping stage at checkout
* page with parameters, like product, category, search, etc.

For general pages, you can only specify the name of the page you want to open. However, for pages with parameters, it is necessary that you specify what page you want to open exactly. 

<aside class="notice">Subscribe to the 'Ecwid.OnAPILoaded' <a href="https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded">JS API event></a> to ensure availability of this function.</aside>

### Pages with parameters

> Open product details page of product with ID: 12345

```js
Ecwid.openPage('product', {'id': 12345});
```

To open a page with parameters, you must specify them in the object in second argument of `Ecwid.openPage` function. See example on the right. 

**Product**

For product details pages, the first argument of `Ecwid.openPage` function will be `'product'`. 

Additional parameters include: `'id'`, `'name'`, `'variation'` and `'options'` 

`'id'` is a required parameter, which helps Ecwid identify which exact product page to open in storefront. If it is not specified, the function call will be ignored. 

> Open product page and set product name in page URL as 'Apple'

```js
Ecwid.openPage('product', {'id': 72585497, 'name': "Apple"});
```

`'name'` is an optional parameter, used to set custom product name for the page URL. If it is not specified, Ecwid will use the original product name set by store owner.

The example on the right opens product details page of a specific product and uses a custom name we set for it in [Ecwid demo store](https://www.ecwid.com/demo/). As a result, the URL will look like: `https://www.ecwid.com/demo/Apple-p72585497`

> Open variation with ID 16351010 of a product with ID 72585497 

```js
Ecwid.openPage('product', {'id': 72585497, 'name': "Rip Curl Channel Island", 'variation': 16351010});
```

`'variation'` is an optional parameter, used to select a specific product variation when opening product details page. If it is not specified, Ecwid will use the default set of product options for that product page. 

> Open a product with ID 72585497 with options: Size - Medium, Color - Dark grey

```js
Ecwid.openPage('product', {'id': 72585497, 'name': "Rip Curl Channel Island", 'options': [2,2]});
```

`'options'` is an optional parameter, used to select specific product option selections. It supports dropdown and radio button product options. 

If it is not specified, Ecwid will use the default set of product options for that product page. 

<aside class='notice'>If you need more information about a product or category, use <a href="https://developers.ecwid.com/api-documentation/access-tokens">Ecwid REST API</a>. Public access token can be used in the client-side code, and private token on the server-side.</aside>

**Category**

For category pages, the first argument of `Ecwid.openPage` function will be `'category'`.

Additional parameters include: `'id'`, `'name'` and `'page'`

`'id'` is a required parameter, which helps Ecwid identify which exact category page to open in storefront. If it is not set, Ecwid will use store home page, i.e. id will be `0` 

> Open category page and set product name in URL as 'Fruit' 

```js
Ecwid.openPage('category', {'id': 20671017, 'name': "Fruit", 'page': 3});
```

`'name'` is an optional parameter, used to set custom product name for the page URL. If it is not specified, Ecwid will use the original category name set by store owner.

`'page'` is an optional parameter, used to open a specific page with products. If the page value is bigger than there are pages in category, Ecwid will open the last available page. If integer value is not passed, `'page'` parameter is ignored.

The example on the right opens product details page of a specific product and uses a custom name we set for it in [Ecwid demo store](https://www.ecwid.com/demo/). As a result, the URL will look like: `https://www.ecwid.com/demo/Apple-p72585497`

<aside class='notice'>If you need more information about a product or category, use <a href="https://developers.ecwid.com/api-documentation/access-tokens">Ecwid REST API</a>. Public access token can be used in the client-side code, and private token on the server-side.</aside>

**Search**

> Open search page and search for 'surfboard' products in a storefront

```js
Ecwid.openPage('search', {'keywords': 'surfboard', 'page': 2});
```

> Open search page and search for products under $50

```js
Ecwid.openPage('search', {'priceTo': '50'});
```

> Open search page and search for in stock shoes that have Brand attribute equal 'Nike'

```js
Ecwid.openPage(
    'search', 
    {
      'keywords': 'shoes', 
      'fieldBrand': 'Nike',
      'inStock': 'true',
      'page': 5
    }
  );
```

For the search page, the first argument of `Ecwid.openPage` function will be `'search'`.

Additional parameters include: 

- `'priceFrom'`: the minimum products’ price (base price). The decimal separator is dot. No currency symbol.
- `'priceTo'`: the maximum products’ price (base price). The decimal separator is dot. No currency symbol. 
- `'category'`: the category ID, to which products belong. This partially duplicates functions of the Categories widget and provides you with the more flexible results. For example, you can mix several parameters: to select all the products which cost less than $30 and belong to the ‘t-shirt’ category
- `'withSubcategory'`: whether to show products from subcategories, if the "category" parameter is indicated. Possible values: true, any other value is treated as false. 
- `'field{Name}=param[,param]'`: search by product attributes. "Name" is the attribute name (spaces will work). "Param" is the attribute value. You can place several values separated by comma. In that case values will be connected through "OR", and if the product has at least one of them it will be shown. Important note: if you need to search for an exact attribute value you should enclose it in the quotation marks. For information about product attributes please refer to this article.
- `'field{id}=param[,param]'`: it is the same parameter as `field[Name]` but attribute ID is used instead attribute name. In this case you need to get the attribute ID number through the API. It is more complex but resistant to attributes renaming.
- `'keywords'`: it is a search string. The search result will be products with title, description, or some other fields containing these keywords.
- `'inStock'`: if `true` returns the list of products "In stock". Otherwise the search shows both "In stock" and "Out of stock" products.
- `'page'` is an optional parameter, used to open a specific page with products. If the page value is bigger than there are pages in category, Ecwid will open the last available page. If integer value is not passed, `'page'` parameter is ignored.

**Sign in**

> Open sign in page and redirect user to a custom page on success

```js
Ecwid.openPage('signin', {'returnurl': 'https://www.ecwid.com/demo/Surfboards-c20671017'});
```

For the sign in page, the first argument of `Ecwid.openPage` function will be `'signin'`.

Additional parameters include: `'returnurl'`

`'returnurl'` is an optional parameter, which can redirect user to a specific URL after the successful login. 

### Pages without parameters

> Open cart page in storefront 

```js
Ecwid.openPage('cart');
```

Open the following pages by setting page name as first argument of `Ecwid.openPage` function. 

- `'cart'`
- `'checkout/shipping'`
- `'checkout/payment'`
- `'checkout/place-order'`
- `'checkout/order-confirmation'`
- `'account/settings'`
- `'account/orders'`
- `'account/address-book'`
- `'account/favorites'`
- `'pages/about'`
- `'pages/shipping-payment'`
- `'pages/returns'`
- `'pages/terms'`
- `'pages/privacy-policy'`

## Subscribe to events

Find various useful events and execute your functions with them.

### Ecwid.OnAPILoaded

> OnAPILoaded usage example

```javascript
Ecwid.OnAPILoaded.add(function() {
    console.log("Ecwid storefront JS API has loaded");
});
```

Subscribe your code to this event so it is called called exactly when the Ecwid Javascript API is loaded and ready. 

### Ecwid.OnPageLoad/Ecwid.OnPageLoaded

> If user visits cart page, do domething

```javascript
Ecwid.OnPageLoaded.add(function(page) {
    if (page.type == "CART") {
      // do something ...
  }
});
```

These events contain callbacks that get called on each page change inside the product browser. 

The difference between **OnPageLoad** and **OnPageLoaded** is that the former is called when the page is changed (e.g. a link is clicked), while the latter is called later when the corresponding page is loaded inside the product browser.

The callback functions accept one parameter of type **Page** specifying which page is to be loaded (or has already been loaded). See **Page Object** details for more info.

<aside class='notice'>If you need more information about a product or category, use <a href="https://developers.ecwid.com/api-documentation/access-tokens">Ecwid REST API</a>. Public access token can be used in the client-side code, and private token on the server-side.</aside>

#### Page object

Describes the page displaying inside the product browser.

> Get page type after new page is loaded 

```js
Ecwid.OnPageLoaded.add(function(page){
  console.log("Current page is of type: " + page.type);
});

// prints
// Current page is of type: CATEGORY
```

**Fields:**

Name | Type | Description
---- | ----- | -----------
name | string | For types: `CATEGORY`, `PRODUCT`. Name of the opened category or product 
type | string, one of the following: `ACCOUNT_SETTINGS`, `ADDRESS_BOOK`, `ORDERS`, `RESET_PASSWORD`, `CATEGORY`, `CART`, `CHECKOUT_ADDRESS_BOOK`, `CHECKOUT_PAYMENT_DETAILS`, `CHECKOUT_PLACE_ORDER`, `CHECKOUT_SHIPPING_ADDRESS`, `ORDER_CONFIRMATION`, `ORDER_FAILURE`, `CHECKOUT_RESULT`, `DOWNLOAD_ERROR`, `PRODUCT`, `SEARCH`, `FAVORITES`, `RESET_PASSWORD` | The type of the page. Some pages may have parameters, like: `productId` of the viewing product. Those parameters are described below.
keywords | string, optional | for type==’ORDERS’: the keywords that are used to find orders in the customer account page. for type==’SEARCH’: the keywords that are used to find products on the product search page.
from | integer timestamp, optional | for type==’ORDERS’: The timestamp of the start of the orders date range.
to | integer timestamp, optional | for type==’ORDERS’: The timestamp of the end of the orders date range.
offset | | for type==’ORDERS’: the position of the current order list page (starting from 0). for type==’CATEGORY’ and SEARCH’: the position of the current product list page (starting from 0).
categoryId | integer | for type==’CATEGORY’: the id of the showing product category or 0 if this is the starting page of the catalog and no categories are selected yet. for type==’PRODUCT’: the category internal id the current product has been navigated from. Zero (0) is the root category, −1 meaning that the category is unknown (e.g. a product opened from a search result).
mainCategoryId | integer | for type==’PRODUCT’ in the OnPageLoaded event: the internal id of category that is considered the default category of this product (in case if the product is assigned to a few different categories). If a product is assigned to a single category, mainCategoryId will be equeal to categoryId; if a product is not assigned to any category, its mainCategoryId is 0 (zero). for type==’PRODUCT’ in the OnPageLoad event: always 0 (zero);
sort | string, one of: ‘normal’, ‘addedTimeDesc’, ‘priceAsc’, ‘priceDesc’, ‘nameAsc’, ‘nameDesc’ | for type==’CATEGORY’ and ’SEARCH’: the order of the product list, as selected by the user in the ‘sort by’ drop-down. ‘Desc’ suffix stands for the descending order, ‘Asc’ suffix stands for the ascending order.
orderId | integer | for type==’CHECKOUT_RESULT’ and type==’ORDER_CONFIRMATION’: the internal id of the order (not to be confused with the store order number)
ticket | integer | for type==’CHECKOUT_RESULT’: the security random code that allows to retrieve information about the order
errorType | one of the following: ‘expired’, ‘invalid’, ‘limit’ | for type==’DOWNLOAD_ERROR’: the type of the error while downloading an e-good file.
key | integer, optional | for type==’DOWNLOAD_ERROR’: the downloading file internal id
productId | integer | for type==’PRODUCT’: the internal id of the displaying product (not to be confused with SKU).
orderNumber | integer | for type==’ORDER_CONFIRMATION’ the number of the order placed by customer(without prefix and suffix).
vendorOrderNumber | string | for type==’CHECKOUT_RESULT’ and type==’ORDER_CONFIRMATION’ the number of the order placed by customer(with prefix and suffix)
hasPrevious | boolean | `true` if customer visited some previous pages earlier and current page is not the entry page. `false` if current page is the first page of entry
variationId | number | Variation ID if specified in the URL. [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#open-specific-product-variation)
filterParams | \<*FilterParameters*\> | Filter parameters used in product search request. For `"CATEGORY"` and `"SEARCH"` pages only

#### FilterParameters

Name | Type | Description
---- | ----- | -----------
attributes | \<*SelectedAttributes*\> | Information about selected attributes and their values
categories | Array of string | List of category IDs where customer searches in
includeProductsFromSubcategories | boolean | `true` if the search needs to include products from subcategories. `false` otherwise
keyword | string | The keyword customer used when searching for products
options | \<*SelectedOptions*\> | Information about selected options and their values

#### SelectedAttributes

Name | Type | Description
---- | ----- | -----------
ATTRIBUTE_NAME | string | Attribute name is the field name and array of attribute value(s) is the value of this field

#### SelectedOptions

Name | Type | Description
---- | ----- | -----------
OPTION_NAME | string | Option name is the field name and array of option value(s) is the value of this field

### Ecwid.OnSetProfile

> Ecwid.OnSetProfile code example

```html
<script src="//app.ecwid.com/script.js?1003" type="text/javascript" charset="UTF-8"></script>
<script>
function dump(arr,level) {
  var dumped_text = "";
    if (!level) level = 0;
    // The padding given at the beginning of the line.
      var level_padding = "";
      for (var j=0;j<level+1;j++) level_padding += " ";
        if (typeof(arr) == 'object') { // Array/Hashes/Objects
          for (var item in arr) {
          var value = arr[item];
            if (typeof(value) == 'object') { // If it is an array,
            dumped_text += level_padding + "'" + item + "' ...\n";
            dumped_text += dump(value,level+1);
            } else {
            dumped_text += level_padding + "'" + item + "' => \"" + value + "\"\n";
            } 
          }
        } else { // Stings/Chars/Numbers etc.
        dumped_text = "===>"+arr+"<===("+typeof(arr)+")";
      }
      return dumped_text;
  }

Ecwid.OnSetProfile.add(function(customer) {
  if (customer == null) document.getElementById('CUSTOMER').innerHTML = 'not logged in';
    else document.getElementById('CUSTOMER').innerHTML = dump(customer);
});
</script>
<div><pre id='CUSTOMER'/></div>
<script>
xProductBrowser();
</script>
```

This event contains callback functions that receive the changes in the current customer profile. 

If no customer is logged in, these functions receive **null**. Whenever a customer logs in/out, the callback functions are called with either the corresponding parameter of type **Customer** or **null**. Example code can be seen on the right.

### Ecwid.OnCartChanged

> Specify a callback function when cart is changed in storefront

```javascript
Ecwid.OnCartChanged.add(function(cart){
    // your code here
});
```

This event contains callback functions that get called each time when a shopping cart is changed — either by the customer or due to system events.

The callback function receives `cart` object as an argument, that has the new shopping cart state after the change is applied.

The callbacks added to `Ecwid.OnCartChanged` will be called when the shopping cart is initialized and on every occasion when either of the properties of the passed `cart` object is changed. These occasions include:

- Cart initialization 
- Adding a product to cart
- Removing a product from cart
- Changing the product’s options
- Clearing the cart
- Applying a discount coupon
- Selecting and changing the selection of the shipping method
- Changing shipping address
- Syncing the cart contents, if there are a few browser tabs with the store are opened
- Clearing the cart upon user’s logout — in this occasion the callback receives `null` as an argument.

The passed `cart` object represents only the basic properties of the shopping cart. It contains the data coming from the customer’s actions (like products, coupons, shipping methods) and **might not contain the calculated aggregates** (like order totals, shipping costs, the discounted amounts or taxes). For the calculated aggregates, it is rather recommended to use the `Ecwid.Cart.calculateTotal()` method.

### Ecwid.OnProductOptionsChanged

> Show an alert if product options were changed

```javascript
Ecwid.OnProductOptionsChanged.add(function(productid) {
   window.alert("Options changed, product id:" + productid);    
});
```

This event executes callback function each time when product option was changed. The callback functions accept one parameter: **productid**, specifying ID of the product changed. 

`OnProductOptionsChanged` works for following options type: dropdown list, radio button and checkbox. Input, textarea and upload files types are not supported yet.

### Ecwid.OnOrderPlaced

> Get order number of a placed order

```js
Ecwid.OnOrderPlaced.add(function(order){
  console.log(order.orderNumber);
});

// 781
```

`Ecwid.OnOrderPlaced()` event allows you to get details of an order placed by a customer in storefront right after an order is placed. This event provides the `order` object in callback function so that you can get order details right after the event occurs.

#### Order object

Describes the details of a placed order by customer. 

**Fields**

Name | Type | Description
---- | ----- | -----------
items | Array\<*OrderItems*\> | Items purchased in order
productsQuantity | integer | Total quantity of purchased items in order
couponName | string | Applied coupon name. `undefined` if no coupon was used
weight | number | Total order weight
paymentMethod | string | Selected payment method name. `undefined` if no method selected
shippingCarrierName | string | Selected shipping carrier name. `undefined` if no method selected
shippingMethod | string | Selected shipping method name. `undefined` if no method selected
total | number | Order total
subtotal | number | Order subtotal
tax | number | Order total tax
couponDiscount | number | Coupon discount applied to order
volumeDiscount | number | Sum of discounts based on subtotal
customerGroupDiscount | number | Sum of discounts based on customer group
discount | number | Total discount amount for order
shipping | number | Total shipping cost for order
handlingFee | number | Handling fee applied to order
shippingAndHandling | number | Sum of `shipping` and `handlingFee` fields
billingPerson | \<*PersonInfo*\> | Customer billing information
shippingPerson | \<*PersonInfo*\> | Customer shipping information
affiliateId | string | Affiliate ID value for order
orderNumber | number | Order number in a store
vendorNumber | string | Order number with custom prefixes and suffixes of that store
date | string (UNIX Timestamp) | Date when order was placed as a UNIX Timestamp, e.g. `"1484638550"`
paymentStatus | string | Payment status of an order. One of: `AWAITING_PAYMENT`, `PAID`, `CANCELLED`, `REFUNDED`, `PARTIALLY_REFUNDED`, `INCOMPLETE`
fulfillmentStatus | string | Fulfillment status of an order. One of: `AWAITING_PROCESSING`, `PROCESSING`, `SHIPPED`, `DELIVERED`, `WILL_NOT_DELIVER`, `RETURNED`, `READY_FOR_PICKUP`
customer | \<*CustomerInfo*\> | Basic customer info

##### OrderItems

Name | Type | Description
---- | ----- | -----------
quantity | integer | Quantity of an item in order
product | \<*ProductInfo*\> | Product details
options | Map\<*OptionsInfo*\> | Selected product options

#### ProductInfo

Name | Type | Description
---- | ---- | -----------
id | Integer | Internal unique product ID
name | String | Product name
price | Integer | Product price
shortDescription | String | Product description truncated to 120 characters
sku | String | Product SKU
url | String | URL to this product details page in store front (store front URL is generated from a field in Ecwid Control panel > Settings > General > Store profile)
weight | Integer | Weight of a product

#### OptionsInfo

Name | Type | Description
---- | ---- | -----------
selectedOption | string | Selected option name as a key, its value as a value of that key. For `dropdown`, `radio`, `textarea`, `textfield` options value is a string; for `checkbox` option the value is a comma-separated list of selected values in a single string; for `date` option the value is a selected date according to the store format; for `files` option the value is a string in a format: `"4 files"`

#### PersonInfo

Name | Type | Description
-----|------|------------
name | string | Customer's name
companyName | string | Customer's company name
street | string | Customer's street address
city | string | Customer's city
countryName | string | Customer's country name. `countryCode` can be used instead
countryCode | string | Customer's country code. `countryName` can be used instead
postalCode | string | Customer's zip code
stateOrProvinceCode | string | Customer's state or province code
stateOrProvinceName | string | Customer's state or province name
phone | string | Customer's phone number

#### CustomerInfo

Name | Type | Description
-----|------|------------
name | string | Customer's name
email | string | Customer's email

## Get customer details

Find out more about customer that is currently logged in a store.

### Ecwid.getTrackingConsent

> Get status of customer's consent to be tracked

```js
Ecwid.getTrackingConsent();

// {  userResponse: "ACCEPTED", askConsent: true }
```

`Ecwid.getTrackingConsent();` provides information about customer's consent to be tracked on store pages. 

**Fields:**

Name | Type | Description
-----|------|------------
userResponse | string | Customer's preferred choice for being tracked. Possible values: `"ACCEPTED"`, `"DECLINED"` or empty
askConsent | boolean | `true` if store requests customer consent to be tracked. `false` otherwise

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Customer Object

Customer object describes details of a logged in customer in a store.

> Get customer email and billing country example

```js
Ecwid.OnSetProfile.add(function(customer) {
  console.log(customer.email);
  console.log(customer.billingPerson.countryName);
});

// prints
// supercoder@matrix.com
// United States
```

**Fields:**

Name | Type | Description
---- | ---- | -----------
billingPerson | \<*Person*\> | Customer’s name along with his/her billing address, as entered in the last order.
email | String | Email address of a customer
id | Number | Unique customer ID in Ecwid
membership | \<*CustomerGroup*\> | Customer group details. Present only if customer belongs to a customer group
ownerId | number | Store ID this customer belongs to
registered | UNIX Timestamp | Registration date of this customer
shippingAddresses | Array of \<*ShippingAddress*\> | A list of addresses in the customer’s address book

### ShippingAddress Object

The customer’s address as stored in the address book.

**Fields:**

Name | Type | Description
---- | ---- | -----------
id | integer | The unique address id Ecwid database
person | Object (Person) | The object describing the address along with the person’s name and phone number.

### Person Object

Describes the person name, company and address.

**Fields:**

Name | Type | Description
---- | ----- | -----------
name | string | The first and the last name of the person, separated by a space.
companyName | string, optional | The person’s company name, if applicable
street | string, optional | The street address of the person, if applicable. If there are two address lines, they are separated by a newline character ‘\n’
city | string, optional | The person’s city, if applicable
countryCode | string, optional | The person’s country code, as listed in ISO 3166-2
postalCode | string, optional | The person’s postal code or ZIP code, if applicable
stateOrProvinceCode | string, optional | The person’s region/state/province code by ISO 3166-2. Please note that not all countries regional codes are listed in the Ecwid database so far.
countryName | string, optional | Country name, if applicable
phone | string, optional | Phone number, if applicable

### CustomerGroup Object

Customer group information 

**Fields:**

Name | Type | Description
---- | ---- | -----------
id | number | The unique id of a customer group
name | string | Name of the customer group
ownerid | number | Ecwid store ID 

## Manage customer cart

Manage cart on customer's behalf.

### Ecwid.Cart.addProduct

This function allows to add a product to shopping cart, modifying the cart on behalf of customer.

There are 2 possible ways to call this function: adding products by product ID or adding products with extended options.

<aside class="notice">Subscribe to the 'Ecwid.OnAPILoaded' <a href="https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded">JS API event></a> to ensure availability of this function.</aside>

#### Adding by product ID

> Add product function

```javascript
Ecwid.Cart.addProduct(productID, callback);
```

`addProduct()` can accept 2 arguments: **productId** and **callback**.

Name | Type | Description
---- | ---- | -----------|
**productID** | integer | the Ecwid’s internal product ID to be added to cart (can be retrieved from product export, seen in the URL of the product page or via [REST API](#search-products))
callback | function | the callback function to be called once the operation is complete (either succeeded or failed). See below for details.

> Add product to cart using ID

```javascript
var productId = 10;
Ecwid.Cart.addProduct(productId); 
```

The most simple call to `Ecwid.Cart.addProduct` only requires to pass the numeric product ID. See example code on the right. This code adds 1 item of the given product ID to cart.

If this product contains variations and the base product is out of stock, the first variation that is in stock will be added to cart instead. If the product is out of stock (and there are no variations in stock for this product), nothing is added to cart.

#### Adding with extended options

> Add product to cart with extended options
 
```javascript
var product = {
  id: 10,
  quantity: 3,
  options: {
    someTextOption: "Your name",
    someDateOption: new Date().getTime().toString(),
    someRadioOption: "Use default color",
    someDropDownOption: "I want custom engraving",
    someCheckboxOption: ["It's a gift", "Gold engraving"]
  },
  callback: function(success, product, cart) {
    // ...  
  }
}

Ecwid.Cart.addProduct(product);
```

If it is necessary to specify options or quantity of product to be added to cart, the product parameter needs to be passed as an object.

Since this method allows to specify the exact options to be added to cart, only the base product or variation matching those options will be added to cart. So, if the base product or matching variation is out of stock, the addProduct call will not add anything to cart, even if there are other variations in stock.

**Callback**

Adding to cart is done asynchronously, so if it is important to know the result of this function. The callback function can be passed as the second agrument of the function.

> Add product to cart with callback function, example 1

```javascript
Ecwid.Cart.addProduct(productID, function(success, product, cart){
  console.log(success); // true or false
  console.log(product.name);
});
```

> Add product to cart with callback function, example 2

```javascript
Ecwid.Cart.addProduct({
  id: 10, 
  quantity: 3, 
  callback: function(success, product, cart){
    console.log(success); // true or false
    console.log(product.name);
  }
});
```

Callback receives 3 arguments: **success**, **product**, **cart**

Name | Type | Description
---- | ---- | -----------
success | Boolean | indicates the overall status of addition (succeeded or failed)
product | \<*Product*\> | conatins the object representation of the product added to cart, or null if adding to cart failed (wrong product ID or product is out of stock).
cart | \<*Cart*\> | contains the object representation of the shopping cart after addition (same as in `Ecwid.OnCartChanged` event)

#### Product

Name | Type | Description
---- | ---- | -----------
id | integer | Product ID
quantity | integer | Amount of products to add to cart
options | \<*OptionsInfo*\> | Selected product options for product
callback | function | Function to run after the function is performed

#### OptionsInfo

Name | Type | Description
---- | ---- | -----------
optionName | string or array of string | Replace `optionName` with the real exact product option name you want to specify. Available values depend on the product option type: for checkbox options the value is `array of strings`, for all other options the value is a `string`

### Ecwid.Cart.removeProduct 

> Remove specific product from cart

```js
Ecwid.Cart.removeProduct(index);
```

> Example

```js
// returns cart details with items' details. Use 'items' array to get the index

Ecwid.Cart.get(function(cart){
  console.log(cart);
})

// remove first product from customer's cart

Ecwid.Cart.removeProduct(0);
```

Removes a product with specified index from customer's cart. `Ecwid.Cart.removeProduct()` receives one argument: **index**

Name | Type | Description
---- | ---- | -----------
index | integer | index of a product in cart object you need to remove from customer's cart

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.Cart.clear

> Clear cart contents

```js
Ecwid.Cart.clear();
```

Clears the cart contents.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.Cart.get

> Get total number of products in cart code example

```javascript
Ecwid.Cart.get(function(cart) {
  alert(cart.productsQuantity + " products in cart now");
});
```

Retrieves the cart contents asynchronously and passes it as an argument of type Cart to the callback.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.Cart.calculateTotal

> Calculate cart total example

```javascript
Ecwid.Cart.calculateTotal(function(order) {
  if (!order)
    alert('An error occurred while calculating the order!')
  else    
    alert('Order total: ' + order.total);    
}); 
```

Calculates the cart asynchronously and passes the result as an argument of type **Order** to the callback. 

Cart calculation involves a request to server, so this method should be called only occasionally. Calling it frequently, e.g. from loops or by timer, is not acceptable.

Since the calculation needs a server connection, it might fail due to network conditions. In this case, null is passed into the callback instead of **Order** object.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.Cart.gotoCheckout

> Send customer to checkout

```js
Ecwid.Cart.gotoCheckout();
```

`Ecwid.Cart.gotoCheckout()` function allows you to send customer to the first step of the checkout process in a store. Customer will be sent there only if agreeing to terms and conditions is not required in the store. You can find this setting in Ecwid Control Panel > Settings > General > Legal Pages > "Show “I agree with Terms & Conditions” checkbox during checkout" or check it using `Ecwid.Cart.canGotoCheckout()` function.

> Callback function after sending to checkout

```js
Ecwid.Cart.gotoCheckout(function(){
  alert("Checkout process started");
});
```

You can also execute a callback function if a customer was successfully sent to the first step of the checkout process in a store. See example code on the right.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.Cart.canGotoCheckout

> Check if possible to send customer to checkout

```js
Ecwid.Cart.canGotoCheckout(function(callback){
  console.log(callback);
});
```

Using `Ecwid.Cart.canGotoCheckout()` function you can check whether you can send customers to the first step of checkout process in a store. 

**Fields:**

Name | Type | Description
---- | ---- | -----------
callback | boolean | `true` if you can send customer to the checkout process, `false` otherwise

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

### Ecwid.Cart.setCustomerEmail

> Function description

```js
Ecwid.Cart.setCustomerEmail(email, successCallback, errorCallback);
```

> Set customer email example

```js
Ecwid.Cart.setCustomerEmail('james@coolstore.com');
```

`Ecwid.Cart.setCustomerEmail()` allows you to set customer's email in checkout process. Make sure to use it on pages, where the email is not shown so that it is always up-to-date.

If a customer is logged in, your set email will overwrite the email for that customer's account (if possible).

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

**Fields:**

Name | Type | Description
-----|------|-------------
**email** | string | Customer email address to be set
successCallback | function | Success callback function
errorCallback | function | Error callback function

<aside class='notice'>
Parameters in bold are mandatory
</aside>

`errorCallback` structure is: errorCallback(errCode, errMsg)

Name | Type | Description
-----|------|-------------
errCode | number | Error code
errMsg | string | Error message

**Errors**

Error code | Error message
-----------|--------------
0 | Missing argument
100 | Incorrect data passed

### Ecwid.Cart.setOrderComments

> Function

```js
Ecwid.Cart.setOrderComments(orderComments, successCallback, errorCallback);
```

> Set order comments example

```js
Ecwid.Cart.setOrderComments('Leave order at the door.');
```

> Full function call example

```js
Ecwid.Cart.setOrderComments('Leave order at the door.',
  function(){
    console.log('Successfully set order comments.')
  },
  function(){
    console.log('Error setting order comments.');
});
```

`Ecwid.Cart.setOrderComments` allows you to set the order comments field on the fly. It's quite useful to pass some additional information for an order, which will be provided in email notifications to customer and store admin as well as the order details in Ecwid Control Panel and Ecwid API.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

**Fields:**

Name | Type | Description
-----|------|-------------
**orderComments** | string | Order comments string to be set for an order
successCallback | function | Success callback function
errorCallback | function | Error callback function

<aside class='notice'>
Parameters in bold are mandatory
</aside>

`errorCallback` structure is: errorCallback(errCode, errMsg)

Name | Type | Description
-----|------|-------------
errCode | number | Error code
errMsg | string | Error message

**Errors**

Error code | Error message
-----------|--------------
1000 | Store owner disabled this functionality

### Ecwid.Cart.setAddress

> Function

```js
Ecwid.Cart.setAddress(address, successCallback, errorCallback);
```

> Set shipping address example

```js
Ecwid.Cart.setAddress({
  "name": "John Carmichael",
  "companyName": "Cool Slippers",
  "street": "5th Ave",
  "city": "New York",
  "countryName": "United States",
  "postalCode": "10002",
  "stateOrProvinceCode": "NY",
  "phone": "+1 234 523 11 42"
  }
);
```

> Full function call example

```js
Ecwid.Cart.setAddress({
  "name": "John Carmichael",
  "companyName": "Cool Slippers",
  "street": "5th Ave",
  "city": "New York",
  "countryName": "United States",
  "postalCode": "10002",
  "stateOrProvinceCode": "NY",
  "phone": "+1 234 523 11 42"
  },
  function(){
    console.log('Address successfully set')
  },
  function(){
    console.log('Error setting the address');
  }
);
```

`Ecwid.Cart.setAddress()` allows you to set shipping address for customer in storefront. You can use it to prefill and hide fields in checkout process to simplify it. 

If you specify some fields only (name, for example), then Ecwid will reset all other fields and they will become empty. So if you need to update only some fields, make sure to send them in your function call as well as the updated values.

When function is called, Ecwid will set the 'My shipping address is the same as the billing address' flag to `true` automatically.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

**Fields:**

Name | Type | Description
-----|------|-------------
**address** | \<*Person*\> | Customer's shipping address details
successCallback | function | Success callback function
errorCallback | function | Error callback function

**Person** fields:

Name | Type | Description
-----|------|------------
name | string | Customer's name
companyName | string | Customer's company name
street | string | Customer's street address. Use `\n` to place text in Address Line 2
city | string | Customer's city
countryName | string | Customer's country name. `countryCode` can be used instead
countryCode | string | Customer's country code. `countryName` can be used instead
postalCode | string | Customer's zip code
stateOrProvinceCode | string | Customer's state or provice code
phone | string | Customer's phone number


<aside class='notice'>
Parameters in bold are mandatory
</aside>

`errorCallback` structure is: errorCallback(errCode, errMsg)

Name | Type | Description
-----|------|-------------
errCode | number | Error code
errMsg | string | Error message

**Errors**

Error code | Error message
-----------|--------------
0 | Missing argument

### Ecwid.Cart.setBillingAddress

> Function

```js
Ecwid.Cart.setBillingAddress(address, successCallback, errorCallback);
```

> Set billing address example

```js
Ecwid.Cart.setBillingAddress({
  "name": "John Carmichael",
  "companyName": "Cool Slippers",
  "street": "5th Ave",
  "city": "New York",
  "countryName": "United States",
  "postalCode": "10002",
  "stateOrProvinceCode": "NY",
  "phone": "+1 234 523 11 42"
  }
);
```

> Full function call example

```js
Ecwid.Cart.setBillingAddress({
  "name": "John Carmichael",
  "companyName": "Cool Slippers",
  "street": "5th Ave",
  "city": "New York",
  "countryName": "United States",
  "postalCode": "10002",
  "stateOrProvinceCode": "NY",
  "phone": "+1 234 523 11 42"
  },
  function(){
   console.log('Address successfully set')
  },
  function(){
   console.log('Error setting the address');
  }
);
```

`Ecwid.Cart.setBillingAddress()` allows you to set billing address for customer in storefront. You can use it to specify some address from your custom form on your website. 

If you specify some fields only (name, for example), then Ecwid will apply the changed fields only, leaving preset ones as they were before.

When function is called, Ecwid will set the 'My shipping address is the same as the billing address' flag to `false` automatically.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

**Fields:**

Name | Type | Description
-----|------|-------------
**address** | \<*Person*\> | Customer's billing address details
successCallback | function | Success callback function
errorCallback | function | Error callback function

**Person** fields:

Name | Type | Description
-----|------|------------
name | string | Customer's name
companyName | string | Customer's company name
street | string | Customer's street address. Use `\n` to place text in Address Line 2
city | string | Customer's city
countryName | string | Customer's country name. `countryCode` can be used instead
countryCode | string | Customer's country code. `countryName` can be used instead
postalCode | string | Customer's zip code
stateOrProvinceCode | string | Customer's state or provice code
phone | string | Customer's phone number

<aside class='notice'>
Parameters in bold are mandatory
</aside>

`errorCallback` structure is: errorCallback(errCode, errMsg)

Name | Type | Description
-----|------|-------------
errCode | number | Error code
errMsg | string | Error message

**Errors**

Error code | Error message
-----------|--------------
0 | Missing argument

### Generate cart with products

Ecwid JavaScript API allows you to add items to customer's cart automatically. This can be useful when you are using custom storefront to provide any type of button to add products to cart.

Using the following steps you will be able to create links to your storefront with products in cart for your customers. So all they have to do is just clikc that promo link to go to your store and have products added to cart automatically.

Let's check out how this can be achieved: 

**Step 1: Add external files to your website**

> Script and CSS to load on your storefront page

```html
<link rel="stylesheet" type="text/css" href="https://djqizrxa6f10j.cloudfront.net/apps/ecwid-cart-app/cartapp.css">
<script src="https://djqizrxa6f10j.cloudfront.net/apps/ecwid-cart-app/1.0.0/cart.js"></script>
```

Add this code to the source code of the page, where your Ecwid storefront is displayed. 

<aside class="notice">
Make sure to add the script <strong>after or below</strong> Ecwid integration code.
</aside>

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

cart = encodeURIComponent(btoa(encodeURIComponent(JSON.stringify(cart))));

console.log(cart);

// prints the resulting string you need to use in step 3
//
// JTdCJTIyZ290b0NoZWNrb3V0JTIyJTNBdHJ1ZSUyQyUyMnByb2R1Y3RzJTIyJTNBJTVCJTdCJTIyaWQlMjIlM0E2NjgyMTE4MSUyQyUyMnF1YW50aXR5JTIyJTNBMyUyQyUyMm9wdGlvbnMlMjIlM0ElN0IlMjJDb2xvciUyMiUzQSUyMldoaXRlJTIyJTJDJTIyU2l6ZSUyMiUzQSUyMjExb3olMjIlN0QlN0QlMkMlN0IlMjJpZCUyMiUzQTY2NzIyNTgxJTJDJTIycXVhbnRpdHklMjIlM0E1JTdEJTVEJTJDJTIycHJvZmlsZSUyMiUzQSU3QiUyMmFkZHJlc3MlMjIlM0ElN0IlMjJuYW1lJTIyJTNBJTIyam9obiUyMHNtaXRoJTIyJTJDJTIyY29tcGFueU5hbWUlMjIlM0ElMjJnZW5lcmFsJTIwbW90b3JzJTIyJTJDJTIyc3RyZWV0JTIyJTNBJTIyNXRoJTIwQXZlJTIyJTJDJTIyY2l0eSUyMiUzQSUyMk5ldyUyMFlvcmslMjIlMkMlMjJjb3VudHJ5Q29kZSUyMiUzQSUyMlVTJTIyJTJDJTIycG9zdGFsQ29kZSUyMiUzQSUyMjEwMDAyJTIyJTJDJTIyc3RhdGVPclByb3ZpbmNlQ29kZSUyMiUzQSUyMk5ZJTIyJTJDJTIycGhvbmUlMjIlM0ElMjIlMkIxJTIwMjM0JTIwMjM1JTIwMjIlMjAxMiUyMiU3RCUyQyUyMmJpbGxpbmdBZGRyZXNzJTIyJTNBJTdCJTIyY291bnRyeUNvZGUlMjIlM0ElMjJVUyUyMiUyQyUyMnN0YXRlT3JQcm92aW5jZUNvZGUlMjIlM0ElMjJBTCUyMiU3RCUyQyUyMmVtYWlsJTIyJTNBJTIydGVzdCU0MHRlc3QuY29tJTIyJTJDJTIyb3JkZXJDb21tZW50cyUyMiUzQSUyMkNvbW1lbnRzISUyMiU3RCU3RA%3D%3D
//
```

In order for the script to add items to cart, we need to 'tell' it what items to add. Check the example on the right on how to do this. The `cart` variable will have the generated cart content in it. 

The syntax is very similar to adding products to cart via [Ecwid JavaScript API](https://developers.ecwid.com/api-documentation/manage-customer-cart#ecwid-cart-addproduct).

**Step 3: Create a link for customers**

> Final link structure

```
http://example.com/store#!/~/cart/create={generatedCartCode}
```

> Final link to generated cart example

```
https://www.ecwid.com/demo#!/~/cart/create=JTdCJTIyZ290b0NoZWNrb3V0JTIyJTNBdHJ1ZSUyQyUyMnByb2R1Y3RzJTIyJTNBJTVCJTdCJTIyaWQlMjIlM0E2NjgyMTE4MSUyQyUyMnF1YW50aXR5JTIyJTNBMyUyQyUyMm9wdGlvbnMlMjIlM0ElN0IlMjJDb2xvciUyMiUzQSUyMldoaXRlJTIyJTJDJTIyU2l6ZSUyMiUzQSUyMjExb3olMjIlN0QlN0QlMkMlN0IlMjJpZCUyMiUzQTY2NzIyNTgxJTJDJTIycXVhbnRpdHklMjIlM0E1JTdEJTVEJTJDJTIycHJvZmlsZSUyMiUzQSU3QiUyMmFkZHJlc3MlMjIlM0ElN0IlMjJuYW1lJTIyJTNBJTIyam9obiUyMHNtaXRoJTIyJTJDJTIyY29tcGFueU5hbWUlMjIlM0ElMjJnZW5lcmFsJTIwbW90b3JzJTIyJTJDJTIyc3RyZWV0JTIyJTNBJTIyNXRoJTIwQXZlJTIyJTJDJTIyY2l0eSUyMiUzQSUyMk5ldyUyMFlvcmslMjIlMkMlMjJjb3VudHJ5Q29kZSUyMiUzQSUyMlVTJTIyJTJDJTIycG9zdGFsQ29kZSUyMiUzQSUyMjEwMDAyJTIyJTJDJTIyc3RhdGVPclByb3ZpbmNlQ29kZSUyMiUzQSUyMk5ZJTIyJTJDJTIycGhvbmUlMjIlM0ElMjIlMkIxJTIwMjM0JTIwMjM1JTIwMjIlMjAxMiUyMiU3RCUyQyUyMmJpbGxpbmdBZGRyZXNzJTIyJTNBJTdCJTIyY291bnRyeUNvZGUlMjIlM0ElMjJVUyUyMiUyQyUyMnN0YXRlT3JQcm92aW5jZUNvZGUlMjIlM0ElMjJBTCUyMiU3RCUyQyUyMmVtYWlsJTIyJTNBJTIydGVzdCU0MHRlc3QuY29tJTIyJTJDJTIyb3JkZXJDb21tZW50cyUyMiUzQSUyMkNvbW1lbnRzISUyMiU3RCU3RA%3D%3D
```

Now we need to fill in customer's cart when your custom link is opened. 

First things first, let's determine where your Ecwid store is displayed and get a direct link to that page. For example, our demo store is located in: `https://www.ecwid.com/demo`

And to fill in customer's cart with items that you seleted earlier, we need to create a link to your storefront with the generated cart part. 

**Example link**

Generated link with products added automatically to cart for Ecwid demo store will be: 

`https://www.ecwid.com/demo#!/~/cart/create=JTdCJTIyZ290b0NoZWNrb3V0JTIyJTNBdHJ1ZSUyQyUyMnByb2R1Y3RzJTIyJTNBJTVCJTdCJTIyaWQlMjIlM0E2NjgyMTE4MSUyQyUyMnF1YW50aXR5JTIyJTNBMyUyQyUyMm9wdGlvbnMlMjIlM0ElN0IlMjJDb2xvciUyMiUzQSUyMldoaXRlJTIyJTJDJTIyU2l6ZSUyMiUzQSUyMjExb3olMjIlN0QlN0QlMkMlN0IlMjJpZCUyMiUzQTY2NzIyNTgxJTJDJTIycXVhbnRpdHklMjIlM0E1JTdEJTVEJTJDJTIycHJvZmlsZSUyMiUzQSU3QiUyMmFkZHJlc3MlMjIlM0ElN0IlMjJuYW1lJTIyJTNBJTIyam9obiUyMHNtaXRoJTIyJTJDJTIyY29tcGFueU5hbWUlMjIlM0ElMjJnZW5lcmFsJTIwbW90b3JzJTIyJTJDJTIyc3RyZWV0JTIyJTNBJTIyNXRoJTIwQXZlJTIyJTJDJTIyY2l0eSUyMiUzQSUyMk5ldyUyMFlvcmslMjIlMkMlMjJjb3VudHJ5Q29kZSUyMiUzQSUyMlVTJTIyJTJDJTIycG9zdGFsQ29kZSUyMiUzQSUyMjEwMDAyJTIyJTJDJTIyc3RhdGVPclByb3ZpbmNlQ29kZSUyMiUzQSUyMk5ZJTIyJTJDJTIycGhvbmUlMjIlM0ElMjIlMkIxJTIwMjM0JTIwMjM1JTIwMjIlMjAxMiUyMiU3RCUyQyUyMmJpbGxpbmdBZGRyZXNzJTIyJTNBJTdCJTIyY291bnRyeUNvZGUlMjIlM0ElMjJVUyUyMiUyQyUyMnN0YXRlT3JQcm92aW5jZUNvZGUlMjIlM0ElMjJBTCUyMiU3RCUyQyUyMmVtYWlsJTIyJTNBJTIydGVzdCU0MHRlc3QuY29tJTIyJTJDJTIyb3JkZXJDb21tZW50cyUyMiUzQSUyMkNvbW1lbnRzISUyMiU3RCU3RA%3D%3D`

<aside class='notice'>
Please note that this is an example link and it will not work in Ecwid's demo store. Please test this feature in your own website.
</aside>

## Get cart details

Find out more about cart in its current state.

### Order Object

> Get order total example

```js
Ecwid.Cart.calculateTotal(function(order) {
  console.log(order.total);
});

// prints
// 13.25
```

Order object represents details of current customer's order.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

**Fields:**

Name | Type | Description
---- | ---- | -----------
cart | \<*Cart*\> | The object describing the state or customer's cart
couponDiscount | integer | An absolute amount of coupon code discount for this order
customerGroupDiscount | integer | An absolute amount of a discount based on customer group for this order
customerGroupVolumeDiscount | integer | An absolute amount of a discount based on customer group and subtotal for this order
discount | integer | An absolute amount of a total discount for this order
handlingFee | integer | An absolute amount of handling fee applied to order
shipping | integer | An absolute amount of shipping rate applied to order
tax | integer | An absolute amount of tax rate applied to order
total | integer | Total amount of this order
volumeDiscount | integer | An absolute amount of a discount based on subtotal for this order

### Cart Object

Cart object is a snapshot of essential shopping cart properties, passed via various callbacks. 

Cart object does not provide direct memory access to the actual cart that Ecwid uses — i.e. changing this exact object will not alter the actual cart Ecwid uses for placing the order.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

> Get full cart details

```js
Ecwid.Cart.get(function(cart){
  console.log(cart);
});
```

> Get quantity of products in cart example

```js
Ecwid.OnCartChanged.add(function(cart) {
  console.log("Products in cart now: " + cart.productsQuantity);
});

// prints
// Products in cart now: 1
```

**Fields:**

Name | Type | Description
---- | ---- | -----------
items | Array\<*CartItem*\> | Enlists all items currently present in customer’s cart
productsQuantity | integer | Total number of product varieties in cart
orderId | integer | Unique internal order ID for this order (available after order is created)
couponName | string | The name of the coupon (if any) applied to the cart. If no coupon was applied, will contain undefined. Does not contain the actual code of coupon, just the name.
weight | number | Total weight of the items in cart
shippingMethod | string | The name of the selected shipping method (if any)
cartId | string | Cart ID you can use later in the [cart endpoint](https://developers.ecwid.com/api-documentation/carts) of Ecwid REST API
shippingPerson | \<*Person*\> | Customer's shipping address

**Person** fields:

Name | Type | Description
-----|------|------------
name | string | Customer's name
companyName | string | Customer's company name
street | string | Customer's street address. Use `\n` to place text in Address Line 2
city | string | Customer's city
countryName | string | Customer's country name. `countryCode` can be used instead
countryCode | string | Customer's country code. `countryName` can be used instead
postalCode | string | Customer's zip code
stateOrProvinceCode | string | Customer's state or provice code
phone | string | Customer's phone number

### CartItem Object

CartItem represents a single item (product variety) in cart.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

> Get quantity of a product in cart example

```js
Ecwid.OnCartChanged.add(function(cart) {
  console.log("There are " + cart.items[0].quantity + " items of " + cart.items[0].product.name + " in cart now.");
});

// prints
// There are 1 items of Apple product in cart now.
```

**Fields:**

Name | Type | Description
---- | ---- | -----------
quantity | integer | Quantity of the given product variety in cart
product | Array\<*Product*\> | The map of product properties (variation properties, if the variation is added to cart)
options | Object with option names and values | Map of the product options (option name as a key and option value as a value). For listboxes and radio buttons value will be the string value of the selected option. For checkboxes — names of the selected options, comma separated. For date options — string representing the selected date according to the shop’s format (Ecwid control panel > System settings > General > Formats and Units). For textboxes и textareas — the text given by the customer. For file upload options — string in the form of „4 files”

### Product Object

Product object represents details of a specific product in cart.

Subscribe to the `Ecwid.OnAPILoaded` [JS API event](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onapiloaded) to ensure availability of this function.

> Get product name and SKU in cart example

```js
Ecwid.OnCartChanged.add(function(cart) {
  console.log(cart.items[0].product.name + " in the cart has SKU: " + cart.items[0].product.sku);
});

// prints
// Apple product in the cart has SKU: TEST-1234
```

**Fields:**

Name | Type | Description
---- | ---- | -----------
id | integer | Internal unique product ID
name | itring | Product name
price | integer | Product price
shortDescription | string | Product description truncated to 120 characters
sku | string | Product SKU
url | string | URL to this product details page in store front (store front URL is generated from a field in Ecwid Control panel > Settings > General > Store profile)
weight | integer | Weight of a product

## JS API examples

Redirect Ecwid Paypal Orders to Custom “Thank You” Page:
[http://stevestruemph.com/redirect-ecwid-paypal-orders-to-custom-thank-you-page/](http://stevestruemph.com/redirect-ecwid-paypal-orders-to-custom-thank-you-page/)
