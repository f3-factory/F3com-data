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

This function allows you to check if a named field is defined 

Example:

```php
$basket->set('cherry',5);
var_dump( $basket->exists('cherry') ); // displays 'boolean true'
```

### set

**Assign value to field**

```php
scalar|FALSE set ( string $key, scalar $val ) 
```

This function allows you to assign a value to a field. 

**Notice**: The $key '`_id`' is a reserved key and would result in `set()` returning FALSE and not saving the key/value pair.

Example:

```php
$val = $basket->set($key, $val);
```

### get

**Retrieve value of field**

```php
scalar|FALSE get ( string $key ) 
```

This function allows you to retrieve the value of a field 

Example:

```php
echo $basket->get('cherry'); // displays '5'
```

### clear

**Delete field**

```php
clear ( string $key ) 
```

This function allows you to delete a field. The field won't exist anymore for this basket. 

Example:

```php
$basket->clear('amount');
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

echo $result[0]->get('amount'); // displays '5' (int)
echo $result[0]->get('name'); // displays 'cherry'

```

### findone

**Return first item that matches key/value pair**

```php
object|FALSE findone ( string $key, mixed $val ) 
```

This function allows you to get the first item that matches the given key/value pair 

Example:

```php
$result = $basket->findone('amount','10');
echo $result->get('name'); // displays 'peach'
```

### load

**Map current item to matching key/value pair**

```php
array load ( string $key, mixed $val ) 
```

This function allows you to map current item to the matching key/value pair 

Example:

```php

$basket->load('name','peach');
echo $basket->get('amount'); // displays '10' (int)


```

### dry

**Return TRUE if current item is empty/undefined**

```php
bool dry (  ) 
```

This function allows you to check if the current item is empty/undefined 

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

This function allows you to return the number of items in the basket 

Example:

```php
echo $basket->count(); // displays '2' (int)
```

### save

**Save current item**

```php
array save (  ) 
```

This function allows you to save the current basket item 

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

This function allows you to erase the basket item matching the given key/value pair 

Example:

```php
$basket->erase('name','peach'); // returns TRUE
```

### reset

**Reset cursor**

```php
reset (  ) 
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
drop (  ) 
```

This function allows you to completely empty the basket contents. The basket is removed from the SESSION as well.

Example:

```php
$basket->drop();
```

### copyfrom

**Hydrate item using hive array variable**

```php
copyfrom ( string $key ) 
```

This function allows you to hydrate the basket using a hive array variable 

Example:

```php
$f3->set('banana',array('name'=>'banana', 'amount'=> 15));
$basket->copyfrom('banana');
$basket->get('name'); // banana
```

### copyto

**Populate hive array variable with item contents**

```php
copyto ( string $key ) 
```

This function allows you to populate a hive array variable with basket contents 

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
$basket_items = $basket->checkout();
```

### __construct

**Instantiate class**

```php
void __construct ( [ string $key = 'basket' ] ) 
```

The constructor allows you to instantiate the class. 

The `$key` parameter is an equivalent to a database table name.

Example:

```php
$basket = new Basket('shoppingcart');
```

