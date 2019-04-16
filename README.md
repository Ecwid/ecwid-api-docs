ecwid-api-docs
==============

Ecwid REST API documentation ([https://developers.ecwid.com/api-documentation](https://developers.ecwid.com/api-documentation))

The docs use Markdown syntax. Syntax reference: [Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax)


# Changelog

## April 16, 2019

1) Added support to disable email notifications to customers for specific orders in REST API. See the `disableAllCustomerNotifications` field in [orders endpoint](https://developers.ecwid.com/api-documentation/orders).

2) Added support for multilanguage catalogs. When getting and updating store catalog, you can work with translated versions of these fields: 

- product names 
- product descriptions
- product option names
- product option values (choices)
- category name
- category description

The old fields remain the same, but the new ones have `Translated` suffix. I.e. `titleTranslated`, `descriptionTranslated` and so on. [Learn more](https://developers.ecwid.com/api-documentation/rest-api-reference)

## April 12, 2019

1) Added support for several structured data fields for [static product pages](https://developers.ecwid.com/api-documentation/static-store-pages#get-product-page): 

- Brand
- MPN
- SKU
- Offer → URL
- Offer → ItemCondition
- Offer → Seller

2) Added field for tracking referals for each order – `refererId`. 

It is available for [REST API](https://developers.ecwid.com/api-documentation/orders), CSV order export, email notifications and invoices, and [in storefront](https://developers.ecwid.com/api-documentation/manage-customer-cart#set-order-referrer). 

## Mar 22, 2019

- Added `Ecwid.getTrackingConsent()` JS API method for storefront. It provides information about customer's consent to be tracked on store pages. [Learn more](https://developers.ecwid.com/api-documentation/get-customer-details#ecwid-gettrackingconsent)

- Added option to offset the default storefront scroll position by X amount of pixels. This can be useful if you use a sticky header on your website that stays displayed when users scroll down website pages. [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#offset-default-storefront-scroll-position)

## Mar 18, 2019

Added new tools to track customers who agreed to receive email promotions. 

`acceptMarketing` field: for the [Orders](https://developers.ecwid.com/api-documentation/orders) and [Customers](https://developers.ecwid.com/api-documentation/customers) REST API endpoints. 

If `acceptMarketing` is `false` – you can't use email of this customer for promotions. If `acceptMarketing` is `true` or `null` – you can send promotional emails to that customer. 

`showAcceptMarketingCheckbox`, `acceptMarketingCheckboxDefaultValue`, `acceptMarketingCheckboxCustomText` for the [Store profile](https://developers.ecwid.com/api-documentation/store-information) REST API endpoint.

## Mar 15, 2019

- [Public app config](https://developers.ecwid.com/api-documentation/public-application-config) size was increased from 64Kb to **256Kb**. 

- Clarified how `quantity` field works when getting product variations from products and variations endpoints. If `sku` is omitted for variation, then `quantity` is nested from base product. If `sku` is present, the variation has its own quantity value.

## Feb 14, 2019

Added `DEFINED_BY_STORE_OWNER` sort method for [searching products](https://developers.ecwid.com/api-documentation/products#search-products) in Ecwid REST API. 

If request is applicable to a specific category (i.e. `category` is set), then `DEFINED_BY_STORE_OWNER` sort method is used. Otherwise, when results are not related to a specific category, `RELEVANCE` sort method is used.

## Feb 5, 2019

We added new feature to Ecwid – **Product Filters**. It can help customers to find products easier when there are many products to choose from. 

- [Product Filters in Ecwid Help Center](https://support.ecwid.com/hc/en-us/articles/207807925)
- [Product filters in Ecwid API Documentation](https://developers.ecwid.com/api-documentation/product-filters)

## Jan 31, 2019

We updated behaviour of `url` field for getting products via REST API. 

Now, if Ecwid knows that this store uses [SEO-friendly URLs](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls), then the API will return URLs in that format by default. 

If it doesn't happen for your store, you can still get product URLs in a clean URL format. See the [getting product URLs](https://developers.ecwid.com/api-documentation/products#q-how-to-get-urls-for-products) section.

## Jan 24, 2019

We updated our webhook retry policy. 

Earlier, if Ecwid couldn't deliver a webhook to an app's URL, it would retry sending it every 15 minutes for 48 hours. 

Now, if webhook delivery fails 1st time, Ecwid will re-send a webhook every 15 minutes for 4 more tries. 

If Ecwid still can't deliver the webhook after that, it will try sending webhooks every 60 minutes until 24 hour limit is reached. 

Once the 24 hour limit is reached, the webhook will be removed from the queue and will not be sent again.

## December 26, 2018

Shipping apps can now provide custom description for their shipping methods displayed at checkout. 

Use the new field `description` when returning shipping rates back to Ecwid when customer is at checkout. 

**Important**: for new Ecwid accounts, we do not use the `transitDays` field. So make sure to update your app to provide `description` field as well as `transitDays` so both old and new users can see the estimates.

[Add Shipping Method Documentation](https://developers.ecwid.com/api-documentation/add-shipping-method)

## December 25, 2018

1) Improved the flow of working with product images. Now all images (main image and gallery images) are contained within the same field – `media`. 

You are able to get their URLs and sort them the way you need with appropriate fields. To set an existing image as main product image, update its `isMain` field to `true`.

All existing fields (`galleryImages`, `imageUrl`, etc.) are still provided in the API, but they are now deprecated in the documentation. 

[Learn more](https://developers.ecwid.com/api-documentation/products)

2) Added `lastUpdated` field to static pages endpoint response. It represents a UNIX timestamp of when the response data was generated. 

[Learn more](https://developers.ecwid.com/api-documentation/static-store-pages)

## December 24, 2018

We updated the order search filters in the [Ecwid REST API](https://developers.ecwid.com/api-documentation/orders#search-orders) in regards to customer details. 

We added two new fields: `email` and `customerId`. They return orders accordingly – for the email or customer ID used to place an order. 

The `customer` field has been deprecated from docs, but it still works in the API itself for now. In your solutions, make sure to use the new fields instead. 

## December 3, 2018

Updated the [Ecwid CSS Framework](https://developers.ecwid.com/ecwid-css-framework) to `1.3.4` version. 

**File URLs** 

- `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.4/ecwid-app-ui.css`
- `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.4/ecwid-app-ui.min.js`

**Changelog**

- Transparent colors are updated to `rgba(0,0,0,0)`
- Added easy loader animcation for buttons – you can just add the `.btn-loading` class for it to become a `progress-button`
- Added the components `.rowable`, `.iconic-dropdown-menu`, `.numeric`
- Updated the components `.horizontal-icolink`, `.iconable-block`, `.gallery`, `.feature-element`

## November 28, 2018

Added option to disallow / allow specific shipping options for certain products. 

You can read and change them for products in the [Ecwid REST API](https://developers.ecwid.com/api-documentation/products) by specifying method IDs in the `enabledMethods` and `disabledMethods` within the `shipping` field.

To get the available shipping method IDs, see the [store profile endpoint](https://developers.ecwid.com/api-documentation/store-information) -> `shipping` field. 

## November 6, 2018

Added new feature toggles for Ecwid stores to JS API and REST API: 

- Login by link for customers
- Latest version of the cart page
- Latest version of the checkout process

See the features available to the store in: 

- [JS API](https://developers.ecwid.com/api-documentation/get-storefront-details#ecwid-getfeaturetoggles)
- [REST API](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)

As always, you can see new features in the *Ecwid Control Panel -> Settings -> What's new*.

## November 2, 2018

Added Facebook Pixel ID value support for Ecwid REST API. The new field is `fbPixelId` and it is available in the store information endpoint.

[Learn more](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)

## November 1, 2018

Updated the [Ecwid CSS Framework](https://developers.ecwid.com/ecwid-css-framework) to `1.3.3` version. 

**File URLs** 

- `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.3/ecwid-app-ui.css`
- `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.3/ecwid-app-ui.min.js`

**Changelog**

- Added colors for `warning` and `info ` states
- Updated `font-size` and `line-height` for some headers
- Updated `border-radius` from `3px` to `5px` for all base components
- Updated components: *a-card*, *draggable-item*, *named-area*, *placeholder-titles*, *status-block*, *iconable-block*, *vertical-filters*, *fieldset* 
- Added new components: *a-card-stack*, *collapsible*, *draggable-area*, *gallery*, *gallery-icon*, *gallery-image*, *inline-alert*, *payment-method*, *list-element*

## October 10, 2018

Added `Accept-Encoding: gzip` header support for the Ecwid REST API requests.

You can use this optional header to get responses from the Ecwid REST API quicker. This header tells Ecwid to provide compressed version of the response, thus it improves the speed of the responses. 

Use it to make 'heavy' requests like getting all products or orders from a store. 

## October 4, 2018

Available date formats for the REST API were changed in the documentation to only include UNIX Timestamp format – for ease of access and understanding. 

**The old formats still work and are available – no need to update your custom solutions / public applications.**

This change is only for the documentation and we do not plan to change the date format support in the REST API itself in the future.

## September 21, 2018

Added UPC attribute and "Compare to" price support for product variations. 

Now you can set specific UPC attribute values as well as "Compare to" prices for a specific product variation in Ecwid Control Panel and Ecwid REST API. 

The changes are available in these API features: 

- [Products endpoint](https://developers.ecwid.com/api-documentation/products)
- [Product Variations endpoint](https://developers.ecwid.com/api-documentation/product-variations)
- [Custom Shipping method](https://developers.ecwid.com/api-documentation/shipping-request-and-response)

## September 18, 2018

Added `coverButtonText` field to starter site endpoint in the Ecwid REST API. It allows you to set custom text for the "Shop now" button in the cover area of a starter site. [Learn more](https://developers.ecwid.com/api-documentation/starter-site)

## August 31, 2018

Added new parameters for opening product details pages with `Ecwid.openPage()` method: `'variation'` and `'options'`. 

Now you can open a specific variation when opening a product details page or just a specific set of product options (dropdown and radio buttons supported only). [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#open-specific-product-variation)

## August 28, 2018

Added PHP template for creating payment integrations: [https://github.com/Ecwid/payment-gateway-template](https://github.com/Ecwid/payment-gateway-template)

Add payment method to Ecwid store docs: [https://developers.ecwid.com/api-documentation/add-payment-method](https://developers.ecwid.com/api-documentation/add-payment-method)

## August 23, 2018

Added `includeInPrice` field for order and cart items in the Ecwid REST API. 

This allows you to know whether the specific tax applied to item was included in the product's price or not. The field can be read, updated or used when creating a new order / cart. [Learn more](https://developers.ecwid.com/api-documentation/rest-api-reference)

## August 7, 2018

Added new read-only field for getting product information in Ecwid REST API – `isSampleProduct`. It indicates whether a product was created automatically when Ecwid store was created (`true`), or manually by a merchant or via API (`false`). [Learn more](https://developers.ecwid.com/api-documentation/products)

## August 3, 2018

Added sale price settings and sale price data for products to the Ecwid REST API. 

Now you can read and change sale price settings of a store and read sale price discount values (abs and percent) when getting product information. 

Learn more: 

- [https://developers.ecwid.com/api-documentation/store-information](https://developers.ecwid.com/api-documentation/store-information)
- [https://developers.ecwid.com/api-documentation/products](https://developers.ecwid.com/api-documentation/products)

## July 30, 2018

Added `storeCoverButton` and `storeLocationAddressSubtitle` fields to the Starter Site endpoint in the Ecwid REST API. 

`storeCoverButton` controls whether to show or hide the Shop Now button at the top of Starter Site and `storeLocationAddressSubtitle` controls the text for store location address subtitle. [Learn more](https://developers.ecwid.com/api-documentation/starter-site)

## July 24, 2018

Added `whiteLabel` flag to the account information of a store. It returns `true` if Ecwid brand is not mentioned in merchant's interface, `false` otherwise. Read only field. [Learn more](https://developers.ecwid.com/api-documentation/store-information)

## July 23, 2018

1) Added abandoned sales settings to store information endpoint in the Ecwid REST API. Now you can read and update the setting for automated email reminders about abandoned sales. [Learn more](https://developers.ecwid.com/api-documentation/store-information)

2) Added option to set custom discounts when calculating order details - `discountInfo` field. [Learn more](https://developers.ecwid.com/api-documentation/carts#calculate-order-details)

## July 6, 2018

Added new type of *view_mode* field for native apps – `INLINE`. This mode is used when app interface is added to the page inside an existing element where other elements are present as well. [Learn more](https://developers.ecwid.com/api-documentation/authentication-in-native-apps)

## June 29, 2018

Added new application type for discount coupons – new customers only. It allows merchants to send out coupons that only work for new customers. 

With this change we are deprecating the `repeatCustomerOnly` field for discount coupons, replacing it with `applicationLimit` with these possible values: `"UNLIMITED"`, `"NEW_CUSTOMER_ONLY"`, `"REPEAT_CUSTOMER_ONLY"`.

If you are creating or updating a discount coupon, make sure to use the old `repeatCustomerOnly` field or the new `applicationLimit` field **only**.

In a situation where both these fields are sent, the priority will be given to the new field – `applicationLimit`. 

[Discount coupons in the Ecwid REST API](https://developers.ecwid.com/api-documentation/discount-coupons)

## June 26, 2018

Added `defaultProductSortOrder` field to the store information endpoint of Ecwid REST API. 

Now you can get the default sort order the merchant has set in their checkout settings. [Learn more](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)

## June 18, 2018

Added `defaultDisplayedPriceFormatted` and `compareToPriceFormatted` fields to products endpoint in Ecwid REST API. 

Now you can get the default displayed price and compare to price in a format merchant has set in store settings when getting info about products. [Learn more](https://developers.ecwid.com/api-documentation/products)

## June 15, 2018

1) Added Store Front page description to the Ecwid REST API – `storeDescription`. Now it is possible to read and update the description of the main store page. [Learn more](https://developers.ecwid.com/api-documentation/store-information)

2) Updated sort order for [search products](https://developers.ecwid.com/api-documentation/products#search-products) (`/products`) endpoint in the Ecwid REST API: 

- If `keyword` parameter is passed but `sortBy` is not, sort order will be `"RELEVANCE"`. No changes here
- If both `keyword` and `sorBy` parameters are not passed, sort order will be **defined by the store owner**

## June 11, 2018

1) Added information about product's taxes in Ecwid REST API. 

Get and update all enabled manual taxes for product, or learn the default tax rate that Ecwid includes in product price for default store location. [Learn more](https://developers.ecwid.com/api-documentation/products)

2) Updated behaviour of the `launchDate` and `expirationDate` fields for [discount coupons endpoint](https://developers.ecwid.com/api-documentation/discount-coupons) in Ecwid REST API. Now the date will be automatically converted to UTC

## June 5, 2018

Added optional parameter to Storefront JS API method `Ecwid.openPage` - `'page'`. It allows you to open any page in the results for `category` and `search` page types. [Learn more](https://developers.ecwid.com/api-documentation/open-page-in-storefront)

## June 1, 2018

- Added control for favorites feature in storefront – `favoritesEnabled` field in store profile endpoint. 

- Added toggle for "Ask for company name" field at checkout - `askCompanyName` field in store profile endpoint.

[Learn more](https://developers.ecwid.com/api-documentation/store-information)

## May 31, 2018

Updated Ecwid CSS Framework version to `1.3.2`

Get it here: 

- `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.2/ecwid-app-ui.css`
- `https://djqizrxa6f10j.cloudfront.net/ecwid-sdk/css/1.3.2/ecwid-app-ui.min.js`

Changelog: 

- OpenSans font was removed. Now the font-family is: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Ubuntu, sans-serif`
- Updated the colors
- Fixed sizes and icon positioning in StatusCard, CTACard
- FeatureCard component now only shows the icon in mobile mode. Image will be hidden
- Removed pointer events for buttons with loading animation
- Added styles for Columned, EditableData, EditableOptionList, CountableBlock, CustomContentCard, DateTimePicker, FlexTableBlock, InputCard, InlineAlert components
- Limited the width of EditableData and Columned inside ACard components to 600px
- Added styles for Subtitle and Footer elements in the FlexTable component
- Added states for NamedArea component: FixedVerticalLayout, WithDescriptionInFooter, WithFooterInMobile, HeadlessMobile
- Added styles for prefixes and suffixes for Input elements

## May 30, 2018

Added option to get and update store design settings in [store profile endpoint](https://developers.ecwid.com/api-documentation/store-information). The design settings and possible values are available in [appearance customization](https://developers.ecwid.com/api-documentation/customize-appearance) section. 

## May 24, 2018

Added a reference spreadsheet for static information in Ecwid stores – currencies, weight units and country codes. [Learn more](https://docs.google.com/spreadsheets/d/1UAYgxdNFpdUAcZ1AXRGBCT-rE4_wu8jR0yUt6ZNVnso/edit?usp=sharing)

## May 22, 2018

Added support for order extra fields in invoice and email notification templates. [Learn more](https://developers.ecwid.com/api-documentation/show-extra-fields-in-an-order#order-extra-fields-in-invoices-and-emails)

## May 4, 2018

Added option to use custom scrolling handler when navigating the Ecwid storefront. Set your custom function to scroll to a custom position on a page with `window.ec.config.custom_scroller` config and `"CUSTOM"` value for `window.ec.config.navigation_scrolling` config. [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#disable-force-scrolling-to-storefront)

## April 17, 2018

- Added option to disable force scrolling to Ecwid storefront on store navigation. [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#disable-force-scrolling-to-storefront)

- Updated the name for main storefront URL configuration from `ecwid_ProductBrowserURL` to: `window.ec.config.store_main_page_url`. The old value is still supported. If both are used on a page – the new one has more priority. [Learn more](https://developers.ecwid.com/api-documentation/customize-behaviour#set-main-storefront-url-for-widgets)

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
