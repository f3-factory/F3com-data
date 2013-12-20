# Cursor

This Cursor class is an abstract foundation of an [Active Record](http://en.wikipedia.org/wiki/Active_Record "Wikipedia :: Active record pattern") implementation, that is used by all F3 Data Mappers.

Have a look at the [SQL Mapper](sql-mapper), [Mongo Mapper](mongo-mapper) or [JIG Mapper](jig-mapper) page to get to know about how to create and use them with their own functions and the following described below.

---

Namespace: `\DB` <br>
File location: `lib/db/cursor.php`

## Cursor Methods

The following methods are available to all Data Mappers. Even if the filter and option syntax may differ from one mapper to another,
the behaviour of these functions are the same. So assume you have this set of data in your DB table when you'll be reading the examples below.

<table class="table table-bordered table-condensed table-striped">
<tr>
    <th>ID</th>
    <th>title</th>
    <th>text</th>
    <th>author</th>
</tr>
<tr>
    <td>1</td>
    <td>F3 for the win</td>
    <td>Use It Now!</td>
    <td>49</td>
</tr>
<tr>
    <td>2</td>
    <td>Once upon a time</td>
    <td>there was a dragon.</td>
    <td>2</td>
</tr>
<tr>
    <td>3</td>
    <td>Barbar the Foo</td>
    <td>foo bar</td>
    <td>25</td>
</tr>
<tr>
    <td>4</td>
    <td>untitled</td>
    <td>lorem ipsum</td>
    <td>8</td>
</tr>
</table>

### load

**Map to first record that matches criteria**

```php
array|false load ( [ string|array $filter = NULL [, array $options = NULL ]] )
```

The `load` method hydrates the mapper object with records. You can define a `$filter` to load only records that matches your criteria.
You can find detailed descriptions about the `$filter` and `$option` syntax on the mapper specific pages: [Jig Mapper](jig-mapper#$filter), [Mongo Mapper](mongo-mapper#$filter) and [SQL Mapper](sql-mapper#$filter).

Let's start with a simple example where we do not specify any filter: the first record will be loaded:

```php
$mapper->load();  // by default, loads the 1st record
echo $mapper->title; // displays 'F3 for the win'
```

<div class="alert alert-warning">
    <strong>Important:</strong><br>
    Make sure there is enough memory for the result set returned by this function. Attempting to <code>load</code> (without filtering) from a huge database might go beyond PHP's <code>memory_limit</code>.
</div>

The `Cursor` class extends the `Magic` class, which implements the [ArrayAccess interface](http://php.net/manual/en/class.arrayaccess.php "PHP Manual :: ArrayAccess").
This way you are able to access your data fields like object properties and array keys:

```php
echo $mapper->title; // 'F3 for the win'
echo $mapper['text']; // 'Use It Now!'
echo $mapper->get('author'); // 49
```

### next

**Map next record**

```php
mixed next ( )
```

When a mapper object is hydrated, its internal cursor pointer points to a single record. To move the pointer forward to the next record, you use this method.

```php
$mapper->load();
echo $mapper->title; // 'F3 for the win'
$mapper->next();
echo $mapper->title; // 'Once upon a time'
```

Internally, the `Cursor` class fetches all records from the underlying database matching the `$filter` specified when calling `load()`. Any attempt to navigate to the next, previous, first or last record will just move the internal pointer, thereby reducing disk I/O.

### prev

**Map previous record**

```php
mixed prev ( )
```

### first

**Move pointer to first record in cursor**

```php
mixed first ( )
```

### last

**Move pointer to last record in cursor**

```php
mixed last ( )
```

### skip

**Map to n-th record relative to current cursor position**

```php
mixed skip ( [ int $ofs = 1 ] )
```

### dry

**Return `TRUE` if current cursor position is not mapped to any record**

```php
bool dry ( )
```

This is some kind of "empty" function. It returns `TRUE` if the cursor is not mapped to any database record, even if records have been `load`ed.

The next example snippet loops through all loaded records and stops when the current cursor pointer is dry (not hydrated) :

```php
$mapper->load();  // by default, loads the 1st record

while ( !$mapper->dry() ) {  // gets dry when we passed the last record
    echo $mapper->title;
    // moves forward even when the internal pointer is on last record
    $mapper->next();
}
```

### findone

**Return first record (mapper object) that matches criteria**

```php
object|false findone ( [ string|array $filter = NULL [, array $options = NULL [, int $ttl = 0 ]]] )
```

Use this method if you only want to process a single entity in your business logic. It is helpful to only `load` that single record.

### paginate

**Return a subset of records with additional pagination information**

```php
array paginate ( [ int $pos = 0 [, int $size = 10 [, string|array $filter = NULL [, array $options = NULL ]]]] )
```

This method returns an array containing a subset of records that are matching the `$filter` criterias,
the total number of records in superset, the number of subsets available and actual subset position.

For example:

```php
$result = $mapper->paginate(0, 2);
/*
array(4) {
  ["subset"] => array(2) {
        [0] => mapper object, #ID: 1, title: F3 for the win
        [1] => mapper object, #ID: 2, title: Once upon a time
        [2] => mapper object, #ID: 3, title: Barbar the Foo
      }
  ["total"] => int(5)
  ["count"] => float(3)
  ["pos"] => int(0)
}
*/

$result = $mapper->paginate(1, 2);
/*
array(4) {
  ["subset"] => array(2) {
        [0] => mapper object, #ID: 3, title: Barbar the Foo
        [1] => mapper object, #ID: 4, title: untitled
      }
  ["total"] => int(5)
  ["count"] => float(2)
  ["pos"] => int(1)
}
*/

```

The subset is the array of mapper objects returned from find(), `total` is the sum of all records for all pages, `count` is the number of records for the current page and `pos` gives you the current subset cursor position ( it's the page number - 1).


### save

**Save mapped record**

```php
mixed save ( )
```

This method saves data to the database. The `Cursor` class automatically determines if the record should be [updated](cursor#update) or [inserted](cursor#insert) as a new entry, based on the return value of `dry`.

### erase

**Delete current record**

```php
null erase ( )
```

### reset

**Reset/dehydrate the cursor**

```php
null reset ( )
```


## Abstract Methods

The following methods must be implemented by all extending mapper classes to work properly.

### find

**Return records (array of mapper objects) that match criteria**

```php
array find ( [ string|array $filter = NULL [, array $options = NULL ]] )
```


### insert

**Insert a new record**

```php
array insert ( )
```


### update

**Update the current record**

```php
array update ( )
```
