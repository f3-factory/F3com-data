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

If you like to create a model class, you might like to wrap it up:

``` php
$f3->set('DB',new DB\SQL('sqlite:db/database.sqlite'));

class User extends \DB\SQL\Mapper {
    public function __construct() {
        parent::__construct( \Base::instance()->get('DB'), 'users' );
    }
}

$user = new User();
$user->load('id = 1');
// etc.
```

## Syntax

### $filter

The `$filter` argument for SQL accepts the following structure:

``` php
// string value for simple where strings
string $whereClause
// array value for parameterized queries
array( string $whereClause, [ string $bindValue1 ], [ string $bindValue2 ], [...] )
```

#### Parameterized Queries

It is recommended to use parameterized queries for all where conditions that may include user input data.
In example with positional parameters:

``` php
array('username = ? and password = ? and deleted = 0','John','acbd18db4cc2f85cedef654fccc4a4d8')
```

Or with named parameters:

``` php
array(
    'username = :user and password = :pass and deleted = 0',
    ':user'=>'John',':pass'=>'acbd18db4cc2f85cedef654fccc4a4d8'
    )
```

<div class="alert alert-info">
Notice: You cannot use a named parameter more than once in a query. Due to a PDO limitation you need to create <code>:user1</code> and <code>:user2</code> with same value.
</div>

#### Search

When you use a `LIKE` operator in your where condition, notice that the `%` wildcards do not belong into the where criteria, but goes into the bind parameter like this:

```php
$user->find(array('email LIKE ?','%gmail%')); // returns all users with an email address at GMAIL
```

### $option

The `$option` argument for SQL accepts the following structure:

``` php
array(
    'order' => string $orderClause,
    'group' => string $groupClause,
    'limit' => integer $limit,
    'offset' => integer $offset
    )
```

i.e:

``` php
array(
    'order' => 'score DESC, team_name ASC',
    'group' => 'score, player',
    'limit' => 20,
    'offset' => 0
    )
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

#### Virtual Fields

If you set a new value to an empty / not hydrated mapper, you create a virtual field on it. This way you can add some aggregate functions to your query:

``` php
$scores = new Scores();
$scores->sum_score = 'SUM(score)';
$scores->avg_score = 'AVG(score)';
$scores->load(null,array('group'=>'player_id'));
echo $scores->sum_score; // returns the sum of all scores made by player_id
echo $scores->avg_score; // returns the avarage score of that player
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
$mapper->select( string $fields, [ string|array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ]); array
```


### find

**Return records that match criteria**

``` php
$mapper->find([ string|array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ]); array
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
