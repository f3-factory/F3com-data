# Basket Mapper

The Basket plugin is a SESSION-based Object-Document-Mapper with a pretty similar syntax as the [Cursor class](cursor) and its derivatives.
This means that any addressed data is just available for the lifetime of the current user session, which makes it useful for shopping cart implementations or any service that works with guest visitors instead of registered members.

Namespace: `\Basket` <br>
File location: `lib/basket.php`

---

## Instantiation

This is how you create a new Basket mapper:

```php
$mapper = new \Basket( [ string $key = 'basket'] )
```

The optional `$key` parameter is an equivalent to a database table name.

## Methods

### exists

**Return TRUE if field is defined**

```php
bool exists( string $key )
```


### set

**Assign value to field**

```php
scalar|FALSE set ( string $key, scalar $val )
```


### get

**Retrieve value of field**

```php
scalar|FALSE get ( string $key )
```


### clear

**Delete field**

```php
NULL clear ( string $key )
```


### find

**Return items that match key/value pair**

```php
array find ( string $key, mixed $val )
```

### findone

**Return first item that matches key/value pair**

```php
object|FALSE findone ( string $key, mixed $val )
```

### load

**Map current item to matching key/value pair**

```php
array load ( string $key, mixed $val )
```

### dry

**Return TRUE if current item is empty/undefined**

```php
bool dry ( )
```


### count

**Return number of items in basket**

```php
int count ( )
```

### save

**Save current item**

```php
array save ( )
```


### erase

**Erase item matching key/value pair**

```php
bool erase( string $key, mixed $val )
```

### reset

**Reset cursor**

```php
NULL reset ( )
```

### drop

**Empty basket**

```php
NULL drop ( )
```

### copyfrom

**Hydrate item using hive array variable**

```php
NULL copyfrom ( string $key )
```


### copyto

**Populate hive array variable with item contents**

```php
NULL copyto ( string $key )
```


### checkout

**Check out basket contents**

```php
array checkout( )
```

This method returns all items and clears the whole basket.