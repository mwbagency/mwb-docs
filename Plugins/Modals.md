Setups of a CPT which can then be selected when using our ACF button helper fields.

By utilising the same content blocks available for use within our pages and posts, modals offer another way to present secondary information without needing to explicitly support them.

## Settings
Sometimes a modal needs to automatically open. When editing a modal post, on the right, you'll be provided with some extra options.

![Modal settings](uploads/c0de4746c7a76f548b7fbb81d535bd14/image.png)

### Trigger type
Currently the following options are available:
- Logged in - only triggers if user is logged in
- Logged out - only triggers if user is logged out

![Trigger type setting](uploads/ca45fcb7935b4a2913095c2e855fa63f/image.png)

### Trigger paths
To limit which urls the modal automatically opens on, you can specify one per line. Using relative urls, you can either specify exact paths, or use regex for dynamic urls. E.g.:
- `/about/` to match the url `https://site.local/about/` 
- `\/checkout\/order-received\/\d+\/` to match urls like `https://site.local/checkout/order-received/949/?key=wc_order_cuwSt0yM8y5jS`

![Trigger path setting](uploads/8789e4d7e76868b4964249817e482a29/image.png)

### Trigger delay
To delay how long until the modal can open, enter the amount of seconds it should wait. Note, that this countdown starts after the user interacts with the page (clicks, scrolls, etc.) to help prevent this from impacting page performance scores, and inactive tabs.

![Trigger delay setting](uploads/771d9dc8d66b4b13c94c85acd8a68cbc/image.png)

### Trigger limit
To limit how many times the user is presented the modal automatically, you can enter how often this can occur.

![Trigger limit setting](uploads/03b9e1c1bd5a1de28c83855d9f4c8544/image.png)
