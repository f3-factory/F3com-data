# Web
The Web class contains several helpers to interact with clients and servers based on HTTP like down- and uploads

## Web

### mime
The mime method detects MIME type by a files extension. It's recommended to pass the type of a file to a download initiated by PHP.

```php
echo Web::instance()->mime('cat.jpg'); // returns 'image/jpeg'
```

If no known extension has been found, it will return 'application/octet-stream'.

### accceptable
Returns the list of acceptable MIME types of the browser.

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

### send
Transmits a file to the client and returns the file size on success

```php
$throttle = '8192' // measured in Kilobit per second; Default: 0
$mime = NULL; // let the framework detect the type; Default: NULL
$force_download = true // force the "save file..." dialog; Default: true

Web::instance()->send('data/documents.zip', $mime, $throttle, $force_download);
```

### receive
Receives and processes files from a client sent via PUT or POST.

```php
function callback() {
    echo 'file uploaded!';
}

$overwrite = false; // set to true, to overwrite an existing file; Default: false 
$slut = true; // rename file to filesystem-friendly version

Web::instance()->receive('callback', $overwrite, $slug);
```

Returns `true` on success.

The callback function will be called for every successfull download if specified.

### progress
Returns the progress of a file upload if `session.upload_progress.enabled` is set to `1` in the php.ini

```php
Web::instance()->progress($session_id);
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
