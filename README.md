ecwid-api-docs
==============

Ecwid REST API documentation ([api.ecwid.com](http://api.ecwid.com))

The docs use Markdown syntax. Syntax reference: [Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax)


#Changelog

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