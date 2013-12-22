# SMTP : SMTP plug-in

The SMTP class is a SMTP plug-in.

---

Namespace: `\` <br>
File location: `lib/smtp.php`

<div class="alert alert-error"><h4 style="text-align:center">Warning</h4>
<p>This function is currently not documented; only its argument list is available.</p></div>

## Instantiation

**Return class instance**

```php
$smtp = new SMTP ( $host, $port, $scheme, $user, $pw );

```
The SMTP class extends the [Magic](magic) class.
Please refer to the [__construct](smtp#&#95;&#95;construct) method for details.



## Methods


### fixheader

**Fix header**

``` php
protected string fixheader ( string $key ) 
```

This function allows you to fix header 

Example:

``` php

echo $smtp->fixheader($key) // displays '@TODO'


```

### exists

**Return TRUE if header exists**

``` php
bool exists ( $key ) 
```

This function allows you to return TRUE if header exists 

Example:

``` php

echo $smtp->exists($key) // displays '@TODO'


```

### set

**Bind value to e-mail header**

``` php
string set ( string $key, string $val ) 
```

This function allows you to bind value to e-mail header 

Example:

``` php

echo $smtp->set($key, $val) // displays '@TODO'


```

### get

**Return value of e-mail header**

``` php
string|NULL get ( string $key ) 
```

This function allows you to return value of e-mail header 

Example:

``` php

echo $smtp->get($key) // displays '@TODO'


```

### clear

**Remove header**

``` php
NULL clear ( string $key ) 
```

This function allows you to remove header 

Example:

``` php

echo $smtp->clear($key) // displays '@TODO'


```

### log

**Return client-server conversation history**

``` php
string log (  ) 
```

This function allows you to return client-server conversation history 

Example:

``` php

echo $smtp->log() // displays '@TODO'


```

### dialog

**Send SMTP command and record server response**

``` php
protected NULL dialog ( [ string $cmd = NULL, bool $log = NULL ] ] ) 
```

This function allows you to send SMTP command and record server response 

Example:

``` php

echo $smtp->dialog($cmd, $log) // displays '@TODO'


```

### attach

**Add e-mail attachment**

``` php
NULL attach ( $file ) 
```

This function allows you to add e-mail attachment 

Example:

``` php

echo $smtp->attach($file) // displays '@TODO'


```

### send

**Transmit message**

``` php
bool send ( string $message ) 
```

This function allows you to transmit message 

Example:

``` php

echo $smtp->send($message) // displays '@TODO'


```

### __construct

**Instantiate class**

``` php
__construct ( string $host, int $port, string $scheme, string $user, string $pw ) 
```

This function allows you to instantiate class 

Example:

``` php
$smtp = new SMTP ( $host, $port, $scheme, $user, $pw )

```
