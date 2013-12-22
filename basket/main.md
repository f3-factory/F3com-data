# Basket : Session-based pseudo-mapper

The Basket plugin is a SESSION-based Object-Document-Mapper with a pretty similar syntax as the [Cursor class](cursor) and its derivatives.
This means that any addressed data is just available for the lifetime of the current user session, which makes it useful for shopping cart implementations or any service that works with guest visitors instead of registered members.

Namespace: `\` <br>
File location: `lib/basket.php`

---

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>

## Instantiation

This is how you create a new Basket mapper:

```php
$mapper = new \Basket( string $key )

```
Please refer to the [__construct](basket#&#95;&#95;construct) method for details.


The `$key` parameter is an equivalent to a database table name.

## Methods


### exists

**Return TRUE if field is defined**

``` php
bool exists ( string $key ) 
```

This function allows you to return TRUE if field is defined 

Example:

``` php

echo $basket->exists($key) // displays '@TODO' 


```

### set

**Assign value to field**

``` php
scalar|FALSE set ( string $key, scalar $val ) 
```

This function allows you to assign value to field. 

The $key == '_id' is a reserved key and would result in `set()` returning FALSE and not saving the key/value pair.

Example:

``` php

$arrayItems = $basket->set($key, $val);


```

### get

**Retrieve value of field**

``` php
scalar|FALSE get ( string $key ) 
```

This function allows you to retrieve value of field 

Example:

``` php

echo $basket->get($key) // displays '@TODO' 


```

### clear

**Delete field**

``` php
NULL clear ( string $key ) 
```

This function allows you to delete field 

Example:

``` php

echo $basket->clear($key) // displays '@TODO' 


```

### find

**Return items that match key/value pair**

``` php
array|FALSE find ( string $key, mixed $val ) 
```

This function allows you to return items that match key/value pair 

Example:

``` php

echo $basket->find($key, $val) // displays '@TODO' 


```

### findone

**Return first item that matches key/value pair**

``` php
object|FALSE findone ( string $key, mixed $val ) 
```

This function allows you to return first item that matches key/value pair 

Example:

``` php

echo $basket->findone($key, $val) // displays '@TODO' 


```

### load

**Map current item to matching key/value pair**

``` php
array load ( string $key, mixed $val ) 
```

This function allows you to map current item to matching key/value pair 

Example:

``` php

echo $basket->load($key, $val) // displays '@TODO' 


```

### dry

**Return TRUE if current item is empty/undefined**

``` php
bool dry (  ) 
```

This function allows you to return TRUE if current item is empty/undefined 

Example:

``` php

echo $basket->dry() // displays '@TODO' 


```

### count

**Return number of items in basket**

``` php
int count (  ) 
```

This function allows you to return number of items in basket 

Example:

``` php

echo $basket->count() // displays '@TODO' 


```

### save

**Save current item**

``` php
array save (  ) 
```

This function allows you to save current item 

Example:

``` php

echo $basket->save() // displays '@TODO' 


```

### erase

**Erase item matching key/value pair**

``` php
bool erase ( string $key, mixed $val ) 
```

This function allows you to erase item matching key/value pair 

Example:

``` php

echo $basket->erase($key, $val) // displays '@TODO' 


```

### reset

**Reset cursor**

``` php
NULL reset (  ) 
```

This function allows you to reset cursor 

Example:

``` php

echo $basket->reset() // displays '@TODO' 


```

### drop

**Empty basket**

``` php
NULL drop (  ) 
```

This function allows you to empty basket 

Example:

``` php

echo $basket->drop() // displays '@TODO' 


```

### copyfrom

**Hydrate item using hive array variable**

``` php
NULL copyfrom ( string $key ) 
```

This function allows you to hydrate item using hive array variable 

Example:

``` php

echo $basket->copyfrom($key) // displays '@TODO' 


```

### copyto

**Populate hive array variable with item contents**

``` php
NULL copyto ( string $key ) 
```

This function allows you to populate hive array variable with item contents 

Example:

``` php

echo $basket->copyto($key) // displays '@TODO' 


```

### checkout

**Check out basket contents**

``` php
array checkout (  ) 
```

This function allows you to check out basket contents 

Example:

``` php

echo $basket->checkout() // displays '@TODO' 


```

### __construct

**Instantiate class**

``` php
void __construct ( [ string $key = basket ] ) 
```

This function allows you to instantiate class 

Example:

``` php
$basket = new Basket ( $key )

```
