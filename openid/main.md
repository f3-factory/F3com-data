# OpenID : OpenID consumer

The OpenID class is a OpenID consumer.

---

Namespace: `Web` <br>
File location: `lib/web/openid.php`

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>

## Instantiation

**Return class instance**

```php

$openid = new OpenID ();

```
The OpenID class extends the [Magic](magic) class.



## Methods


### discover

**Determine OpenID provider**

``` php
protected string|FALSE discover ( string $proxy ) 
```

This function allows you to determine OpenID provider 

Example:

``` php

echo $openid->discover($proxy) // displays '@TODO'


```

### auth

**Initiate OpenID authentication sequence; Return FALSE on failure or redirect to OpenID provider URL**

``` php
bool auth ( [ string $proxy = NULL, array $attr = array(), string|array $reqd = NULL ] ] ] ) 
```

This function allows you to initiate OpenID authentication sequence; Return FALSE on failure or redirect to OpenID provider URL 

Example:

``` php

echo $openid->auth($proxy, $attr, $reqd) // displays '@TODO'


```

### verified

**Return TRUE if OpenID verification was successful**

``` php
bool verified ( [ string $proxy = NULL ] ) 
```

This function allows you to return TRUE if OpenID verification was successful 

Example:

``` php

echo $openid->verified($proxy) // displays '@TODO'


```

### response

**Return OpenID response fields**

``` php
array response (  ) 
```

This function allows you to return OpenID response fields 

Example:

``` php

echo $openid->response() // displays '@TODO'


```

### exists

**Return TRUE if OpenID request parameter exists**

``` php
bool exists ( string $key ) 
```

This function allows you to return TRUE if OpenID request parameter exists 

Example:

``` php

echo $openid->exists($key) // displays '@TODO'


```

### set

**Bind value to OpenID request parameter**

``` php
string set ( string $key, string $val ) 
```

This function allows you to bind value to OpenID request parameter 

Example:

``` php

echo $openid->set($key, $val) // displays '@TODO'


```

### get

**Return value of OpenID request parameter**

``` php
mixed get ( string $key ) 
```

This function allows you to return value of OpenID request parameter 

Example:

``` php

echo $openid->get($key) // displays '@TODO'


```

### clear

**Remove OpenID request parameter**

``` php
NULL clear ( $key ) 
```

This function allows you to remove OpenID request parameter 

Example:

``` php

echo $openid->clear($key) // displays '@TODO'


```
