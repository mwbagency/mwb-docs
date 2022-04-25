Replaces the default WooCommerce notices with an overlay. The idea for this is to avoid the headache of supporting the default notices being injected within the general layouts of the product, cart and checkout pages.

![Example of adding to cart](uploads/0681ed3eeda806905dff5098195d39f1/image.png)

## Filters

### Icons
- Hook name: `mwb/plugin/woocommerce_notices/icons`
- Arguments:
  - Icons (array): Icon names mapped to types of notices.

E.g.
```php
[
  'error'   => 'cancel',  // to use the icon `cancel`
  'notice'  => 'info',
  'success' => false,     // to have no icon
] );
```

### Colours
- Hook name: `mwb/plugin/woocommerce_notices/colours`
- Arguments:
  - Icons (array): Background classes mapped to types of notices.

E.g.
```php
[
  'error'   => 'error',
  'notice'  => 'warning',   // To use the class `bg-warning`
  'success'  => 'success',
]
```