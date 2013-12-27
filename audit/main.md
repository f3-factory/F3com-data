# Audit : Data validator

The Audit class is a data validator.

Namespace: `\` <br>
File location: `lib/./lib/audit.php.php`

---

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This class is currently not documented; only its argument list is available.</p></div>

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
$audit->url($str); // returns TRUE or FALSE
```

### email

**Return TRUE if string is a valid e-mail address; Check DNS MX records if specified**

``` php
bool email ( string $str [ , boolean $mx = FALSE ] ) 
```

This function allows you to check if a given string is a valid e-mail address. 

If `$mx` is set to `TRUE` and the e-mail address is valid, this function will check the DNS MX records as well. 

Example:

``` php
$audit->email($str, $mx); // returns TRUE or FALSE
```

### ipv4

**Return TRUE if string is a valid IPV4 address**

``` php
bool ipv4 ( string $addr ) 
```

This function allows you to check if a given IP is a valid IPV4 address. 

Example:

``` php
$audit->ipv4($addr); // returns TRUE or FALSE
```

### ipv6

**Return TRUE if string is a valid IPV6 address**

``` php
bool ipv6 ( string $addr ) 
```

This function allows you to check if a given IP is a valid IPV6 address. 

Example:

``` php
$audit->ipv6($addr); // returns TRUE or FALSE
```

### isprivate

**Return TRUE if IP address is within private range**

``` php
bool isprivate ( string $addr ) 
```

This function allows you to check if a given IP address is within a private range of addresses. 

Example:

``` php
$audit->isprivate($addr); // returns TRUE or FALSE
```

### isreserved

**Return TRUE if IP address is within reserved range**

``` php
bool isreserved ( string $addr ) 
```

This function allows you to check if a given IP address is within a reserved range of addresses. 

Example:

``` php
$audit->isreserved($addr); // returns TRUE or FALSE
```

### ispublic

**Return TRUE if IP address is neither private nor reserved**

``` php
bool ispublic ( string $addr ) 
```

This function allows you to check if a given IP address is neither private nor reserved. 

Example:

``` php
$audit->ispublic($addr); // returns TRUE or FALSE
```

### isdesktop

**Return TRUE if user agent is a desktop browser**

``` php
bool isdesktop (  ) 
```

This function allows you to check if a given user agent is a desktop browser. 

Example:

``` php
$audit->isdesktop(); // returns TRUE or FALSE
```

### ismobile

**Return TRUE if user agent is a mobile device**

``` php
bool ismobile (  ) 
```

This function allows you to check if a given user agent is a mobile device. 

Example:

``` php
$audit->ismobile(); // returns TRUE or FALSE
```

### isbot

**Return TRUE if user agent is a Web bot**

``` php
bool isbot (  ) 
```

This function allows you to check if a given user agent is a Web bot. 

Example:

``` php
$audit->isbot(); // returns TRUE or FALSE
```

### mod10

**Return TRUE if specified ID has a valid (Luhn) Mod-10 check digit**

``` php
bool mod10 ( string $id ) 
```

This function allows you to check if a specified ID has a valid (Luhn) Mod-10 check digit. 

Example:

``` php
$audit->mod10($id); // returns TRUE or FALSE
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
echo $audit->card($customer_card_id); // displays e.g. 'Discover'
```

### entropy

**Return entropy estimate of a password (NIST 800-63)**

``` php
int|float entropy ( string $str ) 
```

This function allows you to retrieve the entropy estimate of a given password as per [NIST Special Publication 800-63](http://en.wikipedia.org/wiki/Password_strength#NIST_Special_Publication_800-63 "Wikipedia :: NIST Special Publication 800-63") . 

Example:

``` php
echo $audit->entropy($pretty_good_customer_password); // displays e.g. '28.5' 
```
