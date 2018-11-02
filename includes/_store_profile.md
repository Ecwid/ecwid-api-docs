## Store information

Using the endpoints below you can get and update basic Ecwid store information like store name, merchant's email, company address, logos and more. 

<aside class="note">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

### Get store profile

Get basic information about an Ecwid store: settings, store location, email, etc. This request is available with any access token.

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

#### Response

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
        "accountEmail": "example@example.com",
        "whiteLabel": false
    },
    "settings": {
        "closed": false,
        "storeName": "My Super Store",
        "storeDescription": "<p>Welcome to my store! Check out the latest and coolest products below. </p>",
        "invoiceLogoUrl": "https://dpbfm6h358sh7.cloudfront.net/images/4870020/253584290.jpg",
        "emailLogoUrl": "https://dpbfm6h358sh7.cloudfront.net/images/4870020/298177033.jpg",
        "googleRemarketingEnabled": true,
        "googleAnalyticsId": "UA-123456-1",
        "fbPixelId": "12305215151521",
        "orderCommentsEnabled": false,
        "orderCommentsCaption": "Order comments",
        "orderCommentsRequired": false,
        "hideOutOfStockProductsInStorefront": true,
        "askCompanyName": false,
        "favoritesEnabled": true,
        "defaultProductSortOrder": "DEFINED_BY_STORE_OWNER",
        "abandonedSales": {
          "autoAbandonedSalesRecovery": true
        },
        "salePrice": {
          "displayOnProductList": true,
          "oldPriceLabel": "was",
          "displayDiscount": "PERCENT"
        }
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
        "timezone": "Europe/Moscow",
        "dimensionsUnit": "CM",
        "orderNumberPrefix": "00001",
        "orderNumberSuffix": "5000"
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
        },
        "shippingOrigin": {
          "companyName": "My Super Store",
          "email": "example@example.com",
          "street": "W 3d st",
          "city": "New York",
          "countryCode": "US",
          "postalCode": "10001",
          "stateOrProvinceCode": "NY",
          "phone": "234324234"
        },
        "shippingOptions": [
          // Flat rate shipping method
          {
            "id": "8329-1495610692625",
            "title": "Flat rate",
            "enabled": false,
            "orderby": 170,
            "destinationZone": {
              "id": "7715-1423477610739",
              "name": "US",
              "countryCodes": [
                "US"
              ],
              "stateOrProvinceCodes": [
                "US-AL",
                "US-AK",
                "US-AZ",
                "US-AR",
                "US-AA",
                "US-AE",
                "US-AP",
                "US-CA",
                "US-CO",
                "US-CT",
                "US-DE",
                "US-DC",
                "US-FL",
                "US-GA",
                "US-GU",
                "US-HI",
                "US-ID",
                "US-IL",
                "US-IN",
                "US-IA",
                "US-KS",
                "US-KY",
                "US-LA",
                "US-ME",
                "US-MD",
                "US-MA",
                "US-MI",
                "US-MN",
                "US-MS",
                "US-MO",
                "US-MT",
                "US-NE",
                "US-NV",
                "US-NH",
                "US-NJ",
                "US-NM",
                "US-NY",
                "US-NC",
                "US-ND",
                "US-PR",
                "US-RI",
                "US-SC",
                "US-SD",
                "US-TN",
                "US-TX",
                "US-UT",
                "US-VT",
                "US-VI",
                "US-VA",
                "US-WA",
                "US-WV",
                "US-WI",
                "US-WY"
              ]
            },
            "fulfilmentType": "shipping",
            "ratesCalculationType": "flat",
            "flatRate": {
              "rateType": "ABSOLUTE",
              "rate": 22
            },
            "carrier": ""
          },
          // Carrier-calculated rates
          {
            "id": "7835-1442850313554",
            "title": "The World: UPS",
            "enabled": true,
            "orderby": 0,
            "destinationZone": {
              "id": "WORLD",
              "name": "WORLD"
            },
            "fulfilmentType": "shipping",
            "ratesCalculationType": "carrier-calculated",
            "carrierMethodsEnabled": [
                    "UPS Expedited SM",
                    "UPS Express Early A.M. SM",
                    "UPS Express Plus",
                    "UPS Next Day Air Saver®",
                    "UPS Next Day Air®",
                    "UPS Saver",
                    "UPS Second Day Air A.M.®",
                    "UPS Second Day Air®",
                    "UPS Standard",
                    "UPS Three-Day Select®",
                    "UPS Today Dedicated Courrier SM",
                    "UPS Today Express",
                    "UPS Today Express Saver",
                    "UPS Today Intercity",
                    "UPS Today Standard SM",
                    "UPS Worldwide Expedited SM",
                    "UPS Worldwide Express Plus SM",
                    "UPS Worldwide Express SM"
            ],
            "shippingCostMarkup": 0,
            "carrierSettings": {
              "defaultCarrierAccountEnabled": false,
              "defaultPostageDimensions": {
                "length": 0.0254,
                "width": 0.0254,
                "height": 0.0254
              }
            },
            "carrier": "UPS"
          },
          // Custom table rates
          {
            "id": "9327-1430948461738",
            "title": "Standard shipping",
            "enabled": true,
            "orderby": 1250,
            "destinationZone": {
              "id": "WORLD",
              "name": "WORLD"
            },
            "fulfilmentType": "shipping",
            "ratesCalculationType": "table",
            "ratesTable": {
              "tableBasedOn": "weight",
              "rates": [
                {
                  "conditions": {
                    "weightFrom": 0,
                    "weightTo": 12
                  },
                  "rate": {
                    "perOrder": 10,
                    "percent": 2,
                    "perItem": 3,
                    "perWeight": 2
                  }
                },
                {
                  "conditions": {
                    "weightFrom": 12.01
                  },
                  "rate": {
                    "perOrder": 20,
                    "percent": 3,
                    "perItem": 4,
                    "perWeight": 3
                  }
                }
              ]
            },
            "deliveryTimeDays": "1-2",
            "carrier": ""
          },
          // In-store pickup method
          {
            "id": "6633-1504595502395",
            "title": "In-store Pickup",
            "enabled": true,
            "orderby": 130,
            "destinationZone": {
              "id": "WORLD",
              "name": "WORLD"
            },
            "fulfilmentType": "pickup",
            "pickupInstruction": "<p><strong>Pickup location:</strong></p><p>    Cool Guys, 5th Avenue, Москва, Guam, 11950, United States</p><p>    <strong>Store hours:</strong></p><p>    9AM – 6PM Mon-Fri</p>",
            "scheduledPickup": true,
            "pickupPreparationTimeHours": 60,
            "pickupBusinessHours": "{\"MON\":[[\"09:00\",\"18:00\"]], \"TUE\":[[\"09:00\",\"18:00\"]], \"THU\":[[\"09:00\",\"10:00\"]], \"FRI\":[[\"09:00\",\"18:00\"]], \"SAT\":[[\"09:00\",\"10:30\"]]}"
          },
          // Custom shipping app
          {
            "id": "1095449637-1519976902346",
            "title": "Doba Drop Shipping",
            "enabled": true,
            "orderby": 10,
            "destinationZone": {
              "id": "WORLD",
              "name": "WORLD"
            },
            "fulfilmentType": "shipping",
            "ratesCalculationType": "app",
            "appClientId": "doba-integration"
          }
        ]
    },
    "zones": [
        {
            "id": "43-1423477610132",
            "name": "United States",
            "countryCodes": [
                "US",
                "UM",
                "VI"
            ]
        },
        {
            "id": "334-112332610739",
            "name": "US & Canada",
            "countryCodes": [
                "CA",
                "US",
                "UM",
                "VI"
            ]
        },
        {
            "id": "7715-1423477610739",
            "name": "US",
            "countryCodes": [
                "US"
            ],
            "stateOrProvinceCodes": [
                "US-AL",
                "US-AK",
                "US-AZ",
                "US-AR",
                "US-AA",
                "US-AE",
                "US-AP",
                "US-CA",
                "US-CO",
                "US-CT",
                "US-DE",
                "US-DC",
                "US-FL",
                "US-GA",
                "US-GU",
                "US-HI",
                "US-ID",
                "US-IL",
                "US-IN",
                "US-IA",
                "US-KS",
                "US-KY",
                "US-LA",
                "US-ME",
                "US-MD",
                "US-MA",
                "US-MI",
                "US-MN",
                "US-MS",
                "US-MO",
                "US-MT",
                "US-NE",
                "US-NV",
                "US-NH",
                "US-NJ",
                "US-NM",
                "US-NY",
                "US-NC",
                "US-ND",
                "US-PR",
                "US-RI",
                "US-SC",
                "US-SD",
                "US-TN",
                "US-TX",
                "US-UT",
                "US-VT",
                "US-VI",
                "US-VA",
                "US-WA",
                "US-WV",
                "US-WI",
                "US-WY"
            ]
        }
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
    },
    "legalPagesSettings": {
      "requireTermsAgreementAtCheckout": true,
      "legalPages": [
        {
          "type": "LEGAL_INFO",
          "enabled": false,
          "title": "About Us",
          "display": "INLINE",
          "text": "",
          "externalUrl": ""
        },
        {
          "type": "SHIPPING_COST_PAYMENT_INFO",
          "enabled": true,
          "title": "Shipping & Payment Info",
          "display": "EXTERNAL_URL",
          "text": "",
          "externalUrl": "https://mycoolsite.com/shipping"
        },
        {
          "type": "PRIVACY_STATEMENT",
          "enabled": true,
          "title": "Privacy Policy",
          "display": "EXTERNAL_URL",
          "text": "",
          "externalUrl": "https://mycoolsite.com/privacy"
        },
        {
          "type": "TERMS",
          "enabled": true,
          "title": "Terms & Conditions",
          "display": "EXTERNAL_URL",
          "text": "",
          "externalUrl": "https://mycoolsite.com/terms"
        },
        {
          "type": "REVOCATION_TERMS",
          "enabled": false,
          "title": "Return Policy",
          "display": "INLINE",
          "text": "",
          "externalUrl": ""
        }
      ]
    },
    "payment": {
      "paymentOptions": [
        {
          "id": "1794210895-1490949002653",
          "enabled": true,
          "checkoutTitle": "Stripe",
          "checkoutDescription": "Pay with credit card",
          "paymentProcessorId": "stripe",
          "paymentProcessorTitle": "Stripe",
          "orderBy": 0,
          "appClientId": "",
          "instructionsForCustomer": {
            "instructionsTitle": "How to pay via Stripe",
            "instructions": "<p>Here are the instructions on how to pay with Stripe</p>"
          }
        },
        {
          "id":"1794210895-1490949002653",
          "enabled":false,
          "checkoutTitle": "Phone Order", 
          "checkoutDescription": "", 
          "paymentProcessorId": "offline", 
          "paymentProcessorTitle": "",
          "orderBy": 10,
          "appClientId": "",
          "instructionsForCustomer":{
            "instructionsTitle": "Phone Order payment instructions",
            "instructions": "<p>Some instructions.</p>" 
          }
        },        
        {
          "id": "1570107140-1510749490171",
          "enabled": true,
          "checkoutTitle": "Credit or debit card (PayU Latam)",
          "checkoutDescription": "",
          "paymentProcessorId": "customPaymentApp",
          "paymentProcessorTitle": "CUSTOM_PAYMENT_APP-payu-latam-integration",
          "orderBy": 20,
          "appClientId": "payu-latam-integration",
          "instructionsForCustomer": {
            "instructionsTitle": "How to pay via PayU Latam",
            "instructions": "<p>Here are the instructions on how to pay with PayU Latam</p>"
          }
        },
        {
          "id": "1794210895-1490949002653",
          "enabled": true,
          "checkoutTitle": "Credit or Debit Card",
          "checkoutDescription": "",
          "paymentProcessorId": "ecwidPayments",
          "paymentProcessorTitle": "Ecwid Payments",
          "orderBy": 30,
          "appClientId": "",
          "instructionsForCustomer": {
            "instructionsTitle": "How to pay via PayU Latam",
            "instructions": "<p>Here are the instructions on how to pay with PayU Latam</p>"
          }
        }
      ]
    },
    "featureToggles": [
        {
            "name": "CONSECUTIVE_ORDER_IDS",
            "visible": true,
            "enabled": true
        },
        {
            "name": "INDIVIDUAL_GOOGLE_CATEGORY_IN_FEEDS",
            "visible": true,
            "enabled": true
        },
        {
            "name": "NEW_STARTERSITE",
            "visible": true,
            "enabled": true
        },
        {
            "name": "NEW_DASHBOARD",
            "visible": false,
            "enabled": true
        },
        {
            "name": "NEW_SALES_LIST",
            "visible": true,
            "enabled": true
        },
        {
            "name": "VERTICAL_CP_MENU",
            "visible": false,
            "enabled": true
        },
        {
            "name": "CUSTOMER_LOGIN_BY_LINK",
            "visible": true,
            "enabled": true
        },
        {
            "name": "NEW_PRODUCT_LIST",
            "visible": false,
            "enabled": false
        },
        {
            "name": "NEW_DETAILS_PAGE",
            "visible": false,
            "enabled": true
        },
        {
            "name": "NEW_FB_SHOPS_INTEGRATION",
            "visible": true,
            "enabled": false
        },
        {
            "name": "NEW_CATALOG_CP_PAGE",
            "visible": false,
            "enabled": true
        }
    ],
    "designSettings": {
        "product_list_image_size": "MEDIUM",
        "product_list_image_aspect_ratio": "SQUARE",
        "product_list_product_info_layout": "CENTER",
        "product_list_show_additional_image_on_hover": false,
        "product_list_title_behavior": "SHOW",
        "product_list_price_behavior": "SHOW",
        "product_list_sku_behavior": "HIDE",
        "product_list_buybutton_behavior": "SHOW",
        "product_list_category_title_behavior": "SHOW_ON_IMAGE",
        "product_list_image_has_shadow": true,
        "show_signin_link": true,
        "show_footer_menu": true,
        "show_breadcrumbs": true,
        "product_list_show_sort_viewas_options": true,
        "product_details_show_product_sku": true,
        "product_details_layout": "TWO_COLUMNS_SIDEBAR_ON_THE_RIGHT",
        "product_details_two_columns_with_right_sidebar_show_product_description_on_sidebar": false,
        "product_details_show_product_name": true,
        "product_details_show_breadcrumbs": true,
        "product_details_show_product_price": true,
        "product_details_show_sale_price": true,
        "product_details_show_tax": true,
        "product_details_show_product_description": true,
        "product_details_show_product_options": true,
        "product_details_show_wholesale_prices": true,
        "product_details_show_save_for_later": true,
        "product_details_show_share_buttons": true,
        "product_details_show_price_per_unit": true,
        "product_details_show_qty": true,
        "product_details_show_in_stock_label": true,
        "product_details_show_number_of_items_in_stock": true,
        "product_details_gallery_layout": "IMAGE_SINGLE_THUMBNAILS_VERTICAL"
    } 
}
```

> Public token request example

```http
GET /api/v3/4870020/profile?token=public_bpFwyLBELRZa43vaFWVryxu199eYDaaz HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

