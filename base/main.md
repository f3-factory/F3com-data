# Base

The Base class represents the framework core. It contains everything you need to run a simple application.
The file `base.php` also packages the [Cache](cache), [Prefab](prefab), [View](view), [ISO](iso) and [Registry](registry) class.
Feel free to remove all other files in the `lib/`-directory, if you're fine with the features provided by this Package.

---

## The Hive

The hive is all about working with framwork variables.

### set
**Bind value to framework variable**

``` php
$f3->set ( string $key, mixed $value, [ int = 0 $ttl ]); mixed
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

If the `$tll` parameter is > 0, and the F3 CACHE is enabled, the var is going to be cached.
You can cache strings, arrays and all other types - even complete objects, get() will load them automatically from Cache.

``` php
// cache string                         
$f3->set('simplevar','foo bar 1337', 3600); // Cache for 1 hour
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

### cut

### ref

### exists

### set

### get

### clear

### mset

### hive

### copy

### concat

### flip

### push

### pop

### unshift

### shift

### split

## Encoding & Conversion

### fixslashes
### stringify
### csv
### camelcase
### snakecase
### sign
### hash
### base64
### encode
### decode
### scrub
### esc
### raw
### serialize
### unserialize

### Localisation

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