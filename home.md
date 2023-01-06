- [Blocks](Blocks)
- [Components](Components)
- CPTs // TODO (overriding defaults, creating new, etc.)
- [Plugins](Plugins)
- [Analytics, Consent & Legal](Analytics,-Consent-&-Legal)

## Setup
1. Install the base theme by cloning the latest tag of the [MWB Modules Base repo](https://gitlab.com/visix/wordpress/themes/mwb-modules-base) and following the readme instructions.
2. Create a new repo using the latest tag of the [MWB Modules Blank repo](https://gitlab.com/visix/wordpress/themes/mwb-modules-blank/) and follow the [compilation instructions](#compilation).

## Compilation
If you're making customisations to the system, once you've got the themes on your local machine, you'll want to setup a dev environment whilst being able to compile the assets (CSS and JS).

### Browsersync

If you want [browsersync](https://browsersync.io/) local development (automatic reloading, and previewing on other devices on your network):
1. Using `local.example.json` as a template, create a file called `local.json`
1. Add your local development url (e.g. likely provided by "Local by Flywheel") for the `proxy` value

An example of contents for this file:

```json
{
  "proxy": "http://mwb-agency.local/"
}
```

### Asset compilation

#### Development
1. Run `npm install` on the parent theme ([mwb-modules-base](https://gitlab.com/visix/wordpress/themes/mwb-modules-base))
1. Run `npm install` on this child theme
1. Run `npm run dev` (or `npm run watch` to use the file watcher)
1. Go to http://localhost:3000 (unless specified otherwise in the terminal / depending on your browsersync settings)

#### Production
When you're ready for production, run `npm run prod` to minify your JS and CSS, & also purge any unused CSS.

#### Asset files
You may notice files like app.XXXX.css & app.XXXX.js are generated in the `assets` directory. This is for cache busting, the theme uses the generated `assets/mix-manifest.json` file to find the latest assets automatically, so there's no longer a need to change the theme's version each time you update the styling/scripts.

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
  'new-colour',
);
```
or
```scss
$dark-colours: (
  ...
  'new-colour',
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