# Magic : PHP magic wrapper

The Magic class is a PHP magic wrapper.

---

Namespace: `\` <br>
File location: `lib/magic.php`

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>


## ~~Instantiation~~
### Magic is an abstract class


## Methods


### exists

**Return TRUE if key is not empty**

``` php
bool exists ( string $key ) 
```

This function allows you to return TRUE if key is not empty

Example:

``` php
echo $magic->exists($key) // displays '@TODO' 
```

### set

**Bind value to key**

``` php
mixed set ( string $key, mixed $val ) 
```

This function allows you to bind value to key

Example:

``` php
echo $magic->set($key, $val) // displays '@TODO' 
```

### get

**Retrieve contents of key**

``` php
mixed get ( string $key ) 
```

This function allows you to retrieve contents of key

Example:

``` php
echo $magic->get($key) // displays '@TODO' 
```

### clear

**Unset key**

``` php
NULL clear ( string $key ) 
```

This function allows you to unset key

Example:

``` php
echo $magic->clear($key) // displays '@TODO' 
```

### visible

**Return TRUE if property has public visibility**

``` php
private bool visible ( string $key ) 
```

This function allows you to return TRUE if property has public visibility

Example:

``` php
echo $magic->visible($key) // displays '@TODO' 
```

### offsetexists

**Convenience method for checking property value**

``` php
mixed offsetexists ( string $key ) 
```

This function allows you to convenience method for checking property value

Example:

``` php
echo $magic->offsetexists($key) // displays '@TODO' 
```

### __isset

**Alias for offsetexists()**

``` php
mixed __isset ( string $key ) 
```

This function allows you to alias for offsetexists()

Example:

``` php
echo $magic->__isset($key) // displays '@TODO' 
```

### offsetset

**Convenience method for assigning property value**

``` php
mixed offsetset ( string $key, scalar $val ) 
```

This function allows you to convenience method for assigning property value

Example:

``` php
echo $magic->offsetset($key, $val) // displays '@TODO' 
```

### __set

**Alias for offsetset()**

``` php
mixed __set ( string $key, scalar $val ) 
```

This function allows you to alias for offsetset()

Example:

``` php
echo $magic->__set($key, $val) // displays '@TODO' 
```

### offsetget

**Convenience method for retrieving property value**

``` php
mixed offsetget ( string $key ) 
```

This function allows you to convenience method for retrieving property value

Example:

``` php
echo $magic->offsetget($key) // displays '@TODO' 
```

### __get

**Alias for offsetget()**

``` php
mixed __get ( string $key ) 
```

This function allows you to alias for offsetget()

Example:

``` php
echo $magic->__get($key) // displays '@TODO' 
```

### offsetunset

**Convenience method for checking property value**

``` php
NULL offsetunset ( string $key ) 
```

This function allows you to convenience method for checking property value

Example:

``` php
echo $magic->offsetunset($key) // displays '@TODO' 
```

### __unset

**Alias for offsetunset()**

``` php
NULL __unset ( string $key ) 
```

This function allows you to alias for offsetunset()

Example:

``` php
echo $magic->__unset($key) // displays '@TODO' 
```
