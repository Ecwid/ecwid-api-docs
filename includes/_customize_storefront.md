# Customize storefront

The Ecwid API platform allows you to customize storefront in various ways: 

- apply custom styles to the store elements (buttons, fonts, pictures, etc.)
- add extra widgets, e.g. customer reviews, comments, image magnifier
- modify the look and behavior of Ecwid widgets
- add tracking pixels or any other third-party conversion-tracking scripts to the checkout page
- and more

**Most popular modifications include**:

- [Apply custom JavaScript file](https://developers.ecwid.com/api-documentation/customize-behaviour#add-custom-javascript-code)
- [Change store layout](https://developers.ecwid.com/api-documentation/customize-appearance#change-store-layout)
- [Add new element to products in category pages](https://developers.ecwid.com/api-documentation/customize-appearance#add-new-element-for-each-product-in-product-grid)
- [Apply SEO-friendly URLs](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls)
- [Change default colors and fonts](https://developers.ecwid.com/api-documentation/customize-appearance#change-default-colors-and-fonts)
- [Generate link to cart with products](https://developers.ecwid.com/api-documentation/customize-behaviour#generate-cart-with-products)
- [Access storefront JavaScript API](https://developers.ecwid.com/api-documentation/storefront-js-api)

## Customize appearance

### Change store layout

Ecwid integration code provides a number of modifications to the store layout. See the details below. 

#### Add new element for each product in product grid

The latest version of product grid requires specific approach to adding new elements. Check out the example: [https://gist.github.com/riq363/13730a82dbee20bb464daf0713ca1997](https://gist.github.com/riq363/13730a82dbee20bb464daf0713ca1997)

[How to enable latest version of product listing in your test store](https://developers.ecwid.com/api-documentation/customize-appearance#product-listing)

#### Set default product or category page

> Open "Surfboards" category of Ecwid Demo store by default

```html
<div> 
  <script type='text/javascript' src='https://app.ecwid.com/script.js?1003'></script>
  <script type='text/javascript'> xProductBrowser("defaultCategoryId=20671017"); </script> 
</div>
```

By default, the storefront widget opens with a list of root category. But you can configure it to show a different category or a particular product when user opens it for the first time.

You can do it by adding either option to the Product Browser integration code:

`"defaultCategoryId=%categoryId%"`

or

`"defaultProductId=%productId%"`

The example codes are available on the right.

#### Show or hide storefront elements

> Apply new storefront configuration template

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.CONFIG_NAME = VALUE; 
</script>
```

> Hide sign in link example

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.show_signin_link = false; // hides the sign in link
</script>
```

Using additional configuration code you are able to hide some storefront elements without any CSS rules. These settings will override user preferences set in the Ecwid Control Panel. 

Place the codes anywhere on a page where Ecwid integration code is added to apply them.

Config name | Type | Description
------------|------|------------
show_signin_link | boolean | Hide the sign in link for customers in a store. Possible values: `true` to show the sign in link, and `false` to hide it
show_breadcrumbs | boolean | Hide the storefront breadcrumbs for customers in a store. Possible values: `true` to show the breadcrumbs, and `false` to hide them
product_list_show_sort_viewas_options | boolean | Hide the options to 'View as' and 'Sort by' products for customers in a store. Possible values: `true` to show the 'View as' and 'Sort by' options, and `false` to hide them
product_details_show_qty | boolean | Hide the 'Qty' section for customers in product details pages. Possible values: `true` to show the 'Qty' section, and `false` to hide it
product_details_show_in_stock_label | boolean | Hide the 'In stock' label for customers in product details pages. Possible values: `true` to show the 'In stock' label, and `false` to hide it
product_details_show_number_of_items_in_stock | boolean | Hide the number if items in stock for customers in product details pages. Possible values: `true` to show the number of items in stock, and `false` to hide it

#### Center popups in iframe storefronts

> Example of centering popups

```html
<script src='https://d1e443hvef5jf2.cloudfront.net/static/iframeintegration.js'></script>
<script type='text/javascript'>

window.addEventListener('load',  function(e) {
  setupEcwidPopupCentering('#myframe');
});

</script>
```

Ecwid can be embedded to a website in many ways. Sometimes a storefront can be inserted in an iframe container due to the limitations of a platform. 

To make sure that all popup windows such as customer account login popup are displayed in the center of an iframe, use the example code on the right **in a main frame of your page**.

`setupEcwidPopupCentering()` function accepts one argument, which is the ID of an iframe element, where Ecwid storefront is loaded. In order to work, `setupEcwidPopupCentering()` function needs to have `iframeintegration.js` file loaded for that frame. 

P.S. this solution also works for the popups in the Ecwid Control Panel embedded in an iframe container.

### Product listing

> Enable latest product listing

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.enable_new_product_list = true;
</script>
```

The recent version of product listing allows for more customization and better looking storefronts across all Ecwid stores. You can enable it in your Ecwid store using the example code on the right. 

#### Customizing product listing

Check out all the customization options for the product listing available out of the box below.

If you apply any of the changes below after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

**Table of contents:** 

- [Adding new elements to product grid](https://developers.ecwid.com/api-documentation/customize-appearance#adding-new-elements-to-product-grid)
- [Control display of elements in product grid](https://developers.ecwid.com/api-documentation/customize-appearance#control-display-of-elements-in-product-grid)
- [Customize images in product listing](https://developers.ecwid.com/api-documentation/customize-appearance#customize-images-in-product-listing)
- [Set custom spacing between grid elements](https://developers.ecwid.com/api-documentation/customize-appearance#set-custom-spacing-between-grid-elements)

##### Adding new elements to product grid 

Ecwid's latest version of product listing (category pages) works in a different way, which requires different approach to adding new elements to a page.

Check this page for code example: [Add new element to products in category pages](https://developers.ecwid.com/api-documentation/customize-appearance#add-new-element-for-each-product-in-product-grid)

##### Control display of elements in product grid

> Apply new storefront configuration template

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.CONFIG_NAME = VALUE; 
</script>
```

> Show product SKU on hover example

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.product_list_sku_behavior = "SHOW_ON_HOVER"; // shows product SKU on hover in product list
</script>
```

Config name | Type | Description
------------|------|------------
product_list_title_behavior | string | Control display mode of **product title** in product listing (category pages). Possible values: `"SHOW"`, `"HIDE"`, `"SHOW_ON_HOVER"`
product_list_sku_behavior | string | Control display mode of **product SKU**  in product listing (category pages). Possible values: `"SHOW"`, `"HIDE"`, `"SHOW_ON_HOVER"`
product_list_price_behavior | string | Control display mode of **product price** in product listing (category pages). Possible values: `"SHOW"`, `"HIDE"`, `"SHOW_ON_HOVER"`
product_list_price_behavior | string | Control the way **product price** is displayed in product listing (category pages). Possible values: `"SHOW"`, `"HIDE"`, `"SHOW_ON_HOVER"`
product_list_buybutton_behavior | string | Control the way **'Buy now' buttons** are displayed in product listing (category pages). Possible values: `"SHOW"`, `"HIDE"`, `"SHOW_ON_HOVER"`. **Important**: the 'Buy now' button must first be enabled in the Ecwid Control Panel > Settings > General > Cart > Show "Buy now" buttons on products list pages
product_list_show_frame | boolean | Show or hide the **frame / border for products** in product listing (category pages). Possible values: `true`, `false`
product_list_product_info_layout | string | Set **align of sku/price/name** section in product listing (category pages). Works for name/sku/price shown below the image. **Doesn't apply to name/sku/price shown on hover.**. Possible values: `"CENTER"`, `"LEFT"`, `"JUSTIFY"`, `"RIGHT"`
breadcrumbs_separator | string | Set **custom breadcrumbs separator** in product listing (category pages). For example: `Store / SubCategory / SubSubCategory` or `Store -> SubCategory -> SubSubCategory` or any other custom string. Possible values: any string, e.g. `"/"`, `"*"`, `"->"`, etc
product_list_category_title_behavior | string | Control the way **category names** are displayed in product listing (category pages). Possible values: `"SHOW_ON_IMAGE"` – default, `"SHOW_BELOW_IMAGE"`, `"HIDE"`, `"SHOW_ON_HOVER"`

If you apply the change after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

##### Customize images in product listing

> Apply new storefront configuration template

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.CONFIG_NAME = VALUE; 
</script>
```

> Show shadow for products in product list

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.product_list_image_has_shadow = true; // shows shadow for products in product list
</script>
```

Config name | Type | Description
------------|------|------------
product_list_image_size | string | Control the **size of product image** in product listing (category pages). Possible values: `"SMALL"`, `"MEDIUM"`, `"LARGE"`
product_list_image_aspect_ratio | string | Control the **aspect ratio of product image** in product listing (category pages). Possible values: `"PORTRAIT_0667"`, `"PORTRAIT_075"`, `"SQUARE_1"`, `"LANDSCAPE_1333"`, `"LANDSCAPE_15"`
product_list_image_has_shadow | boolean | Show or hide the **shadow effect for product images** in product listing (category pages). If `true`, it also makes images slightly darker to make them stand out against the white background. Possible values: `true`, `false`
product_list_show_additional_image_on_hover | boolean | Show or hide the **additional image on hover** in product listing (category pages). Possible values: `true`, `false`
product_list_image_position | string | Set **product image display mode** in product listing (category pages). <ul><li>**COVER**: product image is set as block background proportionally to remove any empty spaces.</li><li>**FIT**: product image is set to fit into block. Any empty space is filled with an image background color.</li><li>**AUTO**: Ecwid fits the image based on its parameters.</li></ul>Possible values: `"AUTO"`, `"COVER"`, `"FIT"`. Default: `"AUTO"`
product_list_category_image_position | string | Set **category image display mode** in product listing (category pages).<ul><li>**COVER**: category image is set as block background proportionally to remove any empty spaces.</li><li>**FIT**: category image is set to fit into block. Any empty space is filled with an image background color.</li><li>**AUTO**: Ecwid fits the image based on its parameters.</li></ul>Possible values: `"AUTO"`, `"COVER"`, `"FIT"`. Default: depends on the setting in Ecwid Control Panel – category name position. `"COVER"` if category name is shown on image, `"AUTO"` for other values
product_list_show_on_sale_label | boolean | Show or hide the **"ON SALE" label** in product listing (category pages). Label will be displayed only when product has "compare to" price set. Possible values: `true`, `false`. Default: `true`. If both "SOLD OUT" and "ON SALE" labels are enabled, the "SOLD OUT" label will be shown only.
product_list_show_sold_out_label | boolean | Show or hide the **"SOLD OUT" label** in product listing (category pages). Label will be displayed only when product is out of stock. Possible values: `true`, `false`. Default: `true`. If both "SOLD OUT" and "ON SALE" labels are enabled, the "SOLD OUT" label will be shown only

If you apply the change after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

##### Set custom spacing between grid elements

> Apply new storefront configuration template

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.product_list_category_cell_spacing = 20; 
</script>
```

Config name | Type | Description
------------|------|------------
product_list_category_cell_spacing | integer | Set your custom spacing between categories in product listing (category pages) in pixels. If applied, categories will become smaller to allow for the set spacing. Default: not set.
product_list_cell_spacing | integer | Set your custom spacing between products in product listing (category pages) in pixels. If applied, products will become smaller to allow for the set spacing. Default: not set

If you apply the change after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

### Product pages

> Enable latest design of product pages

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.enable_new_product_details = true;
</script>
```

The most recent version of product pages allows for more customization and better looking storefronts across all Ecwid stores. You can enable it in your Ecwid store using the example code on the right.

#### Customizing product pages

Check out all the customization options for the product details pages available out of the box below.

If you apply any of the changes below after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

**Table of contents:** 

- [Setting values for product options](https://developers.ecwid.com/api-documentation/customize-appearance#setting-values-for-product-options)
- [Set layout of product pages](https://developers.ecwid.com/api-documentation/customize-appearance#set-layout-of-product-pages)
- [Product name](https://developers.ecwid.com/api-documentation/customize-appearance#product-name-on-product-pages)
- [Breadcrumbs](https://developers.ecwid.com/api-documentation/customize-appearance#breadcrumbs-on-product-pages)
- [SKU](https://developers.ecwid.com/api-documentation/customize-appearance#sku-on-product-pages)
- [Price](https://developers.ecwid.com/api-documentation/customize-appearance#price-on-product-pages)
- [Sale price](https://developers.ecwid.com/api-documentation/customize-appearance#sale-price-on-product-pages)
- [Product options](https://developers.ecwid.com/api-documentation/customize-appearance#product-options-on-product-pages)
- [Buy button block](https://developers.ecwid.com/api-documentation/customize-appearance#buy-button-block-on-product-pages)
- ['Qty' block](https://developers.ecwid.com/api-documentation/customize-appearance#qty-block-on-product-pages)
- ['In stock' label](https://developers.ecwid.com/api-documentation/customize-appearance#in-stock-label-on-product-pages)
- [Wholesale pricing table](https://developers.ecwid.com/api-documentation/customize-appearance#wholesale-pricing-table-on-product-pages)
- ['Save for later' block](https://developers.ecwid.com/api-documentation/customize-appearance#save-for-later-block-on-product-pages)
- [Share buttons](https://developers.ecwid.com/api-documentation/customize-appearance#share-buttons-on-product-pages)
- [Gallery](https://developers.ecwid.com/api-documentation/customize-appearance#gallery-on-product-pages)

##### Setting values for product options 

> Set value for product options on product pages

```js
// Product details page is opened
// Change value of select in DOM
document.querySelector('.details-product-option--select .form-control--select select').value = 'XLarge';

// At this moment the value was changed, but Ecwid doesn't know about it. So the changes are not applied yet
// Sent 'change' event to Ecwid to record the change
document.querySelector('.details-product-option--select .form-control--select select').dispatchEvent(new Event('change'));

// Now if a customer adds this product to cart, it will be added with this user selection
```

Product details pages can have different product options for customers to choose from. Use the instructions below for changing product options for customer. 

**How to set value for product option**: 

1. Change the value of the input in DOM
2. Send 'change' event to Ecwid that value was changed

**Example**

Check out an example code on the right.



##### Set layout of product pages

> Set layout of product pages

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.product_details_layout = "THREE_COLUMNS_SIDEBAR_ON_THE_RIGHT"; // set product page layout
</script>
```

Set different **layout of product pages**. Defines how many columns the page contains, the column width, etc. 

Possible values: `"TWO_COLUMNS_SIDEBAR_ON_THE_LEFT"`, `"TWO_COLUMNS_SIDEBAR_ON_THE_RIGHT"`, `"THREE_COLUMNS_SIDEBAR_ON_THE_RIGHT"`, `"THREE_COLUMNS_SIDEBAR_ON_THE_LEFT"`.

Defalut value: `"TWO_COLUMNS_SIDEBAR_ON_THE_RIGHT"`

If you apply the change after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

#### Control visibility of elements in product details pages

> Apply new storefront configuration template

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.CONFIG_NAME = VALUE; 
</script>
```

> Hide product name from product details pages

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.product_details_show_product_name = false;
</script>
```

Config name | Type | Description
------------|------|------------
product_details_show_product_name | boolean | Show or hide **product name**. Defines the visibility of the product name on product pages. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_breadcrumbs | boolean | Defines the **visibility of breadcrumbs on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_product_sku | boolean | Defines the **visibility of SKU on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_product_price | boolean | Defines the **visibility of price on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_sale_price | boolean | Defines the **visibility of sale price on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_price_per_unit | boolean | Defines the **visibility of price per unit on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_tax | boolean | Defines the **visibility of tax on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_product_options | boolean | Defines the **visibility of product options on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_options.hide_price_modifiers | boolean | Defines the **visibility of price modifiers on product page**. Possible values: `true`, `false`. Defalut value: `false`
product_details_show_buy_button | boolean | Defines the **visibility of buy button block on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_qty | boolean | Defines the **visibility of 'Qty' on product page**. Possible values: `true`, `false`. Defalut value: `false`
product_details_show_number_of_items_in_stock | boolean | Defines the **visibility of number of items in stock on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_in_stock_label | boolean | Defines the **visibility of 'In stock' label on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_wholesale_prices | boolean | Defines the **visibility of wholesale prices on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_product_description | boolean | Defines the **visibility of product description on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_save_for_later | boolean | Defines the **visibility of 'Save for Later' block on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_show_share_buttons | boolean | Defines the **visibility of share buttons on product page**. Possible values: `true`, `false`. Defalut value: `true`
product_details_additional_images_has_shadow | boolean | Defines the **shadow visibility for gallery images**. If "true", adds shadow to gallery thumbnails. Makes the thumbnails slightly darker to make them stand out against the white background. Possible values: `true`, `false`. Defalut value: `false`
product_details_image_carousel | boolean | Defines the **shadow visibility for gallery images**. If `true`, adds the ability to scroll product images without opening full-screen image viewer. Possible values: `true`, `false`. Defalut value: `true`

If you apply the change after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

#### Control the layout of product details page

> Apply new storefront configuration template

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.CONFIG_NAME = VALUE; 
</script>
```

Config name | Type | Description
------------|------|------------
product_details_show_product_name_always_first_on_mobile | boolean | Defines the **position of the product name** on mobile. If `true`, the product name always shows on top on the mobile version. Possible values: `true`, `false`. Defalut value: `false`
product_details_cut_product_description_in_sidebar | boolean | If `true`, cuts long product description and displays only one paragraph and "Show more" link to see full info. If `false`, the description shows as is no matter its size. Possible values: `true`, `false`. Defalut value: `true`
product_details_two_columns_with_right_sidebar_show_product_description_on_sidebar | boolean | If `true`, the product description shows in sidebar according to its position. Otherwise it shows below product image. For two column layout with sidebar on the right only. See [product page layout](#set-layout-of-product-pages). Possible values: `true`, `false`. Defalut value: `false`
product_details_two_columns_with_left_sidebar_show_product_description_on_sidebar | boolean | If `true`, the product description shows in sidebar according to its position. Otherwise it shows below product image. For two column layout with sidebar on the left only. See [product page layout](#set-layout-of-product-pages). Possible values: `true`, `false`. Defalut value: `false`
product_details_gallery_layout | string | Set different **layout of gallery on product pages**. <ul><li>`"THUMBNAILS_HORIZONTAL"` shows them below image in a classic way with small thumbnails next to each other.</li><li>`"THUMBNAILS_VERTICAL"` shows additional product images on the left from the main image.</li><li>`"FEED"` shows the them as feed with large detaied images.</li></ul>Defalut value: `"IMAGE_SINGLE_THUMBNAILS_HORIZONTAL"`
product_details_additional_images_preview_on_click | boolean | Defines the **gallery behaviour on image click**. If `true`, shows the additional product image in the place of main product image when clicking on gallery thumbnail. If `false`, the image viewer opens when clicking on image thumbnails. Not relevant if `product_details_gallery_layout`==`FEED`. Possible values: `true`, `false`. Defalut value: `true`
product_details_thumbnails_aspect_ratio | string | Set the **image aspect ratio for gallery tumbnails**. If `"AUTO"` each tumbnails has the ratio that closest to its actual proportions. Hovever you can force different ratio to all tumbnails. Possible values: `"AUTO"`, `"PORTRAIT_0667"`, `"PORTRAIT_075"`, `"SQUARE_1"`, `"LANDSCAPE_1333"`, `"LANDSCAPE_15"`. Defalut value: `"AUTO"` 
{#wide-table-cs}

If you apply the change after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

#### Control position of elements on product details pages

> Apply new storefront configuration template

```html
<script>
  window.ec = window.ec || Object();
  window.ec.storefront = window.ec.storefront || Object();
  window.ec.storefront.CONFIG_NAME = VALUE; 
</script>
```

Config name | Type | Description
------------|------|------------
product_details_position_product_name | number | Defines the **position of the product name**. The less number, the higher the element on the page. Defalut value: `100`
product_details_position_breadcrumbs | number | Defines the **position of the breadcrumbs within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `200`
product_details_position_product_sku | number | Defines the **position of the SKU within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `300`
product_details_position_product_price | number | Defines the **position of the SKU within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `400`
product_details_position_product_options | number | Defines the **position of the product options within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `500`
product_details_position_product_options | number | Defines the **position of the buy button block within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `600`
product_details_position_wholesale_prices | number | Defines the **position of the wholesale pricing table within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `700`
product_details_position_product_description | number | Defines the **position of the product description within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `800`
product_details_position_save_for_later | number | Defines the **position of the 'Save for Later' block within the sidebar**. The less the number is, the higher the element on the page**. Defalut value: `900`
product_details_position_share_buttons | number | Defines the **position of the share buttons within the sidebar**. The less the number is, the higher the element on the page. Defalut value: `1000`

If you apply the change after storefront has loaded, you can update its look on the fly using the `Ecwid.refreshConfig()` function.

### Change storefront labels

> Format example

```html
<script type="text/javascript">
  ecwidMessages = { 
    "LABEL_NAME":"LABEL_VALUE", 
    "LABEL_NAME-2":"LABEL_VALUE-2", 
    "OTHER_LABEL_NAME":"OTHER_LABEL_VALUE" 
  }; 
</script>
```

Every text in Ecwid (that is not added by a merchant) is a label. You can change these labels yourself to translate your storefront or to make the wording more fitting a merchant's individual Ecwid setup.

This translation includes adding of a special JavaScript code with the updated labels **before** the Ecwid integration code. 

> Replace "Shopping Bag" with "Shopping Cart"

```html
<script type="text/javascript">
  ecwidMessages = {
    "BreadCrumbs.your_bag" : "Your shopping cart",
    "Minicart.shopping_bag" : "Shopping Cart",
    "ShoppingCartView.shopping_bag" : "Your Shopping Cart",
    "EmptyShoppingCartPanel.empty" : "Your Shopping Cart is empty"
  };
</script>
```

The format of this code is presented on the right. `LABEL_NAME` is the internal name of the text label. `LABEL_VALUE` is the text that is shown in the storefront. 

For example: 
to replace the "Shopping Bag" text with "Shopping Cart" in the bag widget and "your bag" page, we need to add the following code before this line: 
`<script type='text/javascript' src='http://app.ecwid.com/script.js?store_id'></script>` 

on your web page. See the example one the right!

**Important notes**: 

- each line must be ended with comma except the last line
- do not leave empty lines in the Javascript code
- the special chars should be escaped. For example if the value of a label has double quote(") it should be escaped with the slash(\")
- some buttons are actually images, so you cannot translate them using JavaScript. Use CSS code to replace the images
- keep HTML tags in the label values, they're necessary for correct Ecwid work
- mind the spaces after the label values. Some labels have it.  So make sure that they won't be loaded during the translation.
- UTF-8 charset is strongly recommended
- if you don't like to insert many JavaScript lines to your store page, you can move them to the external JavaScript file: www.ecwid.com/forums/showthread.php?p=2337#15

If you're not familiar with JavaScript, but want to translate a store or change its labels, you can use [Storefront Label Editor](https://www.ecwid.com/apps/tools/storefront-label-editor) or the [Ecwid Translate Tool](http://www.ecwid.com/playground/translate-tool/). It will make the translation much easier.

### Change default colors and fonts

Change any color and main font for user's storefront

#### Apply colors and fonts to storefront automatically from a website

> Set storefront colors and font automatically

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.font = 'auto';
window.ec.config.chameleon.colors = 'auto';
</script>
```

Ecwid can automatically detect the main colors and fonts of a surrounding website and match the storefront automatically. To apply the colors and font automatically for a store, use the code on the right.

#### Set storefront colors

> Set storefront colors automatically

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.colors = 'auto';
</script>
```

> Set storefront colors manually

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.colors = {
  'color-background':'#D3D3D3',
  'color-foreground':'#4EA3F0',
  'color-link':'#FF0606',
  'color-button':'#4EA3F0',
  'color-price':'#FF0606'
}
</script>
```

> Set storefront colors and font example

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon = {
  colors: {
    'color-button':'#4EA3F0',
    'color-price':'#FF0606'
  },
  font: {
    'font-family': 'arial,sans-serif'
  }
}
</script>
```

Using the global Ecwid config object `window.ec.config` you can control the colors in user's storefront. To pre-define colors for a storefront, add JavaScript code on the right in the same frame, where Ecwid integration code is added.

`auto` value allows Ecwid to detect the website colors around Ecwid storefront to adapt its colors automatically. Use the fields below to set colors manually.

**Fields:**

Name | Type | Description
---- | ----- | -----------
color-background | string (HEX and RGBA color) | Background color for storefront and  small buttons ("Clear bag", "Apply", etc.)
color-foreground | string (HEX and RGBA color) | Color of all texts in storefront
color-link | string (HEX and RGBA color) | Color for links in storefront (breadcrumbs, "Sign In", "Favourites", etc.)
color-button | string (HEX and RGBA color) | Color for main buttons in storefront ("Add to bag", "Checkout", etc.) and small buttons on mouse hover ("Clear bag", "Apply", etc.)
color-price | string (HEX and RGBA color) | Color for product prices in storefront

#### Set storefront font

> Set storefront font automatically

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.font = 'auto';
</script>
```

> Set storefront font manually

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon.font = {
  'font-family': 'arial,sans-serif'
}
</script>
```

> Set storefront colors and font example

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.chameleon = window.ec.config.chameleon || Object();
window.ec.config.chameleon = {
  colors: 'auto',
  font: {
      'font-family': 'arial,sans-serif'
    }
  }
</script>
```

Using the global Ecwid config object `window.ec.config` you can control the fonts of all texts displayed in user's storefront. To pre-define a font for a storefront, add JavaScript code on the right in the same fram, where Ecwid integration code is added.

`auto` value allows Ecwid to detect the website's main font to adapt its font automatically. You can also set a specific font-family for a storefront manually.

### Custom themes

> Example of custom CSS to modify storefront

```css
/*
 * Change Ecwid buttons style 
 */
button.gwt-Button, #wrapper button.gwt-Button { 
  border-radius: 13px; 
  -moz-border-radius: 13px; 
  -webkit-border-radius: 13px; 
  height: 28px; 
  background-color: #e4e4e4; 
  padding: 0 15px; 
}  
 
button.gwt-Button:active,
#wrapper button.gwt-Button:active { 
  background-color: #999999; 
}  
 
div.ecwid-productBrowser-details-rightPanel div.ecwid-productBrowser-backgroundedPanel {
  min-width:185px; 
  width:auto; 
}
```

**Add custom CSS theme in Ecwid Control Panel (for a single store)**

Ecwid provides a merchant with a built-in CSS customization tool in their control panel in the 'Design' section: Ecwid automatically loads the CSS code entered by user in their storefront. This allows merchants to customize their store look and feel flexibly. See also ["How to change Ecwid design"](http://help.ecwid.com/customer/portal/articles/1083332-how-to-change-ecwid-design) article in our knowledge base.

**Add custom CSS theme in an app (for multiple stores at once)**

Ecwid API allows you to do the same in more convenient way: you simply specify the URL of file with your custom CSS code and Ecwid automatically loads that code in the user storefront. So you don't need to put the CSS on user site manually or ask a merchant to do that. 

In more details: 

1. After your app is registered, [contact us](/contact) and provide the https URL of the `.css` and file you’d like to load in the user storefront.
2. When asking a user to install your app, Ecwid will request the `customize_storefront` permission from them. (*If your app is for a specific store, make sure to add `customize_storefront` scope in the OAuth process.*)
3. The next time merchant's storefront is loaded in any browser or website, your external CSS file will be automatically appended, loaded, and executed on that page.

<aside class="notice">
Permission required: <strong>customize_storefront</strong> (see <a href="https://developers.ecwid.com/api-documentation/external-applications#access-scopes">Access scopes</a>)
</aside>

#### Q: Can I create new themes as apps?

Yes, you can! Please check out this page for more details: [How to create a theme](/how-to-create-a-theme-for-an-ecwid-store)

#### Store-specific custom CSS

You may want to apply different CSS codes depending on the store your application is loaded. 

For example, if your application provides new design themes for merchant storefront, you may need to give a merchant ability to choose the theme they want to enable and change the applied CSS code according to their choice. 

In such cases, you will need to use custom JS files to dynamically detect merchant store ID and load different styles depending on the user store ID. See [Custom JavaScript files](https://developers.ecwid.com/api-documentation/customize-behaviour#add-custom-javascript-code) for details.

### Storefront interface translations

When the app adds new features to storefront that need translations, most likely the app loads its external JavaScript code too.

The app can determine the current language of storefront using the JS API method: [Ecwid.getStorefrontLang](https://developers.ecwid.com/api-documentation/get-storefront-details#ecwid-getstorefrontlang)

To translate the app in storefront, use these steps: 

1/ [Determine storefront language](https://developers.ecwid.com/api-documentation/get-storefront-details#ecwid-getstorefrontlang) before initializing the app
2. Set/load the required translated text labels for app interface

If the current language is not supported, use the fallback labels. For example, load English texts if user is Spanish and you don't have Spanish translations yet. 

## Customize behaviour

### Add Ecwid to the site

> Add Ecwid storefront to your website

```html
<div id="my-store-1003"></div><div> <script type="text/javascript" src="https://app.ecwid.com/script.js?1003" charset="utf-8"></script><script type="text/javascript"> xProductBrowser("id=my-store-1003");</script></div>
```

This is the most important Ecwid widget. It shows and includes a full-featured shopping cart with products, categories, catalog, checkout pages, etc.

To add an Ecwid store to the website, use the code example on the right. The `1003` is the ID of an Ecwid store. Make sure to specify your desired store ID in order to show your store!

### Set main storefront URL for widgets

When you add store widgets, like search, minicart, categories menu, to a site page separate from the storefront widget, the storefront will open in a pop-up. This pop-up contains the whole store and works just fine. But in some cases it is more convenient to open the store on another website page instead of the pop-up.
 
You can do this with the help of the `window.ec.config.store_main_page_url` option.

> 'Tell' Ecwid widgets that storefront is located at "http://www.example.com/store.html"

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.store_main_page_url = "http://www.example.com/store.html";
</script>
```

**How Ecwid works by default**

- if a visitor uses the "minicart/vertical categories/search box" widget and a storefront is added on this page, all actions (go to cart, to category, etc.) will be made in that storefront widget
- but if a storefront widget doesn't exist on the same page page, **a pop-up window with the storefront will be created on this page** to complete the action

**How Ecwid works with this option**

When a merchant uses a storefront widget placed on another page of a website, you should use the `window.ec.config.store_main_page_url` option. If it's set up, Ecwid works in such way: 

- if a visitor uses the "minicart/vertical categories/search box" widget and the storefront widget isn't added on this page, **they will be redirected to the URL speicified in the code and then the necessary actions will be performed there**

For example, if a customer searches for a product on the page without a storefront, they will be redirected to a URL specified in `window.ec.config.store_main_page_url` variable and then Ecwid will show the search results on that page.

**How to set up this option**

In order to use it, you need to add the JavaScript code on the right to all pages, where Ecwid widgets are installed.

### Disable force scrolling to storefront

> Disable force scrolling to storefront

```js
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.navigation_scrolling = "DISABLED";
```

> Use custom scrolling to any position on the page

```js
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.navigation_scrolling = "CUSTOM";

window.ec.config.custom_scroller = function() {
  window.scrollTo(500, 0);
};
```

When Ecwid storefront widget is added to a website, the page will be scrolled to the beginning of storefront by default. 

If you want to disable this behaviour, use the `window.ec.config.navigation_scrolling` config variable. Possible values: 

- `"DISABLED"`
- `"IMMEDIATELY"`
- `"CUSTOM"`

Default value: `"IMMEDIATELY"`

If you need to scroll to a custom position when navigating the Ecwid store, use the `"CUSTOM"` value and set your custom scrolling function in the `window.ec.config.custom_scroller` configuration. 


### Adapt storefront layout to container width

By default, Ecwid is a responsive widget which adapts to the changing screen width. That's usually when the container for storefront changes its width also.

However, if the screen width stays the same but the container for storefront changes its width, you will need to call the `Ecwid.resizeProductBrowser()` function to adapt Ecwid's layout to this change.

### Open specific product variation

When opening a product that has options or variations, it can be useful to open them with preset values right away. This will save customer's time and improve the buying process. 

**How to open a product variation**

1) Determine the product you want to open: get a direct URL to product details page you would need to use.

For example: `https://www.ecwid.com/demo/Rip-Curl-Channel-Island-p72585497`

2) Add a query parameter `variation` to the URL with a value of desired variation ID. You can [retrieve the variation ID](https://developers.ecwid.com/api-documentation/product-variations) via the Ecwid REST API

Resulting URL: `https://www.ecwid.com/demo/Rip-Curl-Channel-Island-p72585497?variation=16351010`

It will open a product "Rip Curl Channel Island" with options: Size - Any, Color - Dark grey.

**How to open a custom product variation**

If your product doesn't have product variations but has product options, you can still open details page with preset values (selections). Use `options` query parameter instead. 

1) Determine the product you want to open: get a direct URL to product details page you would need to use.

For example: `https://www.ecwid.com/demo/Rip-Curl-Channel-Island-p72585497`

2) Add a query parameter `options` to the URL with values of product options separated by comma: `?options=A,B,C`

Each value will be assigned to product options depending on its index in the list of available selections. The index starts at `1`. **This method supports drop down and radio button product options only**

Any incorrect and out of range values will be ignored.

Example: `?options=4,5,6`

These values mean that: 

- for product option 1 the value will be: 4th selection
- for product option 2 the value will be: 5th selection
- for product option 3 the value will be: 6th selection

And so on. 

Resulting URL: `https://www.ecwid.com/demo/Rip-Curl-Channel-Island-p72585497?options=2,2`

It will open a product "Rip Curl Channel Island" with options: Size - Medium, Color - Dark grey.

### Embed or remove storefront on demand

> Dynamic embedding of Ecwid storefront widget

```html
<div id="my-store-1003"></div>
<script>
window.ecwid_script_defer = true;
window.ecwid_dynamic_widgets = true;

   if (typeof Ecwid != 'undefined') Ecwid.destroy(); 
   window._xnext_initialization_scripts = [{
        widgetType: 'ProductBrowser',
        id: 'my-store-1003',
        arg: ["id=productBrowser"]
      }];

  if (!document.getElementById('ecwid-script')) {
      var script = document.createElement('script');
      script.charset = 'utf-8';
      script.type = 'text/javascript';
      script.src = 'https://app.ecwid.com/script.js?1003';
      script.id = 'ecwid-script'
      document.body.appendChild(script);
    } else {
      ecwid_onBodyDone();
    }
</script>
```

In some cases it is necessary to dynamically create and destroy Ecwid widget within the HTML page. This is useful for dynamic sitebuilders that switch to online store page without actually reloading page (e.g. making it visible). 

<aside class="note">
This method of embedding Ecwid is slower than direct embedding of Ecwid widget. Please use it only when you need the storefront embedded on the fly. 
</aside>

You should use `window.ecwid_dynamic_widgets` variable to enable dynamic widget creating in Ecwid. See the example on the right that shows how to create and destroy Ecwid widget through the JavaScript functions. 

Please note that **this method allows to embed the storefront widget only**. If you need to embed other widgets dynamically, please, use the code for [deferred widget initialization](https://developers.ecwid.com/api-documentation/customize-behaviour#delayed-widget-initialization)

### Delayed widget initialization 

> Delayed widget initialization

```html
<div id="my-store-1003"></div>
<div id="productBrowser"></div>
<script>
  window.ecwid_script_defer = true;
  
  var script = document.createElement('script');
  script.charset = 'utf-8';
  script.type = 'text/javascript';
  script.src = 'https://app.ecwid.com/script.js?1003';

  document.getElementById('my-store-1003').appendChild(script);

  window._xnext_initialization_scripts = [
      { widgetType: 'ProductBrowser', id: 'productBrowser', arg: [
          '"id=productBrowser"'
      ] }
  ];
</script>
```

Sometimes it is necessary to delay widget initialization while the website page finishes the initialization procedures. This is useful when the website is built dynamically using libraries such as **ReactJs**.

See the example of delayed initialization of Ecwid widget on the right.

### Increase widget loading speed

> DNS Prefetching of Ecwid's resources

```html
<link rel="dns-prefetch" href="//images-cdn.ecwid.com/">
<link rel="dns-prefetch" href="//images.ecwid.com/">
<link rel="dns-prefetch" href="//app.ecwid.com/">
```

To increase the loading speed of an Ecwid store, you can start loading Ecwid resources even before a customer opens the store page by the means of so-called prefetching (prerendering) browser feature. Not all browsers support this at this moment though.

> Prefetching and prerendering of storefront page

```html
<link rel="prefetch" href="http://www.example.com/store/"/>
<link rel="prerender" href="http://www.example.com/store/"/>
```

You can also add your storefront page to preload and prerender in the background for further increase in loading speed for browsers that have this feature.

Feel free to learn more about these techniques in [https://css-tricks.com/prefetching-preloading-prebrowsing/](https://css-tricks.com/prefetching-preloading-prebrowsing/)

### Add custom JavaScript code

> Example of custom JavaScript to modify storefront

```js
/*
 * Show a promo 'Free shipping' message on the cart page. 
 * In the example below, users will see a message on the cart page 
 * informing them that they will qualify for free shipping if their 
 * order is more than $99
 */

var promoMessage = "Orders $99 and up ship free!";
var minSubtotal = 99;
var widgets;

// Calculating subtotal and displaying the message
var checkSubtotal = function(order) {
  if (order) {
    var subtotal = order.total - order.shipping;
    if (subtotal > minSubtotal) {
      alert(promoMessage);
    }  
  }
}

// Detecting whether we're on the cart page and get the cart info
Ecwid.OnPageLoaded.add(function(page) {
  widgets = Ecwid.getInitializedWidgets();

  // if storefront widget is present on this page
  for (i = 0; i < widgets.length ; i++) {
    if (widgets[i] == "ProductBrowser") {
      if ('CART' == page.type) {
        Ecwid.Cart.calculateTotal(function(order) {
          checkSubtotal(order);
        });
      }
    }
  }
});

// Get color value for the message and store it in color variable
var color = Ecwid.getAppPublicConfig(appId);

// your code here
// ...
```

As soon as your script is loaded on the page with Ecwid storefront, you can access the page DOM and do pretty much everything you want by means of native JavaScript or whatever framework you want. If you use a JS framework, please make sure it's already loaded on the page by the moment you start using it. 

In addition, Ecwid provides a [JavaScript API](https://developers.ecwid.com/api-documentation/storefront-js-api) that you can use to retrieve store information and track Ecwid events on the page. For example:

* `Ecwid.getOwnerId()` returns the store ID. You may want to use it to identify which store your script is loaded in now
* `Ecwid.OnPageLoad()` and `Ecwid.OnPageLoaded()` help you to track store page switch and identify which page is opened
* `Ecwid.Cart` object and its methods allow to manage the customer cart
* `EcwidApp.getAppPublicConfig` will return store-specific data from application storage

More details: [Ecwid JavaScript API](https://developers.ecwid.com/api-documentation/storefront-js-api)

#### How to load custom JavaScript anytime storefront is loaded

Ecwid API allows you to do the same in more convenient way: you simply specify the URL of file with your custom JavaScript code and Ecwid automatically loads that code in the user storefront. So you don't need to put the JS on merchant site manually or ask them to do that. 

In more details: 

1. After your app is registered, [contact us](/contact) and provide the https URL of the `.css` and/or `.js` file you’d like to load in the user storefront.
2. When asking a user to install your app, Ecwid will request the `customize_storefront` permission from them. (*If your app is for a specific store, make sure to add `customize_storefront` scope in the OAuth process.*)
3. The next time the user storefront is loaded in any browser or website, the specified external JS/CSS files will be automatically appended, loaded, and executed on that page.

<aside class="notice">
Permission required: <strong>customize_storefront</strong> (see <a href="https://developers.ecwid.com/api-documentation/external-applications#access-scopes">Access scopes</a>)
</aside>

#### Q: How to know what Ecwid widgets are on a page?

If your applcation customizes storefront, your script will be loaded at **all times** when `app.ecwid.com/script.js?{store_id}` is on a page. It means that if a simple search widget is present on a page, your script will be executed. 

Ecwid widgets can be also embedded in many ways: 

- as a part of a website page
- in an iframe
- certain widgets are displayed only (like search and minicart, no storefront) 

So, it is important to know where exactly your application is loaded: if there is a storefront present on a page or if it's just a search widget. To check that, use `Ecwid.getInitializedWidgets()` function of [Ecwid Javascript API](#storefront-js-api). Using it, you can determine whether your app functionality needs to be initialized or not.

#### Q: What is the best way to add new scripts from my code? 

> Add external script to the page example

```js
var s = document.createElement("script");
s.type = "text/javascript";
s.src = "https://example.com/example.js";
document.head.appendChild(s);
```

In case if you need more than one JavaScript code executed on a page, you can load additional scripts from within your base app JavaScript code.

We recommend that you load external JS files using a standard function `createElement()`. See example code on the right. 
It allows you to create a new `<script>` element on a page with the parameters you need and then append it to the DOM tree to get loaded and executed.

More details about this approach: [W3CSchools](http://www.w3schools.com/jsref/met_document_createelement.asp)

<aside class="note">Please make sure to avoid using the <em>document.write()</em> function as it works synchronously and will slow down the page load.</aside>

#### Access merchant in-app preferences

Applications can work and look differently in various stores, so you can provide tailored user experience to the store owner and a website they have in your application.

App public config allows apps to access user specific data in storefront as soon as it starts to load, so no calls to external servers are required and result is available right away. It’s recommended that you use app storage to access public user data in storefront area of your application.

**How it works:**

1. App saves public data in public storage in backend
2. App gets public data from public storage in storefront
3. App tailors user experience to merchant settings

For example, using this method you can pass store ID to a storefront, category or product ID where the changes need to apply or whether your widget needs to be shown in storefront. 

To find out more about this process, please check [Public App Config](https://developers.ecwid.com/api-documentation/public-application-config) section.

#### Store-specific custom JavaScript

> Example of the script that dynamically loads store-specific code

```js
// Load external script with store
function loadConfig(ecwidStoreId, callback) {
  var script = document.createElement("script");
  script.setAttribute("src", '//example.com/myapp/store' + ecwidStoreId + '.js');
  script.charset = "utf-8";
  script.setAttribute("type", "text/javascript");
  script.onreadystatechange = script.onload = callback;
  document.body.appendChild(script);
}

// Application functionality
function doCoolStuff() {
  //...
}

// Get color value for the message and store it in color variable
var color = Ecwid.getAppPublicConfig(appId);

// Get Ecwid store ID and start working
loadConfig(Ecwid.getOwnerId(), doCoolStuff);
```

In most cases, your application behavior will vary depending on the store it is opened in. Although Ecwid API allows you to specify only one JS file per application, your application can detect the current Ecwid store ID and act correspondingly. Use the `Ecwid.getOwnerId()` method to detect the user store ID in your script. 

For example, let's say you need to dynamically add a store-specific configuration to your script when it's executed in some particular storefront. You can detect the store ID in your script and call a script on your server containing the store-specific code. 

To get information for that specific store you can use `Ecwid.getAppPublicConfig` function in [Ecwid Javascript API](https://developers.ecwid.com/api-documentation/storefront-js-api). See more details on how to access that data in **Application data** section of documentation.

#### Caching JavaScript file contents

When your external JS file is loaded on a page, it is loaded like any other resource via a GET request by a web browser. If you'd like to use caching to increase the loading speeds of your script, we can suggest several options:

- invalidate cache for your JS file link when deploying a new version
- in your JS file, place a code that will get the latest JS assets file from your server dynamically and append it to a page (instead of the using an actual code of your app on that URL)
- don't use caching, so that your files for storefront are always up-to-date for any user

#### Using jQuery on store pages

> Checking for jQuery on a page example

```js
// function to load jQuery

function loadjQuery(){
  var jq = document.createElement('script'); jq.type = 'text/javascript';
  jq.src = 'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js';
  document.getElementsByTagName('head')[0].appendChild(jq);
}

if (typeof jQuery == 'undefined') {  
  loadjQuery();
} else {}

// your app code
// ...
```

jQuery is a very popular JavaScript library that is used in many websites. The developers of themes for Wordpress CMS, for example, include this library right into the theme to have access to its functions and to operate various HTML elements on the page easily.

Ecwid storefronts can be customized in many ways, for example: you can set specific behaviour for each [page or each user](https://help.ecwid.com/customer/portal/articles/1085558) that visits those pages. Some developers prefer using native JavaScript functions to change the position or the display of certain elements. But many use different versions of popular jQuery library.

To ensure that your jQuery version code doesn't conflict with other applications that change Ecwid storefront, please see our guidelines: 

##### For simple modifications

When your modifications don't require specific versions of jQuery loaded on a page and are fairly simple, we suggest you follow this algorithm: 

First, check for jQuery object on current page. If no object is defined, load any jQuery version you prefer using the example code on the right. If there is already jQuery object available - skip the loading and use the curreny jQuery on this page.

##### For complex modifications

When you are using advanced jQuery's features such as jQuery UI, version specific functions, etc. it will be hard to work with, if a website already has varuous jQuery versions on a page. To avoid any version conflicts with current page jQuery version, we suggest you use jQuery noconflict feature. Check out how it works: [https://api.jquery.com/jquery.noconflict/](https://api.jquery.com/jquery.noconflict/)

<aside class='note'>
We recommend loading your jQuery from <a href='https://developers.google.com/speed/libraries/#jquery'>Google's CDN</a> to ensure it is available at all times.
</aside>

### Custom URL redirect after an online purchase 

> Setting custom URL redirect after online purchase

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.custom_redirect_after_purchase = 'YOUR CUSTOM URL HERE';
</script>
```

```html
<script>
window.ec = window.ec || Object();
window.ec.config = window.ec.config || Object();
window.ec.config.custom_redirect_after_purchase = 'https://mycoolstore.com/thank-you.html';
</script>
```

Some merchants may want to send their customers after a successful online purchase to a custom URL. Using the configuration on the right you can set the URL where the customer should be redirected. 

**Important**: the redirect will work only for [online payment methods](https://support.ecwid.com/hc/en-us/articles/207100429-Payment-options#OnlinepaymentmethodsOrIwanttoacceptcreditcards). For offline payment methods, consider using the [Custom URL for 'Thank you' page](https://github.com/Ecwid/custom-thank-you-page-app) application.

### Generate cart with products

Ecwid JavaScript API allows you to add items to customer's cart automatically. This can be useful when you are using custom storefront to provide any type of button to add products to cart.

Using the following steps you will be able to create links to your storefront with products in cart for your customers. So all they have to do is just clikc that promo link to go to your store and have products added to cart automatically.

Let's check out how this can be achieved: 

**Step 1: Add external files to your website**

> Script and CSS to load on your storefront page

```html
<link rel="stylesheet" type="text/css" href="https://s3.amazonaws.com/ecwid-addons/apps/ecwid-cart-app/cartapp.css">
<script src="https://s3.amazonaws.com/ecwid-addons/apps/ecwid-cart-app/cart.js"></script>
```

Add this code to the source code of the page, where your Ecwid storefront is displayed. Add the script **after** or below Ecwid integration code.

**Step 2: Generate your cart**

> Generate items in cart example

```js
var cartItems = [{
    "id": 66821181, // ID of the product in Ecwid
    "quantity": 3, // Quantity of products to add
    "options": { // Specify product options
        "Color": "White",
        "Size":"11oz"
    }
}, {
    "id": 66722581,
    "quantity": 5
}];

var cart = {
     "gotoCheckout": true, // go to next checkout step right away (after 'Cart' page)
     "products": cartItems, // products to add to cart
     "profile": {
         "address": { // Shipping address details
             "name": "john smith",
             "companyName": "general motors",
             "street": "5th Ave",
             "city": "New York",
             "countryCode": "US",
             "postalCode": "10002",
             "stateOrProvinceCode": "NY",
             "phone": "+1 234 235 22 12"
         },
         "billingAddress": { // Billing address details
             "countryCode": "US",
             "stateOrProvinceCode": "AL",
         },
         "email": "test@test.com", // Customer email
         "orderComments": "Comments!" // Order comments
     }
 }

cart = JSON.stringify(cart);
cart = encodeURIComponent(cart);

console.log(cart);

// prints the resulting string you need to use in step 3
//
// %7B%22gotoCheckout%22%3Atrue%2C%22products%22%3A%5B%7B%22id%22%3A66821181%2C%22quantity%22%3A3%2C%22options%22%3A%7B%22Color%22%3A%22White%22%2C%22Size%22%3A%2211oz%22%7D%7D%2C%7B%22id%22%3A66722581%2C%22quantity%22%3A5%7D%5D%2C%22profile%22%3A%7B%22address%22%3A%7B%22name%22%3A%22john%20smith%22%2C%22companyName%22%3A%22general%20motors%22%2C%22street%22%3A%225th%20Ave%22%2C%22city%22%3A%22New%20York%22%2C%22countryCode%22%3A%22US%22%2C%22postalCode%22%3A%2210002%22%2C%22stateOrProvinceCode%22%3A%22NY%22%2C%22phone%22%3A%22%2B1%20234%20235%2022%2012%22%7D%2C%22billingAddress%22%3A%7B%22countryCode%22%3A%22US%22%2C%22stateOrProvinceCode%22%3A%22AL%22%7D%2C%22email%22%3A%22test%40test.com%22%2C%22orderComments%22%3A%22Comments!%22%7D%7D
//
```

In order for the script to add items to cart, we need to 'tell' it what items to add. Check the example on the right on how to do this. The `cart` variable will have the generated cart content in it. 

The syntax is very similar to adding products to cart via [Ecwid JavaScript API](https://developers.ecwid.com/api-documentation/manage-customer-cart#ecwid-cart-addproduct).

**Step 3: Create a link for customers**

> Final link structure

```
http://example.com/store#!/~/cart/create={generatedCartCode}
```

> Final link to generated cart example

```
https://www.ecwid.com/demo#!/~/cart/create=%7B%22gotoCheckout%22%3Atrue%2C%22products%22%3A%5B%7B%22id%22%3A66821181%2C%22quantity%22%3A3%2C%22options%22%3A%7B%22Color%22%3A%22White%22%2C%22Size%22%3A%2211oz%22%7D%7D%2C%7B%22id%22%3A66722581%2C%22quantity%22%3A5%7D%5D%2C%22profile%22%3A%7B%22address%22%3A%7B%22name%22%3A%22john%20smith%22%2C%22companyName%22%3A%22general%20motors%22%2C%22street%22%3A%225th%20Ave%22%2C%22city%22%3A%22New%20York%22%2C%22countryCode%22%3A%22US%22%2C%22postalCode%22%3A%2210002%22%2C%22stateOrProvinceCode%22%3A%22NY%22%2C%22phone%22%3A%22%2B1%20234%20235%2022%2012%22%7D%2C%22billingAddress%22%3A%7B%22countryCode%22%3A%22US%22%2C%22stateOrProvinceCode%22%3A%22AL%22%7D%2C%22email%22%3A%22test%40test.com%22%2C%22orderComments%22%3A%22Comments!%22%7D%7D
```

Now we need to fill in customer's cart when your custom link is opened. 

First things first, let's determine where your Ecwid store is displayed and get a direct link to that page. For example, our demo store is located in: `https://www.ecwid.com/demo`
And to fill in customer's cart with items that you seleted earlier, we need to create a link to your storefront with the generated cart part. 

**Example link**

Generated link with products added automatically to cart for Ecwid demo store will be: 

`https://www.ecwid.com/demo#!/~/cart/create=%7B%22gotoCheckout%22%3Atrue%2C%22products%22%3A%5B%7B%22id%22%3A66821181%2C%22quantity%22%3A3%2C%22options%22%3A%7B%22Color%22%3A%22White%22%2C%22Size%22%3A%2211oz%22%7D%7D%2C%7B%22id%22%3A66722581%2C%22quantity%22%3A5%7D%5D%2C%22profile%22%3A%7B%22address%22%3A%7B%22name%22%3A%22john%20smith%22%2C%22companyName%22%3A%22general%20motors%22%2C%22street%22%3A%225th%20Ave%22%2C%22city%22%3A%22New%20York%22%2C%22countryCode%22%3A%22US%22%2C%22postalCode%22%3A%2210002%22%2C%22stateOrProvinceCode%22%3A%22NY%22%2C%22phone%22%3A%22%2B1%20234%20235%2022%2012%22%7D%2C%22billingAddress%22%3A%7B%22countryCode%22%3A%22US%22%2C%22stateOrProvinceCode%22%3A%22AL%22%7D%2C%22email%22%3A%22test%40test.com%22%2C%22orderComments%22%3A%22Comments!%22%7D%7D`

<aside class='note'>
Please note that this is an example link and it will not work in Ecwid's demo store. Please test this feature in your own website.
</aside>

### Storefront JavaScript API

Any Ecwid storefront has a JavaScript API available at all times to help users create various customizations for their storefronts. 

Its features include: 

- detect currently opened page details
- set shipping and billing address for customer
- get customer's cart details
- manage customer's cart
and others.

To find our more about it, plesae see the [Storefront JavsScript API Documentation](#storefront-js-api).


## SEO 

Check out the tools to improve the store's SEO. 

**Table of contents:**

- [SEO-friendly URLs](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls)
- [Canonical URLs](https://developers.ecwid.com/api-documentation/seo#canonical-urls)

### SEO-friendly URLs

By default, Ecwid URLs address store pages in a hash part of the URL (after the # sign). Example: `https://www.mysite.com/store/#!/My-Product/p/123/category=0`

Search engines nowadays index such pages well. However, the general SEO recommendation is to keep URLs as simple as possible and address the site pages in the path part of the url, i.e. in the part before '?' or '#' signs. 

Ecwid allows to change the store URLs accordingly, so that they look like this:
`https://www.mysite.com/store/My-Product-p123`

Follow the steps below to enable SEO-friendly URLs in an Ecwid store.

<aside class='note'>
  If you have your Ecwid store installed on a platform or sitebuilder including Wordpress, Wix, Adobe Muse or Ecwid Starter Site, you should have the clean URLs enabled automatically (see the <a target="_blank" href='https://www.ecwid.com/blog/new-clean-seo-urls-in-ecwid.html'>blog post</a> for the details). 
</aside>

#### How to enable SEO-friendly URLs

##### Step 1. Configure your server: add URL rewrite rules

In the SEO friendly URL scheme, Ecwid places a page title right after the store page path in the URL, so that it looks like a subdirectory: `www.mysite.com/store/My-Product-p123` . 

On the other hand, this is just a page on your server, not a directory. So if the server will try to find something inside the store page, it will fail and return 404 errors. To make your server properly process these URLs, you will need to configure URL rewrite rules. 

The general rule is to map everything after slash on the store page to the store page itself, i.e.:
`www.mysite.com/store/anything` → `www.mysite.com/store`

Specific configuration depends on your server: 

- **In Apache**, you will need to enable mod_rewrite and specify rewrite urls in `.htaccess` file See [https://httpd.apache.org/docs/2.4/rewrite/remapping.html](https://httpd.apache.org/docs/2.4/rewrite/remapping.html)
- **In Nginx**, you will need to add rewrite instruction to the config file. See [https://www.nginx.com/blog/creating-nginx-rewrite-rules/](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

See [examples codes](https://gist.github.com/makfruit/00e1c298f11bf12392f67c240e35e927)

<aside class='notice'>
<strong>Do not redirect</strong> the "store/anything/" to "/store" . The rewrite rules should only map one URL to another, without redirection. If you redirect the visitor to the /store page instead, the store pages will not be properly indexed by Google. 
</aside>

**When server configuration can't be changed**

The enabling of the SEO-friendly URLs involves modification of the server settings and some website owners simply can't access it sometimes. The **query-based clean URLs** is a workaround for this situation – all you need to enable them is to modify the Ecwid integration code (ignoring the **Step 1 and 2** of SEO-friendly URLs)

**Query-based clean URLs** for Ecwid allow to change the store URLs so that they look like this: `https://www.example.com/shop?store-page=Apple-p123` thus imrpoving the SEO standings of the store pages, but without any changes to the server configuration.

**How to enable query-based clean URLs**

On the site page where you add Ecwid store, you will need to add a few lines of javascript code before the Ecwid integration code. It will tell Ecwid to enable query-based URLs on this page. See [example code](https://gist.github.com/makfruit/00e1c298f11bf12392f67c240e35e927)

If the `baseUrl` is specified, Ecwid will use this as a base for the query-based URLs. It is an **optional** field that is helpful when you want to have a permanent query parameter when a customer browses the store. In other cases, we suggest not using it in your code.

##### Step 2. Adjust Ecwid integration code: enable the SEO URLs

On the site page where you add Ecwid store, you will need to add a few lines of javascript code before the Ecwid integration code. It will tell Ecwid to enable SEO friednly URLs on this page. See [example codes](https://gist.github.com/makfruit/00e1c298f11bf12392f67c240e35e927)

Make sure to replace the "/store" dummy value in the last line with the actual store page path. Ecwid will use this as a base for the clean URLs. E.g. if you specify this URL as your base URL: `/subfolder/shop` and your site is on `https://www.mysite.com`

Ecwid will address store pages like this:
- `https://www.mysite.com/subfolder/shop/My-product-p123`
- `https://www.mysite.com/subfolder/shop/cart`
- `https://www.mysite.com/subfolder/shop/checkout`

etc. 

<aside class='notice'>
We recommend using <strong>relative URLs</strong> in the <em>baseUrl</em> parameter. But you can also specify an absolute baseUrl if needed, e.g. "https://www.mysite.com/subfolder/shop/cart". <br/><br/>In such case, make sure it is under the same domain a visitor browses the site. E.g. if it includes 'www', do include 'www' in the base URL; if it contains https, do use https in the base URL. If the base URL domain or schema differ from the actual ones, the visitor browser will block URL rewrites and the SEO URls will not work. 
</aside>

#### Examples

Please see the example codes on [this page](https://gist.github.com/makfruit/00e1c298f11bf12392f67c240e35e927)

#### FAQ

#### Q: Will the regular hash-bang URLs continue working if I enable the new SEO URLs? 

Yes, the old URLs will be supported, so if someone clicks the old link anywhere on the web, they will get to the corresponding store page. Upon opening the page, Ecwid will automatically replace the hash part so that the visitor will see the new URLs in the browser address bar. 

#### Q: Can I get SEO-friendly URLs in Ecwid REST API?

Yes, when getting information about products and categories in an Ecwid store, you can choose the URL format you wish to receive in response. For more details, see the links below: 

- [How to get URLs for products](https://developers.ecwid.com/api-documentation/products#q-how-to-get-urls-for-products)
- [How to get URLs for categories](https://developers.ecwid.com/api-documentation/categories#q-how-to-get-urls-for-categories)

#### Q: How can I imrpove SEO if I can't change the server settings?

SEO-friendly URLs require changes on the server to enable them. If you don't have access to those settings, please see the **Query-based clean URLs** above.

### Canonical URLs

Canonical URLs on an HTML web page help the website's search rankings. When a website has multiple pages for a single content, you can specify a single canonical URL for search engines to use when referring to all those pages. This way, you can avoid having duplicate results in the search results thus improving the search engine rankings of your website pages.

This can be helpful, when your store products are located in multiple categories at once – with canonical URL, you will direct potential buyers to that single URL and improve its positions in the search results.

#### How canonical URLs work

When canonical URLs are enabled on the page with a store, Ecwid will dynamically update the canonical URL of the page the visitor is on. 

**Store Front Page**

For the store front page, Ecwid will always use the canonical URL that is already present on the page before Ecwid store was loaded.

**Product and Category pages**

If canonical URLs functionality is enabled, Ecwid will dynamically update the canonical URL of that page to match the current product or category that is currently opened. Ecwid will always refer to a unique canonical URL so that the products in multiple categories are addressed in the same way to get the most of the rankings on search engines. 

#### How to enable canonical URLs

> Adjusting Ecwid integration code to enable canonical URLs

```html
<script>
  window.ec = window.ec || {};
  window.ec.config = window.ec.config || {};
   
  window.ec.config.enable_canonical_urls = true;
</script>
```

In order to enable canonical URLs functionality for your store pages, you will need to adjust the Ecwid integration code on your website by adding some JavaScript code. If you are using Ecwid starter site as the main store website then no action is required from you – the canonical URLs are already enabled there.

The `enable_canonical_urls` setting will 'tell' Ecwid to enable the canonical URLs functionality and it will work as described above. Check out the example on the right.
