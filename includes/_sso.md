# Single sign-on

# Overview

Single sign-on (SSO) is a tool that allows a user to have one login and password to access multiple applications. If you already have customer base on your site and you are using Ecwid, your customers may find it rather inconvenient that they have to log in to your site and store separately. Single sign-on API allows your customers to sign into your site and fully use your store without having to log in to Ecwid. I.e. if a customer is logged in to your site, he/she is logged in to your store automatically, even if he/she didn’t have an account in your store before.

# Implement SSO on your site

## Pass user data to Ecwid

To enable Single Sign-on (SSO) on your website, you need to pass encrypted user information to Ecwid JavaScript API. The user data itself needs to be prepared and encrypted on the server. See SSO Payload for the details. Then, in the code of your store web page, you should send the encrypted user data to Ecwid. There are two ways you can do that:

1. Declaring a `ecwid_sso_profile` JS variable on the store web page containing the user information
2. Dynamically sending a user info to Ecwid by means of the `Ecwid.setSsoProfile()` JS API method


### ecwid_sso_profile

> Send user information in an SSO payload to Ecwid

```js
...
var ecwid_sso_profile = '93n3ufncucndndner9872h34b43ird8djd8d8 8787aa437a4aaa3ds8787 1421317550';
...
```

You can inform Ecwid of the logged in user on your site in the `ecwid_sso_profile` JavaScript variable declared on your store pages. The `ecwid_sso_profile` variable can be declared anywhere in your web page, either before the Ecwid's script.js or after. The `ecwid_sso_profile` is initialized once – when Ecwid loads on the page.

### Ecwid.setSsoProfile()

> Sign in a user dynamically without page reloading by means of Ecwid.setSsoProfile()

```js
// This is supposed to be executed after a user is logged in on your site

// Retrieve SSO payload from the server
var ssoPayload = ... 

Ecwid.setSsoProfile(ssoPayload);
```

The abovementioned SSO approach with setting `ecwid_sso_profile` variable works well when login/logout actions on your site are done along with the page reload. If your site utilizes AJAX for login/logout functionality, the page won't be reloaded and thus `ecwid_sso_profile` will not be updated instantly. In this case, you can use `Ecwid.setSsoProfile()` API method. The `Ecwid.setSsoProfile()` method does exactly the same as setting of the `ecwid_sso_profile` variable, but can be used multiple times within the same "session".

Here is how you will use that:

- Handle all logins/logouts events on the page 
- When a user is logged in, send an AJAX request to your server to retrieve SSO payload
- Execute `Ecwid.setSsoProfile()` function and pass the retrieved SSO payload as parameter

<aside class="notice">
`Ecwid.setSsoProfile` works only when Ecwid is in SSO mode, that is, when the global variable ecwid_sso_profile is also defined.
</aside>


### When no one is logged in
To specify that no user is logged in, pass an empty payload either to the `ecwid_sso_profile` variable or to `Ecwid.setSsoProfile()` method. Ecwid will 'understand' that nobody is logged in at the moment and log out the current user if any.


## SSO payload

> Generate SSO payload and send it to Ecwid to log the user in Ecwid

```php
<?php
$sso_secret = "TEST";
$message = base64_encode("{appId:'123',userId:'234',profile:{email:'test@example.com'}}");
$timestamp = time();
$hmac = hash_hmac('sha1', "$message $timestamp", $sso_secret);
echo "<script> var ecwid_sso_profile = '$message $hmac $timestamp' </script>";
?>
```

When you passing the user information to Ecwid either in `ecwid_sso_profile` JS variable or with `Ecwid.setSsoProfile()` method, you need to create and sign the payload. SSO payload contains of the following parts connected with a space character:

- Message
- Signature
- Timestamp

### Message
The message is a Base64-encoded JSON string containing user profile fields and the application ID. See the `Message` field format explained below.

Field | Type | Description
------| ---- | -----------
**appId** | string | Any arbitrary string identifying the 3rd-party authentication system (your site). Example: `my_cool_website1`
**userId** | string | Any string identifying the user in the 3rd-party authentication system. Examples: `72551`, `johnsmith87`. The combination of appId and userId identifies the customer profile in Ecwid connected with your site. 
**profile** | *Profile* | User information. If a customer with the given combination of `appId` and `userId` already associated with some customer in your Ecwid store, the profile on file will be merged with the provided fields. Exception is the customer address book (`shippingAddresses`), which is only filled once when profile is created in Ecwid.

<aside class="notice">
Never change your `appId`! Changing appId of an existing authentication system will result in inability of registered customers to sign on your website, because new customers will have different appId/userId combination and conflict with existing database records with the same email.
</aside>

#### Profile
Field | Type  | Description
----- | ----- | -----------
email | string |  Customer email
billingPerson | *Person* | Customer's billing name/address
shippingAddresses | Array\<*Person*\> | Customer address book items
registered | number (UNIX timestamp) | Registration date

#### Person
Field | Type  | Description
----- | ----- | -----------
**name** | string | Customer full name
companyName | string | Customer company name
street | string | Street
city | string | City
countryCode | string | Country code (2-letter code)
countryName | string | Country name
postalCode | string | Postal code (zip code)
stateOrProvinceCode | string | State/province code
phone | string | Phone number


### Signature
The signature should be generated by HMAC SHA1 encryption of the Base64-encoded profile data and timestamp. Use your **SSO secret key** as a signature key. The secret key can be obtained in the user's [Ecwid Control panel](https://my.ecwid.com/cp/CP.html#legacy_api).

