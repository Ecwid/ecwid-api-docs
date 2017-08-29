# Order extra fields

Order extra fields allow you to save additional information into the order and get it via Ecwid REST API. 

For example, if you need to save a cookie into an order or if you need to ask a customer a couple of questions at checkout – it's totally possible with the order extra fields.

### How it works

Learn how to save private information about storefront or a customer into an order. 

1. Modify Ecwid integration code to initialize the fields
2. Set values for fields when customer visits a store
3. Ecwid saves the values to order details when it gets placed
4. Get saved info in order details Ecwid REST API

Check out more details on how to do this in the docs below. 

#### Limits

When saving information to an order, make sure your code follows these limits: 

1. Total storage 
All extra fields for order **must not exceed 8Kb of data in total**. If an order has over 8Kb of data saved, Ecwid will only save the data not exceeding the limit. 

2. Limit per field's attribute
The field's value, title or other attribute must not exceed a limit of `255` characters. If it exceeds the limit, the field **will not be saved to order**.

## Save private data to order

With order extra fields you can save any custom information into an order privately, optionally showing it in order details in storefront to customer or in the Ecwid Control Panel to merchant. 

This is helpful when you need to save some technical stuff like campaign id, referring website address or something else. Check out more details on how it works below. 

### Saving private data in storefront

#### Step #1: Modify Ecwid integration code to initialize the fields

> Initialize extra fields object

```js
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};
```

Your private data will be saved into a config object used by Ecwid: `ec.order` and `ec.order.extraFields`. But in order to use it, you need to initialize it on a page. 

See the example code on the right.

#### Step #2: Set values for fields when customer visits a store

> Save your extra field to an order placed by customer

```js
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Save a single value for 'platform'
ec.order.extraFields.platform = {
	'value': 'adobe_muse'
}
```

Now that we've initialized the extra fields object, we can save some value into an order. 

Code on the right is an example of adding an extra field with the key `platform` and value `adobe_muse`.  

After customer places that order, you will be able to get that field in the `extraFields` field when getting order details.  

#### Save private data to order and show it to merchant and customer in order details

> Save extra field and show it in order details to merchant 

```js
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Save extra field and show it in order details in Ecwid Control Panel
ec.order.extraFields.referred_by = {
	'title': 'Referred by', // title of an extra field
	'value': 'Referrer is: Facebook Ads', 
	'orderDetailsDisplaySection': 'customer_info' // where to display the new field
}
```

To show the saved data from the storefront to merchant and customer in order details, use the example code on the right. 

After an order gets placed, you will be able to get that field in the `extraFields` when getting order details.

**To show the field successfully, all requirements must be met**: 

- `title` is not empty 
- `value` is not empty
- `orderDetailsDisplaySection` contains supported value: `shipping_info`, `billing_info`, `customer_info`, `order_comments`. More on this below

If `orderDetailsDisplaySection` contains an unsupported value, the field will still be saved in an order, but it will not be shown to merchant.

#### Step #3: Ecwid saves the values to order details when it gets placed

If your code executed successfully, these fields will be saved when a customer places their order in an Ecwid store. See below on how you can access them afterwards. 

### Get saved private data from order

After customer placed their order, you can get the saved information using the Ecwid REST API in the order details.

#### Step #4: Get saved info in order details from Ecwid REST API

> Get extra fields in order details

> 1) Get order details 

```http
GET /api/v3/4870020/orders/20?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
``` 

> 2) Find your saved information in 'extraFields' field

```json
{
	"total": 12.35,
	"orderNumber": 104,
	//...
	"extraFields": {
		"referred_by": "Referrer is: Facebook Ads"
	}
}
```

