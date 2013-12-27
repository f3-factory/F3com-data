# UTF : An Unicode string manager

The UTF class is an utility class that eases the handling of Unicode strings.

Namespace: `\` <br>
File location: `lib/utf.php`

---

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This class is currently not documented; only its argument list is available.</p></div>

## Instantiation

**Return class instance**


```php
$utf = \UTF::instance();
```

The UTF class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.


## Methods


### stripos

**Find position of first occurrence of a string (case-insensitive)**

``` php
int|FALSE stripos ( string $stack, string $needle [ , int $ofs = 0 ] ) 
```

This function allows you to find the position of the first occurrence of a string (case-insensitive)

Example:

``` php
int|FALSE stripos ( string $stack, string $needle [, int $ofs = 0 ]] )
```

### strlen

**Get string length**

``` php
int strlen ( string $str ) 
```

This function allows you to retrieve the length of a string

Example:

``` php
echo $utf->strlen($str); // displays 8 (int)
```

### strpos

**Find position of first occurrence of a string**

``` php
int|FALSE strpos ( string $stack, string $needle [ , int $ofs = 0, bool $case = FALSE ] ] ) 
```

This function allows you to find the position of the first occurrence of a string (case-sensitive)

Example:

``` php
echo $utf->strpos($stack, $needle, $ofs, $case); // displays '@TODO' 
```

### strripos

**Finds position of last occurrence of a string (case-insensitive)**

``` php
int|FALSE strripos ( string $stack, string $needle [ , int $ofs = 0 ] ) 
```

This function allows you to finds the position of the last occurrence of a string (case-insensitive)

Example:

``` php
echo $utf->strripos($stack, $needle, $ofs); // displays '@TODO' 
```

### strrpos

**Find position of last occurrence of a string**

``` php
int|FALSE strrpos ( string $stack, string $needle [ , int $ofs = 0, bool $case = FALSE ] ] ) 
```

This function allows you to find the position of the last occurrence of a string

Example:

``` php
echo $utf->strrpos($stack, $needle, $ofs, $case); // displays '@TODO' 
```

### stristr

**Returns part of haystack string from the first occurrence of the needle to the end of haystack (case-insensitive)**

``` php
string|FALSE stristr ( string $stack, string $needle [ , bool $before = FALSE ] ) 
```

This function allows you to returns part of haystack string from the first occurrence of the needle to the end of the haystack (case-insensitive)

Example:

``` php
echo $utf->stristr($stack, $needle, $before); // displays '@TODO' 
```

### strstr

**Returns part of haystack string from the first occurrence of the needle to the end of the haystack**

``` php
string|FALSE strstr ( string $stack, string $needle [ , bool $before = FALSE, bool $case = FALSE ] ] ) 
```

This function allows you to returns part of haystack string from the first occurrence of needle to the end of haystack (case-sensitive)

Example:

``` php
echo $utf->strstr($stack, $needle, $before, $case); // displays '@TODO' 
```

### substr

**Return part of a string**

``` php
string|FALSE substr ( string $str, int $start [ , int $len = 0 ] ) 
```

This function allows you to return part of a string

Example:

``` php
echo $utf->substr($str, $start, $len); // displays '@TODO' 
```

### substr_count

**Count the number of occurrences of a substring**

``` php
int substr_count ( string $stack, string $needle ) 
```

This function allows you to retrieve the number of occurrences of a given substring

Example:

``` php
echo $utf->substr_count($stack, $needle); // displays '@TODO' 
```

### ltrim

**Strip whitespaces from the beginning of a string**

``` php
string ltrim ( string $str ) 
```

This function allows you to strip the whitespaces from the beginning of a string

Example:

``` php
echo $utf->ltrim($str); // displays '@TODO' 
```

### rtrim

**Strip whitespaces from the end of a string**

``` php
string rtrim ( string $str ) 
```

This function allows you to strip the whitespaces from the end of a string

Example:

``` php
echo $utf->rtrim($str); // displays '@TODO' 
```

### trim

**Strip whitespaces from the beginning and end of a string**

``` php
string trim ( string $str ) 
```

This function allows you to strip the whitespaces from the beginning and from the end of a string

Example:

``` php
echo $utf->trim($str); // displays '@TODO' 
```

### bom

**Return UTF-8 byte order mark ([BOM](http://en.wikipedia.org/wiki/Byte_order_mark "Wikipedia :: Byte order mark"))**

``` php
string bom (  ) 
```

Return the byte order mark ([BOM](http://en.wikipedia.org/wiki/Byte_order_mark "Wikipedia :: Byte order mark")) Unicode character used to signal the byte order of a text file or a stream. The BOM character may also indicate which of the several Unicode representations the text is encoded in. BOM use is optional, and, if used, must appear at the start of the text stream.

Example:

``` php
$bom = \UTF::instance()->bom();  // $bom = 0xefbbbf
echo '0x'.dechex(ord($bom[0])).dechex(ord($bom[1])).dechex(ord($bom[2])); // displays '0xefbbbf'

// convert/save a file with a BOM at its beginning
$f3->write( $filename, $bom . $f3->read($filename) );
```

### translate

**Convert code points to Unicode symbols**

``` php
string translate ( string $str ) 
```

Converts and returns [code points](http://en.wikipedia.org/wiki/Code_point "Wikipedia :: Code point") (e.g. U+0E8D U+053D) to the equivalent Unicode symbols (e.g. ຍ Խ)

### emojify

**Translate emoji tokens to Unicode font-supported symbols**

``` php
string emojify ( string $str ) 
```

Converts and returns the Unicode font-supported symbols equivalent of emoji tokens. 

emoji tokens translated by default are: 

```php 
':(' => '\u2639', // frown
':)' => '\u263a', // smile
'<3' => '\u2665', // heart
':D' => '\u1f603', // grin
'XD' => '\u1f606', // laugh
';)' => '\u1f609', // wink
':P' => '\u1f60b', // tongue
':,' => '\u1f60f', // think
':/' => '\u1f623', // skeptic
'8O' => '\u1f632', // oops
```

Example:

``` php
echo \UTF::instance()->emojify('Thanks :) I <3');  //  displays 'Thanks ☺ I ♥'
```

<div class="alert alert-warning">Keep in mind you need a font that supports these characters to even have a hope of seeing them correctly in your browser pages.</div>

You can specify your own additional emoji tokens with the [EMOJI system variable](quick-reference#emoji). When provided, those emoji tokens are added to the basic set above and will be used when translating a string to Unicode font-supported symbols.

Examples:

``` php
$f3->set('EMOJI', array('(c)' => '&#169;', '?' => '&#191') );
echo \UTF::instance()->emojify( 'Do you like (c)opyrights ??');  //  displays 'Do you like ©opyrights ¿¿'
// 
$f3->set('EMOJI', array('@om' => '\U0F00', '&oooooom' => '\U0F02', '%om' => '\U0F00') );
echo \UTF::instance()->emojify( '@om Greets &oooooom from Tibet %om');  //  displays 'ༀ Greets ༂ from Tibet ༀ'
```
As you can see, it's up to you to define your emoji tokens as you fancy. You can even imagine to automate the call to the `emojify` function for the variables you use in your templates.