## Batch requests

Sometimes you may have to send many requests at once or in a short period of time. 

You can send them in parallel to Ecwid API and they will work well with each other. However, in the end, there will still be many connections and requests sent. 

Instead, you can send just one request and Ecwid will do the heavy lifting for you. 

### Create new batch request

You will need to send the URL, method and optional request body to Ecwid. 

The `https://app.ecwid.com/api/v3/{STORE_ID}/` part of the request URL will be added automatically. 

<aside class="notice">
Max amount of API requests for one batch request is 500.
</aside>

In response, Ecwid will start executing your requests and provide you with a unique **ticket ID** for your batch request. Use that ticket ID to [check the status of your batch request](https://developers.ecwid.com/api-documentation/batch-requests#get-batch-request-status).

<aside>
You can use this endpoint to upload files or images. However, they must be provided as a URL in the `externalUrl` parameter, instead binary data in request body.
</aside>

> Request example

```http
POST /api/v3/4870020/batch?token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
Accept-Encoding: gzip
```

> Request format

```json
[
  {
    "id": "{OPTIONAL_UNIQUE_ID}",
    "path": "{API_URL_UNIQUE_PART_WITH_PARAMETERS}",
    "method": "{HTTP_METHOD}",
    "body": "{API_V3_REQUEST_BODY_AS_IS}"
  },
  {
    "id": "{OPTIONAL_UNIQUE_ID}",
    "path": "{API_URL_UNIQUE_PART_WITH_PARAMETERS}",
    "method": "{HTTP_METHOD}",
    "body": "{API_V3_REQUEST_BODY_AS_IS}"
  }
]
```

> Batch request example to get first 100 found orders and store profile

```json
[
  {
    "id": "123456",
    "path": "/orders?limit=1&customeremail=mscott@gmail.com",
    "method": "GET",
    "body": ""
  },
  {
    "id": "1234567",
    "path": "/profile",
    "method": "GET",
    "body": ""
  }
]
```

`POST https://app.ecwid.com/api/v3/{storeId}/batch?token={token}&stopOnFirstFailure={stopOnFirstFailure}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
stopOnFirstFailure | boolean | If `true`, Ecwid will stop executing requests when it detects first error response for an API request. Set `false` to continue executing requests anyway. Default is `true`. [Learn more](https://developers.ecwid.com/api-documentation/batch-requests#handling-failed-requests)

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Request body

An array of JSON objects of type 'BatchRequest' with the following fields:

#### BatchRequest

Field | Type |  Description
------| -----| ------------
id | string | Set an optional request ID so you can find a specific request easier in the response
**path** | string | Path to Ecwid REST API endpoint. Example: `/orders?offset=100&customer=johnsmith@example.com&paymentStatus=PAID,AWAITING_PAYMENT`. It is not necessary to provide access token in this field. List of [all API endpoints](https://developers.ecwid.com/api-documentation/rest-api-reference)
**method** | string | HTTP method you would like to use. Available methods: `"GET"`, `"POST"`, `"PUT"`, `"DELETE"`
body | string | Optional request body for your requests. Required when creating new entities or updating them. For example: send new order details when creating orders, new product details when creating products and so on

#### Response

> Response example (JSON)

```json
{
    "ticket": "11d140a2-966f-4a9f-b4e6-fc451bc78979"
}
```

A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus

Field | Type |  Description
-------------- | -------------- | --------------
ticket | string | Ticket ID for your batch request. Use it to [Get batch request status](https://developers.ecwid.com/api-documentation/batch-requests#get-batch-request-status)

#### Errors

> Error response example

```http
HTTP/1.1 400 Bad request
Content-Type application/json; charset=utf-8
```

```json
{
    "error_message": "Can't create batch. Batch request body is invalid. Path $[0].id contains bad value"
}
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
------------|--------
400 | Request parameters are invalid
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot update the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Get batch request status

Get status of a previously created batch request by its ticket ID. 

> Request example

```http
GET /api/v3/4870020/batch?token=1234567890qwqeertt&ticket=11d140a2-966f-4a9f-b4e6-fc451bc78979 HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/batch?token={token}&ticket={ticket}&escapedJson={escapedJson}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**ticket** | number | Ticket ID you received from Ecwid when creating a batch request
escapedJson | boolean | Set to `true` to get responses to each of your API requests as escaped JSON string in the `escapedHttpBody` field. Set to `false` to get a JSON object in response for your API requests. Default: `false`

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response format example (JSON)

```json
{
  "status": "{QUEUED,IN_PROGRESS,COMPLETED}",
  "totalRequests": "{TOTAL_NUMBER_OF_REQUESTS}",
  "completedRequests": "{COMPLETED_NUMBER_OF_REQUESTS}",
  "responses": [
    {
      "id": "{OPTIONAL_UNIQUE_ID}",
      "status": "{COMPLETED,FAILED,NOT_EXECUTED}",
      "httpBody": "{API_V3_RESPONSE_BODY_AS_IS}",
      "escapedHttpBody": "{ESCAPED_JSON_OF_API_V3_RESPONSE_BODY}", // 'escapedHttpBody' is returned instead of 'httpBody' if 'escapedJson' request parameter is 'true'
      "httpStatusLine": "{HTTP_STATUS_LINE}",
      "httpCode": "{HTTP_STATUS_CODE}"
    },
    {
      "id": "{OPTIONAL_UNIQUE_ID}",
      "status": "{COMPLETED,FAILED,NOT_EXECUTED}",
      "httpBody": "{API_V3_RESPONSE_BODY_AS_IS}",
      "escapedHttpBody": "{ESCAPED_JSON_OF_API_V3_RESPONSE_BODY}", // 'escapedHttpBody' is returned instead of 'httpBody' if 'escapedJson' request parameter is 'true'      
      "httpStatusLine": "{HTTP_STATUS_LINE}",
      "httpCode": "{HTTP_STATUS_CODE}"
    }
  ]
}
```

> Response example 

```json
{
    "status": "COMPLETED",
    "totalRequests": 2,
    "completedRequests": 2,
    "responses": [
        {
            "id": "123456",
            "httpBody": {
                "total": 134,
                "count": 1,
                "offset": 0,
                "limit": 1,
                "items": [
                    {
                        // Basic information
                        "vendorOrderNumber": "lala1028001",
                        "orderNumber": 1028,
                        "subtotal": 1076.64,
                        "total": 2014.97,
                        "usdTotal": 2014.97,
                        "tax": 488.48,
                        "paymentMethod": "Credit or debit card (Mollie)",
                        "paymentStatus": "PARTIALLY_REFUNDED",
                        "fulfillmentStatus": "DELIVERED",
                        
                        // Additional information
                        "refererUrl": "https://mdemo.ecwid.com/",
                        "globalReferer": "https://my.ecwid.com/",
                        "createDate": "2018-05-31 15:08:36 +0000",
                        "updateDate": "2018-05-31 15:09:35 +0000",
                        "createTimestamp": 1527779316,
                        "updateTimestamp": 1527779375,
                        "hidden": false,
                        "orderComments": "Test order comments",
                        "privateAdminNotes": "Must be delivered till Sunday.",

                        // Basic customer information
                        "email": "mscott@gmail.com",
                        "ipAddress": "123.431.234.243",
                        "customerId": 40201284,
                        "customerGroupId": 12345,
                        "customerGroup": "Gold",
                        "customerTaxExempt": false,
                        "customerTaxId": "",
                        "customerTaxIdValid": false,
                        "reversedTaxApplied": false,

                        // Discounts in order
                        "discount": 4,
                        "couponDiscount": 22,
                        "volumeDiscount": 4,
                        "membershipBasedDiscount": 0,
                        "totalAndMembershipBasedDiscount": 0,
                        "customDiscount": [],
                        "discountCoupon": {
                            "id": 29567026,
                            "name": "API Testing",
                            "code": "APITESTING",
                            "discountType": "ABS",
                            "status": "ACTIVE",
                            "discount": 22,
                            "launchDate": "2018-05-24 20:00:00 +0000",
                            "usesLimit": "UNLIMITED",
                            "applicationLimit": "UNLIMITED",
                            "creationDate": "2018-05-31 15:08:33 +0000",
                            "updateDate": "2018-05-24 13:40:32 +0000",
                            "orderCount": 0
                        },
                        "discountInfo": [
                            {
                                "value": 4,
                                "type": "ABS",
                                "base": "ON_TOTAL",
                                "orderTotal": 1
                            }
                        ],
                        
                        // Order items details
                        "items": [
                            {
                                "id": 140273658,
                                "productId": 66722487,
                                "categoryId": 19563207,
                                "price": 1060,
                                "productPrice": 1000,
                                "sku": "ABCA-IAC",
                                "quantity": 1,
                                "shortDescription": "",
                                "tax": 331.01,
                                "shipping": 0,
                                "quantityInStock": 0,
                                "name": "iMac",
                                "isShippingRequired": true,
                                "weight": 0,
                                "trackQuantity": false,
                                "fixedShippingRateOnly": false,
                                "imageUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/391870914.jpg",
                                "smallThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/650638292.jpg",
                                "hdThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/650638293.jpg",
                                "fixedShippingRate": 0,
                                "digital": false,
                                "couponApplied": true,
                                "selectedOptions": [
                                    {
                                        "name": "Price-Optimizer",
                                        "value": "6",
                                        "valuesArray": [
                                            "6"
                                        ],
                                        "selections": [
                                            {
                                                "selectionTitle": "6",
                                                "selectionModifier": 6,
                                                "selectionModifierType": "PERCENT"
                                            }
                                        ],
                                        "type": "CHOICE"
                                    }
                                ],
                                "taxes": [
                                    {
                                        "name": "State tax",
                                        "value": 12,
                                        "total": 124.13,
                                        "taxOnDiscountedSubtotal": 124.13,
                                        "taxOnShipping": 0,
                                        "includeInPrice": false
                                    },
                                    {
                                        "name": "TVA",
                                        "value": 20,
                                        "total": 206.88,
                                        "taxOnDiscountedSubtotal": 206.88,
                                        "taxOnShipping": 0,
                                        "includeInPrice": true
                                    }
                                ],
                                "dimensions": {
                                    "length": 0,
                                    "width": 0,
                                    "height": 0
                                },
                                "couponAmount": 21.66,
                                "discounts": [
                                    {
                                        "discountInfo": {
                                            "value": 4,
                                            "type": "ABS",
                                            "base": "ON_TOTAL",
                                            "orderTotal": 1
                                        },
                                        "total": 3.94
                                    }
                                ]
                            },
                            {
                                "id": 140273659,
                                "productId": 66821181,
                                "categoryId": 0,
                                "price": 16.64,
                                "productPrice": 16,
                                "sku": "001001",
                                "quantity": 1,
                                "shortDescription": "This sturdy white, glossy ceramic mug is an essential to your cupboard. This brawny version of ceramic mugs shows it’s ...",
                                "tax": 157.47,
                                "shipping": 471.85,
                                "quantityInStock": 0,
                                "name": "Mug",
                                "isShippingRequired": true,
                                "weight": 0.4,
                                "trackQuantity": false,
                                "fixedShippingRateOnly": false,
                                "imageUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/389900000.jpg",
                                "smallThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/475772545.jpg",
                                "hdThumbnailUrl": "https://ecwid-images-ru.gcdn.co/images/5035009/408631478.jpg",
                                "fixedShippingRate": 0,
                                "digital": false,
                                "couponApplied": true,
                                "selectedOptions": [
                                    {
                                        "name": "Color",
                                        "value": "White",
                                        "valuesArray": [
                                            "White"
                                        ],
                                        "selections": [
                                            {
                                                "selectionTitle": "White",
                                                "selectionModifier": 0,
                                                "selectionModifierType": "ABSOLUTE"
                                            }
                                        ],
                                        "type": "CHOICE"
                                    },
                                    {
                                        "name": "Size",
                                        "value": "11oz",
                                        "valuesArray": [
                                            "11oz"
                                        ],
                                        "selections": [
                                            {
                                                "selectionTitle": "11oz",
                                                "selectionModifier": 0,
                                                "selectionModifierType": "ABSOLUTE"
                                            }
                                        ],
                                        "type": "CHOICE"
                                    },
                                    {
                                        "name": "Price-Optimizer",
                                        "value": "4",
                                        "valuesArray": [
                                            "4"
                                        ],
                                        "selections": [
                                            {
                                                "selectionTitle": "4",
                                                "selectionModifier": 4,
                                                "selectionModifierType": "PERCENT"
                                            }
                                        ],
                                        "type": "CHOICE"
                                    }
                                ],
                                "taxes": [
                                    {
                                        "name": "State tax",
                                        "value": 12,
                                        "total": 59.05,
                                        "taxOnDiscountedSubtotal": 1.95,
                                        "taxOnShipping": 57.1,
                                        "includeInPrice": false
                                    },
                                    {
                                        "name": "TVA",
                                        "value": 20,
                                        "total": 98.42,
                                        "taxOnDiscountedSubtotal": 3.25,
                                        "taxOnShipping": 95.17,
                                        "includeInPrice": true
                                    }
                                ],
                                "dimensions": {
                                    "length": 0,
                                    "width": 0,
                                    "height": 0
                                },
                                "couponAmount": 0.34,
                                "discounts": [
                                    {
                                        "discountInfo": {
                                            "value": 4,
                                            "type": "ABS",
                                            "base": "ON_TOTAL",
                                            "orderTotal": 1
                                        },
                                        "total": 0.06
                                    }
                                ]
                            }
                        ],

                        // Refund information
                        "refundedAmount": 3.5,
                        "refunds": [
                            {
                                "date": "2017-09-12 10:12:56 +0000",
                                "source": "CP",
                                "reason": "Testing!",
                                "amount": 3.5
                            }
                        ],

                        // Customer addresses
                        "billingPerson": {
                            "name": "Michael Scott",
                            "companyName": "",
                            "street": "555 Lackawanna Ave",
                            "city": "Scranton",
                            "countryCode": "US",
                            "countryName": "United States",
                            "postalCode": "18508",
                            "stateOrProvinceCode": "PA",
                            "stateOrProvinceName": "Pennsylvania",
                            "phone": ""
                        },
                        "shippingPerson": {
                            "name": "Michael Scott",
                            "companyName": "",
                            "street": "555 Lackawanna Ave",
                            "city": "Scranton",
                            "countryCode": "US",
                            "countryName": "United States",
                            "postalCode": "18508",
                            "stateOrProvinceCode": "PA",
                            "stateOrProvinceName": "Pennsylvania",
                            "phone": ""
                        },

                        // Shipping information    
                        "shippingOption": {
                            "shippingCarrierName": "Shipping app the-printful",
                            "shippingMethodName": "USPS Priority Mail",
                            "shippingRate": 471.85,
                            "estimatedTransitTime": "1-3",
                            "isPickup": false
                        },
                        "handlingFee": {
                            "name": "Handling Fee",
                            "value": 4,
                            "description": ""
                        },
                        "predictedPackage": [
                            {
                                "length": 0,
                                "width": 0,
                                "height": 0,
                                "weight": 0.4,
                                "declaredValue": 1076.64
                            }
                        ],
                        "taxesOnShipping": [
                            {
                                "name": "State tax",
                                "value": 12,
                                "total": 57.1
                            },
                            {
                                "name": "TVA",
                                "value": 20,
                                "total": 95.17
                            }
                        ],    

                        // Other information
                        "paymentModule": "CUSTOM_PAYMENT_APP-mollie-pg",
                        "additionalInfo": {
                            "google_customer_id": "2008512504.1526280224"
                        },
                        "paymentParams": {},
                        "extraFields": {
                            "lang": "en",
                            "askHowYouFoundUsApp": "From a friend"
                        },
                        "acceptMarketing": true,
                        "refererId": "Amazon",
                        "disableAllCustomerNotifications": false
                    }
                ]
            },
            "httpStatusCode": 200,
            "httpStatusLine": "OK",
            "status": "COMPLETED"
        },                
        {
            "id": "1234567",
            "httpBody": {
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
                    },
                    "showAcceptMarketingCheckbox": true,
                    "acceptMarketingCheckboxDefaultValue": false,
                    "acceptMarketingCheckboxCustomText": "I accept to receive marketing offers to my email",
                    "askConsentToTrackInStorefront": false,
                    "pinterestTagId": "1251515431215"
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
                    "facebookPreferredLocale": "en_US",
                    "defaultLanguage": "en"
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
                        "carrierMethods": [
                          {
                              "id": "7835-1442850313554:S1",
                              "name": "UPS Next Day Air®",
                              "enabled": true,
                              "orderBy": 1240
                          },
                          {
                              "id": "7835-1442850313554:S2",
                              "name": "UPS Second Day Air®",
                              "enabled": true,
                              "orderBy": 1250
                          },
                          {
                              "id": "7835-1442850313554:S3",
                              "name": "UPS Ground",
                              "enabled": false,
                              "orderBy": 0
                          },
                          {
                              "id": "7835-1442850313554:S4",
                              "name": "UPS Worldwide Express SM",
                              "enabled": true,
                              "orderBy": 1120
                          },
                          {
                              "id": "7835-1442850313554:S5",
                              "name": "UPS Worldwide Expedited SM",
                              "enabled": true,
                              "orderBy": 1100
                          },
                          {
                              "id": "7835-1442850313554:S6",
                              "name": "UPS Standard",
                              "enabled": true,
                              "orderBy": 1030
                          },
                          {
                              "id": "7835-1442850313554:S7",
                              "name": "UPS Three-Day Select®",
                              "enabled": true,
                              "orderBy": 1040
                          },
                          {
                              "id": "7835-1442850313554:S8",
                              "name": "UPS Next Day Air Saver®",
                              "enabled": true,
                              "orderBy": 1000
                          },
                          {
                              "id": "7835-1442850313554:S9",
                              "name": "UPS Next Day Air® Early A.M. SM",
                              "enabled": false,
                              "orderBy": 0
                          },
                          {
                              "id": "7835-1442850313554:S10",
                              "name": "UPS Worldwide Express Plus SM",
                              "enabled": true,
                              "orderBy": 1110
                          },
                          {
                              "id": "7835-1442850313554:S11",
                              "name": "UPS Second Day Air A.M.®",
                              "enabled": true,
                              "orderBy": 1020
                          },
                          {
                              "id": "7835-1442850313554:S12",
                              "name": "UPS Saver",
                              "enabled": false,
                              "orderBy": 1010
                          }
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
                        "description": "1-2",
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
                        "name": "NEW_SALES_LIST",
                        "visible": true,
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
                        "name": "NEW_SHOPPING_CART_PAGE",
                        "visible": false,
                        "enabled": true
                    },
                    {
                        "name": "NEW_CHECKOUT_PAGE",
                        "visible": true,
                        "enabled": true
                    },
                    {
                        "name": "NEW_FB_SHOPS_INTEGRATION",
                        "visible": true,
                        "enabled": false
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
                    "product_filters_position_search_page": "LEFT",
                    "product_filters_position_category_page": "RIGHT",        
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
                },
                "productFiltersSettings": {
                  "enabledInStorefront": true
                }
            },
            "httpStatusCode": 200,
            "httpStatusLine": "OK",
            "status": "COMPLETED"
        }
    ]
}                
```

A JSON object of type 'BatchRequestStatus' with the following fields:

#### BatchRequestStatus

Field | Type | Description
----- | ---- | -----------
status | string | `"QUEUED"` – no requests were completed yet; `"IN_PROGRESS"` – there is at least one completed request; `"COMPLETED"` – All requests were completed
totalRequests | number | Total number of API requests
completedRequests | number | Number of completed API requests
responses | Array /<*RequestDetails*/> | Details of requests made to Ecwid API

#### RequestDetails 

Field | Type | Description
----- | ---- | -----------
id | string | Optional request ID you specified for each API request
status | string | `"COMPLETED"` – All requests were completed; `"FAILED"` – if response HTTP code was not `200OK`; `"NOT_EXECUTED"` – request was not executed, because previous requests failed. See [Handling failed requests](https://developers.ecwid.com/api-documentation/batch-requests#handling-failed-requests) to learn more details
httpBody | json | Response body for your requests. `escapedHttpBody` is returned instead if `escapedJson` parameter is `true` in your batch status request. For example: details of orders found, status of a discount coupon update, id of a product created, etc. Check examples for [each endpoint](https://developers.ecwid.com/api-documentation/rest-api-reference)
escapedHttpBody | string | Escaped JSON string of response body for each API request in a batch. Returned if `escapedJson` parameter is `true` in your batch status request
httpStatusCode | number | HTTP status code from Ecwid API for a request
httpStatusLine | string | HTTP status reason phrase

#### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Description
------------|--------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the order info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Handling failed requests

When making requests to Ecwid API, you may run into errors like 400, 500 and so on. This can happen to your batch requests too. 

So it's important to know how Ecwid handles them and how you can control this. 

**Stopping on first failure**

You can use `stopOnFirstFailure` parameter when [creating your batch request](https://developers.ecwid.com/api-documentation/batch-requests#create-new-batch-request).

It allows you to: ignore failed requests and continue with the batch requests – `false` OR stop execution when a failed request is detected – `true`. 

By default, Ecwid will stop execution of your batch requests. So the default value is `true`.

**Continuing with failed requests**

Ecwid has two modes for working with failed requests when `stopOnFirstFailure` is `false`: 

1) If error code is `4XX`, Ecwid will move on to the next request

2) If error code is `5XX` or request has timed out, Ecwid will retry 5 times with a 3 second interval

