# Base

The Base class represents the framework core. It contains everything you need to run a simple application.
The file `base.php` also packages the [Cache](cache), [Prefab](prefab), [View](view), [ISO](iso) and [Registry](registry) classes.
Feel free to remove all other files in the `lib/`-directory, if all you need are the basic features provided by this package.

Namespace: `\` <br/>
File location: `lib/base.php`

---

## The Hive

The hive is a memory array to hold your framework variables in a key / value pair. Storing a value in the hive ensures it is globaly available to all classes and methods in your application.

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

objects properties

``` php
$f3->set('a',new \stdClass);
$f3->set('a->hello','world');
```

If the `$ttl` parameter is > 0, and the CACHE framework variable is enabled, the specified variable will be cached for X seconds. You can cache strings, arrays and all other types - even complete objects. `get()` will load them automatically from Cache.

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

The framework has it's own [system variables] (system-variables). You can change them using set() to modify a framework behaviour, for example 

``` php
$f3->set('CACHE', TRUE);
```

Hive keys are case-sensitive. Root hive keys are checked for validity against these valid chars: [ a-z A-Z 0-9 _ ]



### get
**Retrieve contents of hive key**

``` php
$f3->get( string $key, [ string|array $args = NULL ]);mixed
```

to get a previously saved framework var, use:

``` php
$f3->set('myVar','hello world');
echo $f3->get('myVar'); // outputs the string 'hello world'
$local_var = $f3->get('myVar'); // $local_var holds the string 'hello world'
```

<div class="alert alert-info"><strong>Notice:</strong> F3 tries to load the var from Cache when using get(), if the var was not defined at runtime before and caching is enabled.</div>

Accessing arrays is easy. You can also use the JS dot notation, which makes it much easier to read and write.
<!-- special alignment ez 2 read 4 beginners -->

``` php
$f3->set('myarray', 
           array(
                    0 => 'value_0',  
                    1 => 'value_1',
                'bar' => 123,
                'foo' => 'we like candy',
                'baz' => 4.56,
                )
         );

echo $f3->get('myarray[0]'); // value_0
echo $f3->get('myarray.1'); // value_1
echo $f3->get('myarray.bar'); // 123
echo $f3->get('myarray["foo"]'); // we like candy
echo $f3->get('myarray[baz]'); // 4.56, notice alternate use of single, double and no quotes 
``` 




### sync
**Sync PHP global variable with corresponding hive key**

``` php
$f3->sync( string $key ); array
```

Usage:

``` php
$f3->sync('SESSION');
// ensures php global var SESSION is the same as F3 framework variable SESSION
```

<div class="alert alert-info">
    F3 will automatically sync the following PHP globals:
    <b>GET</b>, <b>POST</b>, <b>COOKIE</b>, <b>REQUEST</b>, <b>SESSION</b>, <b>FILES</b>, <b>SERVER</b>, <b>ENV</b>
</div>



### ref

**Get reference to hive key and it's contents**

``` php
&$f3->ref( string $key, [ bool $add = true ]); mixed
```

Usage:

``` php
$f3->set('name','John');
$b = &$f3->ref('name'); // $b is a reference to framework variable 'name' , not a copy
$b = 'Chuck'; // modifiying the reference updates the framework variable 'name'
echo $f3->get('name'); // Chuck
```

You can also add non-existent hive keys, array elements, and object properties, when 2nd argument is TRUE by default.

``` php

$new = &$f3->ref('newVar'); // creates new framework hive var 'newVar' and returns reference to it
$new = 'hello world'; // set value of php variable, also updates reference
echo $f3->get('newVar'); // hello world

$new = &$f3->ref('newObj->name'); 
$new = 'Sheldon';
echo $f3->get('newObj')->name; // Sheldon
echo $f3->get('newObj->name'); // Sheldon
echo $f3->get('newObj.name'); // Sheldon

$a = &$f3->ref('hero.name');
$a = 'SpongeBob';
// or
$b = &$f3->ref('hero'); // variable
$b['name'] = 'SpongeBob';  // becomes array with key 'name'
$my_array = $f3->get('hero'); 
echo $my_array['name']; // 'SpongeBob'
```

If the 2nd argument `$add` is `false`, it just returns the read-only hive key contents. This behaviour is used by get(). If the hive key does not exist, it returns NULL. 



### exists
**Return TRUE if hive key is not empty**

``` php
$f3->exists( string $key ); bool
```

<div class="alert alert-info"><strong>Notice:</strong> exists uses PHP's `isset()` function to determine if a variable is set and is not NULL.</div>

Usage:

``` php
$f3->set('foo','value');

