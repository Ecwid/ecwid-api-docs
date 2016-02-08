# REST API Reference

# API basics

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
Date/time values returned by Ecwid API are in UTC.

### API Version
This document describes *Ecwid REST API v.3* 

### API calls limits
You are free to build your app with as many API calls as you need to make your service awesome for Ecwid merchants, but keep in mind the [usage policy](#usage-policy).

# Usage policy
By default, we do not limit API usage for applications. You are free to build your app with as many calls as you need to make your service awesome for Ecwid merchants. However, to protect us and our users from abusing, we ask you to optimize your app code to make fewer API requests. For example:

- Cache store data locally if you need to use or display it many times in your app. 
- If you need to synchronize store data with your database, use Webhooks to get timely updates about changes in a store. More details: [Webhooks](#webhooks)

We constantly monitor API activity and servers load on our side to make sure every application uses API properly. In case an app abuses Ecwid API by generating huge amount of requests every day, we'll attempt to get in touch with the developer to talk about the issue. Don't worry, you will unlikely face such trouble and even if you do, we will advice on how to fix that. But of course, if the usage is high enough to significantly affect other users of the platform and you don't react on our warnings, we can temporarily disable your application. 
