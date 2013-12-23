# Pingback : Pingback 1.0 protocol (client and server) implementation

The Pingback class is a pingback 1.0 protocol (client and server) implementation.

Namespace: `Web` <br>
File location: `lib/web/pingback.php`

---

## Instantiation

**Return class instance**


```php
$pingback = Web\Pingback::instance();
```

The Pingback class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.


## Methods


### inspect

**Load local page contents, parse HTML anchor tags, find permalinks, and send XML-RPC calls to corresponding pingback servers**

``` php
inspect ( string $source ) 
```

This function loads local page contents, parse the HTML anchor tags, look for permalinks inside the page, and then send XML-RPC calls to corresponding pingback servers 

Example:

``` php
$pingback->inspect($source);

```

### listen

**Receive ping, check if local page is pingback-enabled, verify source contents, and return XML-RPC response**

``` php
string listen ( callback $func [ , string $path = NULL ] ) 
```

This function allows you to receive ping, check if local page is pingback-enabled, verify source contents, and use a given callback function to return a XML-RPC response 

If `$path` is not provided, the [BASE](quick-reference#base) system variable is used.

On error, `die` with the related [xmlrpc_encode_request](http://php.net/manual/en/function.xmlrpc-encode-request.php "php.net :: xmlrpc_encode_request") message.

Example:

``` php
echo $pingback->listen($func, $path);
```

### log

**Return transaction history**

``` php
string log (  ) 
```

This function returns the transaction history containing the list of the permalinks in the page, if the permalink was found or not, and the associated request body.

Example:

``` php
echo $pingback->log();
```

### __construct

**Instantiate class**

``` php
object __construct (  ) 
```

The constructor allows you to instantiate the class. 


Example:

``` php
$pingback = new Pingback (  )
```

**Notice**: As a convenience, this constructor calls [libxml_use_internal_errors(TRUE)](http://php.net/manual/en/function.libxml-use-internal-errors.php "php.net :: libxml_use_internal_errors") to disable libxml errors and thus allows you to fetch error information as needed.


### enabled

**Return TRUE if URL points to a pingback-enabled resource**

``` php
protected bool enabled ( $url ) 
```

This is a _protected_ function used internally by inspect() and listen() or by child classes.

This function returns TRUE if the given URL points to a pingback-enabled resource 


