ecwid-api-docs
==============

Ecwid REST API documentation ([api.ecwid.com](http://api.ecwid.com))

The docs use Markdown syntax. Syntax reference: [Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax)


#Changelog

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