Some functionality within the base theme is more likely to be disabled/enabled on a per-site basis. For these kinds of situations, you'll likely find them packaged up as an "MWB plugin". These are essentially a PHP class that gets instantiated and may also come with some styling, scripting and Twig templates.

These are enabled via the `mwb/plugins/list` filter.

The currently implemented MWB plugins are:
- [CookieConsent](/mwb-plugins/cookie-consent)
- TODO - complete list