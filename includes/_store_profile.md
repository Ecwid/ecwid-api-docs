# Store information

## Get store profile

> Request example

```http
GET /api/v3/4870020/profile?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/profile?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

### Response

> Response example (JSON)

```json
{
    "generalInfo": {
        "storeId": 4870020,
        "storeUrl": "http://www.example.com",
        "starterSite": {
            "ecwidSubdomain": "mysuperstore",
            "generatedUrl": "http://mysuperstore.ecwid.com/"
        }
    },
    "account": {
        "accountName": "John Smith",
        "accountNickName": "JSmith",
        "accountEmail": "example@example.com"
    },
    "settings": {
        "closed": false,
        "storeName": "My Super Store",
        "invoiceLogoUrl": "https://dpbfm6h358sh7.cloudfront.net/images/4870020/253584290.jpg",
        "emailLogoUrl": "https://dpbfm6h358sh7.cloudfront.net/images/4870020/298177033.jpg",
        "googleRemarketingEnabled": true,
        "googleAnalyticsId": "UA-123456-1",
        "orderCommentsEnabled": false,
        "orderCommentsCaption": "Order comments",
        "orderCommentsRequired": false
    },
    "mailNotifications": {
        "adminNotificationEmails": [
            "john@example.com",
            "steve@example.com",
        ],
        "customerNotificationFromEmail": "john@example.com"
    },
    "company": {
        "companyName": "My Super Store",
        "email": "example@example.com",
        "street": "W 3d st",
        "city": "New York",
        "countryCode": "US",
        "postalCode": "10001",
        "stateOrProvinceCode": "NY",
        "phone": "234324234"
    },
    "formatsAndUnits": {
        "currency": "USD",
        "currencyPrefix": "$",
        "currencySuffix": "",
        "currencyGroupSeparator": " ",
        "currencyDecimalSeparator": ".",
        "currencyTruncateZeroFractional": false,
        "currencyPrecision": 2,
        "currencyRate": 1,
        "weightUnit": "KILOGRAM",
        "weightGroupSeparator": " ",
        "weightDecimalSeparator": ".",
        "weightTruncateZeroFractional": false,
        "timeFormat": "hh:mm a",
        "dateFormat": "MMM d, yyyy",
        "timezone": "Europe/Moscow"
    },
    "languages": {
        "enabledLanguages": [
            "en",
            "es",
            "pt"
        ],
        "facebookPreferredLocale": "en_US"
    },
    "shipping": {
        "handlingFee": {
            "name": "Handling Fee",
            "value": 5,
            "description": "Silk paper wrapping"
        }
    },
    "zones": [
        {
            "id": "1",
            "name": "United States",
            "countryCodes": [
                "US",
                "UM",
                "VI"
            ]
        },
        {
            "id": "2",
            "name": "US & Canada",
            "countryCodes": [
                "CA",
                "US",
                "UM",
                "VI"
            ]
        },
        {
            "id": "3",
            "name": "Europe (EC)",
            "countryCodes": [
                "AT",
                "BE",
                "BG",
                "CY",
                "CZ",
                "DK",
                "EE",
                "FI",
                "FR",
                "DE",
                "GR",
                "HU",
                "IE",
                "IT",
                "LV",
                "LT",
                "LU",
                "MT",
                "NL",
                "PL",
                "PT",
                "RO",
                "SK",
                "SI",
                "ES",
                "SE",
                "GB"
            ]
        },
    ],
    "taxSettings": {
        "automaticTaxEnabled": true,
        "taxes": [
            {
                "id": 1485869949,
                "name": "Tax X",
                "enabled": true,
                "includeInPrice": false,
                "useShippingAddress": true,
                "taxShipping": false,
                "appliedByDefault": true,
                "defaultTax": 7,
                "rules": [
                    {
                        "zoneId": "3",
                        "tax": 8
                    },
                    {
                        "zoneId": "2",
                        "tax": 5
                    }
                ]
            }
        ]
    },
    "businessRegistrationID": {
        "name": "VAT Reg No",
        "value": "GB999 9999 73"
    }
}
```

A JSON object of type 'Profile' with the following fields:

#### Profile
Field | Type | Description
----- | ---- | -----------
generalInfo | \<GeneralInfo\> | Store basic data
account | \<Account\> | Store owner's account data
settings | \<Settings\> | Store general settings
mailNotifications | \<MailNotifications\> | Mail notifications settings
company | \<Company\> | Company info
formatsAndUnits | \<FormatsAndUnits\> | Store formats/untis settings
languages | \<Languages\> | Store language settings
shipping | \<Shipping\> | Store shipping settings (only handling fee is included at the moment)
taxSettings | \<TaxSettings\> | Store taxes settings
zones | Array\<Zone\> | Store destination zones
businessRegistrationID | \<BusinessRegistrationID\> | Company registration ID, e.g. VAT reg number or company ID, which is set under Settings / Invoice in Control panel.

#### GeneralInfo
Field | Type | Description
----- | ---- | -----------
storeId | number | Ecwid Store ID
storeUrl | string | Storefront URL
starterSite | \<StarterSiteInfo\> | Starter Site settings

#### Account
Field | Type | Description
----- | ---- | -----------
accountName | string | Full store owner name
accountNickName | string | Store owner nickname on the Ecwid forums
accountEmail | string | Store owner email
availableFeatures | Array\<string\> | A list of the premium features available on the store's pricing plan


#### Settings
Field | Type | Description
----- | ---- | -----------
closed | boolean | `true` if the store is closed for maintenance, `false` otherwise
storeName | string | The store name displayed in Starter Site
invoiceLogoUrl | string | Company logo displayed on the invoice
emailLogoUrl | string | Company logo displayed in the store email notifications
googleRemarketingEnabled | boolean | `true` if Remarketing with Google Analytics is enabled, `false` otherwise
googleAnalyticsId | string | [Google Analytics ID](https://help.ecwid.com/customer/en/portal/articles/1170264-google-analytics) connected to a store
orderCommentsEnabled | boolean | `true` if order comments feature is enabled, `false` otherwise
orderCommentsCaption | string | Caption for order comments field in storefront
orderCommentsRequired | boolean | `true` if order comments are required to be filled, `false` otherwise

#### MailNotifications
Field | Type | Description
----- | ---- | -----------
adminNotificationEmails | Array\<string\> | Email addresses, which the store admin notifications are sent to
customerNotificationFromEmail | string | The email address used as the 'reply-to' field in the notifications to customers

#### Company
*System Settings → General → Store Profile*

Field | Type | Description
----- | ---- | -----------
companyName | string | The company name displayed on the invoice
email | string | Company (store administrator) email
street | string | Company address. 1 or 2 lines separated by a new line character
city    | string  | Company city
countryCode | string  | A two-letter ISO code of the country
postalCode  | string  | Postal code or ZIP code
stateOrProvinceCode | string  | State code (e.g. `NY`) or a region name
phone | string  | Company phone number

#### FormatsAndUnits
*System settings → General → Formats & Units.*

Field | Type | Description
----- | ---- | -----------
currency | string | 3-letters code of the store currency (ISO 4217). Examples: `USD`, `CAD`
currencyPrefix | string |  Currency prefix (e.g. $)
currencySuffix | string | Currency suffix
currencyPrecision | number  | Numbers of digits after decimal point in the store prices. E.g. `2` ($2.99) or `0` (¥500).
currencyGroupSeparator | string | Price thousands separator. Supported values: space ` `, dot `.`, comma `,`  or empty value ``.
currencyDecimalSeparator |  string | Price decimal separator. Possible values: `.` or `,` 
currencyTruncateZeroFractional | boolean | Hide zero fractional part of the prices in storefront. `true` or `false` . 
currencyRate | number | Currency rate in U.S. dollars, as set in the merchant control panel
weightUnit |    string |    Weight unit. Supported values: `CARAT`, `GRAM`, `OUNCE`, `POUND`, `KILOGRAM`
weightPrecision | number | Numbers of digits after decimal point in weights displayed in the store
weightGroupSeparator | string | Weight thousands separator. Supported values: space ` `, dot `.`, comma `,`  or empty value ``
weightDecimalSeparator | string | Weight decimal separator. Possible values: `.` or `,` 
weightTruncateZeroFractional |  boolean | Hide zero fractional part of the weight values in storefront. `true` or `false` . 
dateFormat | string | Date format, e.g. `MMM d, yyyy`
timeFormat | string | Time format, e.g. `hh:mm a`
timezone | string | Store timezone, e.g. `Europe/Moscow`

#### Languages
*System Settings → General → Languages*

Field | Type | Description
----- | ---- | -----------
enabledLanguages | Array\<string\> | A list of enabled languages in the storefront. First language code is the default language for the store.
facebookPreferredLocale | string | Language automatically chosen be default in Facebook storefront (if any)

#### Shipping
*System Settings → Shipping*

Field | Type | Description
----- | ---- | -----------
handlingFee | \<HandlingFee\> | Handling fee settings

#### HandlingFee
*System Settings → Shipping → Handling Fee*

Field | Type | Description
----- | ---- | -----------
name | string | Handling fee name set by store admin. E.g. `Wrapping`
value | number | Hndling fee value
description | string | Handling fee description for customer


#### TaxSettings
*System Settings → Taxes*

Field | Type | Description
----- | ---- | -----------
automaticTaxEnabled | boolean | `true` if taxes are calculated automatically, `else` otherwise
taxes | Array\<Taxes\> | Manual tax settings for a store

#### Taxes
Field | Type | Description
----- | ---- | -----------
id | number | Unique internal ID of the tax
name |  string | Displayed tax name
enabled | boolean | Whether tax is enabled `true` / `false` 
includeInPrice | boolean | `true` if the tax rate is included in product prices. More details: [Taxes in Ecwid](http://help.ecwid.com/customer/portal/articles/1182159-taxes)
useShippingAddress | boolean | `true` if the tax is calculated based on shipping address, `false` if billing address is used
taxShipping | boolean | `true` is the tax applies to subtotal+shipping cost . `false` if the tax is applied to subtotal only
appliedByDefault | boolean | `true` if the tax is applied to all products. `false` is the tax is only applied to thos product that have this tax enabled
defaultTax | number | Tax value, in %, when none of the destination zones match
rules | Array\<TaxRule\>  | Tax rates

#### TaxRule
Field | Type | Description
----- | ---- | -----------
zoneId | string | Destination zone ID
tax | number | Tax rate for this zone in %

#### Zone
*System Settings → Zones*

Field | Type | Description
----- | ---- | -----------
id | string | Unique internal zone ID
name | string | Zone displayed name
countryCodes | Array\<string\> | Country codes this zone includes . 
stateOrProvinceCodes |  Array\<string\> | State or province codes the zone includes
postCodes | Array\<string\> |   Postcode (or zipcode) templates this zone includes. More details: [Destination zones in Ecwid](http://help.ecwid.com/customer/portal/articles/1163922-destination-zones)

#### BusinessRegistrationID
Field | Type | Description
----- | ---- | -----------
name | string | ID name, e.g. `Vat ID`, `P.IVA`, `ABN` 
value | string | ID value

#### StarterSiteInfo
*System Settings → General → Starter site*

Field | Type | Description
----- | ---- | -----------
ecwidSubdomain | string | Store subdomain on ecwid.com domain, e.g. `mysuperstore.ecwid.com`
customDomain | string | Custom Starter site domain, e.g. `www.mysuperstore.com`
generatedUrl | string | Starter Site generated URL, e.g. `http://mysuperstore.ecwid.com/`
storeLogoUrl | string | Starter Site logo URL


### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
500 | Cannot retrieve the coupon info because of an error on the server

## Update store profile

> Request example -- update general info

```http
PUT /api/v3/4870020/profile?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache

{
    generalInfo: {
        storeUrl: "http://www.example.com/store/"
    },
    settings: {
        closed: false,
        storeName: "My Cool Store",
        "googleRemarketingEnabled": false,
        "googleAnalyticsId": "UA-654321-1"
    },
    company: {
      companyName: "My Company, Inc",
      email: "store@example.com",
      street: "144 West D Street",
      city: "Encinitas",
      countryCode: "US",
      postalCode: "92024",
      stateOrProvinceCode: "CA",
      phone: "1(800)5555555"
    }
}
```

> Request example -- create destination zones

```http
PUT /api/v3/4870020/profile?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache

{
    "zones": [
        {
            "name": "United States - California",
            "countryCodes": [
                "US"
            ],
            "stateOrProvinceCodes": [
                "US-CA"
            ]      
        }
    ]
}
```

`PUT https://app.ecwid.com/api/v3/{storeId}/profile?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

### Request body

A JSON object of type 'Profile' with the following fields:

#### Profile
Field | Type | Description
----- | ---- | -----------
generalInfo | \<GeneralInfo\> | Store basic data
account | \<Account\> | Store owner's account data
settings | \<Settings\> | Store general settings
mailNotifications | \<MailNotifications\> | Mail notifications settings
company | \<Company\> | Company info
formatsAndUnits | \<FormatsAndUnits\> | Store formats/untis settings
languages | \<Languages\> | Store language settings
shipping | \<Shipping\> | Store shipping settings (only handling fee is included at the moment)
taxSettings | \<TaxSettings\> | Store taxes settings
zones | Array\<Zone\> | Store destination zones
businessRegistrationID | \<BusinessRegistrationID\> | Company registration ID, e.g. VAT reg number or company ID, which is set under Settings / Invoice in Control panel.

<aside class="notice">
All fields are optional. Omitted field will not be affected
</aside>

#### GeneralInfo
Field | Type | Description
----- | ---- | -----------
storeUrl | string | Storefront URL. If this field is empty in the store settings and omitted in the request, it will be automatically copied from the current Starter Site URL
starterSite | \<StarterSiteInfo\> | Starter Site settings

#### Account
Field | Type | Description
----- | ---- | -----------
accountName | string | Full store owner name
accountNickName | string | Store owner nickname on the Ecwid forums
accountEmail | string | Store owner email

#### Settings
Field | Type | Description
----- | ---- | -----------
closed | string | `true` if the store is closed for maintenance, `false` otherwise
storeName | string | The store name displayed in Starter Site
googleRemarketingEnabled | boolean | `true` if Remarketing with Google Analytics is enabled, `false` otherwise
googleAnalyticsId | string | [Google Analytics ID](https://help.ecwid.com/customer/en/portal/articles/1170264-google-analytics) connected to a store
orderCommentsEnabled | boolean | Use `true` to enable order comments feature, `false` otherwise
orderCommentsCaption | string | Caption for order comments field in storefront. If the value is empty, the default 'Order comments' caption will be used
orderCommentsRequired | boolean | Use `true` to require order comments to be filled, `false` otherwise

#### MailNotifications
Field | Type | Description
----- | ---- | -----------
adminNotificationEmails | Array\<string\> | Email addresses, which the store admin notifications are sent to
customerNotificationFromEmail | string | The email address used as the 'reply-to' field in the notifications to customers

#### Company
*System Settings → General → Store Profile*

Field | Type | Description
----- | ---- | -----------
companyName | string | The company name displayed on the invoice
email | string | Company (store administrator) email
street | string | Company address. 1 or 2 lines separated by a new line character
city    | string  | Company city
countryCode | string  | A two-letter ISO code of the country
postalCode  | string  | Postal code or ZIP code
stateOrProvinceCode | string  | State code (e.g. `NY`) or a region name
phone | string  | Company phone number

#### FormatsAndUnits
*System settings → General → Formats & Units.*

Field | Type | Description
----- | ---- | -----------
currency | string | 3-letters code of the store currency (ISO 4217). Examples: `USD`, `CAD`
currencyPrefix | string |  Currency prefix (e.g. $)
currencySuffix | string | Currency suffix
currencyGroupSeparator | string | Price thousands separator. Supported values: space ` `, dot `.`, comma `,`  or empty value ``.
currencyDecimalSeparator |  string | Price decimal separator. Possible values: `.` or `,` 
currencyTruncateZeroFractional | boolean | Hide zero fractional part of the prices in storefront. `true` or `false` . 
currencyRate | number | Currency rate in U.S. dollars, as set in the merchant control panel
weightUnit |    string |    Weight unit. Supported values: `CARAT`, `GRAM`, `OUNCE`, `POUND`, `KILOGRAM`
weightPrecision | number | Numbers of digits after decimal point in weights displayed in the store
weightGroupSeparator | string | Weight thousands separator. Supported values: space ` `, dot `.`, comma `,`  or empty value ``
weightDecimalSeparator | string | Weight decimal separator. Possible values: `.` or `,` 
weightTruncateZeroFractional |  boolean | Hide zero fractional part of the weight values in storefront. `true` or `false` . 
dateFormat | string | Date format, e.g. `MMM d, yyyy`
timeFormat | string | Time format, e.g. `hh:mm a`
timezone | string | Store timezone, e.g. `Europe/Moscow`

#### Languages
*System Settings → General → Languages*

Field | Type | Description
----- | ---- | -----------
enabledLanguages | Array\<string\> | A list of enabled languages in the storefront. Use first item to set default storefront language

#### Shipping
*System Settings → Shipping*

Field | Type | Description
----- | ---- | -----------
handlingFee | \<HandlingFee\> | Handling fee settings

#### HandlingFee
*System Settings → Shipping → Handling Fee*

Field | Type | Description
----- | ---- | -----------
name | string | Handling fee name set by store admin. E.g. `Wrapping`
value | number | Handling fee value. If handling fee is `0` then it's disabled.
description | string | Handling fee description for customer

#### TaxSettings
*System Settings → Taxes*

Field | Type | Description
----- | ---- | -----------
automaticTaxEnabled | boolean | `true` if taxes are calculated automatically, `else` otherwise
taxes | Array\<Taxes\> | Manual tax settings for a store

#### Taxes
Field | Type | Description
----- | ---- | -----------
id | number | Unique internal ID of the tax
name |  string | Displayed tax name
enabled | boolean | Whether tax is enabled `true` / `false` 
includeInPrice | boolean | `true` if the tax rate is included in product prices. More details: [Taxes in Ecwid](http://help.ecwid.com/customer/portal/articles/1182159-taxes)
useShippingAddress | boolean | `true` if the tax is calculated based on shipping address, `false` if billing address is used
taxShipping | boolean | `true` is the tax applies to subtotal+shipping cost . `false` if the tax is applied to subtotal only
appliedByDefault | boolean | `true` if the tax is applied to all products. `false` is the tax is only applied to thos product that have this tax enabled
defaultTax | number | Tax value, in %, when none of the destination zones match
rules | Array\<TaxRule\>  | Tax rates

#### TaxRule
Field | Type | Description
----- | ---- | -----------
**zoneId** | string | Destination zone ID
**tax** | number | Tax rate for this zone in %

#### Zone
*System Settings → Zones*

Field | Type | Description
----- | ---- | -----------
id | string | Unique internal zone ID
**name** | string | Zone displayed name
countryCodes | Array\<string\> | Country codes this zone includes
stateOrProvinceCodes |  Array\<string\> | State or province codes the zone includes
postCodes | Array\<string\> |   Postcode (or zipcode) templates this zone includes. More details: [Destination zones in Ecwid](http://help.ecwid.com/customer/portal/articles/1163922-destination-zones)

#### BusinessRegistrationID
Field | Type | Description
----- | ---- | -----------
name | string | ID name, e.g. `Vat ID`, `P.IVA`, `ABN` 
value | string | ID value

#### StarterSiteInfo
*System Settings → General → Starter site*

Field | Type | Description
----- | ---- | -----------
ecwidSubdomain | string | Store subdomain on ecwid.com domain, e.g. `mysuperstore.ecwid.com`
customDomain | string | Custom Starter site domain, e.g. `www.mysuperstore.com`

### Response

> Response example (JSON)

```json
{
    "updateCount": 1,
    "success": true
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus
Field | Type |  Description
------| ---- | --------------
updateCount | number | The number of updated entities (`1` or `0` depending on whether the update was successful)
success | boolean | `true` if the coupon has been updated, `false` otherwise


### Errors

> Error response example

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
409 | Cannot update profile because such nickname or email is already registered in the system
402 | Cannot update settings because the limit of number of zones or taxes is reached
500 | Cannot retrieve the coupon info because of an error on the server

## Upload store logo

Upload store logo displayed on Starter Site. 
The logo itself is to be placed in the request body. Maximum allowed file size is 20Mb.

> Request example

```http
POST /api/v3/4870020/profile/logo?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/profile/logo?token=abcdefg123456';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $file);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: image/jpeg;'));

$result = curl_exec($ch);
curl_close ($ch);
```

`POST https://app.ecwid.com/api/v3/{storeId}/profile/logo?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token

When uploading a store logo, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

### Response

> Response example

```json
{
    "id": 240198557
}
```

A JSON object of type 'UploadStatus' with the following fields:

#### UploadStatus
Field | Type |  Description
----- | -----| ------------
id | number | Internal image ID

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Uploading of the image file failed or there was an internal server error while processing a file
413 | The image file is too large (Maximum allowed file size is 20Mb)
422 | The uploaded file is not an image
400 | Request parameters are malformed

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



## Remove store logo

Remove store logo, which is displayed on Starter site

> Request example

```http
DELETE /api/v3/4870020/profile/logo?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/profile/logo?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token

### Response

> Response example

```json
{
    "deleteCount": 1,
    "success": true
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus
Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted images (`1` or `0` depending on whether the request was successful)
success | boolean | `true` if the image has been deleted, `false` otherwise

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Uploading of the image file failed or there was an internal server error while processing a file
400 | Request parameters are malformed

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



## Upload invoice logo

Upload store logo displayed on order invoices. The logo itself is to be placed in the request body. Maximum allowed file size is 20Mb.

> Request example

```http
POST /api/v3/4870020/profile/invoicelogo?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/profile/invoicelogo?token=abcdefg123456';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $file);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: image/jpeg;'));

