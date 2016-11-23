# Audit

The Audit class is a data validator.

Namespace: `\` <br>
File location: `lib/audit.php`

---


## Instantiation

**Return class instance**


```php
$audit = \Audit::instance();
```

The Audit class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.


## Methods


### url

**Return TRUE if string is a valid URL**

``` php
bool url ( string $str )
```

This function allows you to check if a given string is a valid URL.

Example:

``` php
$audit->url('http://fatfreeframework.com'); // returns TRUE
```

### email

**Return TRUE if string is a valid e-mail address; Check DNS MX records if specified**

``` php
bool email ( string $str [ , boolean $mx = TRUE ] )
```

This function allows you to check if a given string is a valid e-mail address.

If `$mx` is set to `TRUE` and the e-mail address is valid, this function will check the DNS MX records as well.

Example:

``` php
$audit->email('example@example.com', FALSE); // returns TRUE
$audit->email('example@example.com', TRUE); // returns FALSE
```

### ipv4

**Return TRUE if string is a valid IPV4 address**

``` php
bool ipv4 ( string $addr )
```

This function allows you to check if a given IP is a valid IPV4 address.

Example:

``` php
$audit->ipv4('178.7.35.202'); // returns TRUE
```

### ipv6

**Return TRUE if string is a valid IPV6 address**

``` php
bool ipv6 ( string $addr )
```

This function allows you to check if a given IP is a valid IPV6 address.

Example:

``` php
$audit->ipv6('2001:db8::1428:57ab'); // returns TRUE
```

### isprivate

**Return TRUE if IP address is within private range**

``` php
bool isprivate ( string $addr )
```

This function allows you to check if a given IP address is within a private range of addresses.

Example:

``` php
$audit->isprivate('192.168.0.1'); // returns TRUE
$audit->isprivate('178.7.35.202'); // returns FALSE
```

### isreserved

**Return TRUE if IP address is within reserved range**

``` php
bool isreserved ( string $addr )
```

This function allows you to check if a given IP address is within a reserved range of addresses.

Example:

``` php
$audit->isreserved('127.0.0.1'); // returns TRUE
$audit->isreserved('178.7.35.202'); // returns FALSE
```

### ispublic

**Return TRUE if IP address is neither private nor reserved**

``` php
bool ispublic ( string $addr )
```

This function allows you to check if a given IP address is neither private nor reserved.

Example:

``` php
$audit->ispublic('192.168.0.1');  // returns FALSE
$audit->ispublic('178.7.35.202'); // return TRUE
```

### isdesktop

**Return TRUE if the given user agent is a desktop browser**


``` php
bool isdesktop ( [ string $agent = NULL ] )
```

This function allows you to check if a given user agent is a desktop browser.

If no user agent is given, the check is performed against the current user-agent: [AGENT](quick-reference#AGENT).

Example:

``` php
$audit->isdesktop(); // returns TRUE if the current user-agent is a desktop browser

$audit->isdesktop('Mozilla/5.0 (Windows NT 6.1; WOW64; rv:31.0) etc..'); // returns TRUE
```

### ismobile

**Return TRUE if the given user agent is a mobile device**

``` php
bool ismobile ( [ string $agent = NULL ] )
```

This function allows you to check if a given user agent is a mobile device.

If no user agent is given, the check is performed against the current user-agent: [AGENT](quick-reference#AGENT).

Example:

``` php
$audit->ismobile(); // returns TRUE if the current user-agent is a mobile device 

$audit->ismobile('Mozilla/5.0 (BlackBerry; U; BlackBerry 9900; en) etc..'); // returns TRUE
```

### isbot

**Return TRUE if the given user agent is a Web bot**

If no user agent is given, the check is performed against the current user-agent: [AGENT](quick-reference#AGENT).

``` php
bool isbot ( [ string $agent = NULL ] )
```

This function allows you to check if a given user agent is a Web bot.

Example:

``` php
$audit->isbot(); // returns TRUE if the current user-agent is a Web bot.

$audit->isbot('Mozilla/5.0 (compatible; Googlebot/2.1 etc..'); // returns TRUE
```

### mod10

**Return TRUE if specified ID has a valid (Luhn) Mod-10 check digit**

``` php
bool mod10 ( string $id )
```

This function allows you to check if a specified ID has a valid ([Luhn](http://en.wikipedia.org/wiki/Luhn_algorithm "Luhn algorithm on Wikipedia")) Mod-10 check digit.

Example:

``` php
$audit->mod10('446667651'); // returns TRUE
$audit->mod10('123123123'); // returns FALSE
```

### card

**Return credit card type if number is valid**

``` php
string|FALSE card ( string $id )
```

This function allows you to retrieve the type of a credit card if the given number is valid. Returns `FALSE` otherwise.

Returned values for possible credit card types are: `'American Express'`, `'Diners Club'`, `'Discover'`, `'JCB'`, `'MasterCard'` & `'Visa'`

Example:

``` php
echo $audit->card('343760667618602'); // displays 'American Express'
```

### entropy

**Return entropy estimate of a password (NIST 800-63)**

``` php
int|float entropy ( string $str )
```

This function allows you to retrieve the entropy estimate of a given password as per [NIST Special Publication 800-63](http://en.wikipedia.org/wiki/Password_strength#NIST_Special_Publication_800-63 "Wikipedia :: NIST Special Publication 800-63").

Example:

``` php
$audit->entropy('sex'); // returns 8
$audit->entropy('secret'); // returns 14
$audit->entropy('password'); // returns 18
$audit->entropy('p4ss_w0rd'); // returns 19.5
$audit->entropy('dK2#!b846'); // returns 25.5
```
