# Web
The Web class (and its descendants, [Geo](geo) and [Pingback](pingback)) contains several helpers to interact with HTTP clients and servers.

Namespace: `\` <br/>
File location: `lib/web.php`

---

### instance

**Return class instance**

``` php
$web = \Web::instance();
```

The Web class uses the [Prefab](prefab) factory wrapper, so you can grab the same instance of that class at any point of your code.


### mime

**Detect MIME type using file extension**

``` php
$web->mime( string $file ); string
```

Detects MIME type by comparing the file extension against a predefined array. Use this as a lightweight alternative to Fileinfo, which is not available on some server setups and would be more costly in terms of performance.
In the following example, we see how to set the right response header content type to display an image by PHP.

```php
$web = \Web::instance();
$file = 'ui/images/south-park.jpg';
$mime = $web->mime($file); // returns 'image/jpeg'

header('Content-Type: '.$mime);
echo $f3->read($file);
```

If an extension is not known to this method, it will return 'application/octet-stream'.

### acceptable

**Returns the list of acceptable MIME types of the browser.**

``` php
$web->acceptable([ string|array $list = NULL ]); array | string | false
```

It returns the MIME types stated by the browser in the HTTP Accept header as an array.

```php
print_r(Web::instance()->acceptable());

/*
Returns for example:

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

The 2nd argument `$kbps` defines the throttle speed, measured in Kilobits per second, if you like to limit the download bandwidth for the user.

In `$mime` argument you can explicitly set a mime type. Leave it to `NULL` to let the framework detect the type.

Use `$force` to force the "save file..." download dialog, otherwise it would just display the file in some cases, because an image or a .pdf or .mp3 file can be displayed by the browser directly and will not prompt you to download it.

Example:

```php
$throttle = 32; // throttle the download to 32Kbps
$mime = NULL; // let the framework detect the type

$web->send('data/documents.zip', $mime, $throttle);
```


### receive

**Receive and process files from a client sent via PUT or POST.**

``` php
$web->receive([ callback $func = NULL ], [ bool $overwrite = FALSE], [ bool $slug = TRUE ]); int | false
```

This function fetches the user uploaded file(s) and move it into a directory, specified in [UPLOADS](quick-reference#uploads) system var. It returns `true` on success.

You can set an optional upload handler function to validate uploaded files, before moving them to their desired location. Lets have a look at this:

``` php
$f3->set('UPLOADS','uploads/'); // don't forget to set an Upload directory, and make it writable!

$overwrite = false; // set to true, to overwrite an existing file; Default: false
$slug = true; // rename file to filesystem-friendly version

$web->receive(function($file){
        var_dump($file);
        /* looks like:
          array(5) {
              ["name"] =>     string(19) "csshat_quittung.png"
              ["type"] =>     string(9) "image/png"
              ["tmp_name"] => string(14) "/tmp/php2YS85Q"
              ["error"] =>    int(0)
              ["size"] =>     int(172245)
            }
        */
        // maybe you want to check the file size
        if($file['size'] > (2 * 1024 * 1024)) // if bigger than 2 MB
            return false; // this file is not valid, return false will skip moving it

        // everything went fine, hurray!
    },
    $overwrite,
    $slug
);
```
<div class="alert alert-info">
    <b>Notice:</b> Having trouble to get this working? Don't forget to set the <b>enctype="multipart/form-data"</b> attribute in your <b>&lt;form&gt;</b> tag.
</div>

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

The `$session_id` is the "key" of the `$_SESSION` array and is necessary to retrieve the status of a specific user. Please read the [PHP Docs](http://php.net/manual/session.upload-progress.php) for more information.



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

**Specify the HTTP request engine to use**

``` php
$web->engine([ string $arg = 'socket' ]); string
```

Sets the engine to be used by `Web->request()`. If the selected engine is not available, it falls back to an applicable substitute.
Possible values are:

* curl
* stream
* socket (default)

<div class="alert alert-info">
    <b>Notice:</b> The cURL and stream wrapper engines need their appropriate php extension to be installed and activated to work properly. Otherwise, sockets are used.
</div>

### subst

**Replace old headers with new elements**

``` php
$web->subst( array &$old , string|array $new ); NULL
```



### request

**Perform a HTTP request**

``` php
$web->request( string $url, [ array $options = NULL ]); array | false
```

You can use [HTTP context options](http://www.php.net/manual/en/context.http.php) to configure the request for your needs.
This way you can build request of every type, including the usage of cookies or auth mechanisms, proxy, user-agent, and so on.
The requested page will be cached as instructed by the remote server.

Returns an array containing the content and the header. For example, let us have a look at a simple GET request to download a remote file:

``` php
$result = \Web::instance()->request('http://www.golem.de/1303/98339-55766-i_rc.jpg');
var_dump( $result );
/* returns:

array(4) {
    ["body"] => string(7761) " IMAGE BINARY DATA "
    ["headers"]=>
        array(11) {
          [0] =>  string(15) "HTTP/1.1 200 OK"
          [1] =>  string(13) "Server: nginx"
          [2] =>  string(35) "Date: Thu, 16 May 2013 06:00:33 GMT"
          [3] =>  string(24) "Content-Type: image/jpeg"
          [4] =>  string(20) "Content-Length: 7761"
          [5] =>  string(17) "Connection: close"
          [6] =>  string(44) "Last-Modified: Fri, 22 Mar 2013 09:41:02 GMT"
          [7] =>  string(38) "Expires: Sun, 16 Jun 2013 06:00:33 GMT"
          [8] =>  string(30) "Cache-Control: max-age=2678400"
          [9] =>  string(19) "X-Cache-Status: HIT"
          [10] => string(20) "Accept-Ranges: bytes"
        }
    ["engine"] => string(6) "socket"
    ["cached"] => bool(false)
}
*/
```

It's easy as that!

Let's move on to some more advanced examples.



#### GET

Perform a GET request with URL parameters:

``` php
$url = 'http://www.mydomain.com/index.php';
$params = array(
    'parameter1' => 'value1',
    'parameter2' => 'value2'
);
$options = array('method' => 'GET');
$url .= '?'.http_build_query($params);