$result = curl_exec($ch);
curl_close ($ch);
```

`POST https://app.ecwid.com/api/v3/{storeId}/profile/invoicelogo?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token

When uploading an invoice logo, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

### Response

> Response example

```json
{
    "id": 240198557
}
```

A JSON object of type 'UploadStatus' with the following fields:

#### UploadStatus
Field | Type |  Description
----- | -----| ------------
id | number | Internal image ID

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Uploading of the image file failed or there was an internal server error while processing a file
413 | The image file is too large. Maximum allowed file size is 20Mb.
400 | Request parameters are malformed
422 | The uploaded file is not an image

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



## Remove invoice logo

Remove store logo, which is displayed on order invoices

> Request example

```http
DELETE /api/v3/4870020/profile/invoicelogo?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/profile/invoicelogo?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token

### Response

> Response example

```json
{
    "deleteCount": 1,
    "success": true
}
```

A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus
Field | Type |  Description
----- | ---- | --------------
deleteCount | number | The number of deleted images (`1` or `0` depending on whether the request was successful)
success | boolean | `true` if the image has been deleted, `false` otherwise

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | Uploading of the image file failed or there was an internal server error while processing a file
400 | Request parameters are malformed

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Get store update statistics

This method provides simple 'Latest updates' statistics about store profile, products and orders. Use it to check whether something was changed in an Ecwid store. This could be helpful to keep data in your application up-to-date and avoid abusing API to get and parse large amounts of data to check its state.

> Request example

```http
GET /api/v3/4870020/latest-stats?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/latest-stats?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

