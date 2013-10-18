# Mongo Mapper

The Mongo Object-Document-Mapper (ODM) is an implementation of the abstract [Active Record Cursor class](cursor). Have a look into it for inherited method descriptions.

Namespace: `\DB\Mongo` <br/>
File location: `lib/db/mongo/mapper.php`

---

## Instantiation

To use the Mongo ODM, [create a valid MongoDB Connection](mongo#constructor) and follow this example:

``` php
$mapper = new \DB\Mongo\Mapper(\DB\Mongo $db, string $collection);
```

If you like to create a model class, you might like to wrap it up:

``` php
$f3->set('DB', new \DB\Mongo('mongodb://localhost:27017','fatfree');

class User extends \DB\SQL\Mongo {
    public function __construct() {
        parent::__construct( \Base::instance()->get('DB'), 'users' );
    }
}

$user = new User();
$user->load(array('_id' => new \MongoId('50ecaa466afa2f8c1c000004')));
// etc.
```

## Syntax

### $filter

The `$filter` argument for Mongo accepts the common mongo find array,
see [db.collection.find reference](http://docs.mongodb.org/manual/reference/method/db.collection.find/)
and [query-documents tutorial](http://docs.mongodb.org/manual/tutorial/query-documents/).

``` php
array([ array $find ]);
```


### $option

The `$option` argument for Mongo accepts the following structure:

``` php
array(
    'group' => array (
        'keys',
        'initial',
        'reduce',
        'finalize'
    ),
    'order' => string $order,
    'limit' => integer $limit,
    'offset' => integer $offset
    )
```

For more details about `group`, go to the [db.collection.group reference](http://docs.mongodb.org/manual/reference/method/db.collection.group/).
And for more about `order`, and a look at the [cursor.sort reference](http://docs.mongodb.org/manual/reference/method/cursor.sort/).


## Methods

### exists

**Return TRUE if field is defined**

``` php
$mapper->exists( string $key ); bool
```


### set

**Assign value to field**

``` php
$mapper->set( string $key, scalar $val ); scalar|FALSE
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
$mapper->get( string $key ); scalar|FALSE
```


### clear

**Delete field**

``` php
$mapper->clear( string $key ); NULL
```


### cast

**Return fields of mapper object as an associative array**

``` php
$mapper->cast([ object $obj = NULL ]); array
```


### select

**Build query and execute**

``` php
$mapper->select( string $fields, [ array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ]); array
```


### find

**Return records that match criteria**

``` php
$mapper->find([ array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ]); array
```


### count

**Count records that match criteria**

``` php
$mapper->count([ array $filter = NULL ],[ int $ttl = 0 ]); int
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
$mapper->erase([ array $filter = NULL ]); bool
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
