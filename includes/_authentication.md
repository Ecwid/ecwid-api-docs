# Authentication

The Ecwid platform provides two ways of installing and operating applications in an online store: native and external apps.

**Native apps** work solely in the Ecwid Control Panel by adding a new tab into one of its sections. When a user visits that tab, Ecwid will load the app page in an IFRAME window. The app can interact with a store (get access token, get store information) and with the Ecwid Control Panel using [Ecwid JS SDK](#ecwid-javascript-sdk).

**External apps** work outside of the Ecwid Control Panel on an external website or in the background with no setup required. These applications need to [handle the oAuth process](https://developers.ecwid.com/api-documentation/external-applications#get-access-token) themselves in order to get an access token. Usually, they work on the server side with their own database to store the access tokens of their users’ stores.

## Native applications

> Get access token in native apps

```js
var storeData = EcwidApp.getPayload();

var storeId = storeData.store_id;
var accessToken = storeData.access_token;

// Use accessToken and storeId variables to make requests to Ecwid REST API
// ...
```

> Check native apps guideline for general guides 

> [https://developers.ecwid.com/native-applications](https://developers.ecwid.com/native-applications)

Native applications work in a separate tab in Ecwid Control Panel. After user installs an application, Ecwid will redirect user to the new tab. In that tab, your application can interact with the store on behalf of the user. 

To get access token and start making requests to Ecwid API, use Ecwid JS SDK and a couple of Javascript lines of code - see example on the right.

See [native apps documentation](#embedded-apps) and start working on your application.

## External applications

External applications work outside of Ecwid control panel and they also have an [app details page](#app-details-page) for easy installation process. These apps use oAuth2 flow to get access token for a specific Ecwid store. 

> Check external apps guideline for general guides

> [https://developers.ecwid.com/external-applications](https://developers.ecwid.com/external-applications)

The installation process starts with user visiting app details page. On that page, they can install the app by providing the necessary permissions to it. Once the user clicks the "Install" button, Ecwid sends a user to your app's **Return URL** along with the temporary **code** in the URL parameters.

Mind that in this case the installation process starts on Ecwid side and it might be the first time they open your site. Make sure your page on the return URL onboards users well – this is a landing page for them in your service. We recommend prefilling user details like: Name, email, Store name, Store ID and other fields that your service needs.

See [External applications guideline](/external-applications) for more information.

### Get access token

Retrieving an access token for external apps includes the following steps:

1. User installs application, Ecwid redirects the user to the return URL. [Contact us](/contact) to change it
2. Your code requests an access token from Ecwid in the background. This access_token will be used as API key in all API calls.

User will get to the first step by accessing your [App details page](#app-details-page) in the Ecwid App Market. It is a separate page dedicated to describe your application in Ecwid Control Panel. 

Before you proceed, make sure you have a [registered app](/register) to install and the application is registered for the Ecwid App Market.

<aside class="notice">
User needs to go through all these steps only <strong>once</strong> in order for your app to get and store access token for that user. This token will be used in any call you make to Ecwid API on behalf of the user.
</aside>

#### Step 1. Ecwid redirects the user to a return URL of your app

> Step #1: Return URL example

```shell
# Successfull authorization
https://www.example.com/myapp?code=1234567890
```

Upon successful installation, Ecwid redirects the user to the application's `redirect_uri` with a `code` parameter in the URL ([contact us](/contact) to change it). The value of this parameter **is not an actual token for the store** and it must be exchanged for the token in the next step of the process.

##### Return URL parameters

Parameter | Description
--------- | -----------
code | If the user successfully authorizes the application, the query will contain the `code` parameter. That is a temporary code that your app should exchange to an access token. This code can be used only once and 'lives' for a few minutes so your app should request the token as soon as it gets the code. See step #2 for the details.
error | If the user does not allow authorization to the application, query parameters indicate the user canceled authorization in error field


#### Step 2. Retrieve access_token from Ecwid in background

> Step #2: Request example

```shell
https://my.ecwid.com/api/oauth/token?client_id=abcd0123&client_secret=01234567890abcdefg&code=987654321hgfdsa&redirect_uri=https%3A%2F%2Fwww%2Eexample%2Ecom%2Fmyapp&grant_type=authorization_code
```

`POST https://my.ecwid.com/api/oauth/token?client_id={client_id}&client_secret={client_secret}&code={code}&redirect_uri={redirect_uri}&grant_type=authorization_code`

Parameter | Required | Description
--------- | -------- | -----------
client_id | required | Application ID
client_secret | required | Application secret key
code | required | The temporary code received on the step #1
redirect_uri | required | Redirect URL of your application. [Contact us](/contact) to change it
grant_type | required | Must be `authorization_code`

> Step #2: Response example

```json
{
 "access_token":"secure_123453lasdADSKasasdjasdklasASkmns",
 "token_type":"bearer",
 "scope":"read_store_profile update_catalog",
 "store_id":1003,
 "public_token":"public_qKDUqKkNXzcj9DejkMUqEkYLq2E6BXM9"
}
```

Ecwid responds with a JSON-formatted data containing the access token and additional information. The response fields are listed below:

Field | Description
----- | -----------
access_token | Private authorization token. This is a key your app will use to access Ecwid API on behalf of the user. 
token_type | `bearer` (it's always `bearer`)
scope | List of permissions (API access scopes) given to the app, separated by space
store_id | Ecwid store ID (a unique Ecwid account identificator)
public_token | [Public authorization token](#access-tokens). Provided if requested access scopes contain `public_storefront` scope.

<aside class="notice">
For security reasons, a temporary code can be exchanged to an access token only once. In case of second attempt, the previously provided access token is automatically disabled.
</aside>

### Access scopes

Scopes are permissions that identifies the scope of access your application requests from the user. Below you can see the names of access scopes that exist in Ecwid API and their description.

Each application has their specified set of access scopes which are required for this applicaiton. If you specify additional scopes, that excess the specified ones for the app in Ecwid, you will see an error message. So if you need to add more access scopes - please [contact us](/contact) to update your app.

Access scope | Notes
------------ | -----
read_store_profile | Get store name and general settings, get store admin email, get updates statistics etc. *Requested in all cases even if not specified*
update_store_profile | Set taxes, update invoice logo, change Starter Site domain, close store for maintenance etc.
read_catalog | Search products, get product options/variations etc. Also allows to receive push updates (webhooks) about changes in store products.
update_catalog | Update product prices, upload images and e-goods, modify product attributes etc.
create_catalog | Create new products
read_orders | Get sales for a given period, retrieve order details etc. Also allows to receive push updates (webhooks) about changes in store orders.
update_orders | Change order totals, switch order status, cancel orders etc. Requires `read_orders` scope to function
create_orders | Place a new order in the store
read_customers | Search customers or retrieve some particular customer data
update_customers | Change customer profile data, add items to the customer address book etc.
create_customers | Add a new customer to the store's Customers list
read_discount_coupons | Get the list of discount coupons or retrieve some particular coupon details
update_discount_coupons | Change the coupon expiration date or limit its number of use, update coupon code etc.
create_discount_coupons | Add a new discount coupon
customize_storefront | Attach a custom JS/CSS to the storefront on the fly to modify its look and feel (see [Customizing storefront](#customize-storefront))
add_to_cp | Add a new tab to merchant control panel (see [Embedding apps](#embedded-apps))
add_shipping_method | Add a new shipping method to the store (see [Custom Shipping API](#add-shipping-method))
add_payment_method | Add a new payment method to the store (see [Add Payment Method](#add-payment-method))
public_storefront | Get public store details with public access token
customize_cart_calculation | Apply custom discounts to orders in real time

### Complete oAuth flow

This method of getting access token is meant for apps that are installed outside of the Ecwid App Market, for example: apps that work on a device, apps for mobile devices, CMS plugins, etc. 

These kinds of apps can be also displayed in the Ecwid App Market, but the oAuth flow will be performed on an external website, where developer will decide how to handle the installation. 

We recommend using the simplified installation flow from the [Get access token](https://developers.ecwid.com/api-documentation/external-applications#get-access-token) section, however if it's not possible or the application is created **for a specific Ecwid store only**, you can use this complete oAuth flow. Before you proceed, make sure you have a [registered app](/register) to install.

Retrieving an access token in a complete oAuth flow includes the following steps:

1. You send the user to Ecwid authorization dialog available on the Ecwid's oAuth endpoint.
2. Upon authorization, Ecwid redirects the user to the return URL specified in the request.
3. The your code requests an access token from Ecwid. This access_token will be used as API key in all API calls.

<aside class="notice">
User needs to go through all these steps only <strong>once</strong> in order for your app to get and store access token for that user. This token will be used in any call you make to Ecwid API on behalf of the user.
</aside>

#### Step 1. Send user to Ecwid authorization dialog

Your application sends the user to Ecwid authorization dialog available on the Ecwid's oAuth endpoint. 

> Step #1: Request example

```shell
https://my.ecwid.com/api/oauth/authorize?client_id=abcd0123&redirect_uri=https%3A%2F%2Fwww%2Eexample%2Ecom%2Fmyapp&response_type=code&scope=read_store_profile+read_catalog+update_catalog+read_orders
```

`GET https://my.ecwid.com/api/oauth/authorize?client_id={client_id}&redirect_uri={redirect_uri}&response_type=code&scope={scope}`

Parameter | Required | Description
--------- | -------- | -----------
**client_id** | required | Application ID
**redirect_uri** | required | URI in your app where users will be sent after authorization. It must match the domain/URL of the registered return_url specified in the app settings (contact us to change it). I.e. if the registered return_url is `http://www.example.com`, the redirect_uri in request might be `http://www.example.com/oauth/callback.php` , but not `http://www.example2.com/`
**response_type** | required | `code` (must always be `code`)
**scope** | required | Scope of access that your app requests from the user, separated by space. See all possible values in [Scopes](#access-scopes) section

<aside class="notice">
	Parameters in <strong>bold</strong> are mandatory.
</aside>

<aside class="notice">
This step is omitted if the application is installed from the app details page inside Ecwid Control Panel. See details in <a href="#external-applications">External applications</a> section
</aside>

#### Step 2. Ecwid redirects the user back to a return URL

> Step #2: Return URLs example

```shell
# Successfull authorization
https://www.example.com/myapp?code=1234567890

# The user denied to provide access
https://www.example.com/myapp?error=access_denied
```

Upon authorization, Ecwid redirects the user to the application's `redirect_uri` specified in request with a `code` parameter in that URL. The value of this parameter **is not an actual token for the store** and it must be exchanged for the token in the next step of the process. You can contact us to change the `redirect_uri` value

If your application is installed on a device (mobile, desktop, etc.) we suggest using a deep link to your app as a `redirect_uri`. Deep linking allows applications to direct user to a specific page within the application, just like a regular URL on the Internet. [Learn more about deep linking and how to enable it in your application](https://savvyapps.com/blog/how-to-use-deep-linking-in-your-mobile-app).

##### Return URL parameters

Parameter | Description
--------- | -----------
code | If the user successfully authorizes the application, the query will contain the `code` parameter. That is a temporary code that your app should exchange to an access token. This code can be used only once and lives for a few minutes so your app should request the token as soon as it gets the code. See the Authorization step #3 for the details.
error | If the user does not allow authorization to the application, query parameters indicate the user canceled authorization in error field


#### Step 3. Retrieve access_token from Ecwid in background

> Step #3: Request example

```shell
https://my.ecwid.com/api/oauth/token?client_id=abcd0123&client_secret=01234567890abcdefg&code=987654321hgfdsa&redirect_uri=https%3A%2F%2Fwww%2Eexample%2Ecom%2Fmyapp&grant_type=authorization_code
```

`POST https://my.ecwid.com/api/oauth/token?client_id={client_id}&client_secret={client_secret}&code={code}&redirect_uri={redirect_uri}&grant_type=authorization_code`

Parameter | Required | Description
--------- | -------- | -----------
client_id | required | Application ID
client_secret | required | Application secret key
code | required | The temporary code received on the step #2
redirect_uri | conditional | If the `redirect_uri` parameter was included in the authorization request, you must include it in the token request and their values must be identical.
grant_type | required | Must be `authorization_code`

> Step #3: Response example

```json
{
 "access_token":"12345",
 "token_type":"bearer",
 "scope":"read_store_profile update_catalog",
 "store_id":1003,
 "email": "john@store.com",
 "public_token":"public_qKDUqKkNXzcj9DejkMUqEkYLq2E6BXM9"
}
```

Ecwid responds with a JSON-formatted data containing the access token and additional information. The response fields are listed below:

Field | Description
----- | -----------
access_token | Private authorization token. This is a key your app will use to access Ecwid API on behalf of the user. 
token_type | `bearer` (it's always `bearer`)
scope | List of permissions (API access levels) given to the app, separated by space
store_id | Ecwid store ID (a unique Ecwid account identificator)
email | Store owner's `accountEmail`, as provided in [store profile](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)
public_token | [Public authorization token](#access-tokens). Provided if requested access scopes contain `public_storefront` scope.

<aside class="notice">
For security reasons, a temporary code can be exchanged to an access token only once. In case of second attempt, the previously provided access token is automatically disabled.
</aside>

#### Q: What if my app always redirects users to different domain?

Some applications requires user to download and install them on their site rather than providing a hosted solution. For example, plugins for Wordpress, Joomla or other CMS systems do that. Every instance of such application resides on different domain and thus has different Return URL. 

To implement oAuth flow in such an app, you will need enable support for multiple domains in `redirect_uri` and improve security: application will always ask user for permissions and will show the domain that requests them in the oAuth flow. [Contact us](/contact) if your application is of this kind and we will make the necessary changes.  

## Access tokens

All Ecwid REST API requests require authentication. Ecwid supports **oAuth2** protocol to provide applications with an easy way to authenticate and access store data on behalf of the user with access tokens. 

There are two types of access tokens in Ecwid: **private** and **public** tokens. **Private** ones can't be shared publicly as they provide access to modify store data through the REST API. **Public** tokens on the other hand have read-only access, so they can be used anywhere to get store details through the REST API in your apps.

Ecwid user grants or denies access to certain data in their store for the particular application - then the application gets its own secure access token (and optional public token) upon authorization and uses that token as a key to make REST API calls to Ecwid.

**Both types of access tokens do not expire, so you can use them as long as the application is installed in a merchant's store.**

<aside class="notice">
To get private or public access token, user would need to go through Ecwid oAuth app installation flow. Learn more in the <a href="#native-applications">Native</a> and <a href="#external-applications">External</a> applications sections.
</aside>

### Private access token

> Private access token example

```json
{
	"access_token": "secure_adasjkhndjksnmkasjhdASDHasjdnhasa"
}
```

Private access token provides access to Ecwid API to retrieve and change store data on behalf of the user who installed your application in their store. **It doesn't expire, so it is available to you at all times**. You will only need to get a new access token in case a user uninstalls the application from their store and installs your application back again. 

With private access token you can use any method within the access scope range that you requested from an Ecwid user on initial steps of oAuth process.

After the moment user installs your app, it can store that token securely in your database for that user. So it's not necessary to go through the standard oAuth flow each time you need to make a request to Ecwid API. 

<aside class="note">Private access token <strong>must not be visible/available in public</strong> (widget integration code, client-side JavaScript, etc.).</aside>

### Public access token

> Public access token example

```json
{
	"public_token": "public_asdkjlsaASKDjaslkdASmndcasmrdgaSj"
}
```

Public token provides limited access to public store data via REST API interface. While private tokens allow you to modify something in a store, like update an order status or change storkc levels, public tokens can only get limited information from a store and create orders with limitations.

With public access token you can use several REST API endpoints from anywhere (client-side JavaScript, widget integration codes, etc.). These methods are available for public token **regardless of the other access scope your requested** from a store – you only need the `public_storefront` scope to use them: 

- [Get store profile](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)
- [Search enabled products](https://developers.ecwid.com/api-documentation/products#search-products)
- [Get enabled product details](https://developers.ecwid.com/api-documentation/products#get-a-product)
- [Search enabled categories](https://developers.ecwid.com/api-documentation/categories#get-categories)
- [Get enabled category details](https://developers.ecwid.com/api-documentation/categories#get-category)
- [Get all variations of an enabled product](https://developers.ecwid.com/api-documentation/product-variations#get-all-product-variations)
- [Get variation details of an enabled product](https://developers.ecwid.com/api-documentation/product-variations#get-product-variation)
- [Search visible product types](https://developers.ecwid.com/api-documentation/product-types#get-product-types)
- [Get visible product type details](https://developers.ecwid.com/api-documentation/product-types#get-product-type)
- [Get products and profile update stats](https://developers.ecwid.com/api-documentation/store-information#get-store-update-statistics)
- [Get deleted products statistics](https://developers.ecwid.com/api-documentation/store-information#get-deleted-items-statistics)
- [Create new orders](https://developers.ecwid.com/api-documentation/orders#create-order)
- [Calculate order details](https://developers.ecwid.com/api-documentation/carts#calculate-order-details)

<aside class='note'>To get public token for your application, specify <strong>public_storefront</strong> <a href="https://developers.ecwid.com/api-documentation/external-applications#access-scopes">access scope</a> in the oAuth process.</aside>

## App details page

To install any application from the Ecwid App Market, user would need to visit an app details page for that application first. Typical app details link in Ecwid Control Panel looks like this: 

`https://my.ecwid.com/cp/#apps:view=app&name=my-cool-app`

Where `my-cool-app` is the **appId** provided to you when you registered your application with us. App details page contains various information like app description, screenshots, pricing and other details. 

<aside class='notice'>
App details page is available for public apps registered for the Ecwid App Market only. In all other cases 404 error will be displayed when accessing app page.
</aside>

> Public app details page of Ecwid app for iOS

> [https://www.ecwid.com/apps/storemanagement/ecwid-iphone-app](https://www.ecwid.com/apps/storemanagement/ecwid-iphone-app)

User can install the application into their Ecwid store from this page and this process replaces the oAuth dialog, because user is already in the Ecwid Control Panel. After that, your app will request store permissions that you specified when you registered the application and proceed according to the type of the application (see below).

> To set up an app details page for your public application, see [Publishing Your App](/publishing-your-app) page for details.

The app installation works different for different app types: 

- Ecwid will redirect user to app tab in Ecwid Control Panel if it's a **Native app**
- Ecwid will redirect user to external website with temporary code if it's an **External app**
- Ecwid will not redirect user anywhere if no redirect is specified for the app