### Response

> Response example (JSON)

```json
{
    "productsUpdated": "2014-10-19 18:56:21 +0400",
    "ordersUpdated": "2014-10-15 16:54:11 +0400",
    "profileUpdated": "2014-10-19 18:55:35 +0400"
}
```

A JSON object containing the update dates statistics

#### UpdateStats
Field | Type | Description
----- | ---- | -----------
productsUpdated | string | Date of the latest product update, e.g. `2014-10-15 16:54:11 +0400`
ordersUpdated | string | Date of the latest orders update, e.g. `2014-10-15 16:54:11 +0400`
profileUpdated | string | Date of the latest profile update, e.g. `2014-10-15 16:54:11 +0400`

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | There was an internal server error while processing the request
400 | Request parameters are malformed

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


## Get deleted items statistics
Using this group of API methods, you can get a list of products, orders, customers and coupons that have been deleted from the store. In combination with the update statistics, you can use these endpoints to check whether something was changed in an Ecwid store and keep data in your application synchronized with the Ecwid store you're working with. 

> Get deleted products stats

```http
GET /api/v3/4870020/products/deleted?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

> Get deleted orders stats

```http
GET /api/v3/4870020/orders/deleted?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

> Get deleted customers stats

```http
GET /api/v3/4870020/customers/deleted?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

> Get deleted coupons stats

```http
GET /api/v3/4870020/discount_coupons/deleted?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

