# Product filters

> Product filters interface in storefront

> ![Product filters interface](https://don16obqbay2c.cloudfront.net/wp-content/uploads/filters-1549440733.png)

When stores have more than 50-100 products customers can find it hard to find the right product. Use the product filters functionality to let your customers find products fast and easy.

For example, you can filter the search results or category products by: price, product options, product attributes, on sale status and stock availability and more. 

**Table of contents**

- [Manage store filters](https://developers.ecwid.com/api-documentation/manage-store-filters)
- [Use filters in storefront](https://developers.ecwid.com/api-documentation/use-filters-in-storefront)
- [Get store filters](https://developers.ecwid.com/api-documentation/get-store-filters)

Learn more about product filters in our [Help Center](https://support.ecwid.com/hc/en-us/articles/207807925). 

## Manage store filters

Merchants can manage their store filters in the *Ecwid Control Panel → Settings → Product filters*. 

There you can enable the product filters feature and enable/disable specific filters, like: specific product options, attributes and more.

[Product filters in Ecwid Control Panel](https://my.ecwid.com/cp/#product-filters)

### Enabling filters

Product filters can be enabled when a store is on the **Ecwid Business plan** or higher. [Enable product filters](https://my.ecwid.com/cp/#product-filters)

**Check widget availability**

You can check whether filters are enabled in storefront using the Store profile endpoint → `productFiltersSettings`. [Get store profile endpoint](https://developers.ecwid.com/api-documentation/store-information#get-store-profile)

### Change filters position

You can display product widgets on the left or right side of the storefront area. 

It is possible to set position for both the search page and all category pages in a store. 

**Change position using REST API at any time**

Use the  `product_filters_position_search_page`,`product_filters_position_category_page` to get and update position of filters widget. See the `designSettings` field.

[Store profile endpoint](https://developers.ecwid.com/api-documentation/store-information)

**Change position using JS in storefront**

Use `product_filters_position_category_page` and `product_filters_position_search_page` in Ecwid's storefront config to change their position on corresponding pages. 

[Customize appearance documentation](https://developers.ecwid.com/api-documentation/customize-appearance#control-display-of-elements-in-product-grid)

### Display filters by default

Product filters can be displayed right away when a customer opens a category page. 

Use `product_filters_opened_by_default_on_category_page` in Ecwid's storefront config to display filters by default. 

[Customize appearance documentation](https://developers.ecwid.com/api-documentation/customize-appearance#control-display-of-elements-in-product-grid)

## Use filters in storefront

When enabled, product filters can be used by your customers in the storefront on search and categories pages. Look for "Refine by" block on those pages.

See how to enable filters and manage them in our [Help Center](https://support.ecwid.com/hc/en-us/articles/207807925).

### Filters in query parameters

> Filters in query parameters on search page example

```http
GET https://mdemo.ecwid.com/search?keyword=surfboard&inventory=instock
```

If you'd like to create your own custom filter widget and use Ecwid storefront widget, you can use query parameters in the storefront to filter products on a page. 

Filters in query parameters are available for search and category pages. 

**Query parameters for filters**

Field | Type |  Description
--------- | ---------| -----------
keyword | string | Search by a specific phrase
priceFrom | number | Minimum product price
priceTo | number | Maximum product price
categories | string | Search within these categories. Supports IDs separated by comma and value `"home"` that refers to the Store Home Page
includeProductsFromSubcategories | boolean | Defines whether Ecwd should search in subcategories too. Only makes sense when `categories` parameter is set
createdFrom | string | Product create date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul> Example: <ul><li>`1447804800`</li> </ul>
createdTo | string | Product last create date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedFrom | string | Product last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedTo | string | Product last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
option_{optionName} | string | Filter by product option values. Format: `option_{optionName}=param[,param]`, where `optionName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `option_Size=S,M,L&option_Color=Red,Black` 
attribute_{attributeName} | string | Filter by product attribute values. Format: `attribute_{attributeName}=param[,param]`, where `attributeName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `attribute_Brand=Apple&attribute_Capacity=32GB,64GB` 
inventory | string | Search instock or out of stock products. Possible values: `"instock"`,`"outofstock"`
onsale | string | Search on sale products. Possible values: `"onsale"`, `"notonsale"`

### Filters in custom product listing

Some developers create a whole entirely new and custom product listing for their stores. 

Since the listing is retrieved from the API, you will need a way to get available filters and products to display. 

**1. Get available filters**

Check the [Filters in REST API](/api-documentation/get-store-filters#filters-in-rest-api) section on getting filters for your particular store. 

**2. Get products according to filters**

When you received all the needed filters from the Ecwid API, use those filters when [searching for products](https://developers.ecwid.com/api-documentation/products#search-products).

As a result, you will have the list of products that follow your filters and you'll just need to display them in your custom product listing. 

## Get store filters

Each store can have its own set of product filters: some will have pricing and stock availability; some will have specific product options, like Size and Color. 

Learn about adding more filters or disabling some of them in [Manage store filters](/api-documentation/manage-store-filters) section.

### Filters in Ecwid REST API

Get available store filters from Ecwid API. Filter out the results by specific product options, price, keywords, attributes and more. 

<aside class="notice">
To access the Ecwid API Platform features, make sure you have a registered application and a test Ecwid store on a paid plan. <a href="/begin-development">Learn more</a>
</aside>

> Request example

```http
GET /api/v3/4870020/products/filters?filterParentCategoryId=123532&inventory=instock&token=1234567890qwqeertt HTTP/1.1
Host: app.ecwid.com
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache
Accept-Encoding: gzip
```

`GET https://app.ecwid.com/api/v3/{storeId}/products/filters?filterFields={filterFields}&filterFacetLimit={filterFacetLimit}&filterParentCategoryId={filterParentCategoryId}&keyword={keyword}&priceFrom={priceFrom}&priceTo={priceTo}&categories={categories}&includeProductsFromSubcategories={includeProductsFromSubcategories}&createdFrom={createdFrom}&createdTo={createdTo}&updatedFrom={updatedFrom}&updatedTo={updatedTo}&enabled={enabled}&inventory={inventory}&onsale={onsale}&field{attributeName}={attributeValues}&field{attributeId}={attributeValues}&option_{optionName}={optionValues}&attribute_{attributeName}={attributeValues}&token={token}`

<aside class="notice">
Parameters in <strong>bold</strong> are mandatory
</aside>

**Filter limits**

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token
**filterFields** | string | Comma-separated list of filters for Ecwid to return. Supported filters: `"price"`,`"inventory"`,`"onsale"`,`"categories"`, `"option_{optionName}"`, `"attribute_{attributeName}"`. Example: `"price,inventory,option_Size,attribute_Brand,categories"`
filterFacetLimit | string | Set the number of filter values in response. Individual limit example: `"onsale:all,attribute_Brand:50,option_Color:10"`. General limit example: `"10"`. Use `"all"` to return all facets. Default limit is 50
filterParentCategoryId | string | Set the parent category ID for subcategories filter. `"0"` or `"home"` or empty value means there is no parent category

**Product limits**

Name | Type    | Description
---- | ------- | --------------
keyword | string | Search term. Use quotes to search for exact match. Ecwid searches products over multiple fields: <ul><li>title</li><li>description</li><li>SKU</li><li>product options</li><li>category name</li><li>gallery image descriptions</li><li>attribute values (except for hidden attributes). If your keywords contain special characters, it may make sense to URL encode them before making a request
priceFrom | number | Minimum product price
priceTo | number | Maximum product price
categories | string | Category IDs separated by a comma. Use `"home"` to get Store Home Page products. Example: `"18265,12324,home"`
includeProductsFromSubcategories | boolean | Use `true` if you need to include products from subcategories. Use `false` otherwise. `false` is the default value
createdFrom | string | Product create date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul> Example: <ul><li>`1447804800`</li> </ul>
createdTo | string | Product last create date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedFrom | string | Product last update date/time (lower bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
updatedTo | string | Product last update date/time (upper bound). Supported formats: <ul><li>*UNIX timestamp*</li> </ul>
enabled | boolean | Use `true` if you need only enabled products. Use `false` if you need both enabled and disabled products
field{attributeName} | string | Filter by product attribute values. Format: `field{attributeName}=param[,param]`, where `attributeName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `fieldBrand=Apple&fieldCapacity=32GB,64GB` 
field{attributeId} | string | Filter by product attribute values. Works the same as the filter by `field{attributeName}` but attribute IDs are used instead of attribute names. This way is insensitive to attributes renaming.
option_{optionName} | string | Filter by product option values. Format: `option_{optionName}=param[,param]`, where `optionName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `option_Size=S,M,L&option_Color=Red,Black` 
attribute_{attributeName} | string | Filter by product attribute values. Format: `attribute_{attributeName}param[,param]`, where `attributeName` is the attribute name and `param` is the attribute value. You can place several values separated by comma. In that case, values will be connected through logical "OR", and if the product has at least one of them it will get to the search results. Example:<br /> `attribute_Brand=Apple&attribute_Capacity=32GB,64GB` 
inventory | string | Use `"instock"` to get in stock items only or `"outofstock"` for out of stock items. 
onsale | string | Use `"onsale"` to get on sale items only or `"notonsale"` for items not currently on sale.


#### Response

> Response example (JSON)

```json
{
    "productCount": 45,
    "filters": {
        "price": {
            "minValue": 0,
            "maxValue": 14776.3
        },
        "inventory": {
            "values": [
                {
                    "id": "instock",
                    "title": "In stock",
                    "productCount": 35
                },
                {
                    "id": "outofstock",
                    "title": "Out of stock",
                    "productCount": 10
                }
            ]
        },
        "onsale": {
            "values": [
                {
                    "id": "notonsale",
                    "title": "Regular price",
                    "productCount": 43
                },
                {
                    "id": "onsale",
                    "title": "On sale",
                    "productCount": 2
                }
            ]
        },
        "categories": {
            "values": [
                {
                    "id": 19563207,
                    "title": "Shorts",
                    "productCount": 5
                },
                {
                    "id": 21579001,
                    "title": "Hats",
                    "productCount": 4
                },
                {
                    "id": 21898001,
                    "title": "Sale",
                    "productCount": 2
                },
                {
                    "id": 22515002,
                    "title": "Accessories",
                    "productCount": 2
                }
                {
                    "id": 19976009,
                    "title": "Apparel",
                    "productCount": 2
                }
            ]
        },
        "attribute_Brand": {
            "values": [
                {
                    "title": "Sunshine",
                    "productCount": 2
                },
                {
                    "title": "Envelope",
                    "productCount": 1
                }
            ]
        },
        "option_Size": {
            "values": [
                {
                    "title": "S",
                    "productCount": 7
                },
                {
                    "title": "L",
                    "productCount": 6
                },
                {
                    "title": "M",
                    "productCount": 6
                },
                {
                    "title": "Large",
                    "productCount": 4
                },
                {
                    "title": "Medium",
                    "productCount": 4
                },
                {
                    "title": "Small",
                    "productCount": 4
                },
                {
                    "title": "XL",
                    "productCount": 4
                },
                {
                    "title": "2XL",
                    "productCount": 3
                }
            ]
        }
    }
}
```

A JSON object of type 'SearchResult' with the following fields:

#### SearchResult

Field | Type | Description
----- | ---- | -----------
productCount | number | The total number of found products
filters | \<*ProductFilters*\> | Store filters returned for this request

#### ProductFilters

Field | Type | Description
----- | ---- | -----------
price | \<*PriceFilter*\> | Range of available prices
inventory | Array \<*FilterValue*\> | Available values for inventory filter
onsale | Array \<*FilterValue*\> | Available values for on sale filter
categories | Array \<*FilterValue*\> | List of categories result products are assigned to
option_{optionName} | Array \<*FilterValue*\> | List of available values for a specific product option
attribute_{attributeName} | Array \<*FilterValue*\> | List of available values for a specific product attribute

#### PriceFilter

Field | Type | Description
----- | ---- | -----------
minValue | number | The minimum price found for these products
maxValue | number | The maximum price found for these products

#### FilterValue

Field | Type | Description
----- | ---- | -----------
id | number/string | Filter value ID: category ID, `"onsale"`, `"outofstock"`
**title** | string | Filter value name: option value, attribute value, "On sale", "Out of Stock", etc.  
**productCount** | number | Number of products found for this specific filter

#### Errors

> Error response example

```http
HTTP/1.1 400 Bad request
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning | Code (optional)
------------|--------|-----------
400 | Request parameters are malformed | 
403 | Access token doesn't have `read_catalog` scope | 
415 | Unsupported content-type: expected `application/json` or `text/json` | 
500 | Cannot complete request because of an error on the server | 

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

