A global block is either a header, mobile menu or footer. They're all optional (e.g. you could skip having a mobile menu if your header can convert to one using breakpoints). 

## Creating a new global block

### Files
First, decide on a code/name, for organisational reasons, any site specific blocks should go under `SITE` (i.e. `SITE/Header`, `SITE/MobileMenu` and `SITE/Footer`). 

Create the "backend" and "frontend" files for the global block, in the same manner as before, e.g.
* "Backend file" `{{child-theme}}/includes/MWB/Block/NAVI/NAVIHeader.php`
* "Frontend file" `{{child-theme}}/resources/views/block/NAVI/NAVIHeader.twig`

It'll be easiest to duplicate an existing global block's files for that type, like `NAVI001` for headers, e.g.:
- ["Backend file"](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/includes/MWB/Block/NAVI/NAVI001.php)
- ["Frontend file"](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/resources/views/block/NAVI/NAVI001.twig)

### Admin selection
In the WordPress admin "appearance" menu, you can choose between different pre-built global blocks. Naturally you'll want to lock this down, as you'll likely be building a site with one bespoke global block of each type, and will want to prevent the admin from changing it.

To do this, find the respective filter for your global block type (see below) and return your one bespoke global block instead of the default list. E.g.
```
/**
 * Alter site header choices
 */
add_filter( 'mwb/blocks/headers_list', function( array $headers ): array {
  return [ 'SITE/Header' ];
}, 10, 1 );
```

The global block list selection filters are:
- `mwb/blocks/headers_list` 
- `mwb/blocks/mobile_menus_list`
- `mwb/blocks/footers_list`.

## ACF fields

Their ACF field groups are displayed under "Appearance" > "Navigation".