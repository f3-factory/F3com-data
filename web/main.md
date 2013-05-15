# Web
The Web class contains several helpers to interact with clients and servers based on HTTP like down- and uploads

### mime

**Detect MIME type using file extension**

``` php
$web->mime( string $file ); string
```

Detects MIME type by comparing the file extension against a predefined array. This is more save than using Fileinfo, which is not available on some server setups and would be more costly in terms of performance. 
In the following example, we see how to set the right response header content type to display an image by PHP.

```php
$web = \Web::instance();

$file = 'ui/images/south-park.jpg';
$mime = $web->mime($file); // returns 'image/jpeg'
header('Content-Type: '.$mime);
echo $f3->read($file);
```

If no known extension has been found, it will return 'application/octet-stream'.

### acceptable

**Returns the list of acceptable MIME types of the browser.**

``` php
$web->acceptable([ string|array $list = NULL ]); array | string | false
```

It returns the MIME types stated in the HTTP Accept header as an array.

```php
print_r(Web::instance()->acceptable();

/* 
Returns for example

Array
(
    [text/html] => 1
    [application/xhtml+xml] => 1
    [application/xml] => 0.9
)
*/
```

If a list of MIME types is specified, it returns the best match, or FALSE if none found.

``` php
$web->acceptable(array('application/xml','application/xhtml+xml')); // returns application/xhtml+xml
```



### send

**Transmits a file to the client and returns the file size on success**

``` php
$web->send( string $file, [ string $mime = NULL ], [ int $kbps = 0 ], [ bool $force = TRUE ]); int | false
```

The 2nd argument `$kbps` defines the throttle speed, measured in Kilobits per second, if you like to slow down the file download for the user.

In `$mime` argument you can explicitly set a mime type. Leave it to `NULL` to let the framework detect the type.

Use `$force` to force the "save file..." download dialog, otherwise it would just display the file in some cases, because an image or a .pdf or .mp3 file can be displayed by the browser directly and will not prompt you to download it.

Example:

```php
$throttle = '8192' // throttle the download to 1024 Kilobytes per second
$mime = NULL; // let the framework detect the type

$web->send('data/documents.zip', $mime, $throttle);
```



### receive

**Receives and processes files from a client sent via PUT or POST.**

``` php
$web->receive([ callback $func = NULL ], [ bool $overwrite = FALSE], [ bool $slug = TRUE ]); int | false
```

This function fetches the user uploaded file(s) and move it into a directory, specified in [UPLOADS](quick-reference#uploads) system var. It returns `true` on success.

You can set an optional upload handler function to validate uploaded files, before moving them to their desired location. Lets have a look at this:

``` php
$f3->set('UPLOADS','uploads/'); // don't forget to set an Upload directory, and make it writable!

$overwrite = false; // set to true, to overwrite an existing file; Default: false
$slug = true; // rename file to filesystem-friendly version

$web->receive( function($file){
        var_dump($file);
        /* looks like:
          array(5) {
              ["name"]=>
              string(19) "csshat_quittung.png"
              ["type"]=>
              string(9) "image/png"
              ["tmp_name"]=>
              string(14) "/tmp/php2YS85Q"
              ["error"]=>
              int(0)
              ["size"]=>
              int(172245)
            }
        */

        // maybe you want to check the file size
        if($file['size'] > (2 * 1024 * 1024)) // if bigger than 2 MB
            return false; // this file is not valid, return false will not move it

        // everything went fine, hurray!
    },
    $overwrite,
    $slug
);
```

A callback can also be another function or class method. Have a look at the [call()](base#call) function description to see all possibilities.

```php
function callback() {
    echo 'file uploaded!';
}

Web::instance()->receive('callback', false, true);
```

<div class="alert alert-info">
    <b>Notice:</b> The callback function will be called for every successful upload if specified.
</div>



### progress

**Returns the progress of a file upload if `session.upload_progress.enabled` is set to `1` in the php.ini**

```php
Web::instance()->progress( string $session_id ); int | false
```

The `$session_id` ist the "key" of the `$_SESSION` array and is necessary to retrieve the status of a specific user. Please read the [PHP Docs](http://php.net/manual/session.upload-progress.php) for more information.



### _curl
Perform HTTP requests via cURL. It's a protected method and is used by `Web->request()`.

Returns an array containing the content and the header.

### _stream
Perform HTTP requests via PHP stream wraper. It's a protected method and is used by `Web->request()`.

Returns an array containing the content and the header.

### _socket
Perform a HTTP request on low-level via TCP/IP sockets. It's a protected method and is used by `Web->request()`.

Returns an array containing the content and the header.

### engine
Sets the engine to be used by `Web->request()`. Possible values are

* curl
* stream
* socket (default)

### subst
Replace old headers with new elements

### request
Perform a HTTP request

### minify
Minify CSS and Javascript files by strippping whitespaces and comments. Returns a combined output as a string.

```php
$minified = Web::instance()->minify('style.css,framework.css,null.css'); 
```

To learn more about `minify` you may check the [User Guide](http://fatfreeframework.com/optimization#keeping-javascript-and-css-on-a-healthy-diet)

### rss
Parse a RSS feed and returns an array of all tags if not specified.

```php
$count = 20; // Default: 10
$tags = array('title', 'link', 'pubDate'); // Default: NULL (all tags)

Web::instance()->rss('http://example.org/feed.rss', $count, $tags);
```

### slug
Method to return a URL- and filesystem-friendly string. 

```php
echo Web::instance()->slug('ĤÈĹĹŌ');

// returns: HELLO
```

### gzdecode
Decodes a gzip-compressed string if `gzdecode()` is available
