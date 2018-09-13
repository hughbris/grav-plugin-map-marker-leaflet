# Map Leaflet Plugin

The **Map Leaflet** Plugin is for [Grav CMS](http://github.com/getgrav/grav). Embed a map with markers using fontawesome icons or letters/numbers. Uses open source [Leaflet js library](https://leafletjs.com) with data from [OpenStreetMap](https://www.openstreetmap.org). Fancy styling can be obtained from  [Thunderforest](https://www.thunderforest.com) or [MapBox](https://www.mapbox.com).

## Purpose

Google changed its policy - effective June 2018 - about maps so that a credit card account had to be provided to access parts of the map api. OpenStreetMap provides map data as a community service, and Leaflet provides an opensource js library to access the map data. Maps can be enhanced by other third-party providers, who - like Google (at the time of writing) - require an apikey, but unlike Google do not require bank details.

This plugin was written to provide an alternative to the `map-google` plugin and is based on `map-quest`.

Since OpenStreetMap provides tiles without charge, there are limitations on use and the service should not be treated with respect. 

## Installation

Installing the Map Leaflet plugin can be done in one of two ways. The GPM (Grav Package Manager) installation method enables you to quickly and easily install the plugin with a simple terminal command, while the manual method enables you to do so via a zip file.

### GPM Installation (Preferred)

The simplest way to install this plugin is via the [Grav Package Manager (GPM)](http://learn.getgrav.org/advanced/grav-gpm) through your system's terminal (also called the command line).  From the root of your Grav install type:

    bin/gpm install map-leaflet

This will install the Map Leaflet plugin into your `/user/plugins` directory within Grav. Its files can be found under `/your/site/grav/user/plugins/map-leaflet`.

### Manual Installation

To install this plugin, just download the zip version of this repository and unzip it under `/your/site/grav/user/plugins`. Then, rename the folder to `map-leaflet`. You can find these files on [GitHub](https://github.com/finanalyst/grav-plugin-map-leaflet) or via [GetGrav.org](http://getgrav.org/downloads/plugins#extras).

You should now have all the plugin files under

    /your/site/grav/user/plugins/map-leaflet

> NOTE: This plugin is a modular component for Grav which requires [Grav](http://github.com/getgrav/grav) and the [Error](https://github.com/getgrav/grav-plugin-error) and [Problems](https://github.com/getgrav/grav-plugin-problems) to operate.

### Admin Plugin

If you use the admin plugin, you can install directly through the admin plugin by browsing the `Plugins` tab and clicking on the `Add` button.

## Configuration

Before configuring this plugin, you should copy the `user/plugins/map-leaflet/map-leaflet.yaml` to `user/config/plugins/map-leaflet.yaml` and only edit that copy.

Here is the default configuration and an explanation of available options:

```yaml
enabled: true
provider: opensteetmap
m-style: 'mapbox.streets'
t-style: 'cycle'
```

- provider If the Admin plugin is used, then three provider options are provided.
    1. OpenStreetMap - no further options are needed
    1. Thunderforest - two further options are given
        1. apikey - the provider requires an apikey, which for hobbyists is free.
        1. t-style - the style of the map. A list of available options is given
    1. MapBox
        1. apikey - as above
        1. m-style - as t-style above.

Note that if you use the admin plugin, a file with your configuration, and named map-leaflet.yaml will be saved in the `user/config/plugins/` folder once the configuration is saved in the admin.

## Usage

The following example demonstrates how to embed an interactive map and place on it two sets of markers.

The plugin provides two shortcodes:
- `[map-leaflet]`
    - options:
        - lat -- the latitude of the centre of the map, defaults to 51.505 (London).
        - lng -- the longitude of the centre of the map  defaults to -0.09
        - zoom -- an integer from 1-24, see MapLeaflet documentation. Defaults to 13.
        - width -- the width in browser coordinates for the map, defaults to 100%
        - height -- the height in browser coordinates, defaults to 530px
    - contents:
        - Empty, in which case only a map is generated.
        - A set of `marker` codes.
- `[awesome-markers]`
    - options that if they are included in the shortcode become the default for each marker:
        - `markerColor` -- one of 'red', 'darkred', 'orange', 'green', 'darkgreen', 'blue', 'purple', 'darkpurple', 'cadetblue' (default = 'blue')
        -` iconColor` -- an HTML colour code, (default = 'white')
        - `draggable` -- see MapLeaflet documentation. By default False. True if either option is present, or given as draggable=True
        - `spin` -- the icon spins (default = False), can be set as for draggable
    - content between opening and closing shortcodes:
        - A JSON **Array** of **Hash**, eg
            `{"title": "popup text", "lat": 122.222, "lng": 22.9, "markerColor": "red" , "icon": "coffee" }`
        - `lat` and `lng` are mandatory, and the others are optional. If not included, the default value is given to them.
            - `title` -- the text for the popup of the marker (default = '')
            - `text` -- the text inside the marker (default = '') Should be kept short.
            - `markerColor`,` iconColor`,`draggable` and `spin` can be set in the hash or a default set in the shortcode.

### Example
The following code is in <path to grav>/user/map/default.md
```yaml
---
title: MapLeaflet
cache_enable: false
---
[map-leaflet lat=37.7749 lng=-122.4194 zoom=13]
[a-markers markerColor="darkblue"
iconColor="white"
]
[{ "lat": 37.7749, "lng": -122.4194, "icon": "home", "title": "Home Position" } ]
[/a-markers]
[a-markers icon=""]
[  { "lat": 37.775,  "lng": -122.48 , "text": 1, "draggable": true  },
{ "lat":  37.77,  "lng": -122.414 , "text": 2, "markerColor": "cadetblue" },
{ "lat":   37.765,  "lng": -122.409, "text": 3, "spin": true },
{ "lat":   37.76,  "lng": -122.3995, "text": 4, "spin": false },
{ "lat":   37.755,  "lng": -122.499, "icon": "coffee", "markerColor": "red", "title": "Lovely bistro"}
]
[/a-markers]
[/map-leaflet]

```
### Comments
- cache_enable should be set to false as there has to be a call to the tile provider to get the data.
- Any of the fontawesome icons can be included as markers.
- Any alphanumeric text can be included, but too many characters > 3? will be ugly.
- OpenStreetMap is the repository for map tiles, Leaflet is the library for managing the map tiles. However, styling the maps is subjective and there are multiple providers, such as Thunderforest and MapBox.

### Disclaimer
The coordinates in this illustration have no meaning.

### Limitations
- Due to the simple implementation of the shortcode, it is possible that there should only be one map on one page. It is possible that if more maps are defined, the marker will be placed on each, or only one, map.
- do not set both 'text' and 'icon' for each marker.

## Credits

- Awesome work by Leaflet, OpenStreetMap, Thunderforest and MapBox.
- The `a-markers` js and css are based on the [Awesome markers Leaflet plugin](https://github.com/lvoogdt/Leaflet.awesome-markers) but with modifications from [StackOverflow rockXrock](https://stackoverflow.com/a/25563023/6293949).
    - modifications: added 'salmon' to colors (missed in original)
    - allowed for text in marker

## To Do
- Generalise the map provider list, initializing plugin from providers.yaml, so to add a new provider can be done by adding an entry to providers.yaml.
    - This will happen if a use case is requested.
