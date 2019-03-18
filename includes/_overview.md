# Overview

This documentation describes various ways to customize Ecwid stores by using the Ecwid API Platform. 

Each section has a detailed description as well as code examples to help you create a public app or customize your own store.

**Customize your Ecwid store**

There are many ways to customize a store with the Ecwid API. 

Check this page to get started: [Customizing your Ecwid store](https://developers.ecwid.com/customize-your-ecwid-store)

**Create a public app**

You can use Ecwid API to create apps which help Ecwid merchants and earn money from it. 

Check this page to get started: [Creating an app for the Ecwid App Market](https://developers.ecwid.com/)

## API features

#### REST API

Ecwid REST API allows your application to manage Ecwid store on behalf of an Ecwid user. Create products, update orders, delete a customer and many many more.

Check out all available methods: [REST API](https://developers.ecwid.com/api-documentation/rest-api-reference)

#### Native Apps

Will your app have a settings page for a user to update them quickly? Place it right in Ecwid Control Panel! It is a great way to put your app up front to the users and let them use if more often.

More information: [Native applications](https://developers.ecwid.com/api-documentation/native-applications)

#### Webhooks

Webhooks allow you to get an instant notification after an event ocurred in an Ecwid store. Send information to accounting right after an order was placed, send a note after a product was updated - it's totally up to you.

More information: [Webhooks](https://developers.ecwid.com/api-documentation/webhooks)

#### Order Extra Fields

Save technical information about an order in its details or add new fields to the store checkout process with Order Extra Fields. 

More information: [Order Extra Fields](https://developers.ecwid.com/api-documentation/order-extra-fields)

#### Add Shipping Methods

Custom Shipping API allows you to add new shipping methods in an Ecwid store as if they were available out of the box. Merchant can configure the integration in Ecwid Control Panel and new shipping methods will be displayed at checkout for store customers.

More information: [Custom Shipping API](https://developers.ecwid.com/api-documentation/add-shipping-method)

> Find out more about Ecwid Platform:

> [https://developers.ecwid.com/intro](https://developers.ecwid.com/intro)

#### Add Payment Method

Custom Payment API allows you to add new payment methods to an Ecwid store. Merchant will install your app, set it up and they will be able to accept payments through your payment system for orders in their store.

More information: [Custom Payment API](https://developers.ecwid.com/api-documentation/add-payment-method)

#### Apply Custom Discount

Custom Discount API allows you to apply a custom discount amount to an order being placed at the moment. Merchant can configure the discount rules in their Ecwid Control Panel using the Native apps feature and customers will have discounts on their orders according to those rules at checkout.

More information: [Custom Discount API](https://developers.ecwid.com/api-documentation/add-custom-discount)

#### Application Storage

Application storage is a key/value storage that can serve as a database for your application and a way to send details to user's storefront. All data is kept on Ecwid servers, so you don't need to worry about keeping the data safe. 

More information: [Application storage](https://developers.ecwid.com/api-documentation/application-storage)

#### Storefront customization

Ecwid has a beautifully designed storefront that is aimed to direct customers to make a purchase in a store. If you want to customize how it looks or its logic - you can do that too: attach an external Javascript file or adapt a store to make it stylish. 

More information: [Customize Storefront](https://developers.ecwid.com/api-documentation/customize-storefront)

#### Storefront Single Sign-On

Ecwid is a widget, that can be inserted to any website. Many websites have their own login functionality as well as Ecwid. To allow customers to sign in to their account in storefront automatically right after they sign in to merchant's website or service, use Single Sign-On feature. It will save customers' time and let them concentrate on shopping in the store.

More information: [Storefront Single Sign-On](https://developers.ecwid.com/api-documentation/single-sign-on)

## Access Ecwid API Platform

> Check out app examples

> [https://developers.ecwid.com/examples](https://developers.ecwid.com/examples)

**All modifications** to Ecwid stores, incl. REST API, Webhooks, Payment and Shipping methods and others, **are done by applications**. [Register](/register) your app to get started. 

There are two types of apps for Ecwid: **Native** and **External**. Learn about each of them below. 

Both app types are hosted on external services and Ecwid only stores URLs to corresponding pages: for webhooks, for payment and shipping requests and others.

### Native apps

Native applications work inside of Ecwid Control Panel in a separate tab, just like Shipping or Design Settings. 

This puts your app right into the day-to-day workflow of an Ecwid store owner and it is a great way to design an app for Ecwid. Using Ecwid JS SDK it's very easy to get access token for a store.

More details: [Native apps](https://developers.ecwid.com/api-documentation/native-applications)

### External apps

External applications work outside of Ecwid Control panel and user would interact and control them on a separate website. 

This approach is great for applications that provide extensive feature set for store owners and would require implementing OAuth flow to get access token for a specific Ecwid store. 

More details: [External apps](https://developers.ecwid.com/api-documentation/external-applications)

### Use cases

**Social marketing**

Create an application that will get product details from an Ecwid store and provide user option to share and schedule posts to social media accounts with products from their Ecwid store. 

Suggested API requests: 

- [Get store profile](https://developers.ecwid.com/api-documentation/store-information#get-store-profile) to set up account
- [Search for products](https://developers.ecwid.com/api-documentation/products#search-products) to save in your database

**Product marketplace**

Get product details from an Ecwid store to display them on a marketplace. Use webhooks to notify marketplace about changes of a product. 

Suggested API features:

- [Get store profile](https://developers.ecwid.com/api-documentation/store-information#get-store-profile) to set up account
- [Get product details](https://developers.ecwid.com/api-documentation/products#get-a-product) to save in your database

**Fulfillment service**

Synchronize Ecwid catalog with the fulfillment service of your choice. Update stock levels in both systems based on new orders in the store. 

Suggested API features:

- Use [Webhooks](https://developers.ecwid.com/api-documentation/webhooks) to get notifications about orders
- [Get order details](https://developers.ecwid.com/api-documentation/orders#get-order-details) to send information to 3rd party system
- [Check current stock levels](https://developers.ecwid.com/api-documentation/products#search-products) of products
- [Update stock levels](https://developers.ecwid.com/api-documentation/products#update-a-product) of products 

**Affiliate program**

Get information about new orders and provide commissions to your ambassadors. Use webhooks to get order details after it was placed. 

Suggested API features: 

- [Get store profile](https://developers.ecwid.com/api-documentation/store-information#get-store-profile) to set up account
- Use [Webhooks](https://developers.ecwid.com/api-documentation/webhooks) to get notifications about orders
- [Get order details](https://developers.ecwid.com/api-documentation/orders#get-order-details) to send information to affiliate system

**Custom content in storefront**

Create an app to show custom content in Ecwid storefront. Store owner can update it using your app interface in Ecwid Control Panel. 

Suggested API features: 

- [Add your app](https://developers.ecwid.com/api-documentation/embedded-apps) to Ecwid Control Panel to show its settings page
- [Save and pass user preferences to storefront](https://developers.ecwid.com/api-documentation/application-storage#public-application-config) script
- [Append your JS and CSS file](https://developers.ecwid.com/api-documentation/customize-storefront) to storefront

**Membership system for customers**

Use customer groups in the store as a tool to manage memberships. Provide discounts to regular shoppers like absolute discount or free shipping. 

Suggested API features:

- [Get information about all customers](https://developers.ecwid.com/api-documentation/customers#search-customers) to save in 3rd party service
- [Control customer groups](https://developers.ecwid.com/api-documentation/customer-groups) in the store
- [Get existing discount coupons](https://developers.ecwid.com/api-documentation/discount-coupons#search-coupons) details in Ecwid store
- [Update customers' details](https://developers.ecwid.com/api-documentation/customers#update-customer) to change their customer group
- [Update discount coupons](https://developers.ecwid.com/api-documentation/discount-coupons#update-coupon) in the store

**Storefront Themes**

Customizing Ecwid's design never been easier - apply your CSS file to all storefronts of a specific store. Help Ecwid users adapt the design of their storefront for their business or a website. 

Suggested API features:

- [Apply your custom CSS](https://developers.ecwid.com/api-documentation/look-and-feel#apply-custom-css) file for Ecwid storefront

Learn more on how to create a theme for Ecwid: [https://developers.ecwid.com/how-to-create-a-theme-for-an-ecwid-store](https://developers.ecwid.com/how-to-create-a-theme-for-an-ecwid-store)

## API availability on Ecwid plans

[Ecwid pricing](http://www.ecwid.com/pricing) includes four tiers: Free, Venture, Business, Unlimited. The API is available on **paid** Ecwid plans, i.e. on Venture, Business and Unlimited. 

Merchants on Free plans cannot use Ecwid API. This means that, if the user is on Free plan, all API functions will fail including requests to read or update store data, embedding of the app interfaces into Ecwid Control Panel and customizing storefront. 

### Q: I need to start developing my app. How can I get a paid account?

If you’re building an app for Ecwid App Market, we would be happy to provide you with a paid Ecwid account for free for development and testing purposes. 

Feel free to [contact us](http://developers.ecwid.com/contact), provide the details of your application and we will help you.

Please note that this account must not be used for selling the application or any other products to real customers.

## API playground

To quickly learn Ecwid API and see it in action even before you start development of your application, you can use Ecwid API playground. 

The playground is basically a collection of presets for [PostMan](http://www.getpostman.com/). You can load it in PostMan application and try accessing and managing your store data via API. 

Using the playground is easy. All you will need is:

* "PostMan" Chrome extension (it's free). You can download it [here](https://chrome.google.com/webstore/detail/postman-rest-client-packa/fhbjgbiflinjbdggehcddcbncdddomop)
* An Ecwid store which has access to API (the store needs to be on a paid plan)

To get more details and install the playground, please open this page: [api-playground.ecwid.com](https://api-playground.ecwid.com/)

<img src="https://dj925myfyz5v.cloudfront.net/wp-content/uploads/postman_api-1485266906.png"></img>
