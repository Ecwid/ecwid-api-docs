# Store profile

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
        "storeName": "My Super Store"
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
            "rules": []
        }
    ],
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
        }
    ]
}
```

A JSON object of type 'Profile' with the following fields:

#### Profile
Field | Type | Description
----- | ---- | -----------
generalInfo | \<GeneralInfo\> | Store's basic data
account | \<Account\> | Seller's account data
settings | \<Settings\> | Store's general settings
company | \<Company\> | Company info
formatsAndUnits | \<FormatsAndUnits\> | Store's formats/untis settings
languages | \<Languages\> | Store's language settings
taxes | Array\<Tax\> | Store's taxes settings
zones | Array\<Zone\> | Store's destination zones

#### GeneralInfo
Field | Type | Description
----- | ---- | -----------
storeId | number | Ecwid Store ID
storeUrl | string | Storefront URL
starterSite | <\StarterSiteInfo\> | Starter Site settings

#### Account
Field | Type | Description
----- | ---- | -----------
accountName | string | Full user name
accountNickName | string | User nickname on the Ecwid forums
accountEmail | string | User email


#### Settings
Field | Type | Description
----- | ---- | -----------
closed | string | Full user name
storeName | string | The store name displayed in Starter Site
invoiceLogoUrl | string | Company logo displayed on the invoice

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
currency | string | Currency code, used in this store, as in ISO 4217 (`USD`, `CAD` etc.).
currencyPrefix | string |  Prefix of the currency notation (e.g. $)
currencySuffix | string | Suffix of the currency notation
currencyPrecision | number  | Numbers of digits after decimal point in the store prices. E.g. `2` ($2.99) or `0` (¥500).
currencyGroupSeparator | string | Price thousands separator. Supported values: space ` `, dot `.`, comma `,`  or empty value ``.
currencyDecimalSeparator |  string | Price decimal separator. Possible values: `.` or `,` 
currencyTruncateZeroFractional | boolean | Hide zero fractional part of the prices in storefront. `true` or `false` . 
currencyRate | number | Currency rate in U.S. dollars, as set in the 
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
enabledLanguages | Array\<string\> | A list of enabled languages in the storefront
facebookPreferredLocale | string | Language automatically chosen be default in Facebook storefront (if any)


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
zoneId | number | Destination zone ID
tax | number | Tax rate for this zone

#### Zone
*System Settings → Zones*

Field | Type | Description
----- | ---- | -----------
id | string | Unique internal zone ID
name | string | Zone displayed name
countryCodes | Array\<string\> | Country codes this zone includes . 
stateOrProvinceCodes |  Array\<string\> | State or province codes the zone includes
postCodes | Array\<string\> |   Postcode (or zipcode) templates this zone includes. More details: [Destination zones in Ecwid](http://help.ecwid.com/customer/portal/articles/1163922-destination-zones)

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
<aside class="notice">
The documentation is in progress...
</aside>