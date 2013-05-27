# Cursor

This Cursor class is an abstract foundation of an [Active Record](http://en.wikipedia.org/wiki/Active_Record) implementation, that is used by all F3 Data Mapper.

Have a look at the [SQL Mapper](sql-mapper), [Mongo Mapper](mongo-mapper) or [JIG Mapper](jig-mapper) page to get to know about how to create and use them with their own functions and the follwing described below.


Namespace: `\DB` <br/>
File location: `lib/db/cursor.php`

---

## Cursor Methods

The following methods are available for all Data Mappers. Even if the filter and option syntax may differ from one mapper to another,
the behaviour of these functions are the same. So imagine you have this data in your DB table, when reading the next examples.

<table class="table table-bordered table-condensed table-striped">
    <tr>
        <th>ID</th>
        <th>title</th>
        <th>text</th>
        <th>author</th>
    </tr>
    <tr>
        <td>1</td>
        <td>iPhone 5S is coming</td>
        <td>lorem ipsun</td>
        <td>1</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Once upon a time</td>
        <td>there was a dragon.</td>
        <td>0</td>
    </tr>
    <tr>
        <td>3</td>
        <td>F3 for the win</td>
        <td>get it now</td>
        <td>49</td>
    </tr>
    <tr>
        <td>4</td>
        <td>untitled</td>
        <td>foo bar</td>
        <td>25</td>
    </tr>
</table>


### load

**Map to first record that matches criteria**

``` php
$mapper->load([ strgin|array $filter = NULL ], [ array $options = NULL ]); array|false
```

The `load` method hydrates the mapper object with records. You can define a `$filter` to load only records that matches your criterias.
You'll find detailed descriptions about the `$filter` and `$option` syntax on the mapper specific pages.

If you do not specify any filter, than the first occuring record will be loaded:

``` php
$mapper->load();
echo $mapper->title; // iPhone 5S is coming
```

The Cursor class extends the Magic class, which implement [ArrayAccess](http://php.net/manual/en/class.arrayaccess.php).
This way you are able to access your data fields like object properties and array keys:

``` php
echo $mapper->title; // iPhone 5S is coming
echo $mapper['text']; // lorem ipsun
```


### next

**Map next record**

``` php
$mapper->next(); mixed
```

When the mapper object is hydrated, an internal cursor pointer points to a single record. To move the pointer forward to another record, you can use this method.

``` php
$mapper->load();
echo $mapper->title; // iPhone 5S is coming
$mapper->next();
echo $mapper->title; // Once upon a time
```

### prev

**Map previous record**

``` php
$mapper->prev() mixed
```


### first

**Move pointer to first record in cursor**

``` php
$mapper->first(); mixed
```

### last

**Move pointer to last record in cursor**

``` php
$mapper->last(); mixed
```

### skip

**Map to n-th record relative to current cursor position**

``` php
$mapper->skip([ int $ofs = 1 ]); mixed
```

### dry

**Return TRUE if current cursor position is not mapped to any record**

``` php
$mapper->dry(); bool
```

This is some kind of exist function. It returns TRUE if there is an active hydrated record on the current cursor position.

The next example loops through all loaded records and stops when the current cursor pointer is dry (not hydrated);

``` php
$mapper->load();
while(!$mapper->dry()) {
    echo $mapper->title;
    $mapper->next();
}
```

### findone

**Return first record (mapper object) that matches criteria**

``` php
$mapper->findone([ string|array $filter = NULL], [ array $options = NULL ], [ int $ttl = 0 ]); object|false
```

If you only want to process a single entity in your business logic, it is helpful to only load that single record too.


### paginate

**Return a subset of records with additional pagination information**

``` php
$mapper->paginate([ int $pos = 0 ], [ int $size = 10 ], [ string|array $filter = NULL ], [ array $options = NULL ]); array
```

This method returns an array containing a subset of records that are matching the `$filter` criterias,
the total number of records in superset, the number of subsets available and actual subset position.

In example:

``` php
$result = $mapper->paginate(0, 2);
/*
array(4) {
  ["subset"] => array(2) {
        [0] => mapper object, #ID: 1, title: iPhone 5S is coming
        [1] => mapper object, #ID: 2, title: Once upon a time
      }
  ["total"] => int(10)
  ["count"] => float(5)
  ["pos"] => int(0)
}
*/

$result = $mapper->paginate(1, 2);
/*
array(4) {
  ["subset"] => array(2) {
        [0] => mapper object, #ID: 3, title: F3 for the win
        [1] => mapper object, #ID: 4, title: untitled
      }
  ["total"] => int(10)
  ["count"] => float(5)
  ["pos"] => int(1)
}
*/

```


### save

**Save mapped record**

``` php
$mapper->save(); mixed
```

This method determines if the record should be updated or inserted as new entry.

### erase

**delete current record**

``` php
$mapper->erase(); null
```

### reset

**Reset cursor**

``` php
$mapper->reset(); null
```


## Abstract Methods

The following methos must be implemented by all extending mapper classes to work properly.

### find

**Return records (array of mapper objects) that match criteria**

``` php
$mapper->find([ string|array $filter = NULL ], [ array $options = NULL ]); array
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