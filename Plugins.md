Some functionality within the base theme is more likely to be disabled/enabled on a per-site basis. For these situations you'll likely find them packaged up as an "MWB plugin". These are essentially a PHP class that gets instantiated and may also come with some styling, scripting and Twig templates too.

## List
- [AdvancedForms](Plugins/AdvancedForms)
- [Analytics](Plugins/Analytics)
- [Coreprint](Plugins/Coreprint)
- [CookieConsent](Plugins/CookieConsent)
- [Iubenda](Plugins/Iubenda)
- [Leaflet](Plugins/Leaflet)
- [LoginPage](Plugins/LoginPage)
- [Modals](Plugins/Modals)
- PostForms
- [RankMath](Plugins/RankMath)
- Share
- [Social](Plugins/Social)
- WooCommerce
- WooCommerceNotices
- WooCommerceSubscriptions
- WooCommerceTracking
- WooCommerceWishlist
- [Yoast](Plugins/Yoast)

## Usage
These are enabled via the `mwb/plugins/list` filter. E.g.:

```
/**
 * Alter enabled site plugins
 * (make sure to only enable ones you need)
 */
add_filter( 'mwb/plugins/list', function( array $plugins ): array {
    return [
        'AdvancedForms',
        'Analytics',
        'CookieConsent',
        'LoginPage',
        'PostForms',
        'RankMath',
        'Share',
        'Social',
    ];
}, 10, 1 );
```