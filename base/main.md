# Base

The Base class represents the framework core. It contains everything you need to run a simple application.
The file `base.php` also packages the [Cache](cache), [Prefab](prefab), [View](view), [ISO](iso) and [Registry](registry) classes.
Feel free to remove all other files in the `lib/`-directory, if all you need are the basic features provided by this package.

Namespace: `\` <br/>
File location: `lib/base.php`

---

## The Hive

The hive is all about working with framework variables.

### set
**Bind value to hive key**

``` php
$f3->set ( string $key, mixed $val, [ int $ttl = 0 ]); mixed
```

setting framework variables

``` php 
$f3->set('a',123); // a=123, integer 
$f3->set('b','c'); // b='c', string 
$f3->set('c','whatever'); // c='whatever', string 
$f3->set('d',TRUE); // d=TRUE, boolean
```

setting arrays

``` php
$f3->set('hash',array( 'x'=>1,'y'=>2,'z'=>3 ) ); 
// dot notation is also possible 
$f3->set('hash.x',1); 
$f3->set('hash.y',2); 
$f3->set('hash.z',3);
```

If the `$ttl` parameter is > 0, and the CACHE variable is enabled, the specified variable will be cached. You can cache strings, arrays and all other types - even complete objects. `get()` will load them automatically from Cache.

``` php
// cache string                         
$f3->set('simplevar','foo bar 1337', 3600); // cache for 1 hour
//cache big computed arrays
$f3->set('fruits',array(
    'apple',
    'banana',
    'peach',
), 3600);
// cache objects
$f3->set('myClass1', new myClass('arg1'), 3600);
```

### sync
**Sync PHP global with corresponding hive key**

``` php
$f3->sync( string $key ); array
```



### ref

**Get hive key reference/contents**

``` php
&$f3->ref( string $key, [ bool $add = true ]); mixed
```

Usage:

``` php
$f3->set('name','John');
$b = &$f3->ref('name');
$b = 'Chuck';
$f3->get('name'); // Chuck
```

You can also add non-existent hive keys, array elements, and object properties by default.

``` php

$new = &$f3->ref('newVar');
$new = 'hello world';
$f3->get('newVar'); // hello world

$new = &$f3->ref('newObj->name');
$new = 'Sheldon';
$f3->get('newObj')->name; // Sheldon
$f3->get('newObj->name'); // Sheldon

$a = &$f3->ref('hero.name');
$a = 'Chuck';
// or
$b = &$f3->ref('hero');
$b['name'] = 'Chuck';
$f3->get('hero'); // array ('name' => 'Chuck')
```

If the `$add` argument is `false`, it just returns the read-only hive key contents. This behaviour is used by get().



### exists
**Return TRUE if hive key is not empty**

``` php
$f3->exists( string $key ); bool
```

Usage:

``` php
$f3->set('foo','value');

$f3->exists('foo'); // true
$f3->exists('bar'); // false

$f3->exists('COOKIE.userid');
$f3->exists('SESSION.login');
$f3->exists('POST.submit');
```

The exists function also checks the Cache backend, if the key was not found in the hive.

<div class="alert alert-info"><strong>Notice:</strong> If you check the existence of a SESSION key, the session get started automatically.</div>



### clear
**Unset hive key**

``` php
$f3->clear( string $key ); void
```

If you want to remove a hive key, you can clear it like this:

``` php
$f3->clear('foobar');
$f3->clear('myArray.param1'); // removes key `param1` from array `myArray`
```

If the given hive key was cached before, it will be cleared from cache too.

Some more special usages:

``` php
$f3->clear('SESSION'); // destroys the user SESSION
$f3->clear('COOKIE.foobar'); // removes a cookie
$f3->clear('CACHE'); // clears all cache contents
```

<div class="alert alert-info"><strong>Notice:</strong> Clearing all cache contents at once is not supported for the XCache cache backend</div>



### mset
**Multi-variable assignment using associative array**

``` php
$f3->mset(array $vars, [ string $prefix = ''], [ integer $ttl = 0 ]); void
```

Usage:

``` php
$f3->mset(array(
    'var1'=>'value1',
    'var2'=>'value2',
    'var3'=>'value3',
));

$f3->get('var1'); // value1
$f3->get('var2'); // value2
$f3->get('var3'); // value3
```

You can append all key names using the `$prefix` argument.

``` php
$f3->mset(array(
    'var1'=>'value1',
    'var2'=>'value2',
    'var3'=>'value3',
),
'pre_');

