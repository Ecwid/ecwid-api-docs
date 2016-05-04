# Stores

## Create store

> Request example

```http
POST /api/v3/stores?appClientId=my-cool-app&appSecretKey=abcdefghijklmnopabcdefghijklmnop HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache

{
   "merchant": {
        "email": "john@example.com",
        "name": "John Doe",
        "password": "12345678", 
        "ip": "127.01.03.02"
    }
    "affiliatePartner": {
      "source": "wporg",
      "ambassador": { 
        "ref": "12bg5",
        "campaignId": "234sdfsd"
      }
    }
    "profile" {
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
}
```

In case if you need this functionality, please contact us and provide details on why you need it.

<aside class="notice">
    Access scope required: <strong>create_stores</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

`POST https://app.ecwid.com/api/v3/stores?appClientId={appClientId}&appSecretKey={appSecretKey}`

Name | Type    | Description
---- | ------- | -----------
**appClientId** |  string | Your application's client_id
**appSecretKey** |  string |  Your application's client_secret

### Request body

A JSON object of type 'Store' with the following fields:

#### Store
Field | Type | Description
----- | ---- | -----------
merchant | \<Merchant\> | Store owner's account data
affiliatePartner | \<AffiliatePartner\> | Affiliate partner data
profile | \<Profile\> | Store details

<aside class="notice">
Fields marked in <strong>bold</strong> are mandatory.
</aside>

#### Merchant
Field | Type | Description
----- | ---- | -----------
**email** | string | Store owner's email
**name** | string | Store owner's name
password | string | Ecwid account password
ip | string | Store owner's IP. Is used to predetermine user's location and set default settings in store

#### AffiliatePartner
Field | Type | Description
----- | ---- | -----------
source | string | Determines the source of the registered account
ambassador | \<Ambassador\> | Ambassador affiliate account details. [More info](https://help.ecwid.com/customer/en/portal/articles/1767383-web-partner-program-faq)

#### Ambassador
Field | Type | Description
----- | ---- | -----------
ref | string | Referral account ID
campaignId | string | Campaign ID

#### Profile

The filling of the below details is optional and it will work as [Update store profile](#update-store-profile) method.

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
taxes | Array\<Tax\> | Store taxes settings
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

#### Tax
*System Settings → Taxes*

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
    storeid: 1234567
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus
Field | Type |  Description
------| ---- | --------------
storeid | number | ID of the created store


### Errors

> Error response example

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8

{
  errorMessage: "Email already exists",
  errorCode: "EMAIL_ALREADY_EXISTS"
}
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
403 | Permission denied. Application client ID or application secret key is invalid or invalid scope parameter
409 | Conflict: Email already exists
422 | Unprocessable entity: invalid email or invalid password or invalid name. The password must be at least 5 characters long. Make sure the name contains first name and last name

## Check if store exists

> Request example

```http
GET /api/v3/stores?appClientId=my-cool-app&appSecretKey=abcdefghijklmnopabcdefghijklmnop&email=john@example.com HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

In case if you need this functionality, please contact us and provide details on why you need it.

<aside class="notice">
    Access scope required: <strong>read_stores</strong> (see <a href="#access-scopes">Access scopes</a>)
</aside>

`GET https://app.ecwid.com/api/v3/stores?appClientId={appClientId}&appSecretKey={appSecretKey}&email={email}`

Name | Type    | Description
---- | ------- | -----------
**appClientId** |  string | Your application's client_id
**appSecretKey** |  string |  Your application's client_secret
**email** | string | Store owner's email address

### Response

> Response example (JSON)

```json
{
}
```

An empty JSON object of type 'UpdateStatus', indicating that the store is already registered in Ecwid.

### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8

{
  errorMessage: "Store with given email is not found",
  errorCode: "EMAIL_IS_NOT_FOUND"
}
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
403 | Permission denied. Application client ID or application secret key is invalid or invalid scope parameter
404 | Store with given email is not found
409 | Conflict: Email already exists
422 | Unprocessable entity: invalid email or invalid password or invalid name. The password must be at least 5 characters long. Make sure the name contains first name and last name