$f3->exists('foo'); // true
$f3->exists('bar'); // false, was not set above

$f3->exists('COOKIE.userid');
$f3->exists('SESSION.login');
$f3->exists('POST.submit');
```

The exists function also checks the Cache backend storage, if the key was not found in the hive.

<div class="alert alert-info"><strong>Notice:</strong> If you check the existence of a SESSION key, the session get started automatically.</div>



### clear
**Unset hive key, key no longer exists**

``` php
$f3->clear( string $key ); void
```

To remove a hive key and it's value completely from memory:

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
$f3->mset( array(
                 'var1'=>'value1',
                 'var2'=>'value2',
                 'var3'=>'value3',
                 )
         );

echo $f3->get('var1'); // value1
echo $f3->get('var2'); // value2
echo $f3->get('var3'); // value3
```

You can append all key names using the 2nd argument `$prefix`.

``` php
$f3->mset( array(
                 'var1'=>'value1',
                 'var2'=>'value2',
                 'var3'=>'value3',
                 ),
            'pre_'
          );

echo $f3->get('pre_var1'); // value1
echo $f3->get('pre_var2'); // value2
echo $f3->get('pre_var3'); // value3
```

To cache all vars, set a positive numeric integer value to `$ttl` in seconds.



### hive
**return all hive contents as array.**

``` php
echo "HIVE CONTENTS <pre>" . var_export( $f3->hive(), true ) . "</pre>";
```



### copy
**Copy contents of hive variable to another**

``` php
$f3->copy( string $src, string $dst); mixed
```

Usage:

``` php
$f3->set('var_1','value123');
$f3->copy('var_1','foo'); // foo = "value123"
echo $f3->get('foo'); // "value123"
```

Returns writable reference to `$dst` hive variable.



### concat
**Concatenate string to hive string variable**

``` php
$f3->concat( string $key, string $val ); void
```

Usage:

``` php
$f3->set('cart_count', 4);
$f3->concat('cart_count,' items in your shopping cart'); 
echo $f3->get('cart_count'); // "4 items in your shopping cart"
```

Returns writable reference to `$key` hive variable.



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

Usage:

```php
$filepath = __FILE__; // \www\mysite\myfile.txt
$filepath = $f3->fixslashes($filepath); // /www/mysite/myfile.txt
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


<!-- testing tocify vs reserved words -->
### csv&nbsp;
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

Usage:

``` php
$str_s_c = 'user_name';
$f3->camelcase($str_s_c); // returns: "userName"
```


### snakecase
**Convert camelCase string to snake_case**

``` php
$f3->snakecase( string $str ); string
```

Usage:

``` php
$str_CC = 'userName';
$f3->snakecase($str_CC); // returns: "user_name"
```


### sign
**Return -1 if specified number is negative, 0 if zero,	or 1 if the number is positive**

``` php
$f3->sign( mixed $num ); integer
```



### hash
**Generate 64bit/base36 hash**

``` php
$f3->hash( string $str ); string
```

Example:

``` php
echo $f3->hash('foobar'); // 0i43fmgps1r
```



### base64
**Return Base64-encoded equivalent**

``` php
$f3->base64( string $data, string $mime ); string
```

Example:

``` php
echo $f3->base64('<h1>foobar</h1>','text/html');
// data:text/html;base64,PGgxPmZvb2JhcjwvaDE+
```



### encode
**Convert special characters to HTML entities**

``` php
$f3->encode( string $str ); string
```

Encodes symbols like `& " ' < >` and other chars, based on your applications ENCODING setting. (default: UTF-8)

Example:

``` php
echo $f3->encode("we <b>want</b> 'sugar & candy'");
// we &amp;lt;b&amp;gt;want&amp;lt;/b&amp;gt; 'sugar &amp;amp; candy'

echo $f3->encode("§9: convert symbols &amp; umlauts like ä ü ö");
// &amp;sect;9: convert symbols &amp;amp; umlauts like &amp;auml; &amp;uuml; &amp;ouml;
```



### decode
**Convert HTML entities back to characters**

``` php
$f3->decode( string $str ); string
```

Example:

``` php
echo $f3->decode("we &amp;lt;b&amp;gt;want&amp;lt;/b&amp;gt; 'sugar &amp;amp; candy'");
// we <b>want</b> 'sugar &amp; candy'

echo $f3->decode("&amp;sect;9: convert symbols &amp;amp; umlauts like &amp;auml; &amp;uuml; &amp;ouml;");
// §9: convert symbols &amp; umlauts like ä ü ö welcome!
```



