Provides a CMSable way to include social profiles for a site. This gets passed to the Timber/Twig context, where other plugins may also automatically populate this.

In WordPress admin, go to "Appearance" > "Social" (`/wp-admin/themes.php?page=acf-options-social`), see below.

![Social settings](uploads/5df48c44ef5845eab33c7fd75059f077/image.png)

## Functions

### Get the list
`\MWB\Site::get_social_profiles()`

Contains an array of items with:
- `icon` - Icon id for rendering
- `label` - Pretty label for text links / descriptions
- `url` - Url to link to

## Filters

### Alter the list
- Hook name: `mwb/site/social_profiles`
- Arguments:
  - `social_profiles` - Array

## Context

### Get the List
`social_profiles`