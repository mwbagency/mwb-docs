A content block is one that can be used within pages, or any other custom post types. By default they're available for all, but you can restrict them via the block's class fields.

## Creating a new content block

### Files

First, decide on a code/name, for organisational reasons, any site specific blocks should go under `SITE` (e.g. `SITE/WinemakerMap`).

Create the "backend" and "frontend" files for the global block, in the same manner as before, e.g.

* "Backend file" `{{child-theme}}/includes/MWB/Block/NAVI/NAVIWinemakerMap.php`
* "Frontend file" `{{child-theme}}/resources/views/block/NAVI/WinemakerMap.twig`

It'll be easiest to duplicate an existing content block's files for that type, like `HERO001`, e.g.:

* ["Backend file"](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/includes/MWB/Block/HERO/HERO001.php)
* ["Frontend file"](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/resources/views/block/HERO/HERO001.twig)

### Admin selection

In the block editor, you'll be able to choose our content blocks within the list. By default none of the content blocks will be available, this is because we only want to enable the ones that have been designed, rather than giving the clients everything.

To add new blocks to the existing list of choices, add them below in your `functions.php` via the `mwb/blocks/list` filter, e.g.

```
/**
 * Alter site block choices
 * use a string representation of the class (e.g. 'HERO/001')
 */
add_filter( 'mwb/blocks/list', function( array $blocks ): array {
	return array_merge( $blocks, [
		'SITE/Timeline',
		'SITE/Spotlight',
		'SITE/StockistMap',
	] );
}, 10, 1 );
```