$f3->get('pre_var1'); // value1
$f3->get('pre_var2'); // value2
$f3->get('pre_var3'); // value3
```

To cache all vars, set a positive numeric integer value to `$ttl` in seconds.



### hive
**Publish hive contents.**

``` php
$f3->hive(); array
```



### copy
**Copy contents of hive variable to another**

``` php
$f3->copy( string $src, string $dst); mixed
```

Usage:

``` php
$f3->set('var_1','value123');
echo $f3->copy('var_1','foo'); // "value123"
echo $f3->get('foo'); // "value123"
```

Returns writable `$dst` reference.



### concat
**Concatenate string to hive string variable**

``` php
$f3->concat( string $key, string $val ); void
```

Usage:

``` php
$f3->set('var','hello');
echo $f3->concat('var',' world'); // hello world
echo $f3->get('var'); // hello world
```

Returns writable `$key` reference.



### flip
**Swap keys and values of hive array variable**

``` php
$f3->flip( string $key ); array
```

Usage:

``` php
$f3->set('data', array(
    'foo1' => 'bar1',
    'foo2 '=> 'bar2',
    'foo3' => 'bar3',
));
$f3->flip('data');

print_r($f3->get('data'));
/* output:
Array
(
    [bar1] => foo1
    [bar2] => foo2
    [bar3] => foo3
)
*/
```



### push
**Add element to the end of hive array variable**

``` php
$f3->push( string $key, mixed $val ); mixed
```

Usage:

``` php
$f3->set('fruits',array(
    'apple',
    'banana',
    'peach',
));
$f3->push('fruits','cherry');

print_r($f3->get('fruits'));
/* output:
Array
(
    [0] => apple
    [1] => banana
    [2] => peach
    [3] => cherry
)
*/
```



### pop
**Remove last element of hive array variable**

``` php
$f3->pop( string $key ); mixed
```

Usage:

``` php
$f3->set('fruits',array(
	'apple',
	'banana',
	'peach'
));
$f3->pop('fruits'); // returns "peach"

print_r($f3->get('fruits'));
/* output:
Array
(
    [1] => apple
    [2] => banana
)
*/
```



### unshift
**Add element to the beginning of hive array variable**

``` php
$f3->unshift( string $key, string $val ); mixed
```

Usage:

``` php
$f3->set('fruits',array(
	'apple',
	'banana',
	'peach'
));
$f3->unshift('fruits','cherry');

print_r($f3->get('fruits'));
/* output:
Array
(
    [0] => cherry
    [1] => apple
    [2] => banana
    [3] => peach
)
*/
```



### shift
**Remove first element of hive array variable**

``` php
$f3->shift( string $key ); mixed
```

Usage:

``` php
$f3->set('fruits',array(
	'apple',
	'banana',
	'peach'
));
$f3->shift('fruits'); // returns "apple"

print_r($f3->get('fruits'));
/* output:
Array
(
    [0] => banana
    [1] => peach
)
*/
```



## Encoding & Conversion



### fixslashes
**Convert backslashes to slashes**

``` php
$f3->fixslashes( string $str ); string
```



### split
**Split comma-, semi-colon, or pipe-separated string**

``` php
$f3->split( string $str ); array
```

Usage:

``` php
$data = 'value1,value2;value3|value4';
print_r($f3->split($data));
/* output:
Array
(
    [0] => value1
    [1] => value2
    [2] => value3
    [3] => value4
)
*/
```



### stringify
**Convert PHP expression/value to compressed exportable string**

``` php
$f3->stringify( mixed $arg ); string
```



### csv
**Flatten array values and return as CSV string**

``` php
$f3->csv( array $args ); string
```

Usage:

``` php
$data = array('value1','value2','value3');
$f3->csv($data); // returns: "'value1','value2','value3'"
```



### camelcase
**Convert snake_case string to camelCase**

``` php
$f3->camelcase( string $str ); string
```



### snakecase
**Convert camelCase string to snake_case**

``` php
$f3->snakecase( string $str ); string
```



### sign
**Return -1 if specified number is negative, 0 if zero,	or 1 if the number is positive**

``` php
$f3->sign( mixed $num ); integer
```



### hash
### base64
### encode
### decode
### scrub
### esc
### raw
### serialize
### unserialize

## Localisation

### format
### language
### lexicon

## Routing

### mock
### route
### reroute
### map
### run
### call
### chain
### relay

---
## File System
### mutex
### read
### write

## Debug

## Misc

### status
### expire
### error
### blacklisted
### config
### highlight
### dump
### autoload
### unload
### instance