Every message should have different signature. If a message has a signature that has already been seen recently, Ecwid denies the sign-on request.

### Timestamp
UNIX timestamp. The system clock on your server must be in sync with the actual time, otherwise signatures generated by your server will fail to validate in Ecwid. Ecwid allows this timestamp to be up to 10 minutes late. The timestamp ensures that the message cannot be intercepted and misused later.



## Customize SSO workflow

### Add log in link to the store in SSO mode

> Enable Sign in / Sign out links

```js
Ecwid.OnAPILoaded.add(function() {
    Ecwid.setSignInUrls({
        signInUrl: 'http://my.site.com/signin',
        signOutUrl: 'http://my.site.com/signout' // signOutUrl is optional
    });
});
```

By default, when SSO is used on a web page, Ecwid hides Sign in and Sign out links in the storefront assuming that the user logs in by means of your site login form. We recommend adding Sign in / Sign out links to your store and link them to your site login form to make login process more convenient for your customers. 

This can be done by means of `Ecwid.setSignInUrls()` API extension, which accepts two parameters:

Field | Type  | Description
----- | ----- | -----------
**signInUrl** | string | URL where your customer will be redirected to log in to your site
signOutUrl | string | URL where your customer will be redirected, when they click the 'Log out' link in the store. This parameter is optional. If not specified, Ecwid will not show the ’sign out’ link. 

When you use `setSignInUrls()` extension, the customer will be redirected to the specified URL in the following situations:

- They click the 'Sign in' link in the product browser
- They open a secured page inside Product Browser (e.g. ’My account’ page)
- They go to checkout and your store settings require a user to be registered to place an order


### Custom Sign in / Sign out handlers

> Customizing of Sign in / Sign out functionality in SSO mode

```js
Ecwid.OnAPILoaded.add(function() {
  Ecwid.setSignInProvider({
    addSignInLinkToPB: function() { return true; },
    signIn: function () { 
      alert('User is logging in!');
      // do something
    },

    canSignOut: function() { return true; },
    signOut: function () { 
      alert('User is signing out');
    }
  });
});
```

Ecwid SSO API provides a tool for advanced customization of the login functionality in Single Sign-on mode – `Ecwid.setSignInProvider()` extension. 

Field | Type  | Description
----- | ----- | -----------
**addSignInLinkToPB** | function returning true or false | Set it to return `true` if you need to show 'Sign in' link inside your store
**signIn** | function | Specify function to be called when Ecwid is trying to authorize the customer, when either the 'Sign in' link is clicked or a secured page is opened inside Product Browser. This function does not accept any arguments nor should return any result.
**canSignOut** | function returning true or false | Set it to return `true` if you need to show 'Sign out' link inside your store
**signOut** | function | This function is called when `canSignOut` returns true and the customer clicks the 'Sign out' link inside the store widget

<aside class="notice">
`Ecwid.setSsoProfile` works only when Ecwid is in SSO mode, that is, when the global variable ecwid_sso_profile is also defined.
</aside>

## Notes

- Ecwid does not allow two customers with the same email address in one store. If a customer with the same email already exists and has different SSO appId/userId, or does not have them at all, then Ecwid will fail to create a customer account and will behave as if no user is logged in. For example, If you have a customer `customer@example.com` from Facebook, you cannot have another `customer@example.com` signing in using SSO. Ecwid will simply ignore `customer@example.com` passed in to SSO.
- SSO API is available for [paid Ecwid users only](http://www.ecwid.com/pricing). If an Ecwid store has no paid subscription, Ecwid stops processing SSO requests for that store and will behave as if no user is logged in.
- Never change your appId! Changing appId of an existing authentication system will result in inability of registered customers to sign on your website, because new customers will have different appId/userId combination and conflict with existing database records with the same email.


## SSO implementation examples

> Ecwid SSO API implementation example

```html
<html><body>
<script>
<?php
if (!$_REQUEST['logoff']) {
        $profile = array(
                'appId' => "My cool app",
                'userId' => "234",
                'profile' => array(
                        'email' => "test@example.com",
                        'billingPerson' => array(
                                'name' => "Tester",
                                'companyName' => "Company Name",
                                'street' => "Street",
                                'city' => "City",
                                'countryCode' => "US",
                                'postalCode' => "10001",
                                'stateOrProvinceCode' => "NY"
                        )
                )
        );
        $message = json_encode($profile);
        $message = base64_encode($message);
        $timestamp = time();
        $hmac = hash_hmac('sha1', "$message $timestamp", "A1Lu7ANIhKD6");
        echo "var ecwid_sso_profile='$message $hmac $timestamp'";
} else {
        echo "var ecwid_sso_profile=''";
}
?>
</script>
<script src="http://app.ecwid.com/script.js?1003"></script>
<script>
xProductBrowser();
function logout() {
        window.Ecwid.OnAPILoaded.add(function() {
                window.Ecwid.setSsoProfile('');
        });
}
</script>
<a href="javascript: logout()">Log out</a>
</body></html>
```

### PHP
See the example here on the right side of the page

### VB.Net
Find an example here: [https://github.com/balajiselcom/Ecwid](https://github.com/balajiselcom/Ecwid) (thanks to Balaji Sridharan)

### Wordpress
Ecwid official Wordpress plugins uses SSO to sync Wordpress site users with customers in an Ecwid store. You can find the code here: [https://github.com/Ecwid/ecwid-wordpress-plugin](https://github.com/Ecwid/ecwid-wordpress-plugin)