$result = \Web::instance()->request($url, $options);
```



#### POST

Perform a POST request with POST data.

``` php
$url = 'http://www.mydomain.com/index.php';

$postVars = array(
    'parameter1' => 'value1',
    'parameter2' => 'value2'
);
$options = array(
    'method'  => 'POST',
    'content' => http_build_query($postVars),
);
$result = \Web::instance()->request($url, $options);
```



#### PUT

Upload a file via PUT request.

``` php
$f3 = \Base__instance();
$web = \Web::instance();

$url = 'http://www.mydomain.com/upload/';
$file = '/path/to/myFile.zip';

$options = array(
    'method'  => 'PUT',
    'content' => $f3->read($file),
    'header' => array('Content-Type: '.$web->mime($file));
);

$result = $web->request($url, $options);
```


### minify

**Minify CSS and Javascript files by stripping whitespaces and comments. Returns a combined output as a string.**

``` php
$web->minify( string|array $files,[ string $mime = NULL ], [ bool $header = TRUE ]); string
```

Example:

```php
$minified = Web::instance()->minify('style.css,framework.css,null.css');
```

Notice that the files processed by this function must be located in one of the directories specified in [UI](quick-reference#ui) system var.

To get maximum performance, enable the system caching. Have a look a the [CACHE](quick-reference#cache) var.

To learn more about `minify`, check the [User Guide](http://fatfreeframework.com/optimization#keeping-javascript-and-css-on-a-healthy-diet).



### rss

**Parse RSS feed and optionally return an array of all tags.**

``` php
$web->rss( string $url, [ int $max = 10], [ string $tags = NULL ]); array | false
```

Example:

```php
$count = 20; // Default: 10
$tags = array('title', 'link', 'pubDate'); // Default: NULL (all tags)

Web::instance()->rss('http://example.org/feed.rss', $count, $tags);
```



### slug

**Method to return a URL- and filesystem-friendly string.**

``` php
$web->slug( string $text); string
```

The purpose of this function is to convert foreign characters to their approximate English keyboard character equivalents.
Therefor it uses ISO-9 transliteration with a lookup table array that can be extended with the [DIACRITICS](quick-reference#diacritics) var.
Furthermore, it is designed to remove all non-alphanumeric characters and convert them to dashes.

```php
echo Web::instance()->slug('ĤÈĹĹŌ'); // returns: HELLO
echo Web::instance()->slug('Ein schöner Artikel über Max & John'); // returns: ein-schoner-artikel-uber-max-john
```
