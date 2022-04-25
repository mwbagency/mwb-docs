Enqueues the required scripts and styling for [Leaflet](https://leafletjs.com/), a JavaScript library for interactive maps. This is used instead of Google Maps, as it is free, and offers similar flexibility.

## Usage
The accompanying JavaScript allows for initialising maps with markers via data attributes.

Gather an array of objects containing the coordinates of each marker you want to display, and pass it into an empty element with the attribute `data-leaflet-markers`. The map will then attempt to automatically pan and zoom to show all of those markers.

The format of this list is as follows:
```
[
	{
		"coordinates": {
			"lat": "51.507351", 
			"long": "-0.127758",
		},
	},
	{
		"coordinates": {
			"lat": "52.507351", 
			"long": "-0.127758",
		},
	},
	{
		"coordinates": {
			"lat": "53.507351", 
			"long": "-0.127758",
		},
	},
]
```

An example of passing this data into an element with an example variable called `fields.map_markers` is as follows:
```
<div data-leaflet-markers='{{ fields.map_markers|default([])|json_encode|e('html') }}'></div>
```

There are also some other data attributes you can apply to this element to customise our default Leaflet configuration.
| Data attribute                    | Example value                                                               | Leaflet documentation                                               |
| --------------------------------- | ----------------------------------------------------------------------------| ------------------------------------------------------------------- |
| `data-leaflet-map-config`         | { "zoomControl": true, "scrollWheelZoom": false }                               | [Link](https://leafletjs.com/reference-1.7.1.html#map-option)       |
| `data-leaflet-tile-url`           | "https://stamen-tiles-{s}.a.ssl.fastly.net/toner-lite/{z}/{x}/{y}{r}.{ext}" | [Link](https://leafletjs.com/reference-1.7.1.html#tilelayer)        |
| `data-leaflet-tile-config`        | { "minZoom": 5 }                                                              | [Link](https://leafletjs.com/reference-1.7.1.html#tilelayer-option) |
| `data-leaflet-marker-icon-config` | { "iconUrl": "https://..." }                                                  | [Link](https://leafletjs.com/reference-1.7.1.html#tilelayer-option) |

