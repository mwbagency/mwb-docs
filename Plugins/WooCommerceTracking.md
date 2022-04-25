Triggers "dataLayer" events to Google Tag Manager's when certain WooCommerce events occur.

NOTE: there are other events like adding an item to cart that this MWB plugin may extend to in the future.

## Transaction event
Triggered when an order is placed on the "thank you" page.

Event fields:
- event (transaction)
- transactionId (Order number)
- transactionAffiliation (Blog name)
- transactionTotal (Order subtotal)
- transactionTax (Order tax total)
- transactionShipping (Order shipping total)
- transactionProducts (List of order product items, more details below)

transactionProducts fields:
- name (Order item name)
- sku (Product/variation SKU)
- category (Product's first category name)
- price (Order item's line total)
- quantity (Order item's quantity)

