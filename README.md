ecwid-api-docs
==============

Ecwid REST API documentation ([https://developers.ecwid.com/api-documentation](https://developers.ecwid.com/api-documentation))

The docs use Markdown syntax. Syntax reference: [Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax)


# Changelog

## March 30, 2018

Added feature to open specific product variation in storefront via query parameter in URL. 

You can use either variation ID or selection index in drop down or radio button product options. [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#open-specific-product-variation)

## March 26, 2018

Updated version of Ecwid CSS Framework – `1.3.1`: 

> https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.1/ecwid-app-ui.css

Changelog: 
- fixes for some colors
- updated fonts for custom-checkbox
- added prefixes for components `fieldsets-batch`, `cta-block`, `iconable-block`, `status-block`
- added disabled state for radio buttons
- added styles for `draggable-item`, `settings-area` components
- fixed paddings for `settings-page`
- fixed display of long titles for `titled-value`
- fixed display of buttons for `FeatureCard`

## March 13, 2018

Added customer selection of product options in order item details. Now you can know how each option influences the price of a purchased product. [Learn more](https://developers.ecwid.com/api-documentation/orders)

## March 7, 2018

1) Added enable/disable controls for "ON SALE" and "SOLD OUT" labels in product listing. 

If enabled, "ON SALE" label will display on all products with "compare to" price set. If enabled, "SOLD OUT" label will display on all products that are out of stock. 

Only one label can be shown on a product at a time. "SOLD OUT" label has priority over "ON SALE" label, meaning if both labels apply to a product and are enabled, Ecwid will only show "SOLD OUT" label.

[Learn more](https://developers.ecwid.com/api-documentation/customize-appearance#product-listing)

2) Updated Ecwid JS SDK for Native Apps to version `1.2.5`: 
`https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.2.5/ecwid-app.js`

**Changelog:**
- Added `EcwidApp.getAppPublicConfig()` function to Ecwid JS SDK. Now you can read the value of public application config easier. [Learn more](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#getapppublicconfig)

## March 6, 2018

Added information about enabled or disabled new features of a store and their visibility status in `featureToggles` object in `/profile` endpoint. [Learn more](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)

## March 5, 2018

Added ability to get all store shipping options via Ecwid REST API in **read-only mode**. If accessed via public token, Ecwid will respond with enabled shipping options only. [Learn more](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)

## March 2, 2018

Updated Ecwid CSS Framework and Sample Native App to use the latest improvements and changes to design of pages in Ecwid Control Panel. 

New CSS Framework version is 1.3.0: [https://developers.ecwid.com/api-documentation/ecwid-css-framework](https://developers.ecwid.com/api-documentation/ecwid-css-framework)

[Sample Native App](https://github.com/Ecwid/sample-native-app) has been improved to utilize vertical page structure and cards for various settings and stats.

## February 28, 2018

Added `defaultDisplayedPrice` field to `/products` endpoint of Ecwid REST API. 

It provides default price of a product that is displayed in storefront. It includes product combinations, product options as well as taxes applied to products. [Learn more](https://developers.ecwid.com/api-documentation/products)

## February 26, 2018

Added store information updated webhook event. [Learn more](https://developers.ecwid.com/api-documentation/webhooks-overview#supported-events)

## February 22, 2018

- Added ability to get all set up payment methods of a store via Ecwid REST API in **read-only mode**. If accessed via public token, Ecwid will respond with enabled payment methods only. [Learn more](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)

- Added options to customize product details pages with Ecwid JS API. [Learn more](https://developers.ecwid.com/api-documentation/customize-appearance#product-pages)

## January 29, 2018

- Added example of updating order status in PHP and cURL for payment applications: [https://developers.ecwid.com/api-documentation/processing-payment-request#updating-order-status](https://developers.ecwid.com/api-documentation/processing-payment-request#updating-order-status)

- Added `entryPage` and `hasPrevious` fields to `page` object in the `Ecwid.OnPageLoad(ed)` event. [Learn more](https://developers.ecwid.com/api-documentation/get-storefront-details#get-opened-page-info)

## January 8, 2018

Added an example for creating new elements for each product block in category pages (new product listing) in storefront. [Learn more](https://developers.ecwid.com/api-documentation/look-and-design#change-the-store-layout#add-new-element-to-products-in-category-pages)

## December 26, 2017

Added `taxOnShipping` field to [orders](https://developers.ecwid.com/api-documentation/orders), [abandoned carts](https://developers.ecwid.com/api-documentation/carts) and [calculation of order details](https://developers.ecwid.com/api-documentation/carts#calculate-order-details) endpoints in Ecwid REST API.

Now you can know how much tax total was applied to shipping charge, only if tax is applied to both subtotal and shipping. 

The values apply 'as is', meaning if an order is changed manually in the future, they will not be automatically recalculated. 

## December 18, 2017

Added `couponAmount` and `discounts` for order and abandoned cart items. Now you can know how much discount or discount coupon amount was applied to a specific item. 

The values apply as is, meaning if an order is changed manually in the future, they will not be automatically recalculated. 

## November 7, 2017

- Added `EcwidApp.sendUserToUpgrade()` function to Ecwid JavaScript SDK for Native applications. Now you are able to send a user to upgrade to a higher Ecwid plan specifying only the target feature needed for your app. [Learn more](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#sendusertoupgrade)

To use it, make sure you are loading the latest version of Ecwid JS SDK: 
`https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.2.4/ecwid-app.js`

## November 1, 2017

- Added legal pages support in the Ecwid REST API. Now you can read and modify legal pages information on merchant's behalf. [Learn more](https://developers.ecwid.com/api-documentation/store-information)

## October 31, 2017

- Added category-related events to webhooks functionality. Now you can subscribe to events like: `category.created`, `category.updated` and `category.deleted`. [Learn more](https://developers.ecwid.com/api-documentation/webhooks)

## October 18, 2017

- We extended information provided from Ecwid for Custom [Shipping](https://developers.ecwid.com/api-documentation/add-shipping-method) and [Discount](https://developers.ecwid.com/api-documentation/add-custom-discount) requests to 3rd party applications. Now apps get more information about order items, selected payment method, customer email, handling fee and extra fields. 

## October 16, 2017

- Added a way to open any page in storefront from within your application – `Ecwid.openPage`. Now you can open any custom page in storefront for your users on demand. [Learn more](https://developers.ecwid.com/api-documentation/open-page-in-storefront)
- Added a way to dynamically set storefront base URL for SEO-friendly URLs – `Ecwid.setStorefrontBaseUrl`. It can be useful when your solution updates query parameters of a page URL dynamically and you need to preserve them. [Learn more](https://developers.ecwid.com/api-documentation/get-storefront-details#ecwid-setstorefrontbaseurl)
- Added `pickupTime` field to order details in Ecwid REST API. Now you can know the order pickup time customer selected at checkout after order was placed or change / set it when updating / creating new orders. [Learn more](https://developers.ecwid.com/api-documentation/orders)

## September 27, 2017

- Added new function to Ecwid JS API in storefront: `Ecwid.getFeatureToggles()`. It returns enabled or disabled new storefront features, like new product listing (category pages with products). [Learn more](https://developers.ecwid.com/api-documentation/get-storefront-details#ecwid-getfeaturetoggles)

## September 21, 2017

- Product Combinations are now renamed to **Product Variations**. All the product combinations-related labels in the Ecwid Control Panel, Help Center and API Documentation have been renamed to product variations. However, **all API endpoints and entities still refer to combinations** so that apps continue working as expected. 

- Product dimensions (`productDimensions`) are now referred to as `dimensions` in the Ecwid REST API endpoints, other dimensions-related tools as well as in the API documentation.

## September 15, 2017

- New version of Ecwid CSS Framework: 1.2.5. It includes a fix for the drop downs display in Firefox. [Get the latest CSS Framework](https://developers.ecwid.com/api-documentation/ecwid-css-framework)

## September 12, 2017

- Added support for full and partial refunds for [Ecwid Payments](https://support.ecwid.com/hc/en-us/articles/211954289-Ecwid-Payments-US-Canada-and-UK-). Refund information is available in read-only mode [in order details in REST API](https://developers.ecwid.com/api-documentation/orders). 

## September 6, 2017

- Added **order extra fields** feature to store extra information in orders from storefront. Order extra fields can save hidden data to an order from storefront (like cookies, referrer, etc.), as well as customer input to new input fields (text, drop down, date picker) added to store checkout by your app. [Learn more](https://developers.ecwid.com/api-documentation/order-extra-fields)

## August 25, 2017

- Lots of additions for customer-related information in API: 

1. new sorting and filtering options when [searching for customers](https://developers.ecwid.com/api-documentation/customers#search-customers)
2. new `updated` field when searching and getting customer details
3. all customer fields are now returned when [searching for customers](https://developers.ecwid.com/api-documentation/customers#search-customers)
4. `password` field is no longer required when [creating a new customer](https://developers.ecwid.com/api-documentation/customers#create-customer)
5. customer latest update date is now available in the [latest stats endpoint](https://developers.ecwid.com/api-documentation/store-information#get-store-update-statistics)
6. added [webhooks](https://developers.ecwid.com/api-documentation/webhooks) for creating, updating, deleting customer with `email` field as part of `data`

## August 22, 2017
- `update_store_profile` access scope is no longer required to use the [Application Storage](https://developers.ecwid.com/api-documentation/application-storage) features. If your app has the `read_store_profile` scope (all apps have it), you can use the application storage in your app.

## July 20, 2017

- We created a sample native application code for developers to get started with native apps easily. It has some basic elements to display the app as a part of the Ecwid Control Panel and tools to access the Ecwid API. [Check it out here](https://github.com/Ecwid/sample-native-app)

## July 14, 2017

- Added `checkLowStockNotification` parameter to [update product](https://developers.ecwid.com/api-documentation/products#update-a-product), [adjust product inventory](https://developers.ecwid.com/api-documentation/products#adjust-product-inventory), [update combination](https://developers.ecwid.com/api-documentation/product-combinations#update-product-combination), [adjust product combination inventory](https://developers.ecwid.com/api-documentation/product-combinations#adjust-combination-inventory) requests. If you send `checkLowStockNotification=true` in your request, Ecwid will check whether the low stock notification needs to be sent to a merchant. This will help store owners keep track of their inventory when using applications and API integrations. 

## July 7, 2017

- Added `Ecwid.resizeProductBrowser()` function to adapt Ecwid storefront layout to changes in parent container's width. [Learn more](https://developers.ecwid.com/api-documentation/look-and-design#change-the-store-layout)

## June 20, 2017

- Added controls to hide certain storefront elements, like 'In stock' label, 'Sign in' link and others with JavaScript code. It needs to be placed anywhere on a page with Ecwid integration code. [Learn more](https://developers.ecwid.com/api-documentation/look-and-feel#show-or-hide-storefront-elements)

## June 9, 2017

- Added query-based clean URLs support for Ecwid storefronts to improve SEO. This is a solution for the store owners who wanted to enable the SEO-friendly URLs, but can't access the server rewrite rules to change them. [Learn more](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls)

## June 5, 2017

- Added account `email` field to the request for access token in the OAuth process. Now you can know the store owner's account email right when you request access token from Ecwid after app installation. [Learn more](https://developers.ecwid.com/api-documentation/external-applications)

## May 30, 2017

- Added ability to upload files via a public URL as a request parameter to Ecwid REST API. Now you can just provide a link to an image or a file in optional `externalUrl` parameter and Ecwid will download it from there. Previous functionality of uploading files as a binary data is still available.

This change is available for all file upload endpoints: 

* Upload store logo (https://app.ecwid.com/api/v3/{storeId}/profile/logo)
* Upload store invoice (https://app.ecwid.com/api/v3/{storeId}/profile/invoicelogo)
* Upload product image (https://app.ecwid.com/api/v3/{storeId}/products/{productId}/image)
* Upload product gallery image (https://app.ecwid.com/api/v3/{storeId}/products/{productId}/gallery)
* Upload product file (https://app.ecwid.com/api/v3/{storeId}/products/{productId}/files)
* Upload category image (https://app.ecwid.com/api/v3/{storeId}/categories/{categoryId}/image)
* Upload product combination image (https://app.ecwid.com/api/v3/{storeId}/products/{productId}/combinations/{combinationId}/image)
* Upload order item option file (https://app.ecwid.com/api/v3/{storeId}/orders/{orderNumber}/items/{itemId}/options/{optionName})
* Upload starter site cover image (https://app.ecwid.com/api/v3/{storeId}/startersite/cover)
* Upload starter site owner portrait (https://app.ecwid.com/api/v3/{storeId}/startersite/ownerportrait)
* Upload starter site quote person image (https://app.ecwid.com/api/v3/{storeId}/startersite/quoteperson)

## May 12, 2017

- Updates to Ecwid JS SDK and Ecwid CSS Framework. 

New version links: 

* [https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.2.4/ecwid-app-ui.css](https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.2.4/ecwid-app-ui.css)
* [https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.2.3/ecwid-app.js](https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/js/1.2.3/ecwid-app.js)

Changes to *JS SDK*: Added `EcwidApp.closeAppPopup()` function. Apps displayed in a popup can close it using this function

Changes to *CSS Framework*: Fixed issue with invisible option elements of a \<select\> increasing the height of an app in Firefox

## April 25, 2017
- We updated the installation flow and now it has only one option - web application. Earlier there was two options: web application and app installed on a device. 

**With this change all existing apps continue to work the same way**. However, for the new applications installed on a device we recommend using the Deep linking functionality for [getting the access token](https://developers.ecwid.com/api-documentation/external-applications#complete-oauth-flow) (as a `redirect_uri`)

- Added examples on how to enable the new SEO-friendly URL format for Ecwid stores. [Learn more](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls)

- Added a way to get not hidden abandoned sales in an Ecwid store using the `showHidden` filter when searching for abandoned sales. [Learn more](https://developers.ecwid.com/api-documentation/carts)

## March 31, 2017
- **NEW** Added new REST API endpoints for working with abandoned carts in Ecwid and the date of the latest changes to them in [latest-stats](https://developers.ecwid.com/api-documentation/store-information#get-store-update-statistics) endpoint. [Learn more](https://developers.ecwid.com/api-documentation/carts)

## March 28, 2017
- **NEW** We added an option to enable canonical URLs for Ecwid stores in storefront. To enable them, you can add a couple of lines of JavaScript code to the page where the Ecwid store is located. [Learn more](https://developers.ecwid.com/api-documentation/seo#canonical-urls)

## March 23, 2017
- We added an option to get order details of multiple specific orders if you know their order number. To do that, use the [search orders endpoint](https://developers.ecwid.com/api-documentation/orders#search-orders) with the `orderNumber` filer - that's where you will need to specify your order numbers separated by a comma.

## March 21, 2017
- We updated the `priceInProductList` field for [products endpoint](https://developers.ecwid.com/api-documentation/products). Now it will take into account the default product options when returning the value as well as the combinations. This way, this field is now returning the price as it is displayed in the storefront (without taxes applied).

## February 16, 2017
- **NEW** Added `showOnFrontpage` field to [products endpoint](https://developers.ecwid.com/api-documentation/products) in the Ecwid REST API. Now when you search/get/update/create products, you can control whether that product will be shown on the store front page.

## February 10, 2017
- Updated [discount coupons endpoints](https://developers.ecwid.com/api-documentation/discount-coupons) in the Ecwid REST API. 

Now you can get/update/delete a discount coupon using either its internal `id` (new) or coupon `code` itself. The `id` field is now returned when getting coupon(s) details or creating it. Also, you can filter coupons by creation/update dates when getting their details. The latest stats endpoint now also returns latest update date of discount coupons. The `discount_coupons/deleted` endpoint now returns unique coupon `id` instead of the coupon `code` field.

## January 18, 2017
- Added support for SEO-friendly URLs for Ecwid storefronts. [Learn more](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls)
- SEO-friendly URLs can be retrieved from Ecwid REST API for [products](https://developers.ecwid.com/api-documentation/products) and [categories](https://developers.ecwid.com/api-documentation/categories) URLs in URL field using `cleanUrls` request parameter. Also, it's possible to change the base storefront URL for such URLs using `baseUrl` request parameter.  

## January 17, 2017
- **NEW** Added `Ecwid.OnOrderPlaced()` event for Ecwid JavaScript API. It allows to get order details in storefront right after it's placed by customer. [Learn more](https://developers.ecwid.com/api-documentation/subscribe-to-events#ecwid-onorderplaced)

## January 16, 2017
- **NEW** You can now get `public_token` value in a native app interface by using `EcwidApp.getPayload()` or decrypting payload on server (for enhanced security authentication apps). [Learn more](https://developers.ecwid.com/api-documentation/ecwid-javascript-sdk#getpayload)
- To keep the digital goods of our merchants protected, we removed access token from `adminUrl` field when [getting product details](https://developers.ecwid.com/api-documentation/products#get-a-product). To download a file, just add the token parameter with the token as a value: `?token=abcde1234`

## January 5, 2017
- **NEW** Added support for product dimensions feature. Now each product and shipping itself can be calculated according to the product dimensions specified for products.

## December 27, 2016

- **NEW** Added support for passing custom application state for Native applications. You can use this to initiate a custom action performed when loading the application tab in Ecwid Control Panel. [Learn more](https://developers.ecwid.com/api-documentation#user-authentication)

## December 26, 2016
- **NEW** Added `READY_FOR_PICKUP` fulfillment status for orders and pickup fields support for selected shipping options in order. [Learn more](https://developers.ecwid.com/api-documentation#orders)

## December 8, 2016
- **NEW** Added Custom Promo API to apply discounts to orders when customer is at checkout. [Learn more](https://developers.ecwid.com/api-documentation#add-custom-discount)

## December 1, 2016
- **NEW** Added new order payment status: Partially Refunded. [Learn more](https://developers.ecwid.com/api-documentation#orders)
- **NEW** Added support for tax exempt customers in the Ecwid REST API. [Learn more](https://support.ecwid.com/hc/en-us/articles/213823045-How-to-handle-tax-exempt-customers-in-Ecwid)

## November 15, 2016
- **NEW** Added ability to get product details using multiple product IDs separated by a comma. [Learn more](https://developers.ecwid.com/api-documentation#search-products)
- **NEW** Added latest category update date to store update stats. [Learn more](https://developers.ecwid.com/api-documentation#get-store-update-statistics)

## November 4, 2016
- **NEW** Added more ways to contact store for the new Starter site: Whatsapp, Viber, Telegram and more. [Learn more](https://developers.ecwid.com/api-documentation#starter-site)

## October 27, 2016
- **NEW** Added new REST API endpoint for getting up-to-date HTML invoice of a specific order. [Learn more](https://developers.ecwid.com/api-documentation#get-order-invoice)

## October 26, 2016
- **NEW** Added `hideOutOfStockProductsInStorefront` field to control the visibility of out of stock products in storefront. [Learn more](https://developers.ecwid.com/api-documentation#store-information)

## October 17, 2016
- **NEW** Added support for custom HTTP headers when using webhooks. [Learn more](https://developers.ecwid.com/api-documentation#custom-http-headers)
- Added Python examples for file upload methods

## August 23, 2016
- **NEW** Added methods to change colors and font of a storefront. [Learn more](https://developers.ecwid.com/api-documentation#colors-and-fonts)

## August 16,2016
- **NEW** Added methods to get and update starter site content details. [Learn more](https://developers.ecwid.com/api-documentation#starter-site)

## August 15, 2016
- **NEW** Added new JavaScript API functions to set various customer details in storefront. [Learn more](https://developers.ecwid.com/api-documentation#manage-customer-39-s-cart)
- Improved 'Generate cart with products' method with new fields, like customer address, customer email and order comments. [Learn more](https://developers.ecwid.com/api-documentation#generate-cart-with-products)

## July 26, 2016
- **New** Added Javascript API Method for getting public token in storefront. [Learn more](https://developers.ecwid.com/api-documentation#ecwid-getapppublictoken)
- Public tokens can now create orders. [Learn more](https://developers.ecwid.com/api-documentation#access-tokens)
- Added webhooks for application changes (install/uninstall, status change). [Learn more](https://developers.ecwid.com/api-documentation#webhooks)

## July 14, 2016
- **NEW** added public access tokens support. It allows to get public information of a store from anywhere with Ecwid REST API. [Learn more](https://developers.ecwid.com/api-documentation#access-tokens)

## July 8, 2016
- **NEW** added `Ecwid.Cart.gotoCheckout()` function to start checkout process for a customer in a storefront. [Learn more](https://developers.ecwid.com/api-documentation#ecwid-cart-gotocheckout)

## July 5, 2016
- Added `hdThumbnailUrl` for products, categories and combinations. This field provides URL to a resized to fit 650x650px image of an entity.

## June 30, 2016
- Added start and end subscription timestamps for application subscription status. [Learn more](https://developers.ecwid.com/api-documentation#get-application-status)

## June 27, 2016
- Added instructions for generating custom customer's cart using a link. [Learn more](https://developers.ecwid.com/api-documentation#generate-cart-with-products)
- Added instruction for centering popup windows in Ecwid storefronts and Control panels, working in an iframe containers. [Learn more](https://developers.ecwid.com/api-documentation#centering-popups-in-iframe-storefronts)

## June 23, 2016
- Added JS method to remove specific products from customer's cart. [Learn more](https://developers.ecwid.com/api-documentation#ecwid-cart-removeproduct)

##June 21, 2016
- Added SEO title and description fields for [each product](https://developers.ecwid.com/api-documentation#products). Now you can control how products are presented in search engine results.

##June 15, 2016
- New version of Ecwid JS SDK: 1.2.1. In this version we fixed height issues with `fixed` positioned elements.

## June 14, 2016
- App public config now can store up to 64Kb of data. [Learn more](https://developers.ecwid.com/api-documentation#public-application-config)

## June 2, 2016
- **NEW** Single Sign On feature now works via Ecwid APIv3 credentials. While old implementations of SSO are still functional, we encourage you to update to the latest specifications. [Learn more](https://developers.ecwid.com/api-documentation#single-sign-on)

## May 30, 2016
- New feature: private admin notes for orders. See orders endpoint for `privateAdminNotes` field

## May 26, 2016
- Added new fields describing order details when sending request for a custom shipping methods.

## May 24, 2016
- Added fields to control manual/automatic tax calculation. To manage taxes, use `taxSettings` field

## May 23, 2016
- New version of Ecwid JS SDK: 1.2.0. Changelog includes a fix for the issues with page height when `autoheight: true` was specified.
- Added `Ecwid.getStorefrontLang()` function to get the language of the storefront as soon as it starts loading.

## May 20, 2016
- Added a new sort type to search for products: *UPDATED_TIME_ASC* and *UPDATED_TIME_DESC*
- Added product and category image details, see `originalImage` field

## May 12, 2016
- Added support to control Order Comments functionality in the store profile section

## May 4, 2016
- Added endpoint for creating Ecwid stores and checking if store exists

## April 27, 2016
- **NEW** Added Custom Payment API to add new payment methods for Ecwid stores

## March 23, 2016
- New minor version 1.1.1 of Ecwid Javascript SDK is out. It includes a fix for using cyrillic characters in the Application storage.

##March 19, 2016
- Added small example for using jQuery library when customizing Ecwid Storefront

## March 18, 2016
- **NEW** Added Custom Shipping API to add new shipping methods for Ecwid stores

## February 25, 2016
- **NEW** Added endpoint for calculating order details on demand for custom checkouts
- **NEW** Added SKU and Product ID filters to ’Search Products’ endpoint

## February 18, 2016
- 'Search products' method will now return all available product fields

## February 16, 2016
- Updated Storefront JS API section

## February 8, 2016
- Updated wording of Authentication section
- Added new content to Overview section

## January 18, 2016
- Added PHP examples for uploading binary data (images)

## January 15, 2016
- Added TRIAL status for Application endpoint
- Updates to the structure of Authentication section
- Added 'isShippingRequired' field for Products endpoint
- Added support for Google Analytics in Store profile endpoint
- Updated structure of Storefront JS API
- New Storefront JS API function: getInitializedWidgets()

## December 14, 2015
- Added unfinished order event types for webhooks
- Added order status changes details for webhooks

## December 2, 2015
- **NEW** Enhanced application storage with easy access from JS APIs. Use this to store, manage and access your users preferences and other app data
- **NEW** Customer groups (membserhips) can now be managed over the REST API. Need an idea on how to use that in an application? Here is one: track customer sales and automatically provide them with a membership based on the money they have spent in your store.

## November 19, 2015
- Moved JS API documentation from Ecwid Help Center to api.ecwid.com

## November 17, 2015
- Added "Application" endpoint

## November 3, 2015
- Added a PHP code example to the webhooks documentation
- New minor version of Ecwid JS SDK is released (v 1.0.2). It contains a fix for the layout issues of embedded apps in some browsers.

## September 22, 2015
- Updated description text for adjustProductInventory and adjustCombinationInventory
- Updated text for compareToPrice field description
- Added webhooks documentation

## September 14, 2015
- New minor version of Ecwid JS SDK is released (v 1.0.1). It contains a fix for the bug that caused layout issues in embedded apps (glitchy footer)

## September 5, 2015
- Updated discountType field. Added mixed type of coupon (absolute discount AND free shipping, percentage discount AND free shipping)

## August 26, 2015
- Added tax details to order items (taxOnDiscountedSubtotal and taxOnShipping fields)

## August 13, 2015
- Added Single Sign On code examples

## July 27, 2015
- Added a tip on how to search products for exact match using a keyword in quotes, e.g. `"keyword"`. This is useful, for example, to get a product by SKU
- Added `emailLogoUrl` and `availableFeatures` fields description in the Store profile endpoint section

## July 25, 2015
- Added two code examples to the Library (A simple Ecwid oAuth client on PHP and a Laravel app template)

## July 8, 2015
- Added description of how to use Ecwid oAuth in self hosted web apps, e.g. CMS plugins.
- Introduced hosted app details pages in Ecwid Control Panel.

## June 14, 2015
- Added handling fee options to the "Store profile" and "Orders" endpoints
- Improved behavior of hidden product attributes in responses from "Products" endpoints

## June 7, 2015
- All dates are returned in UTC now
- Timestamp is now returned for orders and products
- Options are now automatically created when you create a combination
- Added a short usage policy text

## May 5, 2015
- Added description of how to define date and time ranges in the "Search products" and "Search orders filters (http://api.ecwid.com/#search-products)
- Fixed an error in the "Update product" endpoint spec: to update UPC and Brand product attributes, it's possible to use the 'alias' field in the AttributeValue object.

## April 27, 2015
- Added ability to add embedded applications to the "Settings" and "Design" tabs (http://api.ecwid.com/#embedded-apps)

## April 3, 2015
- Added Ecwid CSS Framework documentation (http://api.ecwid.com/ecwid-css-framework/)
- Added Ecwid JS SDK documentation

## April 2, 2015
- Added ability to get the user language in embedded applications ("lang" parameter in the payload)
- Added a link to the new Developers site (http://developers.ecwid.com)

## March 2, 2015
- Added troubleshooting section to the "Embedding apps" section

## March 2, 2015
- Added the "API availability on Ecwid plans" paragraph to the Overview section to explain which merchants are allowed to use the API 
- Added clarifications to the access scopes list in the Authentication section

## February 19, 2015
- Added PHP example of Ecwid payload decryption in embedded apps

## February 16, 2015
- Added mailNotifications field in the store settings. Now you can get and update the mail notifications settings over API
- Added Legacy API links in the Overview section
- The default items limit in the products search response is changed from 10 to 100

## January 15, 2015
- Added Single Sign-on API description

## January 7, 2015
- Added static links for CSS SDK and JS SDK

## December 30, 2014
- Added "Update product file description" endpoint

## December 11, 2014
- Categories requests/response now supports batch requests parameters (limit/offset/total/count)
- Search products now supports ADDED_TIME_ASC sort order
- Upload image requests now respond with 422 error when the uploaded file does not seem to be an image
- "Get categories" request didn’t return category product IDs. Fixed.

## December 2, 2014
- Added "Embedded applications" specification
