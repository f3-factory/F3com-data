# Base

The Base class represents the framework core. It contains everything you need to run a simple application.
This file also packages the [Cache](cache), [Prefab](prefab), [View](view), [ISO](iso) and [Registry](registry) class.
Feel free to remove all other files in the `lib/`-directory, if you're fine with the features provided by this Package.

Namespace: `\` <br/>
File location: `lib/base.php`

---

## The Hive

The hive is all about working with framwork variables.

### set
**Bind value to hive key**

``` php
$f3->set ( string $key, mixed $val, [ int = 0 $ttl ]); mixed
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

objects properties

``` php
$f3->set('a',new \stdClass);
$f3->set('a->hello','world');
```


If the `$ttl` parameter is set and the F3 CACHE is enabled, the var is going to be cached.
You can cache strings, arrays and all other types - even complete objects.
When you access them with get(), they will be loaded automatically from Cache, if present and not expired.

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

It is also possible to set php globals using GET, POST, COOKIE or SESSION.

<div class="alert alert-info"><strong>Notice:</strong> If you set or access a key of SESSION, the session gets started automatically. There's no need for you to do it by yourself.</div>

The framework has some own [system variables] (system-variables). You can change them to get a framework behaviour, that fits best for your app.


### get
**Retrieve contents of hive key**

``` php
$f3->get( string $key, [ string|array = NULL $args ]);mixed
```

to get a previously saved framework var, use:

``` php
$f3->set('myVar','hello world');
$f3->get('myVar'); // returns the string 'hello world'
```

<div class="alert alert-info"><strong>Notice:</strong> F3 tries to load the var from Cache when using get(), if the var was not defined at runtime before and caching is enabled.</div>

Accessing arrays is easy. You can also use the JS dot notation, which makes it much easier to read and write.

``` php
$f3->set('myarray', array(
        0 => 'value_0',  
        1 => 'value_1',
        'bar' => 123,
        'foo' => 'we like candy',
        'baz' => 4.56,
    ));

$f3->get('myarray[0]'); // value_0
$f3->get('myarray.1'); // value_1
$f3->get('myarray[bar]'); // 123
$f3->get('myarray.foo'); // we like candy
$f3->get('myarray["baz"]'); // 4.56
```


### sync

Sync PHP global with corresponding hive key

``` php
$f3->sync( string $key); array
```

### ref

Get hive key reference/contents

``` php
&$f3->ref( string $key, [ bool = TRUE $add ]); mixed
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

Return TRUE if hive key is not empty

``` php
$f3->exists(string $key); bool
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