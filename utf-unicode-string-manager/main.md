# UTF : An Unicode string manager

The UTF class is an utility class that eases the handling of Unicode strings.

Namespace: `\` <br>
File location: `lib/utf.php`

---

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>

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

This function allows you to find position of first occurrence of a string (case-insensitive)

Example:

``` php
int|FALSE stripos ( string $stack, string $needle [, int $ofs = 0 ]] )
```

### strlen

**Get string length**

``` php
int strlen ( string $str ) 
```

This function allows you to get string length

Example:

``` php
echo $utf->strlen($str); // displays 8 (int)
```

### strpos

**Find position of first occurrence of a string**

``` php
int|FALSE strpos ( string $stack, string $needle [ , int $ofs = 0, bool $case = FALSE ] ] ) 
```

This function allows you to find position of first occurrence of a string

Example:

``` php
echo $utf->strpos($stack, $needle, $ofs, $case); // displays '@TODO' 
```

### strripos

**Finds position of last occurrence of a string (case-insensitive)**

``` php
int|FALSE strripos ( string $stack, string $needle [ , int $ofs = 0 ] ) 
```

This function allows you to finds position of last occurrence of a string (case-insensitive)

Example:

``` php
echo $utf->strripos($stack, $needle, $ofs); // displays '@TODO' 
```

### strrpos

**Find position of last occurrence of a string**

``` php
int|FALSE strrpos ( string $stack, string $needle [ , int $ofs = 0, bool $case = FALSE ] ] ) 
```

This function allows you to find position of last occurrence of a string

Example:

``` php
echo $utf->strrpos($stack, $needle, $ofs, $case); // displays '@TODO' 
```

### stristr

**Returns part of haystack string from the first occurrence of needle to the end of haystack (case-insensitive)**

``` php
string|FALSE stristr ( string $stack, string $needle [ , bool $before = FALSE ] ) 
```

This function allows you to returns part of haystack string from the first occurrence of needle to the end of haystack (case-insensitive)

Example:

``` php
echo $utf->stristr($stack, $needle, $before); // displays '@TODO' 
```

### strstr

**Returns part of haystack string from the first occurrence of needle to the end of haystack**

``` php
string|FALSE strstr ( string $stack, string $needle [ , bool $before = FALSE, bool $case = FALSE ] ] ) 
```

This function allows you to returns part of haystack string from the first occurrence of needle to the end of haystack

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

**Count the number of substring occurrences**

``` php
int substr_count ( string $stack, string $needle ) 
```

This function allows you to count the number of substring occurrences

Example:

``` php
echo $utf->substr_count($stack, $needle); // displays '@TODO' 
```

### ltrim

**Strip whitespaces from the beginning of a string**

``` php
string ltrim ( string $str ) 
```

This function allows you to strip whitespaces from the beginning of a string

Example:

``` php
echo $utf->ltrim($str); // displays '@TODO' 
```

### rtrim

**Strip whitespaces from the end of a string**

``` php
string rtrim ( string $str ) 
```

This function allows you to strip whitespaces from the end of a string

Example:

``` php
echo $utf->rtrim($str); // displays '@TODO' 
```

### trim

**Strip whitespaces from the beginning and end of a string**

``` php
string trim ( string $str ) 
```

This function allows you to strip whitespaces from the beginning and end of a string

Example:

``` php
echo $utf->trim($str); // displays '@TODO' 
```

### bom

**Return UTF-8 byte order mark ([BOM](http://en.wikipedia.org/wiki/Byte_order_mark "Wikipedia :: Byte order mark"))**

``` php
string bom (  ) 
```

Return the byte order mark (BOM) Unicode character used to signal the byte order of a text file or a stream. The BOM character may also indicate which of the several Unicode representations the text is encoded in. BOM use is optional, and, if used, should appear at the start of the text stream.

Example:

``` php
$bom = \UTF::instance()->bom();  // $bom == 0xefbbbf
echo '0x'.dechex(ord($bom[0])).dechex(ord($bom[1])).dechex(ord($bom[2]));  //  displays '0xefbbbf'
```

