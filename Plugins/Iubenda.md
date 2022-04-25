When using Iubenda for it's privacy and cookie policy page context, this is how to display content for those pages on your site. 

## Setup

### Get the Iubenda ID
Within Iubenda, go to the "embed" code for the "Cookie Policy". You'll find the ID within the cookie policy link.

![Iubenda Cookie policy ID](uploads/45fa3e6a5f1ec1278fdc359c064a211c/image.png)

### Enter the Iubenda ID into WordPress
In WordPress admin, go to Settings > Iubenda (`/wp-admin/options-general.php?page=acf-options-iubenda`) and enter the cookie policy ID.

![WP Admin Iubenda settings](uploads/26b70aeb1036944c7a06e881817a25de/image.png)

## Displaying
After entering these IDs, the below shortcodes will now be available:
* `[privacy_policy]` (Privacy policy)
* `[cookie_policy]` (Cookie Policy)
