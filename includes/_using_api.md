# ---Using Ecwid REST API---

# Version
This document describes *Ecwid REST API v.3* which is currently in **beta** stage. Please contact us if you want to participate in beta testing or find any bug in the API or the documentation. Thank you. 

For the information on *Ecwid API v.1*, please refer to the [Ecwid knowledge base](http://kb.ecwid.com/w/page/25232810/API)

# Overview
Ecwid provides a REST API that you can use to develop applications interacting with Ecwid stores from outside. By means of Ecwid REST API, your application can read, update, create and delete store data such as products, combinations, orders, customers, discount coupons etc. As any RESTful service, Ecwid REST API use the standard HTTP codes in requests: 

* `GET` to read store data
* `PUT` to update store data
* `POST` to create entries
* `DELETE` to remove entries

For authentication, Ecwid REST API uses **oAuth2** and requires your application to be registered with us prior to calling API. 

All requests are done via secure **HTTPs** protocol. Requests via insecure HTTP are not supported.

