# Overview

## Why you need Ecwid API
With the help of Ecwid API, you can create an application that will extend Ecwid functionality in various ways. You're welcome to be a part of [Ecwid App Market](http://www.ecwid.com/apps) and acquire 600,000 potential users around the world. 

### What you can do with the help of Ecwid API:

1. **Manage store data** including store profile, products, orders, customers, discounts coupons and other.
2. **Add a new page to Ecwid Control Panel** and act as a part of Ecwid backend for the user.
3. **Customize the user storefront** look and behavior on the fly.

For example, your application can do the following:

* add advanced order/product management tools into Ecwid control panel
* generate custom reports and analytics based on the merchant orders data
* print shipping labels for placed orders
* integrate user's store with any third party service, e.g. email marketing
* add a new widget to the merchant's storefront and display any custom information there


## API basics

### RESTful / oAuth2
Ecwid API is a **RESTful** API with **oAuth2** authentication. As any RESTful service, Ecwid REST API use the standard HTTP codes in requests: 

* `GET` to read store data
* `PUT` to update store data
* `POST` to create entries
* `DELETE` to remove entries

### HTTPS
All requests are done via HTTPs. Requests via insecure HTTP are not supported.

### UTF-8
Ecwid API works with UTF-8 encoded data. Please make sure everything you send over in API calls also uses UTF-8.

### JSON
All data received from API and submitted to API is JSON.

### UTC
Date/time values returned by Ecwid API are in the UTC.

### API Version
This document describes *Ecwid REST API v.3* which is currently in **beta** stage. Please contact us if you want to participate in beta testing or find any bug in the API or the documentation. Thank you. 

For the information on *Ecwid API v.1*, please refer to the [Ecwid API v.1 documentation](http://kb.ecwid.com/w/page/25232810/API)
