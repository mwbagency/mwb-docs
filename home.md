- [Blocks](Blocks)
- [Plugins](Plugins)
- [Analytics, Consent & Legal](Analytics,-Consent-&-Legal)

## Common use cases
### Adding a static colour
When you need another colour to be available in the CMS, buttons, tailwind classes, etc. you need to inform the system of them in 3 locations.

### PHP
Within `functions.php`, using the `mwb/theme/colours` filter, add each static colour like so:
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
To generate utility classes for your colour (e.g. `bg-new-colour`), you need to add your colour to `tailwind.js`. First get your child theme to use it's own tailwind config, and then add your colour to `theme.extend.colors`, like so:

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