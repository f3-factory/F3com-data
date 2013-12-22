# Audit : Data validator

The Audit class is a data validator.

---

Namespace: `\` <br>
File location: `lib/./lib/audit.php.php`

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>

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

This function allows you to return TRUE if string is a valid URL

Example:

``` php
echo $audit->url($str) // displays '@TODO' 
```

### email

**Return TRUE if string is a valid e-mail address; Check DNS MX records if specified**

``` php
bool email ( string $str [ , boolean $mx = FALSE ] ) 
```

This function allows you to return TRUE if string is a valid e-mail address; Check DNS MX records if specified

Example:

``` php
echo $audit->email($str, $mx) // displays '@TODO' 
```

### ipv4

**Return TRUE if string is a valid IPV4 address**

``` php
bool ipv4 ( string $addr ) 
```

This function allows you to return TRUE if string is a valid IPV4 address

Example:

``` php
echo $audit->ipv4($addr) // displays '@TODO' 
```

### ipv6

**Return TRUE if string is a valid IPV6 address**

``` php
bool ipv6 ( string $addr ) 
```

This function allows you to return TRUE if string is a valid IPV6 address

Example:

``` php
echo $audit->ipv6($addr) // displays '@TODO' 
```

### isprivate

**Return TRUE if IP address is within private range**

``` php
bool isprivate ( string $addr ) 
```

This function allows you to return TRUE if IP address is within private range

Example:

``` php
echo $audit->isprivate($addr) // displays '@TODO' 
```

### isreserved

**Return TRUE if IP address is within reserved range**

``` php
bool isreserved ( string $addr ) 
```

This function allows you to return TRUE if IP address is within reserved range

Example:

``` php
echo $audit->isreserved($addr) // displays '@TODO' 
```

### ispublic

**Return TRUE if IP address is neither private nor reserved**

``` php
bool ispublic ( string $addr ) 
```

This function allows you to return TRUE if IP address is neither private nor reserved

Example:

``` php
echo $audit->ispublic($addr) // displays '@TODO' 
```

### isdesktop

**Return TRUE if user agent is a desktop browser**

``` php
bool isdesktop (  ) 
```

This function allows you to return TRUE if user agent is a desktop browser

Example:

``` php
echo $audit->isdesktop() // displays '@TODO' 
```

### ismobile

**Return TRUE if user agent is a mobile device**

``` php
bool ismobile (  ) 
```

This function allows you to return TRUE if user agent is a mobile device

Example:

``` php
echo $audit->ismobile() // displays '@TODO' 
```

### isbot

**Return TRUE if user agent is a Web bot**

``` php
bool isbot (  ) 
```

This function allows you to return TRUE if user agent is a Web bot

Example:

``` php
echo $audit->isbot() // displays '@TODO' 
```

### mod10

**Return TRUE if specified ID has a valid (Luhn) Mod-10 check digit**

``` php
bool mod10 ( string $id ) 
```

This function allows you to return TRUE if specified ID has a valid (Luhn) Mod-10 check digit

Example:

``` php
echo $audit->mod10($id) // displays '@TODO' 
```

### card

**Return credit card type if number is valid**

``` php
string|FALSE card ( string $id ) 
```

This function allows you to return credit card type if number is valid

Example:

``` php
echo $audit->card($id) // displays '@TODO' 
```

### entropy

**Return entropy estimate of a password (NIST 800-63)**

``` php
int entropy ( string $str ) 
```

This function allows you to return entropy estimate of a password (NIST 800-63)

Example:

``` php
echo $audit->entropy($str) // displays '@TODO' 
```