### scrub
**Remove HTML tags (except those enumerated) and non-printable characters to mitigate XSS/code injection attacks**

``` php
$f3->scrub( mixed &$var, [ string $tags=NULL ]); string
```

Example:

``` php
$foo = "99 bottles of <b>beer</b> on the wall. <script>alert(1);</script>";
echo $f3->scrub($foo); // 99 bottles of beer on the wall. alert(1);
echo $foo // 99 bottles of beer on the wall. alert(1);
```

You can also define a list of allowed html tags, that will not be removed:

``` php
$foo = "<h1><b>nice</b> <span>news</span> article <em>headline</em></h1>";
$f3->scrub($foo,'h1,span');
// <h1>nice <span>news</span> article headline</h1>
```

<div class="alert alert-info">It is recommended to use this function to sanitize submitted form input.</div>



### esc&nbsp;
**Encode characters to equivalent HTML entities**

``` php
$f3->esc( mixed $arg ); string
```

Usage:

``` php
echo $f3->esc("99 bottles of <b>beer</b> on the wall. <script>alert(1);</script>");
// 99 bottles of &amp;lt;b&amp;gt;beer&amp;lt;/b&amp;gt; on the wall. &amp;lt;script&amp;gt;alert(1);&amp;lt;/script&amp;gt;
```

This also works with arrays and object properties:

``` php
$myArray = array('<b>foo</b>',array('<script>alert(1)</script>'),'key'=>'<i>foo</i>');
print_r($f3->esc($myArray));
/*
    [0] => &amp;lt;b&amp;gt;foo&amp;lt;/b&amp;gt;
    [1] => Array
        (
            [0] => &amp;lt;script&amp;gt;alert(1)&amp;lt;/script&amp;gt;
        )
    [key] => &amp;lt;i&amp;gt;foo&amp;lt;/i&amp;gt;
*/

$myObj = new stdClass();
$myObj->title = '<h1>Hello World</h1>';
var_dump($f3->esc($myObj));
/*
    object(stdClass)#23 (1) {
      ["title"] => string(32) "&amp;lt;h1&amp;gt;Hello World&amp;lt;/h1&amp;gt;"
    }
*/
```

<div class="alert alert-info">If the system var <b>ESCAPE</b> is turned on (it is by default), then every hive key access using a template token like <b>{{@myContent}}</b> automatically get escaped by this function. If this is not desired, look into the <a href="template">template</a> section for further post processors.</div>



### raw
**Decode HTML entities to equivalent characters**

``` php
$f3->raw( mixed $arg ); string
```

Example:

``` php
$f3->raw("99 bottles of &amp;lt;b&amp;gt;beer&amp;lt;/b&amp;gt; on the wall. &amp;lt;script&amp;gt;alert(1);&amp;lt;/script&amp;gt;");
// 99 bottles of <b>beer</b> on the wall. <script>alert(1);</script>
```

This method also handles nested array elements and objects properties, like `$f3->esc()` does too.



### serialize
**Return string representation of PHP value**

``` php
$f3->serialize( mixed $arg ); string
```

Usage:

``` php
// example using json_encode
$myArray = array('a' => 1, 'b' => 2, 'c' => 3, 'd' => 4, 'e' => 5);
echo $f3->serialize($myArray);  // outputs {"a":1,"b":2,"c":3,"d":4,"e":5}
```

