# Quick Reference

## System Variables


### AGENT
**Type:** `string`, `Read-only`

A string containing the auto-detected HTTP user agent, e.g. `'Mozilla/5.0 (Linux; Android 4.2.2; Nexus 7) AppleWebKit/537.31'`


### AJAX
**Type:** `bool`, `Read-only`

`TRUE` if an XML HTTP request is detected, `FALSE` otherwise. Default value: Result of the expression `$headers['X-Requested-With']=='XMLHttpRequest'`


### AUTOLOAD
**Type:** `string` &nbsp; &nbsp; **Default:** `'./'`

Search path(**s**) for user-defined PHP classes that the framework will attempt to autoload at runtime. When specifying multiple paths, you can use a pipe (`|`), comma (`,`), or semi-colon (`;`) as path separator. Remember paths must end with a `'/'`. e.g. `$f3->set('AUTOLOAD', 'app/;inc/,./');`


### BASE
**Type:** `string`, `Read-only` &nbsp; &nbsp; **Default:** auto-detected

Path to the `index.php` main/front controller.


### BODY
**Type:** `string`, `Read-only`

HTTP request body for ReSTful post-processing. Contains the `php://input` stream used by PUT requests, if RAW is `false`.


### CACHE
**Type:** `bool|string` &nbsp; &nbsp; **Default:** `FALSE`

