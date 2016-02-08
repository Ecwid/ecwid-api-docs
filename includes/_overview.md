# Overview

The documentation below describes various ways to interact with Ecwid store using API. In each section you can find detailed description as well as code examples for managing store data to help you create your own application.

To find out more about Ecwid platform and get some basic details about it, please visit [Get started](http://developers.ecwid.com/get-started) page.

# API features

### REST API

Ecwid REST API allows your application to manage Ecwid store on behalf of an Ecwid user. Create products, update orders, delete a customer and many many more.

Check out all available methods: [REST API](#rest-api-reference)

### Webhooks

Webhooks allow you to get an instant notification after an event ocurred in an Ecwid store. Send information to accounting right after an order was placed, send a note after a product was updated - it's totally up to you.

More information: [Webhooks](#webhooks)

### Application Storage

Application storage is a key/value storage that can serve as a database for your application and a way to send details to user's storefront. All data is kept on Ecwid servers, so you don't need to worry about keeping the data safe. 

More information: [Application storage](#application-storage)

### Customize Storefront

Ecwid has a beautifully designed storefront that is aimed to direct customers to make a purchase in a store. If you want to customize how it looks or its logic - you can do that too: attach an external Javascript file or adapt a store to make it stylish. 

More information: [Customize Storefront](#customize-storefront)

### Native Apps

Will your app has a settings page for a user to update them quickly? Place it right in Ecwid Control Panel! It is a great way to put your app up front to the users and let them use if more often.

More information: [Native applications](#embedded-apps)

### Single Sign-On

Ecwid is a widget, that can be inserted to any website. Many websites have their own login functionality as well as Ecwid. You can use Single Sign-On To allow your customers to sign in to their account in Ecwid automatically right after they sign in to your website. It will save your customers' time and let them concentrate on shopping in your store.

More information: [Single Sign-On](#single-sign-on)

# Applications for Ecwid

In Ecwid there can be two main categories of applications: Native and External apps. 

### Native applications

Native applications work inside of Ecwid Control Panel in a separate tab, just like Shipping Settings or Design. This way of embedding your app into user interface puts your app right before the day-to-day workflow of an Ecwid store owner and it is a great way to design an app for Ecwid. Using Ecwid JS SDK it's very easy to get access token for a store.

More details: [Native apps](#native-applications).

### External applications

External applications work outside of Ecwid Control panel and user would interact and control them on a separate website. This approach is great for applications that provide extensive feature set for store owners and would require implementing oAuth flow to get access token for a specific Ecwid store.

More details: [External apps](#external-applications).

### Use cases

**Social marketing**

Create an application that will get product details from an Ecwid store and provide user option to share and schedule posts to social media accounts with products from their Ecwid store. 

Suggested permissions: *read_catalog*, *read_store_profile*

**Product marketplace**

Get product details from an Ecwid store to display them on a marketplace. Use webhooks to notify marketplace about changes of a product. 

Suggested permissions: *read_catalog*, *read_store_profile*

**Fulfillment service**

Synchronize Ecwid catalog with the fulfillment service of your choice. Update stock levels in both systems based on new orders in the store. 

Suggested permissions: *read_orders*, *read_catalog*, *update_catalog*

**Affiliate program**

Get information about new orders and provide commissions to your ambassadors. Use webhooks to get order details after it was placed. 

Suggested permissions: *read_orders*

**Custom text in storefront**

Create an app to show custom text in Ecwid storefront. Store owner can update it using your app interface in Ecwid Control Panel. 

Suggested permissions: *update_store_profile*, *add_to_cp*, *customize_storefront*

**Membership system for customers**

Use customer groups in the store as a tool to manage memberships. Provide discounts to regular shoppers like absolute discount or free shipping. 

Suggested permissions: *read_customers*, *read_discount_coupons*, *update_customers*, *update_discount_coupons*

**Storefront Themes**

Customizing Ecwid's design never been easier - attach your CSS file to all storefronts of a specific store. Help Ecwid users adapt the design of their storefront for their business or a website. 

Suggested permissions: *customize_storefront*

# API availability on Ecwid plans
[Ecwid pricing](http://www.ecwid.com/pricing) includes four tiers: Free, Venture, Business, Unlimited. The API is available on **paid** Ecwid plans, i.e. on Venture, Business and Unlimited. Merchants on Free plans cannot use Ecwid API. This means that, if the user is on Free plan, all API functions will fail including requests to read or update store data, embedding of the app interfaces into Ecwid Control Panel and customizing storefront. 

### Q: I need to start developing my app. How can I get a paid account?

If you’re building an app for Ecwid App Market, we would be happy to provide you with a paid Ecwid account for free for development and testing purposes. Feel free to [contact us](http://developers.ecwid.com/contact), provide the details of your application and we will help you.

Please note that this account must not be used for selling the application or any other products to real customers.