The information you saved earlier is available in the `extraFields` field in the order details. You can get order details via Ecwid REST API with [searching for orders](https://developers.ecwid.com/api-documentation/orders#search-orders) or [getting a specific order details](https://developers.ecwid.com/api-documentation/orders#get-order-details) methods. 

If you specified the section to display that extra field, it will be shown to customer when they view the placed order and to a merchant when they view order details in the Ecwid Control Panel. 

## Get additional info from a customer

With order extra fields you can also requrest additional information from a customer in storefront and show user responses in order details.

This is helpful when a merchant needs to know some additional information like delivery date, delivery comments, company details and other. The responses will be shown in order details in Ecwid Control Panel. 

### Add new fields at checkout

Let's learn how to add new fields at a store's checkout process: 

#### Step #1: Modify Ecwid integration code

> Initialize extra fields object

```js
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};
```
The extra fields you add to checkout will be saved into a config object used by Ecwid: `ec.order` and `ec.order.extraFields`. 

Start by initializing the objects using the code on the right. 

#### Step #2: Create new fields at checkout

> Init and display your custom field at checkout

```js
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Add a new optional text input 'How should we sign the package?' to shipping address form
ec.order.extraFields.wrapping_box_signature = {
	'title': 'How should we sign the package?',
	'textPlaceholder': 'Package sign',
	'type': 'text',
	'tip': 'We will put a label on a box so the recipient knows who it is from',
	'required': false,
	'checkoutDisplaySection': 'shipping_address'
};
```

Now that we've initialized the extra fields object, we can create a new text input in the shipping address form at checkout. See example on the right. 

After an order gets placed, you will be able to get that field in the `extraFields` field when getting order details. Also, it will be visible to customer and merchant when viewing order details. 

**Extra field attributes**

An extra field at checkout has several attributes: 

Field | Type |  Description
--------- | -----------| -----------
**title** | string | Title of a new field. It prompts user to enter the value
**checkoutDisplaySection** | string | Accepts several values: `shipping_address`, `pickup_details`, `billing_address`, `order_comments`. More on this below
type | string | Accepts several values: `text`, `select`, `datetime`. Default is `text`. More on this below
tip | string | Show a tip for filling the field under the input
available | boolean | If `true`, the field is shown and saved as an extra field. If `false`, the field completely ignored. `true` is default
value | string | Default value used to prefill the extra field's value for user
textPlaceholder | string | A text placeholder for `text` input field
selectOptions | Array\<*string*\> | Several options to choose from the `select` field. If `selectOptions` is not set, field is transformed to `text` type
required | boolean | `true` if required, `false` otherwise

<aside class="notice">
Parameters in bold are mandatory to show the extra field at checkout
</aside>

**Types of fields**

There are three types of fields: `text`, `select`, `datetime`

- `text` is a standard text input field. It can accept any text without validation. You can use `textPlaceholder` attribute to add a placeholder to your extra field of type `text`. If `value` is set, this value is prefilled for the field. 

- `select` is a drop down element. The options for it have to be set in `selectOptions` array. If `selectOptions` is not set, field is transformed to `text` type

- `datetime` is a date picker element. It can be customized – more on that below

**Checkout display section**

You can add an extra field for customers in several parts of the checkout: 

- `shipping_address` form at checkout (shipping select step). A new field will be added below the fields in the address form
- `pickup_details` form at checkout (shipping select step). If customer selects in-store pickup shipping method, a new field will be added below the fields in pickup details form
- `billing_address` form at checkout (payment select step). A new field will be added below the fields in the address form
- `order_comments` form at checkout (payment select step). A new field will be added below the order comments field 

If `checkoutDisplaySection` contains an unsupported value, the field will not be shown to a customer. If the corresponding form is not shown to customer (order comments are disabled, etc.) then the field will be ignored too, **even if it's required**.

**Examples of other field types**

Check out the examples of other field types you can add to storefront on the right. 

> Various extra fields added to checkout examples

```js
// The field "how_do_you_find_us" asks user about how they found the store. Drop down type
ec.order.extraFields.how_do_you_find_us = {
	'title': 'How did you find us?',
	'type': 'select',
	'required': false,
	'selectOptions': ['Google Ads', 'Friend told me', 'TV show', 'Other'],
	'value': 'TV show', // Default value
	'checkoutDisplaySection': 'order_comments'
};

// Add pickup time selection for customer
ec.order.extraFields.ecwid_pickup_time = {
	'title': '_msg_ShippingDetails.pickup.customer_header',
	'required': true,
	'type': 'datetime',
	'checkoutDisplaySection': 'pickup_details',
	'orderDetailsDisplaySection': 'order_comments',
}

// Hidden field not shown at checkout
ec.order.extraFields.my_custom_field = {
	'value': 'abcd12345'
};
```

#### Step #3: Ecwid saves the values to order details when it gets placed

If your code executed successfully, these fields will be saved when a customer places their order in an Ecwid store. See below on how you can access them afterwards. 

### Get saved user responses from order

After customer placed their order, you can get the saved information using the Ecwid REST API in the order details.

#### Step #4: Get user responses in order details from Ecwid REST API

> Get extra fields in order details

> 1) Get order details 

```http
GET /api/v3/4870020/orders/20?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
``` 

> 2) Find your saved information in 'extraFields' field

```json
{
	"total": 12.35,
	"orderNumber": 104,
	//...
	"extraFields": {
		"referred_by": "Referrer is: Facebook Ads"
	}
}
```

The information you saved earlier is available in the `extraFields` field in the order details. You can get order details via Ecwid REST API with [searching for orders](https://developers.ecwid.com/api-documentation/orders#search-orders) or [getting a specific order details](https://developers.ecwid.com/api-documentation/orders#get-order-details) methods. 

If you specified the section to display that extra field, it will be shown to customer when they view the placed order and to a merchant when they view order details in the Ecwid Control Panel. 

