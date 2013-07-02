# Quick Reference

## System Variables


### AGENT
**Type:** `string`

Auto-detected HTTP user agent, e.g. `Mozilla/5.0 (Linux; Android 4.2.2; Nexus 7) AppleWebKit/537.31`.


### AJAX
**Type:** `bool`

`TRUE` if an XML HTTP request is detected, `FALSE` otherwise.


### AUTOLOAD
**Type:** `string`

Search path for user-defined PHP classes that the framework will attempt to autoload at runtime. Accepts a pipe (`|`), comma (`,`), or semi-colon (`;`) as path separator.


### BASE
**Type:** `string`

Path to the `index.php` main/front controller.


### BODY
**Type:** `string`

HTTP request body for ReSTful post-processing.


### CACHE
**Type:** `bool|string`

Cache backend. Unless assigned a value like `'memcache=localhost'` (and the PHP memcache module is present), F3 auto-detects the presence of APC, WinCache and XCache and uses the first available PHP module if set to TRUE. If none of these PHP modules are available, a filesystem-based backend is used (default directory: `tmp/cache`). The framework disables the cache engine if assigned a `FALSE` value.


### CASELESS
**Type:** `bool`

Pattern matching of routes against incoming URIs is case-insensitive by default. Set to `FALSE` to make it case-sensitive.


### COOKIE, GET, POST, REQUEST, SESSION, FILES, SERVER, ENV
**Type:** `array`

Framework equivalents of PHP globals. Variables may be used throughout an application. However, direct use in templates is not advised due to security risks.


### DEBUG
**Type:** `integer`

Stack trace verbosity. Assign values 1 to 3 for increasing verbosity levels. Zero (0) suppresses the stack trace. This is the default value and it should be the assigned setting on a production server.


### DNSBL
**Type:** `string`

