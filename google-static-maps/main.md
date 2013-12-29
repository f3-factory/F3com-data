# Google Static Maps v2 plug-in

Namespace: `\Web\Google` <br>
File location: `web/google/staticmap.php`

---

## Class StaticMap

The Google Static Maps class provides a simple way to embed a Google Maps image on your web page without requiring JavaScript or any dynamic page loading.

### Specifying Location

The Static Maps API uses strings (addresses) or numbers (latitude and longitude values) to specify locations on a map. (These values identify what is called a geocoded location)

To specify these locations, you can use either the `center` parameter and/or place any optional placemarks using the `markers` parameter at locations on the map.

As stated above, with both the `center` parameter and the `markers` parameter, you can use either Addresses or Latitudes and Longitudes to identify the location as shown in the examples below.

#### The `center` parameter

`center` defines the center of the map, equidistant from all edges of the map. This parameter takes a location as either a comma-separated {latitude,longitude} pair (e.g. "24.582720, -78.056685") or a string address (e.g. "Turks and Caicos Islands") identifying a unique location on the face of the earth

Examples:

```php
$map = new \Web\Google\StaticMap();
// by area
$map->center('富士山'); // 'Mont Fuji' area
// by address
$map->center("D'Arblay Street, London"); // F3 will escape spaces, quotes, and special characters for you
// by latitude and longitude values
$map->center('35.360496,138.727798'); // for example retrieved from a DB
```

Note that addresses provided may reflect either precise locations, such as an accurate street address, polylines such as named routes, or polygonal areas such as cities, countries, or even national parks. 

If for example you specify `$map->center('Grand Canyon');` as address, the Static Map server will resolve the location and return a map image using the center point of the area as the address center.

#### The `markers` parameter

The `markers` parameter defines a set of one or more markers at a set of locations. Each marker descriptor must contain a set of one or more locations defining where to place the marker on the map. These locations are separated using the pipe character (|).

Example:

```php
$map = new \Web\Google\StaticMap();
$map->markers('color:blue%7Clabel:S%7C11211%7C11206%7C11222'); // 
```

### Images formats

Images may be returned in several common web graphics formats: GIF, JPEG and PNG. You can use the `format` parameter that takes one of the following values:

+ `'png8'` or `'png'` (default) specifies the 8-bit PNG format.
+ `'png32'` specifies the 32-bit PNG format.
+ `'gif'` specifies the GIF format.
+ `'jpg'` specifies the JPEG compression format.
+ `'jpg-baseline'` specifies a non-progressive JPEG compression format.

Example:

```php
$map = new \Web\Google\StaticMap();
$map->format('jpg'); // progressive "lossy" JPEG compression format
```

### Maps Zoom level

Maps on Google Maps have an integer "zoom level" which defines the resolution of the current view. 

Zoom levels between `0` (the lowest zoom level, in which the entire world can be seen on one map) to `21+` (down to individual buildings) are possible within the default roadmap maps view.

Note:  Zoom levels vary depending on location and not all zoom levels appear at all locations on the earth.

Example:

```php
$map = new \Web\Google\StaticMap();    $map->zoom(12); // resolution of the view
```

### Images sizes

The `size` parameter, in conjunction with `center`, defines the coverage area of a map. It also defines the output size of the map in pixels, when multiplied with the `scale` value (which is 1 by default).

Example:

```php
$map = new \Web\Google\StaticMap();    $map->('640x480'); // in pixels and multiplied by scale
```
### Scale Values

The `scale` value is multiplied with the `size` to determine the actual output size of the image in pixels, without changing the coverage area of the map. (Default `scale` value is 1; accepted values are 1, 2, and, for Maps API for Business customers only, 4).

Example:

```php
$map = new \Web\Google\StaticMap();    $map->scale(2); // size multiplier
```
### Maps types

The Google Static Maps API creates maps in several formats, listed below:

+ `roadmap` (default) specifies a standard roadmap image, as is normally shown on the Google Maps website. If no maptype value is specified, the Static Maps API serves roadmap tiles by default.

+ `satellite` specifies a satellite image.

+ `terrain` specifies a physical relief map image, showing terrain and vegetation.

+ `hybrid` specifies a hybrid of the satellite and roadmap image, showing a transparent layer of major streets and place names on the satellite image.

Example:

```php
$map = new \Web\Google\StaticMap();
$map->maptype('satellite'); // Note: not all satellite views appear at all locations on the earth. 
```

## Methods

### dump

**Generate map using the Google Static Maps API v2 **

```php
string dump (  )
```

This method send a request to the Google Static Maps API service and returns the map as a raw image you can either save or display on your web page.

Example:

```php
$map = new \Web\Google\StaticMap();

$map->center('Pulau Tomea'); $map->maptype('satellite');
$map->size('320x240'); $map->zoom('10');

// we'll save the map in TEMP for example
$map_filename = $f3->get('TEMP').'map.png'; // by default returned maps images are in .png format

// save into a file the result of the API call retrieved by dump()
$f3->write($map_filename, $map->dump());

// use the image in the HTML page
printf ('<img src="/%s" alt="Map of Pulau Tomea in Lovely Indonesia" />', $map_filename);
```

There are a couple of drawbacks with this solution: you need to write a file to your server and, most importantly, the script has to wait the answer from the Google API to continue to parse and render your page. If for any reason the Google API is unavailable, your whole page will stuck at the very place the static map should appear.

Let's setup now an asynchronous solution. We need first to define a route for F3 to handle the request. Thus, the page can continue to be rendered while the browser send, in parallel, a request to this new route:

```php
$f3->route('GET /minify/@type', // @type will make `PARAMS.type` variable base point to the correct path
    function($f3, $args) {
        $f3->set('UI',$args['type'].'/'); // make sure you organize your files in sub-folders, per type
        echo Web::instance()->minify($_GET['files']);
    },
    3600*24 // Save minified file in F3 cache for 24 hours
);
```


```php
$f3->route('GET /static-map/@type', // @type will setup `PARAMS.type` variable
    function($f3, $args) {
		$map_settings = $args['type'];
		$map = new \Web\Google\StaticMap();
		$map->maptype('satellite');
        return $map->dump();
    },
    3600
);
```
_Notice_: When targeting mobile devices, use the `scale` parameter to return higher-resolution map images that solve the issues of mobile devices high resolution screens (see the [Google API Scale Values documentation](https://developers.google.com/maps/documentation/staticmaps/#scale_values "Google Maps Image APIs :: Static Maps API V2 Developer Guide") for more details).

Refer to the official [Google Static Maps API V2 Developer Guide](https://developers.google.com/maps/documentation/staticmaps/ "Google Static Maps API V2 Developer Guide") for more details.