Cache backend. When set to `TRUE`, and unless you assign a configuration value like `'memcache=localhost'` (and the [PHP memcache module](http://www.php.net/manual/en/intro.memcache.php "Information on the Memcache module") is available), F3 auto-detects the presence of APC, WinCache and XCache and then uses the first available PHP module. If none of these PHP modules is available, a filesystem-based backend is used (default directory: `tmp/cache`). The framework doesn't use any cache engine when a `FALSE` value is assigned.


### CASELESS
**Type:** `bool` &nbsp; &nbsp; **Default:** `TRUE`

Pattern matching of routes against incoming URIs is case-insensitive by default. Set to `FALSE` to make it case-sensitive.


### COOKIE, GET, POST, REQUEST, SESSION, FILES, SERVER, ENV
**Type:** `array`

Framework equivalents of PHP globals. For your convenience, F3 automatically synchronizes these variables with the underlying PHP globals.
These variables may be used throughout an application. However, direct use in templates is not advised due to security risks.


### DEBUG
**Type:** `integer` &nbsp; &nbsp; **Default:** `0`

Verbosity level of the stack trace. Assign values between 0 to 3 for increasing verbosity levels as follow:

* 0 : suppresses logs of the stack trace.
* 1 : logs files & lines.
* 2 : logs classes & functions as well.
* 3 : logs detailed infos of the objects as well.

<i class="icon-warning-sign"></i> _**NOTICE:** Only the default value of &nbsp;`0` should be used on production servers_.


### DIACRITICS
**Type:** `array` &nbsp; &nbsp; **Default:** `array()`, empty array

Additional key-value pairs for foreign-to-ASCII character translations.


### DNSBL
**Type:** `string` &nbsp; &nbsp; **Default:** `''`, empty string

Comma-separated list of [DNS blacklist servers](http://whatismyipaddress.com/blacklist-check "Blacklist Check List"). Framework generates a `403 Forbidden` error if the user's IPv4 address is listed on the specified server(s).


### EMOJI
**Type:** `array` &nbsp; &nbsp; **Default:** `array()`, empty array

Additional key-value pairs of emoji tokens to add to the basic set used when translating a string to Unicode font-supported symbols. (see [`\UTF->emojify()`](utf-unicode-string-manager#emojify))


### ENCODING
**Type:** `string` &nbsp; &nbsp; **Default:** `'UTF-8'`

Character set used for [document encoding](views-and-templates#document-encoding).


### ERROR
**Type:** `array`, `Read-Only`

Information about the last HTTP error that occurred:

* `ERROR.code` is the [HTTP status code](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html "List of the HTTP Status Code Definitions"). e.g. `307`
* `ERROR.status` is a brief description of the HTTP status code. e.g. `'Temporary Redirect'`
* `ERROR.title` contains a brief description of the error.
* `ERROR.text` provides greater detail.
* `ERROR.trace` is used for HTTP 500 errors, to retrieve the stack trace. `array()`


### ESCAPE
**Type:** `bool` &nbsp; &nbsp; **Default:** `TRUE`

Used to enable/disable auto-escaping [@tokens](/quick-reference#token) used in templates.


### EXEMPT
**Type:** `string` &nbsp; &nbsp; **Default:** `NULL`

Comma-separated list of IPv4 addresses to exempt from DNSBL lookups.


### FALLBACK
**Type:** `string` &nbsp; &nbsp; **Default:** `'en'`

Language (and dictionary) to use if no translation is available.


### HALT
**Type:** `bool` &nbsp; &nbsp; **Default:** `TRUE`

If `TRUE`, the framework, after having logged stack trace and errors, stops execution (`die` without any status) when a _non-fatal_ error is detected.


### HEADERS
**Type:** `array`,`Read-Only`

HTTP request headers received by the server. e.g. (simplified)

```
array ( 
'Host' => 'fatfreeframework.com'
'Accept-Encoding' => 'gzip,deflate,sdch',
'Accept-Language' => 'en-US,en;q=0.8,ja;q=0.6'
)
```

### HIGHLIGHT
**Type:** `bool` &nbsp; &nbsp; **Default:** `TRUE`

Enable/disable syntax highlighting of stack traces. When enabled, requires `code.css` stylesheet.


### HOST
**Type:** `string`,`Read-Only`

Server host name.


### IP
**Type:** `string`,`Read-Only`

Remote IP address. The framework derives the address from headers if HTTP client is behind a proxy server. Default value: First match of `Client-IP` then `X-Forwarded-For` then `$_SERVER['REMOTE_ADDR']`, otherwise set to `''`

### JAR
**Type:** `array`

Default cookie parameters. Consists of the following options:

* `expire` Unix timestamp, when the cookie should expire. Default: `0`  
* `path` The path on the server in which the cookie will be available. Default: `'/'`
* `domain` The domain that the cookie is available to. Default: `$_SERVER['SERVER_NAME']` if available, else `''`
* `secure` Set the cookie when a secure HTTPS connection exists. Default: `$_SERVER['HTTPS']=='on'`
* `httponly` Make the cookie accessible only through the HTTP protocol. Default: `TRUE`

You can refer to [session_set_cookie_params() in the PHP Manual](http://www.php.net/manual/en/function.session-set-cookie-params.php) for more information.

### LANGUAGE
**Type:** `string` &nbsp; &nbsp; **Default:** auto-detected

Current active language(s). Value is used to load the appropriate language(s) translation file(s) in the folder pointed to by `LOCALES`. Default: auto-detected from the HTTP `Accept-Language` request header, e.g. `'en-US,en,es'`.
If set to `NULL`, language is auto-detected from the HTTP `Accept-Language` request header.

### LOCALES
**Type:** `string` &nbsp; &nbsp; **Default:** `'./'`

Location of the language(s) dictionaries.


### LOGS
**Type:** `string` &nbsp; &nbsp; **Default:** `'./'`

Location of custom logs.


### ONERROR
**Type:** `mixed` &nbsp; &nbsp; **Default:** `NULL`

Callback function to use as custom error handler, or `NULL`. 
**Notice**: If no callback function is specified, a default error page is generated (HTML5 for synchronous requests, JSON string for AJAX requests).

### PACKAGE
**Type:** `string` &nbsp; &nbsp; **Default:** `'Fat-Free Framework'`

A string containing the name of the Framework.


### PARAMS
**Type:** `array` &nbsp; &nbsp; **Default:** `array()`

Captured values of tokens defined in a `route()` pattern. `PARAMS[0]` contains the captured URL relative to the Web root.


### PATH
**Type:** `string`, `Read-Only`

The URL relative to BASE. Default value: `parse_url($_SERVER['REQUEST_URI'],PHP_URL_PATH)`


### PATTERN
**Type:** `string`, `Read-Only`

Contains the routing pattern that matches the current request URI.


### PLUGINS
**Type:** `string` &nbsp; &nbsp; **Default:** `&#95;&#95;DIR&#95;&#95;.'/'`

Location of F3 plugins. The default value is the folder where the framework code resides, i.e. the path to `base.php`.


### PORT
**Type:** `integer`, `Read-Only`

TCP/IP listening port used by the Web server. Default value: `$_SERVER['SERVER_PORT']` or `NULL` if not available.


### PREFIX
**Type:** `string` &nbsp; &nbsp; **Default:** `NULL`

Prefix to use with LANGUAGE and LOCALES.


### QUIET
**Type:** `bool` &nbsp; &nbsp; **Default:** `FALSE`

Toggle switch for suppressing or enabling standard output and error messages. Particularly useful in [unit testing](unit-testing).


### REALM
**Type:** `string`, `Read-Only`

Full canonical URL. Default value: Result of `'http(s)://'.$_SERVER['SERVER_NAME'].$_SERVER['REQUEST_URI']`


### RESPONSE
**Type:** `string`, `Read-Only`

The body of the last HTTP response. F3 populates this variable regardless of the `QUIET` setting.


### ROOT
**Type:** `string`, `Read-Only`

Absolute path to document root folder.


### ROUTES
**Type:** `array` &nbsp; &nbsp; **Default:** `array()`

Contains the defined application routes. 

**Notice**: A route is more than just a URL. It's an HTTP verb (or verbs) _AND_ a URL

### SCHEME
**Type:** `string`, `Read-Only`

Server protocol. Default: `'http'` or `'https'`


### SERIALIZER
**Type:** `string` &nbsp; &nbsp; **Default:** auto-detected

Define the default serializer used by the [Base->serialize() method](base#serialize "Definition and usage of the Base->serialize method"). Default value: `igbinary` if available, otherwise set to `php`. Default: First match of `'igbinary'` then `'php'`


### TEMP
**Type:** `string` &nbsp; &nbsp; **Default:** `'tmp/'`

Temporary folder for cache, filesystem locks, compiled F3 templates, etc. The default value is the `'tmp/'` folder inside the Web root. _Adjust accordingly to conform to your site's security policies_.


### TIME
**Type:** `float` &nbsp; &nbsp; **Default:** auto-detected

Starting time of the framework. Default value: The current Unix time in seconds accurate to the nearest microsecond as per the [PHP function microtime(**TRUE**)](http://php.net/microtime).


### TZ
**Type:** `string` &nbsp; &nbsp; **Default:** auto-detected

Timezone to use. Changing this value automatically calls the underlying PHP function `date_default_timezone_set()`. See the [list of supported timezones](http://php.net/manual/en/timezones.php) to get a possible value to use here. Falls back to `'UTC'` if auto-detection fails.


### UI
**Type:** `string` &nbsp; &nbsp; **Default:** `'./'`

Search path for user interface files used by the `View` and `Template` classes' `render()` method.<br>Accepts a pipe (`|`), comma (`,`), or semi-colon (`;`) as separator for multiple paths.


### UNLOAD
**Type:** `callback` &nbsp; &nbsp; **Default:** `NULL`

Defines the shutdown handler the framework will executed on [application shutdown](base#unload).


### UPLOADS
**Type:** `string` &nbsp; &nbsp; **Default:** `'./'`

Directory where file uploads are saved.


### URI
**Type:** `string` &nbsp; &nbsp; **Default:** auto-detected

A reference to the current HTTP request URI.


### VERB
**Type:** `string` &nbsp; &nbsp; **Default:** auto-detected

A reference to the current HTTP request method.


### VERSION
**Type:** `string` &nbsp; &nbsp; **Default:** e.g. `'3.2.1-Release'`

A string containing the version of the Framework.

## Template Directives

### Token

*   `@token`

    Replace `@token` with the value of the equivalent F3 variable.

*   `{{ mixed expr }}`

    Evaluate expression `expr`. Expression can include template tokens, constants, operators (unary, arithmetic, ternary and relational), parentheses, data type converters, and functions. _If not an attribute of a template directive, the result is echo'ed_.

*   `{{ string expr | esc }}`

    Render expression `expr` _escaped_. This is the default framework behavior. The `| esc` suffix is only necessary if the [ESCAPE](/quick-reference#error) global variable has been set to `FALSE`.

*   `{{ string expr | raw }}`

    Render expression `expr` _unescaped_. As F3 auto-escapes string tokens by default, you can use this suffix to by-pass the escaping of a particular token.

*   `{{ string expr, arg1, ..., argN | format }}`

    Render the expression `expr` in ICU-format and pass the comma-separated arguments, where `arg1, ..., argn` is one of: `'date'`, `'time'`, `'number, integer'`, `'number, currency'`, `'number, percent'`, or `'number, decimal'`. Have a look at the [format](base#format) method for additional usage examples. (<small>More information about [ICU formatting of Numbers, Currencies, Dates and Times](http://userguide.icu-project.org/formatparse "International Components for Unicode Formatting and Parsing")</small>).

### Include

```html
<include
    [ if="{{ bool condition }}" ]
    href="{{ string subtemplate }}"
/>
```

Get contents of `subtemplate` and insert at current position in template [ if optional condition is `TRUE` ].

### Exclude

```html
<exclude>text-block</exclude>
```
Exclude `text-block` at runtime. Used for embedding comments in templates. An Alias for this is:

```
{{* text-block *}}
```

### Ignore

```html
<ignore>text-block</ignore>
```
Display `text-block` as it is, without any interpretation/modification by the template engine.

### Check

```html
<check if="{{ bool condition }}">
    <true>true-block</true>
    <false>false-block</false>
</check>
```
Evaluate `condition`. If `TRUE`, the `true-block` is rendered; else the `false-block` is rendered.

***Short form***: If you don't need and don't specify a false block, then, for your convenience, F3 makes the opening and closing tags for true optional:

```html
<check if="{{ @debugmode && @showtrace }}"><code>{{ @showtrace }}</code></check>
```

### Loop

```html
<loop
    from="{{ statement }}"
    to="{{ bool expr }}"
    [ step="{{ statement }}" ]>
    text-block
</loop>
```
Evaluate `from` statement once. Check if the expression in the `to` attribute is `TRUE`, render `text-block` and evaluate `step` statement. Repeat iteration until `to` expression is `FALSE`.

### Repeat

```html
<repeat
    group="{{ array @group|expr }}"
    [ key="{{ scalar @key }}" ]
    value="{{ mixed @value }}
    [ counter="{{ scalar @key }}" ]>
    text-block
</repeat>
```
Repeat `text-block` as many times as there are elements in the array variable `@group` or the expression `expr`. `@key` and `@value` function in the same manner as the key-value pair in the equivalent PHP `foreach()` statement. Variable represented by `key` in `counter` attribute increments by `1` with every iteration.

### Switch

```html
<switch expr="{{ scalar expr }}">
    <case value="{{ scalar @value|expr }}" break="{{ bool TRUE|FALSE }}">
        text-block
    </case>
    .
    .
    .
    <default>
        message
    </default>
</switch>
```
Equivalent of the PHP switch-case jump table structure.

### Set

```html
<!-- set some variables -->
<set foo="{{ 1+2 }}" bar="{{ @foo+3 }}" baz="xyz" />
<!-- set an array -->
<set myarray="{{ array('a','b','c') }}" />
```
Used to set some variables dynamically within the template.

## API Documentation

The framework API documentation is also contained in `lib/api/index.html` of the distribution package. F3 uses [Doxygen](http://www.stack.nl/~dimitri/doxygen/ "Doxygen is a tool for generating documentation from annotated source code") to generate the documentation in HTML format.