```json
{
  "generalInfo": {
    "storeId": 5035009,
    "storeUrl": "https://example.com",
    "starterSite": {
      "ecwidSubdomain": "demo",
      "generatedUrl": "https://demo.ecwid.com"
    }
  },
  "settings": {
    "closed": false,
    "storeName": "Awesome store",
    "storeDescription": "<p>Welcome to my store! Check out the latest and coolest products below. </p>",
    "googleAnalyticsId": "",
    "fbPixelId": "12305215151521"
  },
  "mailNotifications": {
    "customerNotificationFromEmail": "info@example.com"
  },
  "company": {
    "companyName": "Cool slippers for dogs",
    "email": "info@example.com",
    "street": "W 3d st",
    "city": "New York",
    "countryCode": "US",
    "postalCode": "10002",
    "stateOrProvinceCode": "NY",
    "phone": ""
  },
  "formatsAndUnits": {
    "currency": "USD",
    "currencyPrefix": "$",
    "currencySuffix": "",
    "currencyGroupSeparator": " ",
    "currencyDecimalSeparator": ".",
    "currencyPrecision": 2,
    "currencyTruncateZeroFractional": false,
    "currencyRate": 1,
    "weightUnit": "KILOGRAM",
    "weightGroupSeparator": " ",
    "weightDecimalSeparator": ".",
    "weightTruncateZeroFractional": false,
    "timeFormat": "hh:mm a",
    "dateFormat": "EEE, MMM d, ''yy",
    "timezone": "Europe/Samara",
    "dimensionsUnit": "CM"
  },
  "languages": {
    "enabledLanguages": [
      "en",
      "ar",
      "be",
      "bg",
      "ca",
      "cs",
      "cy",
      "da",
      "de",
      "el",
      "es",
      "es_419",
      "et",
      "eu",
      "fi",
      "fa",
      "fr",
      "id",
      "is",
      "it",
      "ja",
      "he",
      "hr",
      "hu",
      "hy",
      "ka",
      "ko",
      "lt",
      "lv",
      "mk",
      "mr",
      "ms",
      "nl",
      "no",
      "pl",
      "pt",
      "pt_BR",
      "ro",
      "ru",
      "sk",
      "sl",
      "sr",
      "sv",
      "sq",
      "th",
      "tr",
      "uk",
      "vi",
      "zh",
      "zh_TW"
    ],
    "facebookPreferredLocale": "en_US"
  },
  "payment": {
      "paymentOptions": [
        {
          "id": "1794210895-1490949002653",
          "enabled": true,
          "checkoutTitle": "Stripe",
          "checkoutDescription": "Pay with credit card",
          "paymentProcessorId": "stripe",
          "paymentProcessorTitle": "Stripe",
          "orderBy": 0,
          "appClientId": "",
          "instructionsForCustomer": {
            "instructionsTitle": "How to pay via Stripe",
            "instructions": "<p>Here are the instructions on how to pay with Stripe</p>"
          }
        },
        {
          "id": "1570107140-1510749490171",
          "enabled": true,
          "checkoutTitle": "Credit or debit card (PayU Latam)",
          "checkoutDescription": "",
          "paymentProcessorId": "customPaymentApp",
          "paymentProcessorTitle": "CUSTOM_PAYMENT_APP-payu-latam-integration",
          "orderBy": 20,
          "appClientId": "payu-latam-integration",
          "instructionsForCustomer": {
            "instructionsTitle": "How to pay via PayU Latam",
            "instructions": "<p>Here are the instructions on how to pay with PayU Latam</p>"
          }
        },        
        {
          "id": "1794210895-1490949002653",
          "enabled": true,
          "checkoutTitle": "Credit or Debit Card",
          "checkoutDescription": "",
          "paymentProcessorId": "ecwidPayments",
          "paymentProcessorTitle": "Ecwid Payments",
          "orderBy": 30,
          "appClientId": "",
          "instructionsForCustomer": {
            "instructionsTitle": "How to pay via Credit Card",
            "instructions": "<p>Here are the instructions on how to pay with Credit Card</p>"
          }
        }
      ]
    },
    "shipping": {
        "handlingFee": {
            "name": "Handling Fee",
            "value": 5,
            "description": "Silk paper wrapping"
        },
        "shippingOptions": [
          {
            "id": "7835-1442850313554",
            "title": "The World: UPS",
            "enabled": true,
            "orderby": 0,
            "destinationZone": {},
            "fulfilmentType": "shipping",
            "ratesCalculationType": "carrier-calculated",
            "carrier": "UPS"
          },
          {
            "id": "9327-1430948461738",
            "title": "Standard shipping",
            "enabled": true,
            "orderby": 1250,
            "destinationZone": {},
            "fulfilmentType": "shipping",
            "ratesCalculationType": "table",
            "ratesTable": {},
              "deliveryTimeDays": "1-2",
              "carrier": ""
          },
          {
            "id": "6633-1504595502395",
            "title": "In-store Pickup",
            "enabled": true,
            "orderby": 130,
            "destinationZone": {},
            "fulfilmentType": "pickup",
            "pickupInstruction": "<p><strong>Pickup location:</strong></p><p>    Cool Guys, 5th Avenue, Москва, Guam, 11950, United States</p><p>    <strong>Store hours:</strong></p><p>    9AM – 6PM Mon-Fri</p>",
            "scheduledPickup": true,
            "pickupPreparationTimeHours": 60,
            "pickupBusinessHours": "{\"MON\":[[\"09:00\",\"18:00\"]], \"TUE\":[[\"09:00\",\"18:00\"]], \"THU\":[[\"09:00\",\"10:00\"]], \"FRI\":[[\"09:00\",\"18:00\"]], \"SAT\":[[\"09:00\",\"10:30\"]]}"
          },
          {
            "id": "1095449637-1519976902346",
            "title": "Doba Drop Shipping",
            "enabled": true,
            "orderby": 10,
            "destinationZone": {},
            "fulfilmentType": "shipping",
            "ratesCalculationType": "app",
            "appClientId": "doba-integration"
          }
        ]
    },
    "designSettings": {
        "product_list_image_size": "MEDIUM",
        "product_list_image_aspect_ratio": "SQUARE",
        "product_list_product_info_layout": "CENTER",
        "product_list_show_additional_image_on_hover": false,
        "product_list_title_behavior": "SHOW",
        "product_list_price_behavior": "SHOW",
        "product_list_sku_behavior": "HIDE",
        "product_list_buybutton_behavior": "SHOW",
        "product_list_category_title_behavior": "SHOW_ON_IMAGE",
        "product_list_image_has_shadow": true,
        "show_signin_link": true,
        "show_footer_menu": true,
        "show_breadcrumbs": true,
        "product_list_show_sort_viewas_options": true,
        "product_details_show_product_sku": true,
        "product_details_layout": "TWO_COLUMNS_SIDEBAR_ON_THE_RIGHT",
        "product_details_two_columns_with_right_sidebar_show_product_description_on_sidebar": false,
        "product_details_show_product_name": true,
        "product_details_show_breadcrumbs": true,
        "product_details_show_product_price": true,
        "product_details_show_sale_price": true,
        "product_details_show_tax": true,
        "product_details_show_product_description": true,
        "product_details_show_product_options": true,
        "product_details_show_wholesale_prices": true,
        "product_details_show_save_for_later": true,
        "product_details_show_share_buttons": true,
        "product_details_show_price_per_unit": true,
        "product_details_show_qty": true,
        "product_details_show_in_stock_label": true,
        "product_details_show_number_of_items_in_stock": true,
        "product_details_gallery_layout": "IMAGE_SINGLE_THUMBNAILS_VERTICAL"
    }
}
```

