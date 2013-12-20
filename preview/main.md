# Preview

Preview is a lightweight template engine class that extends the [View](view) class.

---

Namespace: `\` <br/>
File location: `lib/base.php`

<div class="alert alert-error">
<h2 style="text-align:center">Warning</h2>
<p>This function is currently not documented; only its argument list is available.</p>
</div>

## Why a Preview class?


## Render a Preview

The Preview class makes it easy to separate the two steps defined above, since it requires rendering a filename and a data hive:

``` php
string render ( string $file [, string $mime = 'text/html' [, array $hive = NULL [, $ttl = 0 ]]] )
```


## Methods

### token

**Convert token to variable**

``` php
string token ( string $str )
```

Example:

``` php
 // 
```

### build

**Assemble markup**

``` php
string build ( build $node )
```

Example:

``` php
 // 
```

### resolve

**Render template string**

``` php
string resolve ( string $str [, array $hive = NULL ] )
```

Example:

``` php
 // 
```

### render

**Render template**

``` php
string render ( string $file [, string $mime = 'text/html' [, array $hive = NULL [, $ttl = 0 ]]] )
```

