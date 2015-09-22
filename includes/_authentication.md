# Authentication
All Ecwid API requests require authentication. Ecwid supports **oAuth2** protocol to provide external applications with an easy way to authenticate and access store data on behalf of the user. Ecwid user grants or denies access to certain data in their store for the particular application - the application gets its own secure access token upon authorization and uses that token as a key to make API calls to Ecwid.

## Register your app in Ecwid
To use Ecwid oAuth and REST API, your application needs to be registered in Ecwid. The registration is easy: you will just need to provide us with information about your application and we will send you app ID and app credentials that you will then use to get access to API. The information we're asking is:

1. Application description and logo that will be displayed to your users
2. Technical parameters that will be used in oAuth flow
3. Application details for displaying in Ecwid Control panel

To register you app in Ecwid, fill [this form](https://ecwidcom.typeform.com/to/tRyWJ6) and we will contact you in a few days. 


## Get access token
Retrieving an access token includes the following steps:

1. Your application sends the user to Ecwid authorization dialog available on the Ecwid's oAuth endpoint.
2. Upon authorization, Ecwid redirects the user to the application's redirect URL specified in the request.
3. The application requests an access token from Ecwid. This access_token will be used as API key in all API calls.

Below you will find the details on each of these steps.

###Step 1. Send user to Ecwid authorization dialog

Your application sends the user to Ecwid authorization dialog available on the Ecwid's oAuth endpoint

> Request example

```shell
curl "https://my.ecwid.com/api/oauth/authorize?client_id=abcd0123&redirect_uri=https%3A%2F%2Fwww%2Eexample%2Ecom%2Fmyapp&response_type=code&scope=read_store_profile+read_catalog+update_catalog+read_orders"
```

`GET https://my.ecwid.com/api/oauth/authorize`

Parameter | Required | Description
--------- | -------- | -----------
client_id | required | Application ID
redirect_uri | required | URI in your app where users will be sent after authorization. It must match the domain/URL of the registered return_url specified in the app settings. I.e. if the registered return_url is `http://www.example.com`, the redirect_uri in request might be `http://www.example.com/oauth/callback.php` , but not `http://www.example2.com/`
response_type | required | `code` (must always be `code`)
scope | optional | Scope of access that your app requests from the user, separated by space. See details in [Scopes](#access-scopes) section below


<aside class="notice">
This step is omitted if the application is installed from the app details page inside Ecwid Control Panel. See details: <a href="#installation-from-hosted-app-details-page">Installation from hosted app details page</a>
</aside>


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
code | If the user successfully authorizes the application, the query will contain the `code` parameter. That is a temporary code that your app should exchange to an access token. This code can be used only once and lives for a few minutes so your app should request the token as soon as it gets the code. See the Authorization step #3 for the details.
error | If the user does not allow authorization to the application, query parameters indicate the user canceled authorization in error field


###Step 3. Retrieve access_token from Ecwid in background

> Request example

```shell
curl "https://my.ecwid.com/api/oauth/token" \
-XPOST \
-d client_id=abcd0123 \
-d client_secret=01234567890abcdefg \
-d code=987654321hgfdsa \
-d redirect_uri=https://www.example.com/myapp \
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
 "store_id":1003
}
```



Ecwid responds with a JSON-formatted data containing the access token and additional information. The response fields are listed below:

Field | Description
----- | -----------
access_token | Authorization token. This is a key your app will use to access Ecwid API on behalf of the user. It doesn't expires so there is no need to refresh it unless the user removes (de-authorizes) your application and then decides to instal it again.
token_type | `bearer` (it's always `bearer`)
scope | List of permissions (API access levels) given to the app, separated by space
store_id | Ecwid store ID (a unique Ecwid account identificator)


<aside class="notice">
For security reasons, a temporary code can be exchanged to an access token only once. In case of second attempt, the previously provided access token is automatically disabled.
</aside>


##Access scopes
Scopes are permissions that identifies the scope of access your application requests from the user. You should pass the required scopes along with the authorization request to Ecwid oAuth server when a user installs your application. See details above: [Get access token](#get-access-token).

Access scope | Notes
------------ | -----
read_store_profile | Get store name and general settings, get store admin email, get updates statistics etc. *Requested in all cases even if not specified*
update_store_profile | Set taxes, update invoice logo, change Starter Site domain, close store for maintenance etc.
read_catalog | Search products, get product options/combinations etc. Also allows to receive push updates (webhooks) about changes in store products.
update_catalog | Update product prices, upload images and e-goods, modify product attributes etc.
create_catalog | Create new products
read_orders | Get sales for a given period, retrieve order details etc. Also allows to receive push updates (webhooks) about changes in store orders.
update_orders | Change order totals, switch order status, cancel orders etc.
create_orders | Place a new order in the store
read_customers | Search customers or retrieve some particular customer data
update_customers | Change customer profile data, add items to the customer address book etc.
create_customers | Add a new customer to the store's Customers list
read_discount_coupons | Get the list of discount coupons or retrieve some particular coupon details
update_discount_coupons | Change the coupon expiration date or limit its number of use, update coupon code etc.
create_discount_coupons | Add a new discount coupon
customize_storefront | Attach a custom JS/CSS to the storefront on the fly to modify its look and feel (see [Customizing storefront](#customize-storefront))
add_to_cp | Add a new tab to merchant control panel (see [Embedding apps](#embedded-apps))


##Applications installed on device

In case of applications that are installed on a device (such as a computer, a cell phone, or a tablet), the application client_secret is in general less protected than in case of web services. Thus the process of [retrieving access token](#retrieveing-access-token) is changed as described below.

###Changes in Step #1

> Step #1. Request example

```shell
curl "http://my.ecwid.com/api/oauth/authorize?client_id=abcd0123&redirect_uri=urn:ietf:wg:oauth:2.0:oob&response_type=code"
```

On the [Step #1](#retrieveing-access-token) (app requests a temporarily authorization code), application needs to send the following value as redirect_uri:

`urn:ietf:wg:oauth:2.0:oob`

The rest of request parameters are the same as for web services.

<aside class="notice"> In case of Web services, if the app has been already authorized by user before current authorization attempt, Ecwid will simply redirect user back to the application with the code in request parameters (i.e. Ecwid will not ask user to authorize the app second time). In case of an installed application, Ecwid will ask user to provide permission every time a user is directed by the app to Ecwid authorization endpoint. </aside>


###Changes in Step #2

> Step #2. User grants permission

```html
<title>oauth_response:code=54xgQHmbZrrLTqzCiHPyw&amp;state=code</title>
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


## Selfhosted web apps
Some applications requires user to download and install them on their site rather than providing a hosted solution. For example, plugins for Wordpress, Joomla or other CMS systems do that. Every instance of such application resides on different domain and thus has different Return URL. To implement oAuth flow in such an app, you will need to pass the `redirect_uri` security check oultined in the [step #1](#get-access-token) of the authorization flow. [Contact us](http://developers.ecwid.com/contact) if your application is of this kind and we will mark it as "selfhosted web app" in the app settings – in this case the redirect URL security requirements will be removed for your application. 


## Installation from hosted app details page
Every public application, which is accessible in Ecwid App Market, has a dedicated app information page inside Ecwid Control Panel. Ecwid merchants can find and install the application without leaving their Control Panel. Example: [Order Editor app details page](https://my.ecwid.com/cp/?place=apps:view=app%26name=ecwid-edit-orders) (you will need to log in to your Ecwid account see this page) . 

In this case, the user is already authorized and thus the first step of ["Getting token"](#get-access-token) process ("Send user to ecwid authorization dialog") is omitted. Once the user clicks the "Install" button, Ecwid sends a user to your app Return URL along with the temporary code as outlined in the [authorization step #2](#get-access-token) "Ecwid redirects the user back to your application". Mind that in this case the installation process starts on Ecwid side and the moment when user gets to your application "Return" endpoint might be the first time they open your site. Make sure your page on the return URL onboards users well – this is a landing page for them in your service. 

This is the most popular or even the only way Ecwid merchants install the applications to their stores. So please make sure your application supports this. 

Notes:

- When the app cannot be installed from the app details page, the app details page contains the "Get" button instead of "Install". The "Get" button simply redirects yser to your app web site. Examples of such apps are the apps installed on device (iOS, Android, Desktop applications). More details about such apps: [Applications installed on device](#applications-installed-on-device)
- If an app creates a separate tab inside user's Ecwid Control Panel, Ecwid will redirects user to the new created tab after installation. The redirect URL setting is ignored in this case. This is done this way because the app embedded in a separate Control Panel tab can get the access token without temporary code. More details: [Authentication in embedded apps](#authentication-in-embedded-apps). 

