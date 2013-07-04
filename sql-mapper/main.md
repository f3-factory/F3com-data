# SQL Mapper

The SQL Object-Relational-Mapper is an implementation of the abstract [Active Record Cursor class](cursor).

Namespace: `\DB\SQL` <br/>
File location: `lib/db/sql/mapper.php`

---

## Instantiation

To use the SQL ORM, [create a valid SQL DB Connection](sql#constructor) and follow this example:

``` php
$mapper = new \DB\SQL\Mapper(\DB\SQL $db, string $table, [ int $ttl = 60 ])
```


## Methods

### exists

**Return TRUE if field is defined**

``` php
$mapper->exists( string $key ); bool
```


### set

**Assign value to field**

``` php
$mapper->set( string $key, scalar $val ); scalar
```

This class also takes advantage from the Magic and ArrayAccess class implementation.
This way you can also set and get variable with direct access like this:

``` php
$mapper->foo = 'bar';
$mapper['foo'] = 'bar';
```


### get

**Retrieve value of field**

``` php
$mapper->get( string $key ); scalar
```


### clear

**Clear value of field**

``` php
$mapper->clear( string $key ); NULL
```


### type

**Get PHP type equivalent of PDO constant**

``` php
$mapper->type( string $pdo ); string
```


### value

**Cast value to PHP type**

``` php
$mapper->value( string $type, scalar $val ); scalar
```


### cast

**Return fields of mapper object as an associative array**

``` php
$mapper->cast([ object $obj = NULL ]); bool
```


### select

**Build query string and execute**

``` php
$mapper->select( string $fields, [ array $filter = NULL],[ array $options = NULL ],[ int $ttl = 0 ]); array
```

### find

**Return records that match criteria**

``` php
$mapper->find([ array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ]); array
```


### count

**Count records that match criteria**

``` php
$mapper->count([ string|array $filter = NULL ]); int
```

### insert
**Insert new record**

``` php
$mapper->insert(); array
```


### update
**Update current record**

``` php
$mapper->update(); array
```


### erase
**Delete current record**

``` php
$mapper->erase([ string|array $filter = NULL ]); int
```


### copyfrom
**Hydrate mapper object using hive array variable**

``` php
$mapper->copyfrom( string $key ); NULL
```


### copyto
**Populate hive array variable with mapper fields**

``` php
$mapper->copyto( string $key ); NULL
```


### schema
**Returns the table schema**

``` php
$mapper->schema();
```

See [SQL#Schema](sql#schema) for additional information.
