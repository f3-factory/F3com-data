# Jig Mapper

The Jig Object-Document-Mapper is an implementation of the abstract [Active Record Cursor class](cursor). Have a look into it for additional method descriptions.

Namespace: `\DB\Jig` <br/>
File location: `lib/db/jig/mapper.php`

---

## Instantiation

To use the Jig ODM, [create a valid Jig DB Connection](jig#constructor) and follow this example:

``` php
$mapper = new \DB\Jig\Mapper(\DB\Jig $db, string $file)
```

If you like to create a model class, you might like to wrap it up:

``` php
$f3->set('DB',new DB\Jig('data/'));

class User extends \DB\Jig\Mapper {
    public function __construct() {
        parent::__construct( \Base::instance()->get('DB'), 'users.json' );
    }
}

$user = new User();
$user->load(array('@_id = ?','515c570f28de6'));
// etc.
```

<div class="alert alert-info">
Notice: The primary key of Jig documents is named <code>_id</code>.
</div>


## Syntax

### $filter

The `$filter` argument for Jig accepts the following structure:

``` php
// array value for parameterized queries
array( string $expr, [ string $bindValue1 ], [ string $bindValue2 ], [...] )
```

The `$expr` part must contain a valid code expression, where all mapper fields are prefixed by a `@`-char. You can bind values to them with positional or named tokens.
Here is an example:

``` php
// positional tokens
array('@username = ? and @password = ?','John','acbd18db4cc2f85cedef654fccc4a4d8')
// named tokens
array('@username = :user and @password = :pw',':user'=>'John',':pw'=>'acbd18db4cc2f85cedef654fccc4a4d8')
```



**Important:** Jig is a schema-less document mapper, so the fields of a document may vary from one record to another.
To create a valid `$expr` string, keep in mind to add additional field existence checks to prevent running into weird undefined variable errors.
Adding some checks for that can be achieved easiely by adding some `isset` conditions:

``` php
array(
    '(isset(@username) && @username == ?) && (isset(@password) && @password = ?)',
    'John','acbd18db4cc2f85cedef654fccc4a4d8'
    )
```

<div class="alert alert-info">
Info: You can use all common comparision operators in your condition and a single <code>=</code> works too.
</div>

#### Search

The best way to search in Jig is to use some `preg_match` conditions:

```php
$userList = $user->find(array('(isset(@email) && preg_match(?,@email))','/gmail/'));
// returns all users with an email address that contains GMAIL

// ends with gmail.com => /gmail\.com$/
// starts with john  => /^john/
```

The equivalent of a SQL `IN` operator goes like this:

```php
$user->find(array('in_array('_id',array(1,2,3))'));
```

If your document uses an array field, i.e. tags, you can find all posts by tag with just switching the in_array parameters: 

```php
$post->find(array('isset(@tags) && in_array("fat-free",@tags)'));
```

### $option

The `$option` argument for Jig accepts the following structure:

``` php
array(
    'order' => string $orderClause,
    'limit' => integer $limit,
    'offset' => integer $offset
    )
```

i.e:

``` php
array(
    'order' => 'score SORT_DESC, team_name SORT_ASC',
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

**Clear value of field**

``` php
$mapper->clear( string $key ); NULL
```


### cast

**Return fields of mapper object as an associative array**

``` php
$mapper->cast([ object $obj = NULL ]); bool
```


### token

**Convert tokens in string expression to variable names**

``` php
$mapper->token( string $str ); string
```


### find

**Return records that match criteria**

``` php
$mapper->find([ string|array $filter = NULL ],[ array $options = NULL ],[ int $ttl = 0 ], [ bool $log=TRUE ]); array
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

