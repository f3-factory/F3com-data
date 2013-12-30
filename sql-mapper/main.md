# SQL Mapper

The SQL Object-Relational-Mapper is an implementation of the abstract [Active Record Cursor class](cursor).


Namespace: `\DB\SQL` <br>
File location: `lib/db/sql/mapper.php`

---

## Instantiation

To use the SQL ORM, [create a valid SQL DB Connection](sql#constructor) and follow this example:

```php
$mapper = new \DB\SQL\Mapper(\DB\SQL $db, string $table [, array|string $fields = NULL [, int $ttl = 60 ]] )
```

The `$fields` argument allows you to specify only the fields you need to map. `$fields` is either an array or a list (according to the F3 function [split](base#split)) of the names of columns to include in the returned schema. Defaulted to all fields.

The 4th argument `$ttl` is a TTL to give the schema detector a hint about how often a SHOW COLUMNS call is issued by the mapper. When `$ttl` != 0, a cache check for previous schema is triggered and if expired or not found, the actual result is saved to the cache backend, provided the [CACHE](quick-reference#cache) system variable is set to `TRUE`.

Now, if you'd like to create a model class, you might like to wrap it up:

```php
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

```php
// string value for simple where strings
string $whereClause
// array value for parameterized queries
array ( string $whereClause [, string $bindValue1 [, string $bindValue2 [, ... ]] )
```

#### Parameterized Queries

It is recommended to use parameterized queries for all `where` conditions that may include user input data.

An example with question mark positional parameters:

```php
array('username = ? and password = ? and deleted = 0','John','acbd18db4cc2f85cedef654fccc4a4d8')
```

And with named parameters (recommended):

```php
array(
	'username = :user and password = :pass and deleted = 0',
	':user'=>'John',':pass'=>'acbd18db4cc2f85cedef654fccc4a4d8'
	)
```

<div class="alert alert-warning">
Workaround: Due to a PDO limitation, you cannot use a named parameter more than once in a query. You need to create e.g. 2 parameters <code>:user1</code> and <code>:user2</code> and pass them  the same value.
</div>

<div class="alert alert-info">
Make Your Choice: You cannot use both named and question mark positional parameter markers within the same SQL statement; pick one or the other parameter style but don't mix.
</div>

##### User-specified data type

To force a bind value to be a specific PDO type, use the following syntax:

```php
array(
	'prize > :prize and active = 1',
	':prize' => array(123, \PDO::PARAM_INT)
	)
```


#### Search

When you use a `LIKE` operator in your `where` condition, notice that the `%` wildcards do not belong into the `where` criteria, but goes into the bind parameter like this:

```php
$user->find(array('email LIKE ?','%gmail%')); // returns all users with an email address at GMAIL
```

### $option

The `$option` argument for SQL accepts the following structure:

```php
array(
	'order' => string $orderClause,
	'group' => string $groupClause,
	'limit' => integer $limit,
	'offset' => integer $offset
	)
```

i.e:

```php
array(
	'order' => 'score DESC, team_name ASC',
	'group' => 'score, player',
	'limit' => 20,
	'offset' => 0
	)
```


## Methods

### exists

**Return TRUE if a given field is defined**

```php
bool exists( string $key )
```


### set

**Assign value to field**

```php
scalar set( string $key, scalar $val )
```

This class also takes advantage from the Magic and ArrayAccess class implementation.
This way you can also set and get variable with direct access like this:

```php
$mapper->foo = 'bar';
$mapper['foo'] = 'bar';
```

#### Virtual Fields

If you set a new value to an empty / not hydrated mapper, you create a virtual field on it. This way you can add some aggregate functions to your query:

```php
$scores = new Scores();
$scores->sum_score = 'SUM(score)';
$scores->avg_score = 'AVG(score)';
$scores->load(null,array('group'=>'player_id'));
echo $scores->sum_score; // returns the sum of all scores made by player_id
echo $scores->avg_score; // returns the avarage score of that player
```

### get

**Retrieve value of field**

```php
scalar get( string $key )
```


### clear

**Clear value of field**

```php
NULL clear( string $key )
```


### type

**Get PHP type equivalent of PDO constant**

```php
string type( string $pdo )
```


### value

**Cast value to PHP type**

```php
scalar value( string $type, scalar $val );
```


### cast

**Return fields of mapper object as an associative array**

```php
bool cast( [ object $obj = NULL ] );
```


### select

**Build query string and execute**

```php
$mapper->select( string $fields, [ string|array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ]); array
```


### find

**Return records that match criteria**

```php
$mapper->find([ string|array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ]); array
```


### count

**Count records that match criteria**

```php
$mapper->count([ string|array $filter = NULL ]); int
```

### insert
**Insert new record**

```php
$mapper->insert(); array
```


### update
**Update current record**

```php
$mapper->update(); array
```


### erase
**Delete current record**

```php
$mapper->erase([ string|array $filter = NULL ]); int
```


### copyfrom
**Hydrate mapper object using hive array variable**

```php
$mapper->copyfrom( string $key ); NULL
```


### copyto
**Populate hive array variable with mapper fields**

```php
$mapper->copyto( string $key ); NULL
```


### schema
**Returns the table schema**

```php
$mapper->schema();
```

See [SQL#Schema](sql#schema) for additional information.