Comma-separated list of [DNS blacklist servers](http://whatismyipaddress.com/blacklist-check). Framework generates a `403 Forbidden` error if the user's IPv4 address is listed on the specified server(s).


### DIACRITICS
**Type:** `string`

Key-value pairs for foreign-to-ASCII character translations.


### ENCODING
**Type:** `string`

 Character set used for document encoding. Default value is `UTF-8`.


### ERROR
**Type:** `array`

Information about the last HTTP error that occurred. `ERROR.code` is the HTTP status code. `ERROR.title` contains a brief description of the error. `ERROR.text` provides greater detail. For HTTP 500 errors, use `ERROR.trace` to retrieve the stack trace.


### ESCAPE
**Type:** `bool`

Used to enable/disable auto-escaping.


### EXEMPT
**Type:** `string`

Comma-separated list of IPv4 addresses exempt from DNSBL lookups.


### FALLBACK
**Type:** `string`

Language (and dictionary) to use if no translation is available.


### HEADERS
**Type:** `rray`

HTTP request headers received by the server.


### HIGHLIGHT
**Type:** `bool`

Enable/disable syntax highlighting of stack traces. Default value: `TRUE` (requires `code.css` stylesheet).


### HOST
**Type:** `string`

Server host name. If `$_SERVER['SERVER_NAME']` is not available, return value of `gethostname()` is used.


### IP
**Type:** `string`

Remote IP address. The framework derives the address from headers if HTTP client is behind a proxy server.

### JAR
**Type:** `array`

Default cookie parameters. Consists of the following options:

* `expire` Unix timestamp, when the cookie should expire
* `path` The path on the server in which the cookie will be available on
* `domain` The domain that the cookie is available to
* `secure` only set the cookie, when a secure HTTPS connection exists
* `httponly` Make the cookie accessible only through the HTTP protocol.

### LANGUAGE
**Type:** `string`

Current active language. Value is used to load the appropriate language translation file in the folder pointed to by `LOCALES`. If set to `NULL`, language is auto-detected from the HTTP `Accept-Language` request header.


### LOCALES
**Type:** `string`

Location of the language dictionaries.


### LOGS
**Type:** `string`

Location of custom logs.


### ONERROR
**Type:** `mixed`

Callback function to use as custom error handler.


### PACKAGE
**Type:** `string`

Framework name.


### PARAMS
**Type:** `array`

Captured values of tokens defined in a `route()` pattern. `PARAMS.0` contains the captured URL relative to the Web root.


### PATTERN
**Type:** `string`

Contains the routing pattern that matches the current request URI.


### PLUGINS
**Type:** `string`

Location of F3 plugins. Default value is the folder where the framework code resides, i.e. the path to `base.php`.


### PORT
**Type:** `integer`

TCP/IP listening port used by the Web server.


### QUIET
**Type:** `bool`

Toggle switch for suppressing or enabling standard output and error messages. Particularly useful in unit testing.


### REALM
**Type:** `string`

Full canonical URL.


### RESPONSE
**Type:** `string`

The body of the last HTTP response. F3 populates this variable regardless of the `QUIET` setting.


### ROOT
**Type:** `string`

Absolute path to document root folder.


### ROUTES
**Type:** `array`

Contains the defined application routes.


### SCHEME
**Type:** `string`

Server protocol, i.e. `http` or `https`.


### SERIALIZER
**Type:** `string`

Default serializer. Normally set to `php`, unless PHP `igbinary` extension is auto-detected. Assign `json` if desired.


### TEMP
**Type:** `string`

Temporary folder for cache, filesystem locks, compiled F3 templates, etc. Default is the `tmp/` folder inside the Web root. Adjust accordingly to conform to your site's security policies.


### TZ
**Type:** `string`

Default timezone. Changing this value automatically calls the underlying `date_default_timezone_set()` function. See the [list of supported timezones](http://de2.php.net/manual/en/timezones.php) to get a possible value to use here.


### UI
**Type:** `string`

Search path for user interface files used by the `View` and `Template` classes' `render()` method. Default value is the Web root. Accepts a pipe (`|`), comma (`,`), or semi-colon (`;`) as separator for multiple paths.


### UNLOAD
**Type:** `callback`

Executed by framework on script shutdown.


### UPLOADS
**Type:** `string`

Directory where file uploads are saved.


### URI
**Type:** `string`

Current HTTP request URI.


### VERB
**Type:** `string`

Current HTTP request method.


### VERSION
**Type:** `string`

Framework version.



## Template Directives

### Token

*   `@token`

    Replace `@token` with value of equivalent F3 variable.

*   `{{ mixed expr }}`

    Evaluate. `expr` may include template tokens, constants, operators (unary, arithmetic, ternary and relational), parentheses, data type converters, and functions. If not an attribute of a template directive, result is echoed.

* `{{ string expr | raw }}`

    Render unescaped `expr`. F3 auto-escapes strings by default.

*   `{{ string expr | esc }}`

    Render escaped `expr`. This is the default framework behavior. The `| esc` suffix is only necessary if `ESCAPE` global variable is set to `FALSE`.

*   `{{ string expr, arg1, ..., argN | format }}`

    Render an ICU-formatted `expr` and pass the comma-separated arguments, where `arg1, ..., argn` is one of: `'date'`, `'time'`, `'number, integer'`, `'number, currency'`, or `'number, percent'`.

### Include

``` html
<include
    [ if="{{ bool condition }}" ]
    href="{{ string subtemplate }}"
/>
```

Get contents of `subtemplate` and insert at current position in template if optional condition is `TRUE`.

### Exclude

``` html
<exclude>text-block</exclude>
```

Remove `text-block` at runtime. Used for embedding comments in templates.
An Alias for this is:

```
{{* text-block *}}
```

### Ignore

``` html
<ignore>text-block</ignore>
```
Display `text-block` as-is, without interpretation/modification by the template engine.

### Check

``` html
<check if="{{ bool condition }}">
    <true>true-block</true>
    <false>false-block</false>
</check>
```
Evaluate condition. If `TRUE`, then `true-block` is rendered. Otherwise, `false-block` is used.
If there is no false block, the opening and closing tags for true are optional.

### Loop

``` html
<loop
    from="{{ statement }}"
    to="{{ bool expr }}"
    [ step="{{ statement }}" ]>
    text-block
</loop>
```
Evaluate `from` statement once. Check if the expression in the `to` attribute is `TRUE`, render `text-block` and evaluate `step` statement. Repeat iteration until `to` expression is `FALSE`.

### Repeat

``` html
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

``` html
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

## API Documentation

The framework API documentation is contained in `lib/api.chm` of the distribution package. F3 uses [Doxygen](http://www.stack.nl/~dimitri/doxygen/) to generate output in compiled HTML format. You need a CHM reader to view its tree-structured contents. For Mac users, there's [Chmox](http://chmox.sourceforge.net/) and [iChm](http://code.google.com/p/ichm/). Linux users have more choices: [xCHM](http://xchm.sourceforge.net/), [GnoCHM](http://gnochm.sourceforge.net/), [ChmSee](http://code.google.com/p/chmsee/), and [Kchmviewer](http://www.ulduzsoft.com/linux/kchmviewer/). Windows supports `.chm` files right out of the box.
