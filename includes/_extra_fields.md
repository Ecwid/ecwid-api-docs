# Order extra fields

Order extra fields allow you to save additional information into the order and get it via Ecwid REST API. 

> Save an extra field 'platform' to order placed by customer

```js
// Place Ecwid integration code here
// ...

// Init order fields 
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Save a single value 'adobe_muse' for 'platform' field
ec.order.extraFields.platform = {
    'value': 'adobe_muse'
}
```

For example, if you need to save a cookie into an order or if you need to ask a customer a couple of questions at checkout – it's totally possible with the order extra fields. Later it can be retrieved via the Ecwid REST API when getting order details or searching for orders. 

**How it works**

Learn how to save private information about storefront or a customer into an order. 

1. [Modify Ecwid integration code to initialize the fields](#get-additional-info-from-a-customer)
2. [Set values for extra fields when customer visits a store](#get-additional-info-from-a-customer)
3. [Ecwid saves the values to order details when it gets placed](#get-additional-info-from-a-customer)
4. [Get saved info in order details in Ecwid REST API](#get-extra-fields-in-rest-api)

Check out more details on how to do this in the docs below.

## Get additional info from a customer

With order extra fields you can also request additional information from a customer in storefront and show user responses in order details.

This is helpful when a merchant needs to know some additional information like delivery date, delivery comments, company details and other. The responses will be shown in order details in Ecwid Control Panel. 

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
***title*** | string | Title of a new field. It prompts user to enter the value
**checkoutDisplaySection** | string | Accepts several values: `shipping_address`, `pickup_details`, `billing_address`, `order_comments`. More on this below
*orderDetailsDisplaySection* | string | Where the extra field will be shown to customer and merchant. Supported values: `shipping_info`, `billing_info`, `customer_info`, `order_comments`
*value* | string | Default value used to prefill the extra field's value for user
type | string | Accepts several values: `text`, `select`, `datetime`. Default is `text`. More on this below
tip | string | Show a tip for filling the field under the input
available | boolean | If `true`, the field is shown and saved as an extra field. If `false`, the field completely ignored. `true` is default
textPlaceholder | string | A text placeholder for `text` input field
selectOptions | Array\<*string*\> | Several options to choose from the `select` field. If `selectOptions` is not set, field is transformed to `text` type
required | boolean | `true` if required, `false` otherwise

<aside class="notice">
Parameters in <strong>bold</strong> are mandatory to show the extra field at checkout
</aside>

<aside class="notice">
Parameters in <em>italic</em> are mandatory to show the extra field to customer and merchant after order is placed
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

**To show the field and value to merchant and customer successfully, all requirements must be met**: 

- `title` is not empty 
- `value` is not empty
- `orderDetailsDisplaySection` contains supported value: `shipping_info`, `billing_info`, `customer_info`, `order_comments`

If `orderDetailsDisplaySection` contains an unsupported value, the field will still be saved in an order, but it will not be shown.

**Examples of other field types**

Check out the examples of other field types you can add to storefront on the right. 

> Various extra fields added to checkout examples

```html
<script>
// The field "how_did_you_find_us" asks user about how they found the store. Drop down type
ec.order.extraFields.how_did_you_find_us = {
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

// Hidden field, which is not shown at checkout
ec.order.extraFields.my_custom_field = {
    'value': 'abcd12345'
};
</script>
```

#### Step #3: Ecwid saves the values to order details when it gets placed

If your code executed successfully, these fields will be saved when a customer places their order in an Ecwid store. See below on how you can access them afterwards. 

## Save hidden data to order

With order extra fields you can save any custom information into an order privately, optionally showing it in order details in storefront to customer or in the Ecwid Control Panel to merchant. 

This is helpful when you need to save some technical stuff like campaign id, referring website address or something else. Check out more details on how it works below. 

#### Step #1: Modify Ecwid integration code to initialize the fields

> Initialize extra fields object

```html
<script>
// Initialize extra fields
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};
</script>
```

Your hidden data will be saved into a config object used by Ecwid: `ec.order` and `ec.order.extraFields`. But in order to use it, you need to initialize it on a page. 

See the example code on the right.

#### Step #2: Set values for fields when customer visits a store

> Save your extra field to an order placed by customer

```html
<script>
// Initialize extra fields
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Save a single value for 'platform'
ec.order.extraFields.platform = {
    'value': 'adobe_muse'
}
</script>
```

Now that we've initialized the extra fields object, we can save some value into an order. 

Code on the right is an example of adding an extra field with the key `platform` and value `adobe_muse`.  

After customer places that order, you will be able to get that field in the `extraFields` field when getting order details in REST API.  

#### Save hidden data to order and show it to merchant and customer in order details

> Save extra field and show it in order details to merchant 

```html
<script>
// Initialize Extra fields
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Save extra field and show it in order details in Ecwid Control Panel
ec.order.extraFields.referred_by = {
    'title': 'Referred by', // title of an extra field
    'value': 'Referrer is: Facebook Ads', 
    'orderDetailsDisplaySection': 'customer_info' // where to display the new field
}
</script>
```

To show the saved data from the storefront to merchant and customer in order details, use the example code on the right. 

After an order gets placed, you will be able to get that field in the `extraFields` when getting order details.

**To show the field to merchant and customer successfully, all requirements must be met**: 

- `title` is not empty 
- `value` is not empty
- `orderDetailsDisplaySection` contains supported value: `shipping_info`, `billing_info`, `customer_info`, `order_comments`. More on this below

If `orderDetailsDisplaySection` contains an unsupported value, the field will still be saved to an order, but it will not be shown.

#### Step #3: Ecwid saves the values to order details when it gets placed

If your code executed successfully, these fields will be saved when a customer places their order in an Ecwid store. See below on how you can access them afterwards. 

## Get extra fields in REST API

After customer placed their order, you can get the saved information using the Ecwid REST API in the order details.

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

## Customize extra fields 

Extra fields provide some options for customization. Let's see them in more details below. 

**Change extra field attribute based on selected shipping method**

> Show text input for pickup and regular shipping methods

```
<script>
// Initialize extra fields
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Add a new optional text input 'How should we sign the package?' to shipping address form
ec.order.extraFields.wrapping_box_signature = {
    'title': 'How should we sign the package?',
    'textPlaceholder': 'Package sign',
    'type': 'text',
    'tip': 'We will put a label on a box so the recipient knows who it is from',
    'orderDetailsDisplaySection': 'order_comments',
    'required': false,
    'available': true,
    'checkoutDisplaySection': 'shipping_address', // Default display section

    // Use 'overrides' to change extra field attributes based on condition
    'overrides': [
        {
            // If customer's selected shipping method meets the condition
            'conditions': {
                    'shippingMethod' : 'In-store Pickup'
                },
            // We overrid the field attribute 'checkoutDisplaySection'
            'fieldsToOverride': {
                'checkoutDisplaySection': 'pickup_details' // Display section used if condition is met
            }
        }
    ],
};
</script>
```

> Save different hidden data based on selected shipping method

```
<script>
// Initialize extra fields
ec.order = ec.order || {};
ec.order.extraFields = ec.order.extraFields || {};

// Save type of shipping selected to order as hidden data
ec.order.extraFields.shipping_type = {
    'value': 'none',
    'overrides' : [
        {
            'conditions': {
                'shippingMethod' : 'In-store Pickup'
            },
            'fieldsToOverride': {
                'value': 'pickup' // If selected shipping is 'In-store Pickup', we save 'pickup'
            }
        },
        {
            'conditions': {
                'shippingMethod' : 'Flat Rate'
            },
            'fieldsToOverride': {
                'value': 'flat rate' // If selected shipping is 'Flat Rate', we save 'flate rate'
            }
        }
    ]
}
</script>
```

You can change the attributes of an extra field based on a shipping method selected by customer. This is done by adding overriding rules to the existing extra field attributes – `overrides` array of conditions. 

At the moment you are able to use the selected shipping method name as a condition only. However, you can add several `conditions` to a single extra field, so you can use different override rules for different shipping methods selected. 

**Advanced settings for date picker extra field**

> Customize date and time selection in datepicker

```html
<script>
    // Customize time and date selection for order pickup datepicker
    ec.order.extraFields.pickup_time_select = {
        'title': 'Select date of pickup',
        'required': true,
        'type': 'datetime',
        'checkoutDisplaySection': 'pickup_details',
        'orderDetailsDisplaySection': 'order_comments',
        // Default date picker presets
        'datePickerOptions': {
            'minDate': new Date(new Date().getTime() + 2*60*60*1000), // Order is prepared for 2 hours minimum. Hiding 2 hours from the current time. Default is 0
            'maxDate': new Date(2020, 12, 31),
            'showTime': true,
            'autoClose': false,
            'use24hour': true,
            'incrementMinuteBy': 30,
            // limit available hours for each week day
            'limitAvailableHoursWeekly': {
                'MON': [
                    ['08:30', '13:30'],
                    ['14:00', '17:30']
                ],
                'TUE': [
                    ['14:00', '17:30']
                ],
                'WED': [
                    ['01:00', '13:30']
                ],
                'THU': [
                    ['14:00', '23:30']
                ],
                'FRI': [
                    ['14:00', '17:30']
                ]
            },
            // disallow specific dates
            'disallowDates': [
                // Same-day pickup can start from 3PM only
                ['2017-04-25 15:00:00', '2017-04-25 23:59:59'],

                // Custom dates (some interval has been reserved by other customer)
                ['2017-04-26 08:30', '2017-04-26 10:00']
            ]
        },

        // Change datepicker settings based on selected shipping method
        'overrides': [
            {
                'conditions': {
                    'shippingMethod': 'Pickup from 5th Avenue'
                },
                'fieldsToOverride': {
                    'datePickerOptions': {
                        'minDate': new Date(new Date().getTime() + 2*60*60*1000), // Order is prepared for 2 hours minimum. Hiding 2 hours from the current time. Default is 0
                        'maxDate': new Date(2020, 12, 31),
                        'showTime': true,
                        'autoClose': false,
                        'use24hour': true,
                        'incrementMinuteBy': 30,
                        'limitAvailableHoursWeekly': {
                            'MON': [
                                ['08:30', '13:30'],
                                ['14:00', '17:30']
                            ],
                            'TUE': [
                                ['14:00', '17:30']
                            ]
                        },

                        // disallow specific dates
                        'disallowDates': [
                            // Same-day pickup can start from 3PM only
                            ['2017-04-25 15:00:00', '2017-04-25 23:59:59'],

                            // Custom dates (some interval has been reserved by other customer)
                            ['2017-04-26 08:30', '2017-04-26 10:00']
                        ]
                    }
                }
            },

            {
                'conditions': {
                    'shippingMethod': 'Pickup from Times Square'
                },
                'fieldsToOverride': {
                    'datePickerOptions': {
                        'minDate': new Date(new Date().getTime() + 2*60*60*1000), // Order is prepared for 2 hours minimum. Hiding 2 hours from the current time. Default is 0
                        'maxDate': new Date(2020, 12, 31),
                        'showTime': true,
                        'autoClose': false,
                        'use24hour': true,
                        'incrementMinuteBy': 30,
                        // Limiting working hours for weekends
                        'limitAvailableHoursWeekly': {
                            'SAT': [
                                ['08:30', '13:30'],
                                ['14:00', '17:30']
                            ],
                            'SUN': [
                                ['14:00', '17:30']
                            ]
                        }
                    }
                }
            },

            {
                'conditions': {
                    'shippingMethod': 'Pickup from Empire State Building'
                },
                'fieldsToOverride': {
                    'available': false // Hide datepicker if shipping method is a 'Pickup from Empire State Building'
                }
            }
        ]
    };
</script>
```

You are also able to customize the date picker: what hours are available, what is the minimum time to choose, show or hide time selection and more. 

The example code on the right displays a new custom date picker field at shipping method step for In-store Pickup methods. Customizations applied:  

- it displays time selection in 24-hour format with 30 minute intervals
- the max date to select is 31st of December 2020
- minimum is 2 hours after the current date 
- the available pickup hours are limited by week day

Once an order is placed, this information will be displayed in the order comments section of order details for both customer and a merchant. 

As with the other extra fields, you can override this date picker settings based on the shipping method selected by a customer in storefront. See the `overrides` array of conditions and changes. 

**Datepicker attributes**

Field | Type |  Description
--------- | -----------| -----------
datePickerOptions | \<*DatePickerOptions*\> | Datepicker's customizable attributes

#### DatePickerOptions

Field | Type |  Description
--------- | -----------| -----------
minDate | Date | Min allowed date to select by customer
maxDate | Date | Max allowed date to select by customer
limitAvailableHoursWeekly | Array\<*LimitAvailableHoursWeekly*\> | Limit available hours for each day of the week. Values: `MON`, `TUE`, `WED`, `THU`, `FRI`, `SAT`, `SUN`
disallowDates | Array\<*DisallowDates*\> | Disallow several specific timeframes (from date - to date)
showTime | boolean | Use `true` to show time picker, `false` to hide it
incrementTimeBy | integer | Time intervals in minutes. If `30` is set, the times will be like: `12:30, 13:00, 13:30..`
use24HourFormat | boolean | Use `true` to use 24-hour format, `false` to use 12-hour (AM/PM) format
autoClose | boolean | Use `true` to close datepicker popup right after the day is selected (not time!). `false` to close popup only if customer clicks outside of it

#### LimitAvailableHoursWeekly

Specify time frames for each day of the week, multiple timeframes are available, see example on the right.

#### DisallowDates

Specify time frames which are not available to select, see example on the right.

## Extra fields examples and limits

### Examples

You can find other examples of more complex functionality on this page: [https://gist.github.com/makfruit/e410e1f0ab46656beda8df46c1666154](https://gist.github.com/makfruit/e410e1f0ab46656beda8df46c1666154)

Advanced customizations to extra fiels are also available in the section above.

### Limits

When saving information to an order, make sure your code follows these limits: 

1. Total storage 
All extra fields for order **must not exceed 8Kb of data in total**. If an order has over 8Kb of data saved, Ecwid will only save the data not exceeding the limit. 

2. Limit per field's attribute
The field's value, title or other attribute must not exceed a limit of `255` characters. If it exceeds the limit, the field **will not be saved to order**.
