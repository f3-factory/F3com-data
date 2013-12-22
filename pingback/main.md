# Pingback : Pingback 1.0 protocol (client and server) implementation

The Pingback class is a pingback 1.0 protocol (client and server) implementation.

---

Namespace: `Web` <br>
File location: `lib/web/pingback.php`

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>

## Instantiation

**Return class instance**


```php
$pingback = Web\Pingback::instance();
```

The Pingback class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.



## Methods


### enabled

**Return TRUE if URL points to a pingback-enabled resource**

``` php
protected bool enabled ( $url ) 
```

This function allows you to return TRUE if URL points to a pingback-enabled resource 

Example:

``` php

echo $pingback->enabled($url) // displays '@TODO'


```

### inspect

**Load local page contents, parse HTML anchor tags, find permalinks, and send XML-RPC calls to corresponding pingback servers**

``` php
NULL inspect ( string $source ) 
```

This function allows you to load local page contents, parse HTML anchor tags, find permalinks, and send XML-RPC calls to corresponding pingback servers 

Example:

``` php

echo $pingback->inspect($source) // displays '@TODO'


```

### listen

**Receive ping, check if local page is pingback-enabled, verify source contents, and return XML-RPC response**

``` php
string listen ( callback $func [ , string $path = NULL ] ) 
```

This function allows you to receive ping, check if local page is pingback-enabled, verify source contents, and return XML-RPC response 

Example:

``` php

echo $pingback->listen($func, $path) // displays '@TODO'


```

### log

**Return transaction history**

``` php
string log (  ) 
```

This function allows you to return transaction history 

Example:

``` php

echo $pingback->log() // displays '@TODO'


```

### __construct

**Instantiate class**

``` php
object __construct (  ) 
```

This function allows you to instantiate class 

Example:

``` php
$pingback = new Pingback (  )

```
