## Product types

Product types (or product classes) are groups of products which share the same attributes. Product attributes contain additional product information, e.g. ISBN, UPC, Brand, which is displayed in storefront and included in product feeds when exporting to marketplaces like Google Shopping, eBay, Amazon ads etc. Learn more about product types and attributes in [Ecwid Help center](http://help.ecwid.com/customer/portal/articles/1167365-product-types).

### Get product types

Get all product types present in an Ecwid store. 

#### Request

> Request example

```http
GET /api/v3/4870020/classes?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/classes?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

#### Response

> Response example (JSON)

```json
[
  {
    "id":0,
    "attributes":[
      {
        "id":5029057,
        "name":"Brand",
        "type":"BRAND",
        "show":"DESCR"
      },
      {
        "id":5029058,
        "name":"UPC",
        "type":"UPC",
        "show":"DESCR"
      }
    ]
  },
  {
    "id":4208002,
    "name":"Body Weight Scale Accessories",
    "googleTaxonomy":"Health & Beauty > Health Care > Biometric Monitor Accessories > Body Weight Scale Accessories",
    "attributes":[
      {
        "id":5508061,
        "name":"UPC",
        "type":"UPC",
        "show":"DESCR"
      },
      {
        "id":5508062,
        "name":"Brand",
        "type":"BRAND",
        "show":"DESCR"
      }
    ]
  }
]
```

A JSON array of objects of type 'ProductClass' with the following fields:

#### ProductClass
Field | Type | Description
----- | ---- | -----------
id | number | Product type (class) internal unique ID. Class with ID `0` is the default 'General' type assigned to all products by default
name | string | Product type name. Empty for the "General" type
googleTaxonomy | string | Google taxonomy associated with this type
attributes | Array\<*Attribute*\> | Product type attributes

#### Attribute
Field | Type  | Description
----- | ----- | -----------
id | number | Attribute internal unique ID
name |  string | Attribute title
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit' attributes. The 'type' field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines how and where to display the product attribute value: `NOTSHOW`, `DESCR`, `PRICE`. For [public tokens](#access-tokens), `NOTSHOW` attributes are not returned

#### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the product type info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message





### Get product type

Get the full details of a specific product type referring to its ID.

#### Request

> Request example

```http
GET /api/v3/4870020/classes/4208002?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/classes/{classId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**classId** | number | Product type internal ID

#### Response

> Response example (JSON)

```json
  {
    "id":4208002,
    "name":"Body Weight Scale Accessories",
    "googleTaxonomy":"Health & Beauty > Health Care > Biometric Monitor Accessories > Body Weight Scale Accessories",
    "attributes":[
      {
        "id":5508061,
        "name":"UPC",
        "type":"UPC",
        "show":"DESCR"
      },
      {
        "id":5508062,
        "name":"Brand",
        "type":"BRAND",
        "show":"DESCR"
      }
    ]
  }
```

A JSON object of type 'ProductClass' with the following fields:

#### ProductClass
Field | Type | Description
----- | ---- | -----------
id | number | Product class internal unique ID. Class with ID `0` is the default 'General' type assigned to all products by default
name | string | Product type name. Empty for the "General" type
googleTaxonomy | string | Google taxonomy associated with this type
attributes | Array\<*Attribute*\> | Product type attributes

#### Attribute
Field | Type  | Description
----- | ----- | -----------
id | number | Attribute internal unique ID
name |  string | Attribute title
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit' attributes. The 'type' field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines how and where to display the product attribute value: `NOTSHOW`, `DESCR`, `PRICE`. For [public tokens](#access-tokens), `NOTSHOW` attributes are not returned


#### Errors

> Error response example

```http
HTTP/1.1 404 Not Found
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
404 | Product type is not found
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Cannot retrieve the product type info because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message






### Update product type

Update the details of a specific product type referring to its ID.

#### Request

> Request example

```http
PUT /api/v3/4870020/classes/4208002?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache

{
    "name": "New Class Name",
    "attributes": [
        {
            name: "New attribute name",
            type: "CUSTOM",
            show: "DESCR"            
        }
    ]
}
```

`PUT https://app.ecwid.com/api/v3/{storeId}/classes/{classId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**classId** | number | Product type internal ID

#### Request body

A JSON object of type 'ProductClass' with the following fields:

#### ProductClass
Field | Type | Description
----- | ---- | -----------
name | string | Product type name
attributes | Array\<*Attribute*\> | Product type attributes

#### Attribute
Field | Type  | Description
----- | ----- | -----------
id | number | Attribute internal unique ID (leave empty when adding a new attribute)
name |  string | Attribute title
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit' attributes. The 'type' field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines how and where to display the product attribute value: `NOTSHOW`, `DESCR`, `PRICE`

#### Response

> Response example (JSON)

```json
{
    "updateCount": 1
}
```


A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus
Field | Type |  Description
------| ---- | --------------
updateCount | number | The number of updated types (`1` or `0` depending on whether the update was successful)


#### Errors

> Error response example

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, a JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
409 | The type/class with the given name already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message




### Create product type

Create a new product type in an Ecwid store.

#### Request

> Request example

```http
POST /api/v3/4870020/classes?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache

{
"name": "T-Shirts",
    "attributes": [
        {
            "name": "Gender",
            "type": "CUSTOM",
            "show": "DESCR"
        },
        {
            "name": "Age group",
            "type": "CUSTOM",
            "show": "DESCR"
        },
        {
            "name": "Color",
            "type": "CUSTOM",
            "show": "DESCR"
        },
        {
            "name": "Size",
            "type": "CUSTOM",
            "show": "DESCR"
        }
      ]
}
```

`POST https://app.ecwid.com/api/v3/{storeId}/classes?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

#### Request body

A JSON object of type 'ProductClass' with the following fields:

#### ProductClass
Field | Type | Description
----- | ---- | -----------
name | string | Product type name. Ecwid will try to find a Google taxonomy that matches the name you submit here. If it finds, the taxonomy will be assigned to the new type.
attributes | Array\<*Attribute*\> | Product type attributes

#### Attribute
Field | Type  | Description
----- | ----- | -----------
name |  string | Attribute title
type | string | Attribute type. There are user-defined attributes, general attributes and special 'price per unit' attributes. The 'type' field contains one of the following: `CUSTOM`, `UPC`, `BRAND`, `GENDER`, `AGE_GROUP`, `COLOR`, `SIZE`, `PRICE_PER_UNIT`, `UNITS_IN_PRODUCT`
show | string | Defines how and where to display the product attribute value: `NOTSHOW`, `DESCR`, `PRICE`

#### Response

> Response example (JSON)

```json
{
    "id": 4528008
}
```


A JSON object of type 'CreateStatus' with the following fields:

#### CreateStatus
Field | Type |  Description
------| ---- | --------------
id | number | The internal ID of the just created product type


#### Errors

> Error response example

```http
HTTP/1.1 409 Conflict
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, a JSON-formatted body containing error description.

#### HTTP codes

**HTTP Status** | **Response JSON** | Description
-------------- | -------------- | --------------
400 | Request parameters are malformed
409 | The type/class with the given name already exists
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | The request failed because of an error on the server

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message




### Delete product type

Delete a specific product type and its assigned attributes. The products that belong to this type will not be removed. They will be re-assigned to the General type.

#### Request

> Request example

```http
DELETE /api/v3/4870020/classes/4208002?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`DELETE https://app.ecwid.com/api/v3/{storeId}/classes/{classId}?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**classId** | number | Product type internal ID

#### Response

> Response example (JSON)

```json
{
    "deleteCount": 1
}
```


A JSON object of type 'DeleteStatus' with the following fields:

#### DeleteStatus

Field | Type |  Description
----- | ---- | ------------
deleteCount | number | The number of deleted types (`1` or `0` depending on whether the request was successful). It returns `0` when the type with the given ID is not found


#### Errors

> Error response example

```http
HTTP/1.1 400 Wrong numeric parameter 'id' value: not a number or a number out of range
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code

#### HTTP codes

HTTP Status | Meaning
------------|--------
400 | Request parameters are malformed
500 | Cannot retrieve the product type info because of an error on the server