A JSON object of type 'Profile' with the following fields:

#### Profile
Field | Type | Description
----- | ---- | -----------
generalInfo | \<*GeneralInfo*\> | Store basic data
account | \<*Account*\> | Store owner's account data
settings | \<*Settings*\> | Store general settings
mailNotifications | \<*MailNotifications*\> | Mail notifications settings
company | \<*Company*\> | Company info
formatsAndUnits | \<*FormatsAndUnits*\> | Store formats/untis settings
languages | \<*Languages*\> | Store language settings
shipping | \<*Shipping*\> | Store shipping settings (only handling fee is included at the moment)
taxSettings | \<*TaxSettings*\> | Store taxes settings
zones | Array\<*Zone*\> | Store destination zones
businessRegistrationID | \<*BusinessRegistrationID*\> | Company registration ID, e.g. VAT reg number or company ID, which is set under Settings / Invoice in Control panel
legalPagesSettings | \<*LegalPagesSettingsDetails*\> | Legal pages settings for a store (*System Settings → General → Legal Pages*)
payment | \<*PaymentInfo*\> | Store payment settings information
featureToggles | \<*FeatureTogglesInfo*\> | Information about enabled/disabled new store features and their visibility in Ecwid Control Panel. Not provided via public token. Some of them are available in [Ecwid JS API](https://developers.ecwid.com/api-documentation/get-storefront-details#ecwid-getfeaturetoggles)
designSettings | \<*DesignSettingsInfo*\> | Design settings of an Ecwid store. Can be overriden by [updating store profile](https://developers.ecwid.com/api-documentation/store-information#update-store-profile) or by [customizing design](https://developers.ecwid.com/api-documentation/customize-appearance) via JS config in storefront. 

#### GeneralInfo
Field | Type | Description
----- | ---- | -----------
storeId | number | Ecwid Store ID
storeUrl | string | Storefront URL
starterSite | \<*StarterSiteInfo*\> | Details of Ecwid starter site for account. Learn more about [Starter site](https://support.ecwid.com/hc/en-us/articles/207100069-Starter-site)

#### Account
Field | Type | Description
----- | ---- | -----------
accountName | string | Full store owner name
accountNickName | string | Store owner nickname on the Ecwid forums
accountEmail | string | Store owner email
availableFeatures | Array\<*string*\> | A list of the premium features available on the store's pricing plan
whiteLabel | boolean | `true` if Ecwid brand is not mentioned in merchant's interface, `false` otherwise. Read only

#### Settings
Field | Type | Description
----- | ---- | -----------
closed | boolean | `true` if the store is closed for maintenance, `false` otherwise
storeName | string | The store name displayed in Starter Site
storeDescription | string | HTML description for the main store page – Store Front page
invoiceLogoUrl | string | Company logo displayed on the invoice
emailLogoUrl | string | Company logo displayed in the store email notifications
googleRemarketingEnabled | boolean | `true` if Remarketing with Google Analytics is enabled, `false` otherwise
googleAnalyticsId | string | [Google Analytics ID](https://help.ecwid.com/customer/en/portal/articles/1170264-google-analytics) connected to a store
fbPixelId | string | Your Facebook Pixel ID. [Learn more](https://support.ecwid.com/hc/en-us/articles/115004303345-Step-2-Implement-Facebook-pixel)
orderCommentsEnabled | boolean | `true` if order comments feature is enabled, `false` otherwise
orderCommentsCaption | string | Caption for order comments field in storefront
orderCommentsRequired | boolean | `true` if order comments are required to be filled, `false` otherwise
hideOutOfStockProductsInStorefront | boolean | `true` if out of stock products are hidden in storefront, `false` otherwise. This setting is located in Ecwid Control Panel > Settings > General > Cart
askCompanyName | boolean | `true` if "Ask for the company name" in checkout settings is enabled, `false` otherwise
favoritesEnabled | boolean | `true` if favorites feature is enabled for storefront, `false` otherwise
defaultProductSortOrder | string | Default products sort order setting from *Settings > Cart & Checkout*. Possible values: `"DEFINED_BY_STORE_OWNER"`, `"ADDED_TIME_DESC"`, `"PRICE_ASC"`, `"PRICE_DESC"`, `"NAME_ASC"`, `"NAME_DESC"`
abandonedSales | \<*AbandonedSalesSettings*\> | Abandoned sales settings
salePrice | \<*SalePriceSettings*\ | Sale (compare to) price settings 

#### MailNotifications
Field | Type | Description
----- | ---- | -----------
adminNotificationEmails | Array\<*string*\> | Email addresses, which the store admin notifications are sent to
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
currencyGroupSeparator | string | Price thousands separator. Supported values: space `" "`, dot `"."`, comma `","`  or empty value `""`.
currencyDecimalSeparator |  string | Price decimal separator. Possible values: `.` or `,` 
currencyTruncateZeroFractional | boolean | Hide zero fractional part of the prices in storefront. `true` or `false` . 
currencyRate | number | Currency rate in U.S. dollars, as set in the merchant control panel
weightUnit |    string |    Weight unit. Supported values: `CARAT`, `GRAM`, `OUNCE`, `POUND`, `KILOGRAM`
weightPrecision | number | Numbers of digits after decimal point in weights displayed in the store
weightGroupSeparator | string | Weight thousands separator. Supported values: space `" "`, dot `"."`, comma `","`  or empty value `""`
weightDecimalSeparator | string | Weight decimal separator. Possible values: `.` or `,` 
weightTruncateZeroFractional |  boolean | Hide zero fractional part of the weight values in storefront. `true` or `false` . 
dateFormat | string | Date format. Only these formats are accepted: `"dd-MM-yyyy"`, `"dd/MM/yyyy"`, `"dd.MM.yyyy"`, `"MM-dd-yyyy"`, `"MM/dd/yyyy"`, `"yyyy/MM/dd"`, `"MMM d, yyyy"`, `"MMMM d, yyyy"`, `"EEE, MMM d, ''yy"`, `"EEE, MMMM d, yyyy"`
timeFormat | string | Time format. Only these formats are accepted: `"HH:mm:ss"`, `"HH:mm"`, `"hh.mm.ss a"`, `"hh:mm a"`
timezone | string | Store timezone, e.g. `Europe/Moscow`
dimensionsUnit | string | Product dimensions units
orderNumberPrefix | string | Order number prefix in a store
orderNumberSuffix | string | Order number suffix in a store

#### Languages
*System Settings → General → Languages*

Field | Type | Description
----- | ---- | -----------
enabledLanguages | Array\<*string*\> | A list of enabled languages in the storefront. First language code is the default language for the store.
facebookPreferredLocale | string | Language automatically chosen be default in Facebook storefront (if any)

#### Shipping
*System Settings → Shipping*

Field | Type | Description
----- | ---- | -----------
handlingFee | \<*HandlingFee*\> | Handling fee settings
shippingOrigin | \<*ShippingOrigin*\> | Shipping origin address. If matches company address, company address is returned. Available in read-only mode only
shippingOptions | Array \<*ShippingOption*\> | Details of each shipping option present in a store. **For public tokens enabled methods are returned** only. Available in read-only mode only

#### HandlingFee
*System Settings → Shipping → Handling Fee*

Field | Type | Description
----- | ---- | -----------
name | string | Handling fee name set by store admin. E.g. `Wrapping`
value | number | Hndling fee value
description | string | Handling fee description for customer

#### ShippingOrigin
*Settings → Shipping & Pickup → Origin address*

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

#### ShippingOption

Field | Type | Description
----- | ---- | -----------
id | string | Unique ID of shipping option
title | string | Title of shipping option in store settings
enabled | boolean | `true` if shipping option is used at checkout to calculate shipping. `false` otherwise
orderby | number | Sort position or shipping option at checkout and in store settings. The smaller the number, the higher the position
fulfilmentType | string | Fulfillment type. `"pickup"` for in-store pickup methods, `"shipping"` for everything else
destinationZone | \<*Zone*\> | Destination zone set for shipping option. **Empty for public token**
deliveryTimeDays | string | Estimated delivery time in days. Formats accepted: empty `""`, number `"5"`, several days estimate `"4-9"`
carrier | string | Carrier used for shipping the order. Is provided for carrier-calculated shipping options
carrierMethodsEnabled | Array of string | Carrier-calculated shipping methods available for this shipping option
carrierSettings | \<*CarrierSettings*\> | Carrier-calculated shipping option settings
ratesCalculationType | string | Rates calculation type. One of `"carrier-calculated"`, `"table"`, `"flat"`, `"app"`
shippingCostMarkup | number | Shipping cost markup for carrier-calculated methods
flatRate | \<*FlatRate*\> | Flat rate details
ratesTable | \<*TableRatesDetails*\> | Custom table rates details
appClientId | string | `client_id` value of the app (for custom shipping apps only)
pickupInstruction | string | String of HTML code of instructions on in-store pickup
scheduledPickup | boolean | `true` if pickup time is scheduled, `false` otehrwise. (*Ask for Pickup Date and Time at Checkout* option in pickup settings)
pickupPreparationTimeHours | number | Amount of time required for store to prepare pickup (*Order Fulfillment Time* setting)
pickupBusinessHours | string \<*PickupBusinessHours*\> | Available and scheduled times to pickup orders

#### CarrierSettings

Field | Type | Description
----- | ---- | -----------
defaultCarrierAccountEnabled | boolean | `true` if default Ecwid account is enabled to calculate the rates. `false` otherwise
defaultPostageDimensions | \<*DefaultPostageDimensions*\> | Default postage dimensions for this shipping option

#### DefaultPostageDimensions
Field | Type | Description
----- | ---- | -----------
length | number | Length of postage
width | number | Width of postage
height | number | Height of postage

#### FlatRate

Field | Type | Description
----- | ---- | -----------
rateType | string | One of `"ABSOLUTE"`, `"PERCENT"`
rate | number | Shipping rate

#### TableRatesDetails

Field | Type | Description
----- | ---- | -----------
tableBasedOn | string | What is this table rate based on. Possible values: `"subtotal"`, `"discountedSubtotal"`, `"weight"`
rates | Array \<*TableRate*\> | Details of table rate

#### TableRate

Field | Type | Description
----- | ---- | -----------
conditions | \<*TableRateConditions*\> | Conditions for this shipping rate in custom table
rate | \<*TableRateDetails*\> | Table rate details

#### TableRateConditions

Field | Type | Description
----- | ---- | -----------
weightFrom | number | "Weight from" condition value
weightTo | number | "Weight to" condition value
subtotalFrom | number | "Subtotal from" condition value
subtotalTo | number | "Subtotal to" condition value
discountedSubtotalFrom | number | "Discounted subtotal from" condition value
discountedSubtotalTo | number | "Discounted subtotal from" condition value

#### TableRateDetails

Field | Type | Description
----- | ---- | -----------
perOrderAbs | number | Absolute per order rate
perOrderPercent | number | Percent per order rate
perItem | number | Absolute per item rate
perWeightUnitRate | number | Absolute per weight rate

#### PickupBusinessHours

Field | Type | Description
----- | ---- | -----------
MON | Array time range | Array of time ranges in format `["FROM TIME", "TO TIME"]`. Ex: `['08:30', '13:30'], ['13:30', '19:00']`
TUE | Array time range | Array of time ranges in format `["FROM TIME", "TO TIME"]`. Ex: `['08:30', '13:30'], ['13:30', '19:00']`
WED | Array time range | Array of time ranges in format `["FROM TIME", "TO TIME"]`. Ex: `['08:30', '13:30'], ['13:30', '19:00']`
THU | Array time range | Array of time ranges in format `["FROM TIME", "TO TIME"]`. Ex: `['08:30', '13:30'], ['13:30', '19:00']`
FRI | Array time range | Array of time ranges in format `["FROM TIME", "TO TIME"]`. Ex: `['08:30', '13:30'], ['13:30', '19:00']`
SAT | Array time range | Array of time ranges in format `["FROM TIME", "TO TIME"]`. Ex: `['08:30', '13:30'], ['13:30', '19:00']`
SUN | Array time range | Array of time ranges in format `["FROM TIME", "TO TIME"]`. Ex: `['08:30', '13:30'], ['13:30', '19:00']`

#### TaxSettings
*System Settings → Taxes*

Field | Type | Description
----- | ---- | -----------
automaticTaxEnabled | boolean | `true` if store taxes are calculated automatically, `else` otherwise. As seen in the *Ecwid Control Panel > Settings > Taxes > Automatic*
taxes | Array\<*Taxes*\> | Manual tax settings for a store

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
rules | Array\<*TaxRule*\>  | Tax rates

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
countryCodes | Array\<*string*\> | Country codes this zone includes . 
stateOrProvinceCodes |  Array\<*string*\> | State or province codes the zone includes
postCodes | Array\<*string*\> |   Postcode (or zipcode) templates this zone includes. More details: [Destination zones in Ecwid](http://help.ecwid.com/customer/portal/articles/1163922-destination-zones)

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

#### LegalPagesSettingsDetails
*System Settings → General → Legal Pages*

Field | Type | Description
----- | ---- | -----------
requireTermsAgreementAtCheckout | boolean | `true` if customers must agree to store's terms of service at checkout
legalPages | \<*LegalPagesInfo*\> | Information about the legal pages set up in a store

#### LegalPagesInfo

Field | Type | Description
----- | ---- | -----------
type | string | Legal page type. One of: `"LEGAL_INFO"`, `"SHIPPING_COST_PAYMENT_INFO"`, `"REVOCATION_TERMS"`, `"TERMS"`, `"PRIVACY_STATEMENT"`
enabled | boolean | `true` if legal page is shown at checkout process, `false` otherwise
title | string | Legal page title
display | string | Legal page display mode – in a popup or on external URL. One of: `"INLINE"`, `"EXTERNAL_URL"`
text | string | HTML contents of a legal page
externalUrl | string | URL to external location of a legal page

#### PaymentInfo

Field | Type | Description
----- | ---- | -----------
paymentOptions | Array\<*PaymentOptionInfo*\> | Details of all payment methods set up in that store

#### PaymentOptionInfo

Field | Type | Description
----- | ---- | -----------
id | string | Payment method ID in a store
enabled | boolean | `true` if payment method is enabled and shown in storefront, `false` otherwise
checkoutTitle | string | Payment method title at checkout 
checkoutDescription | string | Payment method description at checkout (subtitle)
paymentProcessorId | string | Payment processor ID in Ecwid
paymentProcessorTitle | string | Payment processor title. The same as `paymentModule` in order details in REST API
orderBy | number | Payment method position at checkout and in Ecwid Control Panel. The smaller the number, the higher the position is
appClientId | 'string' | client_id value of payment application. `""` if not an application 
instructionsForCustomer | \<*InstructionsForCustomerInfo*\> | Customer instructions details

#### InstructionsForCustomerInfo

Field | Type | Description
----- | ---- | -----------
instructionsTitle | string | Payment instructions title
instructions | string | Payment instructions content. Can contain HTML tags

#### FeatureTogglesInfo 

Field | Type | Description
----- | ---- | -----------
name | string | Feature name
visible | boolean | `true` if feature is shown to merchant in *Ecwid Control Panel > Settings > What's new*. `false` otherwise
enabled | boolean | `true` if feature is enabled and active in store

#### DesignSettingsInfo

Field | Type | Description
----- | ---- | -----------
DESIGN_CONFIG_FIELD_NAME | string or boolean | Store design settings as seen in [storefront design customization](https://developers.ecwid.com/api-documentation/customize-appearance). If a specific config field is not provided, it will not be changed

#### AbandonedSalesSettings

Field | Type | Description
----- | ---- | -----------
autoAbandonedSalesRecovery | boolean | `true` if abandoned sale recovery emails are sent automatically, `false` otherwise

#### SalePriceSettings

Field | Type | Description
----- | ---- | -----------
displayOnProductList | boolean | `true` if sale price is displayed on product list and product details page. `false` if sale price is displayed on product details page only
oldPriceLabel | string | Text label for sale price name
displayDiscount | string | Show discount in three modes: `"NONE"`, `"ABS"` and `"PERCENT`

#### Errors

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
402 | The request is not available on the merchant plan. To use Ecwid API, store [must be on a paid plan](https://developers.ecwid.com/api-documentation/api-availability-on-ecwid-plans)
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the coupon info because of an error on the server

### Update store profile

Update basic store information in an Ecwid store: settings, store location, email, etc.

> Request example -- update general info

```http
PUT /api/v3/4870020/profile?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

```json
{
    "generalInfo": {
        "storeUrl": "http://www.example.com/store/"
    },
    "settings": {
        "closed": false,
        "storeName": "My Cool Store",
        "storeDescription": "<p>Welcome to my store!</p>",
        "googleRemarketingEnabled": false,
        "googleAnalyticsId": "UA-654321-1",
        "fbPixelId": "12305215151521",
        "hideOutOfStockProductsInStorefront": false,
        "askCompanyName": true,
        "favoritesEnabled": false,
        "abandonedSales": {
          "autoAbandonedSalesRecovery": true
        },
        "salePrice": {
          "displayOnProductList": true,
          "oldPriceLabel": "was",
          "displayDiscount": "PERCENT"
        }
    },
    "company": {
      "companyName": "My Company, Inc",
      "email": "store@example.com",
      "street": "144 West D Street",
      "city": "Encinitas",
      "countryCode": "US",
      "postalCode": "92024",
      "stateOrProvinceCode": "CA",
      "phone": "1(800)5555555"
    },
    "legalPagesSettings": {
      "legalPages": [
        {
          "type": "LEGAL_INFO",
          "enabled": false
        }
      ]
    },
    "designSettings": {
        "product_list_image_size": "MEDIUM",
        "product_list_image_aspect_ratio": "SQUARE",
        "product_list_product_info_layout": "CENTER",
        "product_list_show_additional_image_on_hover": true,
        "product_list_title_behavior": "SHOW",
    }
}
```

> Request example -- create destination zones

```http
PUT /api/v3/4870020/profile?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

```json
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

#### Request body

A JSON object of type 'Profile' with the following fields:

#### Profile
Field | Type | Description
----- | ---- | -----------
generalInfo | \<*GeneralInfo*\> | Store basic data
account | \<*Account*\> | Store owner's account data
settings | \<*Settings*\> | Store general settings
mailNotifications | \<*MailNotifications*\> | Mail notifications settings
company | \<*Company*\> | Company info
formatsAndUnits | \<*FormatsAndUnits*\> | Store formats/untis settings
languages | \<*Languages*\> | Store language settings
shipping | \<*Shipping*\> | Store shipping settings (only handling fee is included at the moment)
taxSettings | \<*TaxSettings*\> | Store taxes settings
zones | Array\<*Zone*\> | Store destination zones
businessRegistrationID | \<*BusinessRegistrationID*\> | Company registration ID, e.g. VAT reg number or company ID, which is set under Settings / Invoice in Control panel
legalPagesSettings | \<*LegalPagesSettingsDetails*\> | Legal pages settings for a store (*System Settings → General → Legal Pages*)
designSettings | \<*DesignSettingsInfo*\> | Design settings of an Ecwid store. Can be overriden by [updating store profile](https://developers.ecwid.com/api-documentation/store-information#update-store-profile) or by [customizing design](https://developers.ecwid.com/api-documentation/customize-appearance) via JS config in storefront. If `designSettings` field is not provided in request, it will not be changed

<aside class="notice">
All fields are optional. Omitted field will not be affected
</aside>

#### GeneralInfo
Field | Type | Description
----- | ---- | -----------
storeUrl | string | Storefront URL. If this field is empty in the store settings and omitted in the request, it will be automatically copied from the current Starter Site URL. **When updating, make sure to add protocol to the URL (`http://` or `https://`)**.
starterSite | \<*StarterSiteInfo*\> | Starter Site settings

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
storeDescription | string | HTML description for the main store page – Store Front page
googleRemarketingEnabled | boolean | `true` if Remarketing with Google Analytics is enabled, `false` otherwise
googleAnalyticsId | string | [Google Analytics ID](https://help.ecwid.com/customer/en/portal/articles/1170264-google-analytics) connected to a store
fbPixelId | string | Your Facebook Pixel ID. [Learn more](https://support.ecwid.com/hc/en-us/articles/115004303345-Step-2-Implement-Facebook-pixel)
orderCommentsEnabled | boolean | Use `true` to enable order comments feature, `false` otherwise
orderCommentsCaption | string | Caption for order comments field in storefront. If the value is empty, the default 'Order comments' caption will be used
orderCommentsRequired | boolean | Use `true` to require order comments to be filled, `false` otherwise
hideOutOfStockProductsInStorefront | boolean | `true` if out of stock products are hidden in storefront, `false` otherwise. This setting is located in Ecwid Control Panel > Settings > General > Cart
askCompanyName | boolean | `true` if "Ask for the company name" in checkout settings is enabled, `false` otherwise
favoritesEnabled | boolean | `true` if favorites feature is enabled for storefront, `false` otherwise
abandonedSales | \<*AbandonedSalesSettings*\> | Abandoned sales settings
salePrice | \<*SalePriceSettings*\ | Sale (compare to) price settings 

#### MailNotifications
Field | Type | Description
----- | ---- | -----------
adminNotificationEmails | Array\<*string*\> | Email addresses, which the store admin notifications are sent to
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
dateFormat | string | Date format. Only these formats are accepted: `"dd-MM-yyyy"`, `"dd/MM/yyyy"`, `"dd.MM.yyyy"`, `"MM-dd-yyyy"`, `"MM/dd/yyyy"`, `"yyyy/MM/dd"`, `"MMM d, yyyy"`, `"MMMM d, yyyy"`, `"EEE, MMM d, ''yy"`, `"EEE, MMMM d, yyyy"`
timeFormat | string | Time format. Only these formats are accepted: `"HH:mm:ss"`, `"HH:mm"`, `"hh.mm.ss a"`, `"hh:mm a"`
timezone | string | Store timezone, e.g. `Europe/Moscow`
dimensionsUnit | string | Product dimensions units. Possible values: `IN`,`CM`,`MM`,`YD`
orderNumberPrefix | string | Order number prefix in a store
orderNumberSuffix | string | Order number suffix in a store

#### Languages
*System Settings → General → Languages*

Field | Type | Description
----- | ---- | -----------
enabledLanguages | Array\<*string*\> | A list of enabled languages in the storefront. Use first item to set default storefront language

#### Shipping
*System Settings → Shipping*

Field | Type | Description
----- | ---- | -----------
handlingFee | \<*HandlingFee*\> | Handling fee settings

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
taxes | Array\<*Taxes*\> | Manual tax settings for a store

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
rules | Array\<*TaxRule*\>  | Tax rates

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
countryCodes | Array\<*string*\> | Country codes this zone includes
stateOrProvinceCodes |  Array\<*string*\> | State or province codes the zone includes
postCodes | Array\<*string*\> |   Postcode (or zipcode) templates this zone includes. More details: [Destination zones in Ecwid](http://help.ecwid.com/customer/portal/articles/1163922-destination-zones)

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

#### LegalPagesSettingsDetails
*System Settings → General → Legal Pages*

Field | Type | Description
----- | ---- | -----------
requireTermsAgreementAtCheckout | boolean | `true` if customers must agree to store's terms of service at checkout
legalPages | \<*LegalPagesInfo*\> | Information about the legal pages set up in a store

#### LegalPagesInfo

Field | Type | Description
----- | ---- | -----------
type | string | Legal page type. One of: `"LEGAL_INFO"`, `"SHIPPING_COST_PAYMENT_INFO"`, `"REVOCATION_TERMS"`, `"TERMS"`, `"PRIVACY_STATEMENT"`
enabled | boolean | `true` if legal page is shown at checkout process, `false` otherwise
title | string | Legal page title
display | string | Legal page display mode – in a popup or on external URL. One of: `"INLINE"`, `"EXTERNAL_URL"`
text | string | HTML contents of a legal page
externalUrl | string | URL to external location of a legal page

#### DesignSettingsInfo

Field | Type | Description
----- | ---- | -----------
DESIGN_CONFIG_FIELD_NAME | string or boolean | Store design settings as seen in [storefront design customization](https://developers.ecwid.com/api-documentation/customize-appearance). If a specific config field is not provided, it will not be changed

#### AbandonedSalesSettings

Field | Type | Description
----- | ---- | -----------
autoAbandonedSalesRecovery | boolean | `true` if abandoned sale recovery emails are sent automatically, `false` otherwise

#### SalePriceSettings

Field | Type | Description
----- | ---- | -----------
displayOnProductList | boolean | `true` if sale price is displayed on product list and product details page. `false` if sale price is displayed on product details page only
oldPriceLabel | string | Text label for sale price name
displayDiscount | string | Show discount in three modes: `"NONE"`, `"ABS"` and `"PERCENT`

#### Response

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


#### Errors

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
402 | Cannot update settings because the limit of number of zones or taxes is reached
409 | Cannot update profile because such nickname or email is already registered in the system
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the coupon info because of an error on the server

### Upload store logo

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

> Python Example

```python
import requests

request_url = "https://app.ecwid.com/api/v3/1003/profile/logo?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/profile/logo?token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading a store logo, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
413 | The image file is too large (Maximum allowed file size is 20Mb)
415 | Unsupported content-type: expected `application/octet-stream`
422 | The uploaded file is not an image
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



### Remove store logo

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

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



### Upload invoice logo

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

> Python Example

```python
import requests

request_url = "https://app.ecwid.com/api/v3/1003/profile/invoicelogo?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/profile/invoicelogo?token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading an invoice logo, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/octet-stream`
422 | The uploaded file is not an image
413 | The image file is too large. Maximum allowed file size is 20Mb.
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



### Remove invoice logo

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

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message




### Upload email logo

Upload store logo displayed in order emails. The logo itself is to be placed in the request body. Maximum allowed file size is 20Mb.

> Request example

```http
POST /api/v3/4870020/profile/emaillogo?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/profile/emaillogo?token=abcdefg123456';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $file);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: image/jpeg;'));

$result = curl_exec($ch);
curl_close ($ch);
```

> Python Example

```python
import requests

request_url = "https://app.ecwid.com/api/v3/1003/profile/emaillogo?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/profile/emaillogo?token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading an email logo, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/octet-stream`
422 | The uploaded file is not an image
413 | The image file is too large. Maximum allowed file size is 20Mb.
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message



### Remove email logo

Remove store logo, which is displayed on order invoices

> Request example

```http
DELETE /api/v3/4870020/profile/emaillogo?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/profile/emaillogo?token={token}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token

#### Response

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

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message





### Get store update statistics

This method provides simple 'Latest updates' statistics about store profile, products, orders, categories and discount coupons. Use it to check whether something was changed in an Ecwid store. This could be helpful to keep data in your application up-to-date and avoid abusing API to get and parse large amounts of data to check its state.

Also, you can consider using [webhooks](#webhooks) to get a notificaiton when changes are made to orders and catalog items. For example, you can get a webhook when a new order is placed in a store and send order details to a warehouse right away.

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

#### Response

> Response example (JSON)

```json
{
    "productsUpdated": "2014-10-19 18:56:21 +0400",
    "ordersUpdated": "2014-10-15 16:54:11 +0400",
    "profileUpdated": "2014-10-19 18:55:35 +0400",
    "categoriesUpdated": "2014-10-19 12:23:12 +0400",
    "discountCouponsUpdated": "2017-02-10 08:03:43 +0000",
    "abandonedSalesUpdated": "2017-02-10 08:03:43 +0000",
    "customersUpdated": "2017-02-10 08:03:43 +0000"
}
```

A JSON object containing the update dates statistics

#### UpdateStats
Field | Type | Description
----- | ---- | -----------
productsUpdated | string | Date of the latest changes in store catalog (products, categories), e.g. `2014-10-15 16:54:11 +0400`
ordersUpdated | string | Date of the latest changes in store orders, e.g. `2014-10-15 16:54:11 +0400`
profileUpdated | string | Date of the latest changes in store information, e.g. `2014-10-15 16:54:11 +0400`
categoriesUpdated | string | Date of the latest changes in store categories, e.g. `2014-10-19 12:23:12 +0400`
discountCouponsUpdated | string | Date of the latest changes in store discount coupons, e.g. `2014-10-19 12:23:12 +0400`
abandonedSalesUpdated | string | Date of the latest changes to abandoned carts in a store, e.g. `2014-10-19 12:23:12 +0400`
customersUpdated | string | Date of the latest changes to customers in a store, e.g. `2014-10-19 12:23:12 +0400`

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | There was an internal server error while processing the request

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Get deleted items statistics
Using this group of API methods, you can get a list of products, orders, customers and coupons that have been deleted from the store. In combination with the update statistics, you can use these endpoints to check whether something was changed in an Ecwid store and keep data in your application synchronized with the Ecwid store you're working with. 

Also, you can consider using [webhooks](#webhooks) to get a notificaiton when orders and catalog items are deleted. For example, you can get a webhook right after a product is deleted from an Ecwid store and adjust your custom product list in external system.

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


#### Response

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
            "id": 34256365236,
            "date": "2014-10-20 18:00:54 +0400"
        },
        {
            "id": 123123213213,
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
items | Array\<*RemovedItem*\> | The removed items list with IDs and dates

#### Removed item
Field | Type | Description
----- | ---- | -----------
id | number | Item ID. Depending on the request, that is products ID, customer ID, order number or coupon ID.
date | string | Item deletion date

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | There was an internal server error while processing the request

#### Error response body (optional)
Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message
