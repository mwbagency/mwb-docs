Sets up theme support for [WooCommerce](https://woocommerce.com/) and also sets some general functionality and defaults too.

## Improved default placeholder image
In WordPress admin, go to "WooCommerce" > "Settings" > "Products" > "Placeholder image", and set it to blank. The placeholder will now be set to `/assets/img/placeholder.png` from within the parent theme. 

## Single product variation carousel auto-selection
When using the default template for single product images (`resources/views/plugin/WooCommerce/partial/single-product-images.twig`), the system will also inject any images associated to any variations. When the user chooses all the variation dropdowns and that variation has an image, it will automatically get scrolled to.

## Product tabs
An extra field group is added to single products, it's a repeater which allows for admins to add extra tabs for a product.

![Product tabs field group](uploads/542266d22dfc131f7a7a5be517dc367e/image.png)

![Product tabs on single product page](uploads/bc42ac1697b03fb0f7c6345aa350db6d/image.png)

## Variation cart item names
Variation attributes are by default displayed in the cart item names, this can create excessively long names and isn't that well formatted. Therefore, these are removed, and added below the cart item names in a "bullet-point" fashion.

## Functions

### Get cross-sells
- Function: `MWB\Plugin\WooCommerce::get_cross_sell_products()`
- Description: Uses the current cart contents, and calculates products to cross-sell the user. Returns timber posts.

### Get upsells
- Function: `MWB\Plugin\WooCommerce::get_upsell_products($product)`
- Description: Calculates the products to upsell the user using the provided product. Returns timber posts.

### Whether user has just added to cart
- Function: `MWB\Plugin\WooCommerce::has_just_added_to_cart()`
- Description: Whether user has just added to cart (why this page has been loaded)

## Filters

### Account icons
- Hook name: `mwb/plugin/woocommerce/account_icons`
- Arguments:
  - Icons (array): associative array with keys for account page ids, and their respective icon ids.
- Explanation: Used to alter icons used for account page navigation.

### Account icons
- Hook name: `mwb/plugin/woocommerce/account_classes`
- Arguments:
  - Icons (array): associative array with keys for account page ids, and their respective container classes.
- Explanation: Used to apply wysiwyg classes conditionally, and any other specific classes.

