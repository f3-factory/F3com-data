# Basket : Session-based pseudo-mapper

The Basket plugin is a SESSION-based Object-Document-Mapper with a pretty similar syntax as the [Cursor class](cursor) and its derivatives.
This means that any addressed data is just available for the lifetime of the current user session, which makes it useful for shopping cart implementations or any service that works with guest visitors instead of registered members.

Namespace: `\` <br>
File location: `lib/basket.php`

---

## Instantiation

This is how you create a new Basket mapper:

```php
$basket = new \Basket()

```
Please refer to the [__construct](basket#&#95;&#95;construct) method for details.


## Methods


### exists

**Return TRUE if field is defined**

```php
bool exists ( string $key ) 
```

This function allows you to return TRUE if field is defined 

Example:

```php
$basket->set('cherry',5);
echo $basket->exists('cherry'); // TRUE
```

### set

**Assign value to field**

```php
scalar|FALSE set ( string $key, scalar $val ) 
```

This function allows you to assign a value to field. 

The $key == '_id' is a reserved key and would result in `set()` returning FALSE and not saving the key/value pair.

Example:

```php
$val = $basket->set($key, $val);
```

### get

**Retrieve value of field**

```php
scalar|FALSE get ( string $key ) 
```

This function allows you to retrieve value of field 

Example:

```php
echo $basket->get('cherry'); // displays '5'
```

### clear

**Delete field**

```php
NULL clear ( string $key ) 
```

This function allows you to delete field 

Example:

```php
$basket->clear('cherry');
```

### find

**Return items that match key/value pair**

```php
array|FALSE find ( string $key, mixed $val ) 
```

This function allows you to find items that match key/value pair. It returns an array of basket mapper objects.

Example:

```php
// add item
$basket->set('name','cherry');
$basket->set('amount',5);
$basket->save();
$basket->reset();
// add item
$basket->set('name','peach');
$basket->set('amount',10);
$basket->save();
$basket->reset();

$result = $basket->find('name','cherry');

$result[0]->get('amount'); // returns '5'

```

### findone

**Return first item that matches key/value pair**

```php
object|FALSE findone ( string $key, mixed $val ) 
```

This function allows you to return first item that matches key/value pair 

Example:

```php
$result = $basket->findone('name','cherry');

$result->get('amount'); // returns '5'
```

### load

**Map current item to matching key/value pair**

```php
array load ( string $key, mixed $val ) 
```

This function allows you to map current item to matching key/value pair 

Example:

```php

$basket->load('name','peach');
echo $basket->get('amount'); // displays '10'


```

### dry

**Return TRUE if current item is empty/undefined**

```php
bool dry (  ) 
```

This function allows you to return TRUE if current item is empty/undefined 

Example:

```php

$basket->load('name','banana');
$basket->dry(); // TRUE, this mapper is empty

$basket->load('name','peach');
$basket->dry(); // FALSE, this mapper is hydrated


```

### count

**Return number of items in basket**

```php
int count (  ) 
```

This function allows you to return number of items in basket 

Example:

```php
echo $basket->count() // displays '2' 
```

### save

**Save current item**

```php
array save (  ) 
```

This function allows you to save current item 

Example:

```php
$basket->set('name','peach');
$basket->set('amount',10);
$basket->save();
```

### erase

**Erase item matching key/value pair**

```php
bool erase ( string $key, mixed $val ) 
```

This function allows you to erase item matching key/value pair 

Example:

```php
$basket->erase('name','peach'); // returns TRUE
```

### reset

**Reset cursor**

```php
NULL reset (  ) 
```

This function allows you to reset the cursor 

Example:

```php
$basket->load('name','cherry'); // mapper is hydrated now
$basket->dry();	// false
$basket->reset();
$basket->dry();	// true, mapper is empty again
```

### drop

**Empty basket**

```php
NULL drop (  ) 
```

This function allows you to empty basket 

Example:

```php
$basket->drop();
```

### copyfrom

**Hydrate item using hive array variable**

```php
NULL copyfrom ( string $key ) 
```

This function allows you to hydrate item using hive array variable 

Example:

```php
$f3->set('banana',array('name'=>'banana', 'amount'=> 15));
$basket->copyfrom('banana');
$basket->get('name'); // banana
```

### copyto

**Populate hive array variable with item contents**

```php
NULL copyto ( string $key ) 
```

This function allows you to populate hive array variable with item contents 

Example:

```php
$basket->copyto($key);
```

### checkout

**Check out basket contents**

```php
array checkout (  ) 
```

This function allows you to check out basket contents, which means it returns all items and clears the whole basket.

Example:

```php
$basket->checkout();
```

### __construct

**Instantiate class**

```php
void __construct ( [ string $key = basket ] ) 
```

This function allows you to instantiate class 

Example:

```php
$basket = new Basket('shoppingcart');
```

The optional `$key` parameter is an equivalent to a database table name.
