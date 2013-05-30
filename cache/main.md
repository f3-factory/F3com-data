# Cache

This class represents F3's multi protocol Cache engine. It supports [Memcache](http://memcached.org/), [WinCache](http://www.iis.net/downloads/microsoft/wincache-extension), [APC](http://php.net/manual/en/book.apc.php), [XCache](http://xcache.lighttpd.net/) and filesystem based caching.

Caching is a powerful way to get more performance out of your application. It is seamlessly integrated to all core and plugin features
like [setting hive keys](base#set), [HTTP responses](base#caching), DB queries or JS/CSS minification.
There is also a good [user guide section about caching](optimization#cache-engine) you really should have read.

The Cache engine is deactivated by default. To activate it using an auto-detected cache backend, set the [CACHE](quick-reference#cache) system var to `TRUE`.
See [load()](cache#load) function for additional configuration.


Namespace: `\` <br/>
File location: `lib/base.php`

---

### instance

**Return class instance**

``` php
$cache = \Cache::instance();
```

The Cache class uses the [Prefab](prefab-registry) factory wrapper, so you are able to grab the same instance of that class at any point of your code.


### exists

**Check if a cache entry exists**

``` php
$cache->exists( string $key, [ mixed &$val = NULL ]); array|false
```

It returns an array containing the elements [0] creation timestamp and [1] time-to-live (TTL, in seconds) of the cache entry; or `FALSE` if it was not found.

``` php
$cache->set('foo','bar',5);

var_dump($cache->exists('foo'));
/* returns
array(2) {
  [0]=>  float(1369257446.4874)
  [1]=>  int(5)
}
*/
```

You can use the `$val` argument to fetch the cache entry content as well. This could save an additional `get` call to the cache backend.

``` php
if ($cache->exists('foo',$value)) {
    echo $value; // bar
}
```


### set

**Store value in cache**

``` php
$cache->set( string $key, mixed $val, [ int $ttl = 0 ]); mixed|false
```

If `$ttl` is 0 then the entry is saved for an infinite time. Otherwise, the specified time (seconds) is used.


### get

**Retrieve value of cache entry**

``` php
$cache->get( string $key ); mixed|false
```


### clear

**Delete cache entry**

``` php
$cache->clear( string $key ); bool
```


### reset

**Clear contents of cache backend**

``` php
$cache->reset([ string $suffix = NULL ], [ int $lifetime = 0 ]); bool
```

You can use `$suffix` to clear cache entries with a suffix that matches this string.
Set `$lifetime` in seconds to only delete entries that are older than this time.

You can also use `$f3->clear('CACHE')` as a shortcut to this.

### load

**Load/auto-detect cache backend**

``` php
$cache->load( string|bool $dsn ); string
```

Possible configurations for `$dsn` are:

* `apc`
* `wincache`
* `xcache`
* `memcache=localhost:11211`
* `folder=tmp/cache/`

Or set it `TRUE` for auto-detection (use filesystem fallback, if no shared memory engine available), or `FALSE` to disable caching.