There are 4 endpoints: deleted products, orders, customers, coupons.

* `GET https://app.ecwid.com/api/v3/{storeId}/products/deleted?from_date={from_date}&to_date={to_date}&offset={offset}&limit={limit}&token={token}`
* `GET https://app.ecwid.com/api/v3/{storeId}/orders/deleted?from_date={from_date}&to_date={to_date}&offset={offset}&limit={limit}&token={token}`
* `GET https://app.ecwid.com/api/v3/{storeId}/customers/deleted?from_date={from_date}&to_date={to_date}&offset={offset}&limit={limit}&token={token}`
* `GET https://app.ecwid.com/api/v3/{storeId}/discount_coupons/deleted?from_date={from_date}&to_date={to_date}&offset={offset}&limit={limit}&token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
from_date | string | Item deletion date lower bound. Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul> Examples: <ul><li>`1447804800`</li> <li>`2015-04-22 18:48:38 -0500`</li> <li>`2015-04-22` (this is 2015-04-22 00:00:00 UTC)</li></ul>
to_date | string | Item deletion date upper bound. Supported formats: <ul><li>*UNIX timestamp*</li> <li>*yyyy-MM-dd HH:mm:ss Z*</li> <li>*yyyy-MM-dd HH:mm:ss*</li> <li>*yyyy-MM-dd*</li> </ul>
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Default value: `100`


### Response

> Response example (JSON). Removed order stats

```json
{
    "total": 1,
    "count": 1,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            "id": 9,
            "date": "2014-10-20 18:07:24 +0400"
        }
    ]
}
```

> Response example (JSON). Removed coupons stats

```json
{
    "total": 2,
    "count": 2,
    "offset": 0,
    "limit": 100,
    "items": [
        {
            "code": "UNN9GN6XAW2M",
            "date": "2014-10-20 18:00:54 +0400"
        },
        {
            "code": "MOXQ3YCWXRXA",
            "date": "2014-10-20 18:00:57 +0400"
        }
    ]
}
```

Each endpoint returns the information about the batch, the removed items IDs and the deletion dates in JSON object of type DeletedItemsStats

#### DeletedItemsStats
Field | Type | Description
----- | ---- | -----------
total | number | The total number of found items (might be more than the number of returned items)
count | number | The total number of the items returned in this batch
offset | number | Offset from the beginning of the returned items list (for paging)
limit | number | Maximum number of returned items. Maximum allowed value: `100`. Default value: `10`
items | Array<RemovedItem> | The removed items list with IDs and dates

#### Removed item
Field | Type | Description
----- | ---- | -----------
id | number | Item ID. Depending on the request, that is products ID, customer ID, order number or coupon code. In case of coupons the field is called `code` instead if `id` and has string format. 
date | string | Item deletion date

### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
500 | There was an internal server error while processing the request
400 | Request parameters are malformed

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message