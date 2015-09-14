ecwid-api-docs
==============

Ecwid REST API documentation ([api.ecwid.com](http://api.ecwid.com))

The docs use Markdown syntax. Syntax reference: [Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax)


#Changelog

##September 14, 2015
- New minor version of Ecwid JS SDK is released (v 1.0.1). It contains a fix for the bug that caused layout issues in embedded apps (glitchy footer)

##September 5, 2015
- Updated discountType field. Added mixed type of coupon (absolute discount AND free shipping, percentage discount AND free shipping)

##August 26, 2015
- Added tax details to order items (taxOnDiscountedSubtotal and taxOnShipping fields)

##August 13, 2015
- Added Single Sign On code examples

##July 27, 2015
- Added a tip on how to search products for exact match using a keyword in quotes, e.g. `"keyword"`. This is useful, for example, to get a product by SKU
- Added `emailLogoUrl` and `availableFeatures` fields description in the Store profile endpoint section

##July 25, 2015
- Added two code examples to the Library (A simple Ecwid oAuth client on PHP and a Laravel app template)

##July 8, 2015
- Added description of how to use Ecwid oAuth in self hosted web apps, e.g. CMS plugins.
- Introduced hosted app details pages in Ecwid Control Panel.

##June 14, 2015
- Added handling fee options to the "Store profile" and "Orders" endpoints
- Improved behavior of hidden product attributes in responses from "Products" endpoints

##June 7, 2015
- All dates are returned in UTC now
- Timestamp is now returned for orders and products
- Options are now automatically created when you create a combination
- Added a short usage policy text

##May 5, 2015
- Added description of how to define date and time ranges in the "Search products" and "Search orders filters (http://api.ecwid.com/#search-products)
- Fixed an error in the "Update product" endpoint spec: to update UPC and Brand product attributes, it's possible to use the 'alias' field in the AttributeValue object.

##April 27, 2015
- Added ability to add embedded applications to the "Settings" and "Design" tabs (http://api.ecwid.com/#embedded-apps)

##April 3, 2015
- Added Ecwid CSS Framework documentation (http://api.ecwid.com/ecwid-css-framework/)
- Added Ecwid JS SDK documentation

##April 2, 2015
- Added ability to get the user language in embedded applications ("lang" parameter in the payload)
- Added a link to the new Developers site (http://developers.ecwid.com)

##March 2, 2015
- Added troubleshooting section to the "Embedding apps" section

##March 2, 2015
- Added the "API availability on Ecwid plans" paragraph to the Overview section to explain which merchants are allowed to use the API 
- Added clarifications to the access scopes list in the Authentication section

##February 19, 2015
- Added PHP example of Ecwid payload decryption in embedded apps

##February 16, 2015
- Added mailNotifications field in the store settings. Now you can get and update the mail notifications settings over API
- Added Legacy API links in the Overview section
- The default items limit in the products search response is changed from 10 to 100

##January 15, 2015
- Added Single Sign-on API description

##January 7, 2015
- Added static links for CSS SDK and JS SDK

##December 30, 2014
- Added "Update product file description" endpoint

##December 11, 2014
- Categories requests/response now supports batch requests parameters (limit/offset/total/count)
- Search products now supports ADDED_TIME_ASC sort order
- Upload image requests now respond with 422 error when the uploaded file does not seem to be an image
- "Get categories" request didn’t return category product IDs. Fixed.

##December 2, 2014
- Added "Embedded applications" specification