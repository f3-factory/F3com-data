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


### set


### get


### clear


### type


### value


### cast


### select


### find


### count


### skip


### insert


### update


### erase


### reset


### copyfrom


### copyto


### schema
**Returns the table schema**

``` php
$mapper->schema();
```


