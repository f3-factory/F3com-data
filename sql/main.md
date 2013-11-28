# SQL

The SQL class provides a lightweight, consistent interface for accessing SQL databases in PHP. It is a superset of the [PDO class](http://www.php.net/manual/en/class.pdo.php).

Namespace: `\DB` <br/>
File location: `lib/db/sql.php`

---

## Constructor

```php
$db=new \DB\SQL(string $dsn, [ string $user = NULL], [ string $pw = NULL], [ array $options = NULL]);
```

For example, to connect to a MySQL database, the syntax looks like:

``` php
$db=new \DB\SQL('mysql:host=localhost;port=3306;dbname=mysqldb','username','password');
```

While connecting to a SQLite database looks like:

``` php
$db=new \DB\SQL('sqlite:/path/to/db.sqlite');
```

The 4th parameter allows to set additional PDO attributes:

``` php
$options=array(
  \PDO::ATTR_ERRMODE => \PDO::ERRMODE_EXCEPTION, // generic attribute
  \PDO::MYSQL_ATTR_COMPRESS => TRUE, // MySQL-specific attribute
);
$db=new \DB\SQL('mysql:host=localhost;port=3306;dbname=mysqldb','username','password',$options);
```

This is a list of links to DSN connection details of all currently supported engines in the the SQL layer:

* [mysql](http://www.php.net/manual/en/ref.pdo-mysql.php): MySQL 5.x
* [sqlite](http://www.php.net/manual/en/ref.pdo-sqlite.connection.php): SQLite 3 and SQLite 2
* [pgsql](http://www.php.net/manual/en/ref.pdo-pgsql.connection.php): PostgreSQL
* [sqlsrv](http://www.php.net/manual/en/ref.pdo-sqlsrv.connection.php): Microsoft SQL Server / SQL Azure
* [mssql, dblib, sybase](http://www.php.net/manual/en/ref.pdo-dblib.connection.php): FreeTDS / Microsoft SQL Server / Sybase
* [odbc](http://www.php.net/manual/en/ref.pdo-odbc.connection.php): ODBC v3
* [oci](http://www.php.net/manual/en/ref.pdo-oci.connection.php): Oracle

## Methods

### driver

**Returns the SQL driver name**

``` php
echo $db->driver(); // mysql
```

### version

**Returns the server version**

``` php
echo $db->version(); // 5.1.51
```

### name

**Returns the database name**

```php
echo $db->name(); // mysqldb
```

### schema

**Retrieve schema of SQL table**

```php
$db->schema(string $table, [ int $ttl=0]); array
```

For example:

```php
// CREATE TABLE mytable (id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, name varchar(256) NULL DEFAULT 'none')
$columns=$db->schema('mytable');
var_dump($columns);
// outputs
array(2) {
  ["id"]=>array(5) {
    ["type"]=>string(7) "INTEGER"
    ["pdo_type"]=>int(1) // \PDO::PARAM_INT
    ["default"]=>NULL
    ["nullable"]=>bool(false)
    ["pkey"]=>bool(true)
  }
  ["name"]=>array(5) {
    ["type"]=>string(12) "varchar(256)"
    ["pdo_type"]=>int(2) // \PDO::PARAM_STR
    ["default"]=>string(6) "'none'"
    ["nullable"]=>bool(true)
    ["pkey"]=>bool(false)
  }
}
```

### exec

**Execute a SQL command**

```php
$db->exec(string|array $commands, [ array $args = NULL], [ int $ttl = 0 ]); mixed
```

This method execute one or more SQL statements and returns either the resulting rows (for SELECT, CALL, EXPLAIN, PRAGMA, SHOW statements) or the number of affected rows (for INSERT, DELETE, UPDATE statements).

For example, consider the following table `mytable`:

<table class="table table-bordered table-condensed table-striped">
  <thead>
    <tr><th>id</th><th>name</th></tr>
  </thead>
  <tbody>
    <tr><td>1</td><td>Joe</td></tr>
    <tr><td>2</td><td>William</td></tr>
    <tr><td>3</td><td>Jack</td></tr>
    <tr><td>4</td><td>Averell</td></tr>
  </tbody>
</table>

a SELECT statement would return an array of rows:

```php
$rows=$db->exec('SELECT id,name FROM mytable ORDER BY id DESC');
echo count($rows); // outputs 4
foreach($rows as $row)
  echo $row['name'];
// outputs 'Averell,Jack,William,Joe,'
```

while an UPDATE statement would return the number of updated rows:

```php
echo $db->exec('UPDATE mytable SET id=id+10'); // outputs 4
```

#### Parameterized queries

The `exec()` method's 2nd argument is there to pass arguments safely (cf. [here](databases#parameterized-queries)).

For example, instead of writing:

```php
$db->exec('INSERT INTO mytable VALUES(5,\'Jim\')')
```

it is highly encouraged to write:

```php
$db->exec('INSERT INTO mytable VALUES(:id,:name)',array(':id'=>5,':name'=>'Jim'))
```

Here's the equivalent syntax with unnamed placeholders:

```php
$db->exec('INSERT INTO mytable VALUES(?,?)',array(1=>5,2=>'Jim'))
```

and here's the short syntax for a single placeholder:

```php
$db->exec('INSERT INTO mytable(name) VALUES(?)','Jim');
```

#### Query caching

The 3rd argument `$ttl` is used to enable query caching. Set it to your desired time-to-live in seconds and make sure [CACHE](quick-reference#cache) system var is configured.
This way you can speed up your application when processing data that does not change very frequently.

#### Transaction

Several SQL statements can be executed at once, if providing an array of statements. F3 will execute them as transaction, so if one statement fails, the whole query stack is rolled back.

<div class="alert alert-info">
    Be aware that the return value refers to the last executed statement only.
</div>

```php
$result=$db->exec(array(
    'INSERT INTO mytable VALUES(:id,:name)',
    'DELETE FROM mytable'
  ),
  array(
    array(':id'=>6,':name'=>'Bill'),
    NULL
  )
);
echo $result; // outputs 6 (deleted rows)
```

If you need a return value for each statement, then you have to explicitly define a transaction and use `exec()` for each statement (cf. below).

### begin, rollback & commit

**Start, abort or end a SQL transaction**

```php
// state 1: empty table
$db->begin();
$db->exec('INSERT INTO mytable(name) VALUES(?)','Alfred');
$db->exec('INSERT INTO mytable(name) VALUES(?)','Bonnie');
// state 2: table contains 2 rows
$db->rollback();
// state 3: back to state 1
$db->exec('INSERT INTO mytable(name) VALUES(?)','Clyde');
// state 4: table contains 1 row
$db->commit();
// state 5: changes commited
// end of transaction: only Clyde has been inserted to database
```

### count

**Returns the number of rows affected by the last query.**

```php
$db->exec('INSERT INTO mytable(name) VALUES(?)','Alfred');
echo $db->count(); // outputs 1
$db->exec('INSERT INTO mytable(name) VALUES(?)','Bonnie');
echo $db->count(); // outputs 1
$db->exec('SELECT * FROM mytable');
echo $db->count(); // outputs 2
```

### log

**Returns the SQL profiler results**

```php
$db->exec('INSERT INTO mytable(name) VALUES(?)','Clyde');
$db->exec('INSERT INTO mytable(name) VALUES(?)','Don');
$db->exec('INSERT INTO mytable(name) VALUES(?)','Elliott');
echo $db->log();
// outputs:
Mon, 27 May 2013 00:59:57 +0200 (0.1ms) INSERT INTO mytable(name) VALUES('Clyde')
Mon, 27 May 2013 00:59:57 +0200 (0.3ms) INSERT INTO mytable(name) VALUES('Don')
Mon, 27 May 2013 00:59:57 +0200 (0.2ms) INSERT INTO mytable(name) VALUES('Elliott')
```

### uuid

**Returns unique connection identifier hash**

``` php
$db->uuid(); string
```

### type

**Map data type of argument to a PDO constant**

``` php
$db->type( scalar $val ); int
```

### quote

**Quote string**

``` php
$db->quote( string $val, [ int $type = \PDO::PARAM_STR ]); string
```


### quotekey

**Returns quoted identifier name**

``` php
$db->quotekey( string $key ); string
```

This quotes a table or column key name, based on the current database engine. E.g in quotes `page` to `"page"` in sqlite.
