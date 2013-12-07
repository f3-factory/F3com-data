# Geo
The Geo plugin gives you a handful of location-based features.

Namespace: `\Web` <br/>
File location: `lib/web/geo.php`

---

### instance

**Return class instance**

``` php
$geo = \Web\Geo::instance();
```

The Geo class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.


### tzinfo

**Returns information about specified Unix time zone**

``` php
$geo->tzinfo( string $zone); array
```
This method accepts any available timezone string, which can be found here [http://php.net/manual/en/timezones.php](http://php.net/manual/en/timezones.php). For example:

``` php
/** @var \Web\Geo $geo */
$geo = \Web\Geo::instance();
var_dump($geo->tzinfo('Europe/Berlin'));
/* returns:
array(5) {
  ["offset"] => int(1)
  ["country"] => string(2) "DE"
  ["latitude"] => float(52.5)
  ["longitude"] => float(13.36667)
  ["dst"] => bool(false)
}*/
```

### location

**Return geolocation data based on specified/auto-detected IP address**

``` php
$geo->location([ string $ip = NULL]); array|FALSE
```

Example:

```php
$geo = \Web\Geo::instance();
var_dump($geo->location());
/* returns:
array(12) {
  ["request"] => string(12) "46.38.242.62"
  ["credit"] => string(145) "Some of the returned data includes GeoLite data created by MaxMind, available from <a href=\'http://www.maxmind.com\'>http://www.maxmind.com</a>."
  ["city"] => string(0) ""
  ["area_code"] => string(1) "0"
  ["dma_code"] => string(1) "0"
  ["country_code"] => string(2) "DE"
  ["country_name"] => string(7) "Germany"
  ["continent_code"] => string(2) "EU"
  ["latitude"] => string(2) "51"
  ["longitude"] => string(1) "9"
  ["region_code"] => string(0) ""
  ["region_name"] => NULL
}*/
```

### weather
**Return weather data based on specified latitude/longitude**

``` php
$geo->weather( float $latitude, float $longitude); array|FALSE
```

Example:

``` php
$geo = \Web\Geo::instance();
$loc = $geo->location('46.38.242.62');
var_dump($geo->weather($loc['latitude'],$loc['longitude']));
/* returns:
array(17) {
  ["weatherCondition"] => string(3) "n/a"
  ["clouds"] => string(13) "broken clouds"
  ["observation"] => string(59) "ETHF 181320Z 21009KT 9999 BKN017 OVC025 05/02 Q1010 WHT WHT"
  ["windDirection"] => int(210)
  ["ICAO"] => string(4) "ETHF"
  ["elevation"] => int(181)
  ["countryCode"] => string(2) "DE"
  ["cloudsCode"] => string(3) "BKN"
  ["lng"] => float(9.2666666666667)
  ["temperature"] => string(1) "5"
  ["dewPoint"] => string(1) "2"
  ["windSpeed"] => string(2) "09"
  ["humidity"] => int(80)
  ["stationName"] => string(8) "Fritzlar"
  ["datetime"] => string(19) "2013-11-18 13:20:00"
  ["lat"] => float(51.116666666667)
  ["hectoPascAltimeter"] => int(1010)
}*/
```
