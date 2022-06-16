- [Blocks](Blocks)
- [Plugins](Plugins)
- [Analytics, Consent & Legal](Analytics,-Consent-&-Legal)

## Download
* [Latest base theme zip](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/jobs/artifacts/master/raw/mwb-modules-base.zip?job=build)

## Setup
### For a build with no customisations
Install the base theme using the "latest base theme zip". This comes with everything enabled.

### For a build with customisations
1. Install the base theme using the "latest base theme zip" as the parent theme.
2. Create a new repo using the ["MWB Modules Blank" repo](https://gitlab.com/visix/wordpress/themes/mwb-modules-blank/) and follow it's compilation instructions within it's [README.md](https://gitlab.com/visix/wordpress/themes/mwb-modules-blank/-/blob/master/README.md)

## Common use cases
### Adding a static colour
When you need another colour to be available in the CMS, buttons, tailwind classes, etc. you need to inform the system of them in 3 locations.

### PHP
Within `functions.php`, using the `mwb/theme/colours` filter, add each static colour, see below. This will allow the system to present this colour within the CMS with the other colours, and also allow it to work out if it's light or dark (e.g. for our twig functions like `mwb_text_colour_class('new-colour')`).
```php
add_filter( 'mwb/theme/colours', function( array $colours ): array {
  return array_merge( $colours, [
    ...
    'new-colour' => '#228B22',
  ] );
}, 10, 1 )
```

### SCSS
Within `resources/scss/app.scss`, add the colour to either `$light-colours` or `$dark-colours`, this will determine if the colour is seen as light or dark, and then automatically set the opposite text colour for legibility. Doing this will generate any of our internal classes (e.g. our buttons variant classes). See below:
```scss
$light-colours: (
  ...
  'new-colour' => '#228B22',
);
```
or
```scss
$dark-colours: (
  ...
  'new-colour' => '#228B22',
);
```

### Tailwind
To generate utility classes for your colour (e.g. `bg-new-colour`), you need to add your colour to `tailwind.config.js`. 

Add your colour to `theme.extend.colors`, like so:

```js
module.exports = {
  ...
  theme: {
    ...
    extend: {
      colors: {
        ...
        'new-colour': '#228B22',
      },
      ...
    },
    ...
  },
  ...
}
```