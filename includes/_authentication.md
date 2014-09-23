# Authentication
All Ecwid API requests require authentication. Ecwid supports oAuth2 protocol to provide external applications with an easy way to authenticate and access store data on behalf of the user. Ecwid user grants or denies access to certain data in their store for the particular application - the application gets its own secure access token upon authorization and uses that token as a key to make API calls to Ecwid.

##Retrieveing access token

Retrieving an access token includes the following steps:
1. Your application sends the user to Ecwid authorization dialog available on the Ecwid's oAuth endpoint.
2. Upon authorization, Ecwid redirects the user to the application's redirect URL specified in the request.
3. The application requests an access token from Ecwid. This access_token will be used as API key in all API calls.

Below you will find the details on each of these steps.

###Step 1. Send user to Ecwid authorization dialog

Your application sends the user to Ecwid authorization dialog available on the Ecwid's oAuth endpoint

> Request example

```shell
curl "https://app.ecwid.com/api/oauth/authorize?client_id=abcd0123&redirect_uri=https%3A%2F%2Fwww%2Eexample%2Ecom%2Fmyapp&response_type=code"
```

`GET https://my.ecwid.com/api/oauth/authorize`

Parameter | Required | Description
--------- | -------- | -----------
client_id | required | application ID
redirect_uri | required | URI in your app where users will be sent after authorization. It must match the domain/URL of the registered return_url specified in the app settings. I.e. if the registered return_url is `http://www.example.com`, the redirect_uri in request might be `http://www.example.com/oauth/callback.php` , but not `http://www.example2.com/`
response_type | required | `code` (must always be `code`)
scope | optional | Scope of access that your app requests from the user, separated by space. See details in [Scopes](#access-scopes) section below




###Step 2. Ecwid redirects the user back to your application

> Return URLs

```shell
# Successfull authorization
https://www.example.com/myapp?code=1234567890

# The user denied to provide access
https://www.example.com/myapp?error=access_denied
```

Upon authorization, Ecwid redirects the user to the application's `redirect_uri` specified in request.

####Return URL parameters

Parameter | Description
--------- | -----------
code | If the user successfully authorizes the application, the query contains the code parameter. That is a temporary key that the app will exchange to its own access token on the next step
error | If the user does not allow authorization to the application, query parameters indicate the user canceled authorization in error field


###Step 3. Retrieve access_token from Ecwid in background

> Request example

```shell
curl "https://my.ecwid.com/api/oauth/token" \
-XPOST \
-d client_id=abcd0123 \
-d client_secret=01234567890abcdefg \
-d code=987654321hgfdsa \
-d redirect_uri=https://www.example.com/myapp
-d grant_type=authorization_code
```

`POST https://my.ecwid.com/api/oauth/token`

Parameter | Required | Description
--------- | -------- | -----------
client_id | required | Application ID
client_secret | required | Application secret key
code | required | The temporary code received on the step #2
redirect_uri | conditional | If the `redirect_uri` parameter was included in the authorization request, you must include it in the token request and their values must be identical.
grant_type | required | Must be `authorization_code`

> Response example

```json
{
 "access_token":"12345",
 "token_type":"bearer",
 "scope":"read_store_profile update_catalog",
 "store_id":"1003"
}
```



Ecwid responds with a JSON-formatted data containing the access token and additional information. The response fields are listed below:

Field | Description
----- | -----------
access_token | Authorization token
token_type | `bearer` (it's always `bearer`)
scope | List of permissions (API access levels) given to the app, separated by space
store_id | Ecwid store ID (a unique Ecwid account identificator)


<aside class="notice">
For security reasons, a temporary code can be exchanged to an access token only once. In case of second attempt, the previously provided access token is automatically disabled.
</aside>


##Access scopes
Scopes are permissions that identifies the scope of access your application requests from the user. 

* read_store_profile (requested in all cases even if not specified)
* update_store_profile
* read_catalog
* update_catalog
* create_catalog
* read_orders
* update_orders
* create_orders
* read_customers
* update_customers
* create_customers
* create_discount_coupons
* read_discount_coupons
* update_discount_coupons
* customize_storefront *(in development)*
* add_to_cp *(in development)*


##Authentication of installed applications

In case of applications that are installed on a device (such as a computer, a cell phone, or a tablet), the application client_secret is in general less protected than in case of web services. Thus the process of [retrieving access token](#retrieveing-access-token) is changed as described below.

###Changes in Step #1

> Step #1. Request example

```shell
curl "http://app.ecwid.com/api/oauth/authorize?client_id=abcd0123&redirect_uri=urn:ietf:wg:oauth:2.0:oob&response_type=code"
```

On the [Step #1](#retrieveing-access-token) (app requests a temporarily authorization code), application needs to send the following value as redirect_uri:

`urn:ietf:wg:oauth:2.0:oob`

The rest of request parameters are the same as for web services.

<aside class="notice"> In case of Web services, if the app has been already authorized by user before current authorization attempt, Ecwid will simply redirect user back to the application with the code in request parameters (i.e. Ecwid will not ask user to authorize the app second time). In case of an installed application, Ecwid will ask user to provide permission every time a user is directed by teh app to Ecwid authorization endpoint. </aside>


###Changes in Step #2

> Step #2. User grants permission

```html
<title>oauth_response:code=54xgQHmbZrrLTqzCiHPywwEGQNtAFLG7&amp;state=code</title>
```

> Step #2. User denies to provide the app with access

```html
<title>oauth_response:error=access_denied</title>
```

On the [Step #2](#retrieveing-access-token) (Ecwid provides the app with a temporary authorization code), Ecwid will not direct a user to `redirect_uri` after the user agrees or disagrees to provide the application with permissions. Instead, Ecwid opens an empty page and provides the code in the page's `<title>` tag. I.e., all the data is sent to application in the page title tag and no data is sent in GET request to the application. The format of data provided in the `<title>` tag is the following:

`oauth_response:GET-query-as-it-would-be-transferred-via-redirect`

<aside class="notice">On this step, the app needs to react on the changes in the title tag content and parse it the same way as it would parse GET request query. So it is required that the application has access to the system browser or the ability to embed a browser control in the application.</aside>

###Changes in Step #3
The [Step #3](#retrieveing-access-token) (the app exchanges the temporarily authorization code to an access token) is not changed. Everything works the same way for installed apps as it does for web apps.