Depending on the `SERIALIZER` system variable, this method converts anything into a portable string expression. Possible values are **igbinary**, **json** and **php**.
F3 checks on startup, if igbinary is available and prioritize it, as the igbinary extension works much faster and uses less disc space for serializing. Check [igbinary on github](https://github.com/igbinary/igbinary).



### unserialize
**Return PHP value derived from string**

``` php
$f3->unserialize( mixed $arg ); string
```

See [serialize](base#serialize) for further description.

# To Be Continued... #

## Localisation

### format
**Return locale-aware formatted string**

``` php
$f3->format( string $format [, mixed $arg0 [, mixed $arg1...] ] ); string
```

The `$format` string contains one or more placeholders identified by a position index enclosed in curly braces, the starting index being 0.
The placeholders are replaced by the values of the provided arguments.

``` php
echo $f3->format('Name: {0} - Age: {1}','John',23); //outputs the string 'Name: John - Age: 23'
```

The formatting can get preciser if the expected type is provided within placeholders.
Current supported types are:

* date
* time
* number,integer
* number,currency
* number,percent
* plural

``` php
echo $f3->format('Current date: {0,date} - Current time: {0,time}',time());
//outputs the string 'Current date: 04/12/2013 - Current time: 11:49:57'
echo $f3->format('{0} is displayed as a decimal number while {0,number,integer} is rounded',12.54);
//outputs the string '12.54 is displayed as a decimal number while 13 is rounded'
echo $f3->format('Price: {0,number,currency}',29.90);
//outputs the string 'Price: $29.90'
echo $f3->format('Percentage: {0,number,percent}',0.175);
//outputs the string 'Percentage: 18%' //Note that the percentage is rendered as an integer
```

The **plural** type syntax is a bit more complex since it allows you to relate the output to the input quantity. The accepted keywords are *zero*, *one*, *two* and *other*.

``` php
$string='{0,plural,zero {Your cart is empty.},one {One item in your cart.},two {A pair of items in your cart.},other {There are # items in your cart.}}';
echo $f3->format($string,0);//outputs the string 'Your cart is empty.'
echo $f3->format($string,1);//outputs the string 'One item in your cart.'
echo $f3->format($string,2);//outputs the string 'A pair of items in your cart.'
echo $f3->format($string,3);//outputs the string 'There are 3 items in your cart.'
```


### language
### lexicon

## Routing

### mock



### route
**Bind handler to route pattern**

``` php
$f3->route( string|array $pattern, callback $handler, [ int $ttl = 0 ],[ int $kbps = 0 ]); null
```

Basic usage example:

``` php
$f3->route('POST /login','AuthController->login');
```

#### Route Pattern

The `$pattern` var describes a route pattern, that consists of the request type and a request URI, both separated by a space char.

Possible request type (Verb) definitions, that F3 will process, are: **GET**, **POST**, **PUT**, **DELETE**, **HEAD**, **PATCH**, **CONNECT**.

You can combine multiple verbs, to use the same route handler for all of them. They are separated by a pipe char like `GET|POST`.

The request URI may contain one or more **token**, that a meant for defining dynamic routes. Tokens are indicated by a `@`-char. See this example:

``` php
$f3->route('GET|HEAD /@page','PageController->display'); // /about
$f3->route('POST /@category/@thread','ForumThread->saveAnswer'); // /games/battlefield3
$f3->route('GET /image/@width-@height/@file','ImageCompressor->render'); // /image/300-200/mario.jpg
```

After processing the routes (initiated by [run](base#run)), you'll find the value of each of those tokens in the `PARAMS` system variable as named key, like `$f3->get('PARAMS.page')`.
The routing pattern that matches the current request URI is saved in the `PATTERN` var, the current HTTP request URI in the `URI` var and the request method in the `VERB` var.

You can also define wildcards (`/*`) in your routing URI. You can also use them in combination with `@`-tokens.

``` php
$f3->route('GET /path/*/@page',function($f3,$params){
    // called URI: "/path/cat/page1"
    // $params is the same as $f3->get('PARAMS');
    $params[0]; // contains the full route path. "/path/cat/page"
    $params[1]; // and further numeric keys in PARAMS holds the catched wildcard paths and tokens. in this case "cat"
    $params['page']; // is your last segment, in this case "page1"
});
```

<div class="alert alert-info">
    <b>Notice:</b> The `PARAMS` var contains all tokens as named key, and additionally all tokens and wildcards as numeric key, depending on their order of appearance.
</div>

The route above also works with sub categories. Just call `/path/cat/subcat/page1`

``` php
$params[1]; // now contains "/cat/subcat"
```

It even works with some more sub-levels. You just need to explode this value with a `/`-delimiter to handle your sub-categories.
Something like `/path/*/@pagetitle/@pagenum` is also quite easy.

It becomes complicated when you try to use more than one wildcard, because only the first `/*`-wildcard can hold unlimited path-segments.
Any further wildcards can only contain exactly one part between the slashes (`/`). So try to keep it simple.

#### Route Handler

Can be a callable class method like 'Foo->bar' or 'Foo::bar', a function name, or an anonymous function.

F3 automatically passes the framework object to methods of route handler controller classes, i.e.

``` php
$f3->set('hello','world');
$f3->route('GET /foo/@file','Bar->baz');

class Bar {
    function baz($f3,$args) {
        echo $f3->get('hello');
        echo $args['file'];
    }
}
```

#### Caching

The 3rd argument `$ttl` defines the caching time in seconds.

#### Bandwidth Throttling

Set the 4th argument `$kbps` to your desired speed limit, to enable throttling.

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
