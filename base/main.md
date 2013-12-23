# Base

The Base class represents the framework core. It contains everything you need to run a simple application. The file `base.php` also includes the essential [Cache](cache), [Prefab](prefab-registry), [View](view), [ISO](iso) and [Registry](prefab-registry#registry) classes to reduce unnecessary disk I/O for optimal performance.

Feel free to remove all other files in the `lib/` directory, if all you need are the basic features provided by this package.

Namespace: `\` <br/>
File location: `lib/base.php`

---

## The Hive

The hive is a memory array to hold your framework variables in the form of key / value pairs. Storing a value in the hive ensures it is globaly available to all classes and methods in your application.

### set

**Bind value to hive key**

```php
mixed set ( string $key, mixed $val [, int $ttl = 0 ] )
```
*Examples of setting framework variables:*

```php
$f3->set('a',123); // a=123, integer
$f3->set('b','c'); // b='c', string
$f3->set('c','whatever'); // c='whatever', string
$f3->set('d',TRUE); // d=TRUE, boolean
```
*Setting arrays:*

```php
$f3->set('hash',array( 'x'=>1,'y'=>2,'z'=>3 ) );
// dot notation is also possible:
$f3->set('hash.x',1);
$f3->set('hash.y',2);
$f3->set('hash.z',3);
```
*Setting objects properties:*

```php
$f3->set('a',new \stdClass);
$f3->set('a->hello','world');
echo $f3->get('a')->hello; // world
```

#### Caching properties

If the `$ttl` parameter is > 0, and the [framework cache engine](cache) is enabled, the specified variable will be cached for `$ttl` seconds. Already cached vars will be updated and get the new expiration `$ttl`.
If the key was already cached before and `$ttl` is 0, then the key value will also be updated in cache, by reusing the old expiration time.

You can cache strings, arrays and all other types - even complete objects. `get()` will load them automatically from the cache.

*Examples of caching framework variables:*

```php
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

#### System variables

The framework has its own [system variables] (quick-reference#system-variables). You can change them to modify a framework behaviour, for example:

```php
$f3->set('CACHE', TRUE);
$f3->set('HALT', FALSE);
$f3->set('CASELESS', FALSE);
```

It is also possible to set the PHP globals through F3's [COOKIE, GET, POST, REQUEST, SESSION, FILES, SERVER, ENV system variables](quick-reference#cookie,-get,-post,-request,-session,-files,-server,-env). These 8 variables are automatically in synch with the underlying PHP globals.

<div class="alert alert-info"><strong>Notice:</strong> If you set or access a key of SESSION, the session gets started automatically. There's no need for you to do it by yourself.</div>

**Remember**: Hive keys are _case-sensitive_.<br/>Furthermore, root hive keys are checked for validity against these allowed chars: [&nbsp;a-z&nbsp;A-Z&nbsp;0-9&nbsp;_&nbsp;]

### get

**Retrieve contents of hive key**

```php
mixed get ( string $key [, string|array $args = NULL ] )
```

to get a previously saved framework var, use:

```php
$f3->set('myVar','hello world');
echo $f3->get('myVar'); // outputs the string 'hello world'
$local_var = $f3->get('myVar'); // $local_var holds the string 'hello world'
```

<div class="alert alert-info"><strong>Notice:</strong> When caching is enabled and the var hasn't been defined at runtime before, F3 tries to load the var from Cache when using get().</div>

Accessing arrays is easy. You can also use the JS dot notation `'myarray.bar'`, which makes it much easier to read and write.
<!-- special alignment ez 2 read 4 beginners -->

```php
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

```php
array sync ( string $key )
```

Usage:

```php
$f3->sync('SESSION'); // ensures PHP global var SESSION is the same as F3 variable SESSION
```

<div class="alert alert-info">
    F3 will automatically sync the following PHP globals:
    <b>GET</b>, <b>POST</b>, <b>COOKIE</b>, <b>REQUEST</b>, <b>SESSION</b>, <b>FILES</b>, <b>SERVER</b>, <b>ENV</b>
</div>



### ref

**Get reference to hive key and its contents**

```php
mixed &ref ( string $key [, bool $add = true ] )
```

Usage:

```php
$f3->set('name','John');
$b = &$f3->ref('name'); // $b is a reference to framework variable 'name' , not a copy
$b = 'Chuck'; // modifiying the reference updates the framework variable 'name'
echo $f3->get('name'); // Chuck
```

You can also add non-existent hive keys, array elements, and object properties, when 2nd argument is TRUE by default.

```php

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

If the 2nd argument `$add` is `false`, it just returns the read-only hive key content. This behaviour is used by get(). If the hive key does not exist, it returns NULL.

### exists

**Return TRUE if the hive key is not set (or return timestamp and TTL if cached)**

```php
bool exists ( string $key [, mixed &$val=NULL] )
```

The `exists` function also checks the Cache backend storage when the key is not found in the hive. If the key is found in cache, it then returns `array ( $timestamp, $ttl )`.

<div class="alert alert-info"><strong>Notice:</strong> exists uses PHP's `isset()` function to determine if the hive key is set and is not NULL.</div>

Usage:

```php
$f3->set('foo','value');

$f3->exists('foo'); // true
$f3->exists('bar'); // false, was not set above
```

`exists` is especially useful with [PHP global variables automatically synched by F3](base#sync):

```php
// Synched hive keys with PHP global variables
$f3->exists('COOKIE.userid');
$f3->exists('SESSION.login');
$f3->exists('POST.submit');
```

<div class="alert alert-warning"><strong>Notice:</strong> If you check the existence of a SESSION key, the session get started automatically.</div>

### devoid

**Return TRUE if the hive key is empty and not cached**

```php
bool devoid ( string $key )
```

The `devoid` function also checks the Cache backend storage, if the key was not found in the hive.

<div class="alert alert-info"><strong>Notice:</strong> devoid uses PHP's `empty()` function to determine if the hive key is empty and not cached.</div>

Usage:

``` php
$f3->set('foo','');
$f3->set('bar',array());
$f3->set('baz',array(),10);

$f3->devoid('foo'); // true
$f3->devoid('bar'); // true
$f3->devoid('baz'); // false
```

### clear

**Unset hive key, key no longer exists**

```php
void clear ( string $key )
```

To remove a hive key and its value completely from memory:

```php
$f3->clear('foobar');
$f3->clear('myArray.param1'); // removes key `param1` from array `myArray`
```

If the given hive key was cached before, it will be cleared from cache too.

Some more special usages:

```php
$f3->clear('SESSION'); // destroys the user SESSION
$f3->clear('COOKIE.foobar'); // removes a cookie
$f3->clear('CACHE'); // clears all cache contents
```

<div class="alert alert-info"><strong>Notice:</strong> Clearing all cache contents at once is not supported for the XCache cache backend</div>

### mset

**Multi-variable assignment using associative array**

```php
void mset ( array $vars [, string $prefix = '' [, integer $ttl = 0 ]] )
```

Usage:

```php
$f3->mset(
    array(
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

```php
$f3->mset(
    array(
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

**Return the whole hive contents as an array**

```php
array hive ()
```

Usage:

```php
printf ("A Busy Hive: <pre>%s</pre>", var_export( $f3->hive(), true ) );
```

### copy

**Copy contents of a hive variable to another**

```php
mixed copy ( string $src, string $dst )
```

Return a writable reference to a new `$dst` hive key. If `$dst` already exists in the hive, it simply gets overwritten.

Usage:

```php
$f3->set('foo','123');
$f3->set('bar','barbar');
$bar = $f3->copy('foo','bar'); // bar = '123'
$bar = 456;
$f3->set('foo','789');
echo $f3->get('bar'); // '456'
```

### concat

**Concatenate string to hive string variable**

```php
string concat ( string $key, string $val )
```

Return result of the concatenation. **Note**: If `$key` does not exist in the hive, it is automatically created in the hive.

Usage:

```php
$f3->set('count', 99);
$f3->set('item', 'beer');
$text = $f3->concat('count', ' bottles of '.$f3->get('item'));
$text .= ' on the wall';
$f3->concat('wall', $f3->get('count')); // new 'wall' hive key is created
echo $f3->get('wall'); // 99 bottles of beer on the wall
```

### flip

**Swap keys and values of hive array variable**

```php
array flip ( string $key )
```

Usage:

```php
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

```php
mixed push ( string $key, mixed $val )
```

Usage:

```php
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

```php
mixed pop ( string $key )
```

Usage:

```php
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

```php
mixed unshift ( string $key, string $val )
```

Usage:

```php
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

**Remove first element of a hive array variable**

```php
array|NULL shift ( string $key )
```

Return the left-shifted hive array variable, or `NULL` if the hive array variable is empty or is not an array. 

<div class="alert alert-warning">
<b>Notice</b>: <code>shift</code> use the PHP function <code>array_shift()</code>. It means that all numerical array keys of the hive array variable will be modified to start counting from zero while literal keys won't be touched
</div>

Example:

```php
$f3->set('fruits', array(
    'crunchy'=>'apples',
    '11'=>'bananas',
    '6'=>'kiwis',
    'juicy'=>'peaches'
));
$f3->shift('fruits'); // returns "apples"
print_r($f3->get('fruits'));
/* output:
Array
(
    [0] => bananas
    [1] => kiwis
    [juicy] => peaches
)
*/
```

### merge

**Merge array with hive array variable**

```php
array merge ( string $key, array $src )
```

Return the resulting array of the merge.

Example:

``` php
$f3->set('foo', array('blue','green'));
$f3->merge('foo', array('red', 'yellow'));

/* 'foo' now is:
array (size=4)
  [0] => string 'blue' (length=4)
  [1] => string 'green' (length=5)
  [2] => string 'red' (length=3)
  [3] => string 'yellow' (length=6)
*/
```

---

## Encoding & Conversion

### fixslashes

**Convert backslashes to slashes**

```php
string fixslashes ( string $str )
```

Usage:

```php
$filepath = __FILE__; // \www\mysite\myfile.txt
$filepath = $f3->fixslashes($filepath); // /www/mysite/myfile.txt
```

### split

**Split comma-, semi-colon, or pipe-separated string**

```php
array split ( string $str )
```

Usage:

```php
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

```php
string stringify ( mixed $arg [, $detail = TRUE ] )
```

The `$detail` parameter controls whether to walk recursively into nested objects or not.

### csv

**Flatten array values and return as CSV string**

```php
string csv ( array $args )
```

Usage:

```php
$elements = array('water','earth','wind','fire');
echo $f3->csv($elements); // displays "'water','earth','wind','fire'" (string, length 29)
```

### camelcase

**Convert snake_case string to camelCase**

```php
string camelcase ( string $str )
```

Usage:

```php
$str_s_c = 'user_name';
$f3->camelcase($str_s_c); // returns: "userName"
```
### snakecase

**Convert camelCase string to snake_case**

```php
string snakecase ( string $str )
```

Usage:

```php
$str_CC = 'userName';
$f3->snakecase($str_CC); // returns: "user_name"
```

### sign

**Return -1 if specified number is negative, 0 if zero,	or 1 if the number is positive**

```php
int sign ( mixed $num )
```

### hash

**Generate 64bit/base36 hash**

```php
string hash ( string $str )
```

Example:

```php
var_dump($f3->hash('foobar')); // string '0i43fmgps1r' (length=11)
```

### base64

**Return Base64-encoded equivalent**

```php
string base64 ( string $data, string $mime )
```

Example:

```php
echo $f3->base64('<h1>foobar</h1>','text/html');
// data:text/html;base64,PGgxPmZvb2JhcjwvaDE+
```

### encode

**Convert special characters to HTML entities**

```php
string encode ( string $str )
```

Encodes symbols like `& " ' < >` and other chars, based on your applications ENCODING setting. (default: UTF-8)

Example:

```php
echo $f3->encode("we <b>want</b> 'sugar & candy'");
// we &amp;lt;b&amp;gt;want&amp;lt;/b&amp;gt; 'sugar &amp;amp; candy'

echo $f3->encode("§9: convert symbols & umlauts like ä ü ö");
// &amp;sect;9: convert symbols &amp;amp; umlauts like &amp;auml; &amp;uuml; &amp;ouml;
```

### decode

**Convert HTML entities back to characters**

```php
string decode ( string $str )
```

Example:

```php
echo $f3->decode("we &amp;lt;b&amp;gt;want&amp;lt;/b&amp;gt; 'sugar &amp;amp; candy'");
// we <b>want</b> 'sugar &amp; candy'

echo $f3->decode("&amp;sect;9: convert symbols &amp;amp; umlauts like &amp;auml; &amp;uuml; &amp;ouml;");
// §9: convert symbols &amp; umlauts like ä ü ö welcome!
```

### clean

**Remove HTML tags (except those enumerated) and non-printable characters to mitigate XSS/code injection attacks**

```php
string clean ( mixed $var [, string $tags = NULL ] )
```
`$var` can be either a `string` or an `array`. In the latter case, it will be recursively traversed to clean each and every element.

`$tags` defines a list [<small>(as per the split syntax)</small>](base#split) of _allowed_ html tags that will **not** be removed.

<div class="alert alert-success"><strong>Advice</strong>: It is recommended to use this function to sanitize submitted form input.</div>

Examples:

```php
$foo = "99 bottles of <b>beer</b> on the wall. <script>alert('scripty!')</script>";
echo $f3->clean($foo); // "99 bottles of beer on the wall. alert('scripty!')"
```

```php
$foo = "<h1><b>nice</b> <span>news</span> article <em>headline</em></h1>";
$h1 = $f3->clean($foo,'h1,span'); // <h1>nice <span>news</span> article headline</h1>
```

### scrub

**Similar to clean(), except that variable is passed by reference**

```php
string scrub ( mixed &$var [, string $tags = NULL ] )
```

Example:

```php
$foo = "99 bottles of <b>beer</b> on the wall. <script>alert('scripty!')</script>";
$bar = $f3->scrub($foo);
echo $foo; // 99 bottles of beer on the wall. alert('scripty!')
```

### serialize

**Return string representation of PHP value**

```php
string serialize ( mixed $arg )
```

Usage:

```php
// example using json_encode
$myArray = array('a' => 1, 'b' => 2, 'c' => 3, 'd' => 4, 'e' => 5);
echo $f3->serialize($myArray);  // outputs {"a":1,"b":2,"c":3,"d":4,"e":5}
```

Depending on the [SERIALIZER](quick-reference#serializer "Definition and usage of the SERIALIZER system variable") system variable, this method converts anything into a portable string expression. Possible values are **igbinary**, **json** and **php**.
F3 checks on startup if igbinary is available and prioritize it, as the igbinary extension works much faster and uses less disc space for serializing. Check [igbinary on github](https://github.com/igbinary/igbinary "Igbinary is a drop in replacement for the standard php serializer").

### unserialize

**Return PHP value derived from string**

```php
string unserialize ( mixed $arg )
```

See [serialize](base#serialize) for further description.

---

## Localisation

<div class="alert alert-info"><strong>Note:</strong> F3 follows ICU formatting rules but does not require PHP's <tt>intl</tt> module.</div>

### format

**Return locale-aware formatted string**

```php
string format( string $format [, mixed $arg0 [, mixed $arg1...]] )
```

The `$format` string contains one or more placeholders identified by a position index enclosed in curly braces, the starting index being 0.
The placeholders are replaced by the values of the provided arguments.

```php
echo $f3->format('Name: {0} - Age: {1}','John',23); //outputs the string 'Name: John - Age: 23'
```

The formatting can get preciser if the expected type is provided within placeholders.
Current supported types are:

* date
* time
* number,integer
* number,currency
* number,percent
* number,decimal
* plural

```php
echo $f3->format('Current date: {0,date} - Current time: {0,time}',time());
//outputs the string 'Current date: 04/12/2013 - Current time: 11:49:57'
echo $f3->format('{0} is displayed as a decimal number while {0,number,integer} is rounded',12.54);
//outputs the string '12.54 is displayed as a decimal number while 13 is rounded'
echo $f3->format('Price: {0,number,currency}',29.90);
//outputs the string 'Price: $29.90'
echo $f3->format('Percentage: {0,number,percent}',0.175);
//outputs the string 'Percentage: 18%' //Note that the percentage is rendered as an integer
echo $f3->format('Decimal Number: {0,number,decimal,2}', 0.171231235);
//outputs the string 'Decimal Number: 0,17'
```

The **plural** type syntax is a bit more complex since it allows you to relate the output to the input quantity. The accepted keywords are *zero*, *one*, *two* and *other*.

```php
$string='{0,plural,zero {Your cart is empty.},one {One item in your cart.},two {A pair of items in your cart.},other {There are # items in your cart.}}';
echo $f3->format($string,0);//outputs the string 'Your cart is empty.'
echo $f3->format($string,1);//outputs the string 'One item in your cart.'
echo $f3->format($string,2);//outputs the string 'A pair of items in your cart.'
echo $f3->format($string,3);//outputs the string 'There are 3 items in your cart.'
```

### language

**Assign/auto-detect language**

```php
string language ( string $code )
```

This function is used while booting the framework to auto-detect the possible user language, by looking at the HTTP request headers, specifically the Accept-Language header sent by the browser.
Use the [LANGUAGE](quick-reference#language "Definition and usage of the LANGUAGE system variable") system variable to get and set languages, since it also handles some dependencies like changing dictionary files.
The  [FALLBACK](quick-reference#fallback "Definition and usage of the FALLBACK system variable") system variable defines a default language, that will be used, if none of the detected languages are available as a dictionary file.

Example:

```php
$f3->get('LANGUAGE'); // 'de_DE,de,en_US,en'
$f3->set('LANGUAGE', 'en_UK,en_US,en');
```

### lexicon

**Transfer lexicon entries to hive**

```php
array lexicon ( string $path )
```

This function is used while booting the framework to auto-load the dictionary files, located within the defined path in `LOCALES` var.

```php
$f3->set('LOCALES','dict/');
```

A dictionary file can be a php file returning a key-value paired associative array, or an .ini-style formatted config file.
[Read the guide about language files here](views-and-templates#multilingual-support).

---

## Routing

### build

**Replace tokenized URL with current route's token values**

``` php
string build ( string $url )
```

Example:

``` php
// for instance the route is '/subscribe/@channel' with PARAMS.channel = 'fatfree'

echo $f3->build('@channel'); // displays 'fatfree'
echo $f3->build('/get-it/now/@channel'); // displays '/get-it/now/fatfree'
echo $f3->build('/subscribe/@channel');  // displays '/subscribe/fatfree'
```

### mock

**Mock an HTTP request**

```php
NULL mock ( string $pattern [, array $args = NULL [, array $headers = NULL [, string $body = NULL ]]] )
```

This emulates a HTTP request.

Basic usage example:

```php
$f3->mock('GET /page/view');
```

### parse

**Parse a string containing key-value pairs and use them as routing tokens**

``` php
NULL parse ( string $str )
```

Example:

```php
$f3->parse('framework=f3 , speed=fast , features=full');
echo $f3->get('PARAMS.framework');  // 'f3'
echo $f3->get('PARAMS.speed');      // 'fast'
```

### route

**Bind a route pattern to a given handler**

```php
null route ( string|array $pattern, callback $handler [, int $ttl = 0 [, int $kbps = 0 ]] )
```

Basic usage example:

```php
$f3->route('POST /login','AuthController->login');
```

#### Route Pattern

The `$pattern` var describes a route pattern, that consists of the request type(s) and a request URI, both separated by a space char.

##### Verbs

Possible request type (Verb) definitions, that F3 will process, are: **GET**, **POST**, **PUT**, **DELETE**, **HEAD**, **PATCH**, **CONNECT**.

You can combine multiple verbs, to use the same route handler for all of them. Simply separate them by a pipe char, like `GET|POST`.

##### Tokens

The request URI may contain one or more **token(s)**, that a meant for defining dynamic routes. Tokens are indicated by a `@` char prior their name. See this example:

```php
$f3->route('GET|HEAD /@page','PageController->display'); // ex: /about
$f3->route('POST /@category/@thread','ForumThread->saveAnswer'); // /games/battlefield3
$f3->route('GET /image/@width-@height/@file','ImageCompressor->render'); // /image/300-200/mario.jpg
```

After processing the incoming request URI (initiated by [run](base#run)), you'll find the value of each of those tokens in the `PARAMS` system variable as named key, like `$f3->get('PARAMS.file')`. // 'mario.jpg'

<div class="alert alert-info">
    <b>Notice:</b> Routes and their according verbs are grouped by their URL pattern. Static routes _precede_ routes with dynamic tokens or wildcards.
    This means that having a static route, which overloads a matching dynamic route, requires you to define separately all required VERB patterns to this specific route. 
</div>

##### Wildcards

You can also define wildcards (`/*`) in your routing URI. Furthermore, you can use them in combination with `@` tokens.

```php
$f3->route('GET /path/*/@page', function($f3,$params) {
    // called URI: "/path/cat/page1"
    // $params is the same as $f3->get('PARAMS');
    $params[0]; // contains the full route path. "/path/cat/page"
    $params[1]; // and further numeric keys in PARAMS hold the catched wildcard paths and tokens. in this case "cat"
    $params['page']; // is your last segment, in this case "page1"
});
```

<div class="alert alert-info">
    <b>Notice:</b> The `PARAMS` var contains all tokens as named key, and additionally all tokens and wildcards as numeric key, depending on their order of appearance.
</div>

The route above also works with sub categories. Just call `/path/cat/subcat/page1`

```php
$params[1]; // now contains "/cat/subcat"
```

It even works with some more sub-levels. You just need to explode this value with a `/`-delimiter to handle your sub-categories.
Something like `/path/*/@pagetitle/@pagenum` is also quite easy.

It becomes complicated when you try to use more than one wildcard, because only the first `/*`-wildcard can hold unlimited path-segments.
Any further wildcards can only contain exactly one part between the slashes (`/`). So try to keep it simple.

##### Groups

Since `F3 v3.0.7` it is possible to assign multiple routes to the same route handler, using an array of routes in `$pattern`. It would look like this:

```php
$f3->route(
  array(
    'GET /archive',
    'GET /archive/@year',
    'GET /archive/@year/@month',
    'GET /archive/@year/@month/@day'
  ),
  function($f3,$params){
    $params+=array('year'=>2013,'month'=>1,'day'=>1); //default values
    //etc..
  }
);
```

#### Route Handler

Can be a callable class method like 'Foo->bar' or 'Foo::bar', a function name, or an anonymous function.

F3 automatically passes the framework object to methods of route handler controller classes, i.e.

```php
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

The 3rd argument `$ttl` defines the caching time in seconds. Setting this argument to a positive value will call the [expire](base#expire) function to set cache metadata in the HTTP response header. It also caches the route response, but only GET and HEAD requests are cacheable.

If `CACHE` is turned off, `$ttl` will only control the browser cache using [expire](base#expire) header metadata.
If `CACHE` is turned on, and there is a positive `$ttl` value set for the current request URI handler, F3 additionally will cache the output for this page, and refresh it when `$ttl` expires.
Read more about it [here](https://groups.google.com/d/msg/f3-framework/lwaqZjtwCvU/PC-gK8Ki9PMJ) and [here](https://groups.google.com/d/msg/f3-framework/lwaqZjtwCvU/LDUlPhQfc84J).

#### Bandwidth Throttling

Set the 4th argument `$kbps` to your desired speed limit, to enable throttling. [Read more](optimization#bandwidth-throttling) about it in the user guide.

### reroute

**Reroute to specified URI**

```php
null reroute ( string $uri [, bool $permanent = FALSE] )
```

Examples of usage:

```php
// an old page is moved permanently
$f3->route('GET|HEAD /obsoletepage', function($f3) {
        $f3->reroute('/newpage', true);
});

// whereas a Post/Redirect/Get pattern would just redirect temporarily
$f3->route('GET|HEAD /login', 'AuthController->viewLoginForm');
$f3->route('POST /login', function($f3) {
    if( AuthController->checkLogin == true )
        $f3->reroute('/members', false);
    else
        $f3->reroute('/login', false);
});

// we can also reroute to external URLs
$f3->route('GET /partners', function($f3) {
        $f3->reroute('http://externaldomain.com');
});
```

### map

**Provide ReST interface by mapping HTTP verb to class method**

```php
null map ( string $url, string $class [, int $ttl = 0 [, int $kbps = 0 ]] )
```

Its syntax works slightly similar to the **route** function, but you need not to define a HTTP request method in the 1st argument,
because they are mapped as functions in the Class you prodive in the `$class` argument.
[Read more about the REST support in the User Guide](routing-engine#rest-representational-state-transfer).

Example of usage:

```php
$f3->map('/news/@item','News');

class News {
    function get() {}
    function post() {}
    function put() {}
    function delete() {}
}
```

### run

**Match routes against incoming URI and call their route handler**

```php
null run ()
```

Example of usage:

```php
$f3 = require __DIR__.'/lib/base.php';
$f3->route('GET /',function(){
    echo "Hello World";
});
$f3->run();
```

After processing the incoming request URI, the routing pattern that matches that URI is saved in the `PATTERN` var, the current HTTP request URI in the `URI` var and the request method in the `VERB` var.
The `PARAMS` var will contains all tokens as named keys, and additionally all tokens and wildcards as numeric keys, depending on their order of appearance.

<div class="alert alert-info">
    <p><b>Notice:</b> If a static and dynamic route pattern both match the current URI, then the static route pattern has priority.</p>
</div>


### call

**Execute callback/hooks (supports 'class->method' format)**

```php
mixed|false call ( callback $func [, mixed $args = NULL [, string $hooks = '' ]] )
```

This method provides that facility to invoke callbacks and their arguments. F3 recognizes these as valid callbacks:

* Anonymous/lambda functions (aka closures)
* `array('class','method')`
* `class::method` static method
* `class->method` equivalent to `array(new class,method)`

`$args` if specified provides a means of executing the callback with parameters.

`$hooks` is used by the `route()` method to specify pre- and post-execution functions, i.e. `beforeroute()` and `afterroute()`.

### chain

**Execute specified callbacks in succession; Apply same arguments to all callbacks**

```php
array chain ( array|string $funcs [, mixed $args = NULL ] )
```

This method invokes several callbacks in succession:

```php
echo $f3->chain('a; b; c', 0);

function a($n) {
	return 'a: '.($n+1); // a: 1
}

function b($n) {
	return 'b: '.($n+1); // b: 1
}

function c($n) {
	return 'c: '.($n+1); // c: 1
}
```

### relay

**Execute specified callbacks in succession; Relay result of previous callback as argument to the next callback**

```php
array relay ( array|string $funcs [, mixed $args = NULL ] )
```

This method invokes callback in succession like [chain](#chain) but applies the result of the first function as argument of the succeeding function, i.e.:

```php
echo $f3->relay('a; b; c', 0);

function a($n) {
	return 'a: '.($n+1); // a: 1
}

function b($n) {
	return 'b: '.($n+1); // b: 2
}

function c($n) {
	return 'c: '.($n+1); // c: 3
}
```

---
## File System


### mutex

** Create mutex, invoke callback then drop ownership when done**

```php
mixed mutex ( string $id, callback $func [, mixed $args = NULL ] )
```

A [Mutual Exclusion (mutex)](http://en.wikipedia.org/wiki/Mutual_exclusion "Wikipedia :: Mutual Exclusion") is a cross-platform mechanism for synchronizing access to resources, in such a way that a process that has acquired the mutex gains exclusive access to the resource. Other processes trying to acquire the same mutex will be in a suspended state until the mutex is released.

```php
$f3->mutex('test',function() {
	// Critical section
	session_start();
	$contents=file_get_contents('mutex');
	sleep(5);
	file_put_contents('mutex',$contents.date('r').' '.session_id()."\n");
});
```

### read

**Read file (with option to apply Unix LF as standard line ending)**

```php
string|FALSE read ( string $file [, bool $lf = FALSE ] )
```

Uses the [`file_get_contents()`](http://www.php.net/file_get_contents "PHP Manual :: file_get_contents") <small>PHP function</small> to read the entire `$file` and return it as a string. Returns `FALSE` on failure.

IF `$lf` is `TRUE`, an EOL conversion to UNIX LF format is performed on all the lines ending.

### write

**Exclusive file write**

```php
int|FALSE write ( string $file, mixed $data [, bool $append = FALSE ] )
```

Uses the [`file_put_contents()`](http://www.php.net/file_put_contents "PHP Manual :: file_put_contents") <small>PHP function</small> with the `LOCK_EX` <small>PHP</small> flag to acquire an exclusive lock on the file while proceeding to the writing.

If `$append` is `TRUE` and the `$file` already exits, the `$data` is appended to the file content instead of overwriting it.

### rel

**Return path relative to the base directory**

``` php
string rel ( string $url )
```

Example:

```php
$f3->set('BASE','http://fatfreeframework.com/');
echo $f3->rel( 'http://fatfreeframework.com/gui/img/supported_dbs.jpg' ); // 'gui/img/supported_dbs.jpg'
```

---

## Misc

### blacklisted

**Lookup visitor's IP against common DNS blacklist services**

```php
bool blacklisted ( string $ip )
```

This function get called while bootstrapping the application and will lookup the visitors IP against common DNS blacklist services defined by the [DNSBL system variable](quick-reference#dnsbl). This is very useful to protect your application against Spam bots or DOS attacks.

**Return `TRUE` if the IPv4 `$ip` address is present in [DNSBL](quick-reference#dnsbl)**

### config

**Configure framework according to .ini-style file settings**

```php
null config ( string $file )
```

This will parse a configuration file, provided by `$file` and setup the framework with variables and routes.

See the user guide section about [configuration files](framework-variables#configuration-files) to get a full description about how to setup your ini file.


### dump

**Dump (_echo_) expression with syntax highlighting**

```php
null dump ( mixed $expr )
```

<i class="icon-thumbs-up"></i> _NOTICE: The syntax highlighting depends on the [DEBUG level](quick-reference#debug "The DEBUG system variable")_.


### error

**Execute error handler**

```php
null error ( int $code [, string $text = '' [, array $trace = NULL ]] )
```

Calling this function logs an error and executes the [ONERROR](quick-reference#onerror) handler if defined.
Otherwise it will display a default error page in HTML for synchronous requests, or gives a JSON string for AJAX requests.

### expire

**Send cache metadata to HTTP client**

```php
void expire ( [ int $secs = 0 ] )
```

There is little need to call this method directly because it is automatically invoked at runtime by the framework, depending on whether the page should be cached  or otherwise. The framework sends the necessary HTTP cache control headers to the browser so you don't need to send it yourself.

```php
$f3->expire(0); // sends 'Cache-Control: no-cache, no-store, must-revalidate'
```

### highlight

**Apply syntax highlighting**

```php
string highlight ( string $text )
```

### Instantiation

**Return class instance**

```php
$f3 = \Base::instance();
```

This is used to grab the framework instance at any point of your code.

### status

**Send HTTP/1.1 status header; Return text equivalent of status code**

```php
string status ( int $code )
```

Use this method for sending various HTTP status messages to the client, e.g.

```php
$f3->status(403); // Sends 403 Forbidden header
$f3->status(415); // Sends 415 Unsupported media type
```

### unload

**Execute framework/application shutdown sequence**

```php
NULL unload ( $cwd )
```
First, changes PHP's current directory to directory `$cwd`, and write session data and end session by calling the `session_commit()` <small>PHP function</small>.

Then shutdown the application and calls the shutdown handler defined in [UNLOAD](quick-reference#unload).

As a final fallback, an HTTP error `500` is raised if one of the following `E_ERROR, E_PARSE, E_CORE_ERROR, E_COMPILE_ERROR` is detected and not handled by the shutdown handler.