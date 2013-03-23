# Routing Engine

## Overview

Our first example wasn't too hard to swallow, was it? If you like a little more flavor in your
Fat-Free soup, insert another route before the `$f3->run()` command:

``` php
$f3->route('GET /about',
    function() {
        echo 'Donations go to a local charity... us!';
    }
);
```

You don't want to clutter the global namespace with function names? Fat-Free recognizes
different ways of mapping route handlers to OOP classes and methods:

``` php
class WebPage {
    function display() {
        echo 'I cannot object to an object';
    }
}

$f3->route('GET /about','WebPage->display');
```

HTTP requests can also be routed to static class methods:

``` php
$f3->route('GET /login','Auth::login');
```

## Routes and Tokens

As a demonstration of Fat-Free's powerful domain-specific language (DSL),
you can specify a single route to handle different possibilities:

``` php
$f3->route('GET /brew/@count',
    function($f3) {
        echo $f3->get('PARAMS.count').' bottles of beer on the wall.';
    }
);
```

This example shows how we can specify a token `@count` to represent part of a URL. The framework
will serve any request URL that matches the `/brew/` prefix, like `/brew/99`, `/brew/98`,
etc. This will display `'99 bottles of beer on the wall'` and
`'98 bottles of beer on the wall'`, respectively. Fat-Free will also accept a page request for
`/brew/unbreakable`. (Expect this to display `'unbreakable bottles of beer on the wall'`.) When
such a dynamic route is specified, Fat-Free automagically populates the global `PARAMS` array
variable with the value of the captured strings in the URL. The `$f3->get()` call inside the
callback function retrieves the value of a framework variable. You can certainly apply this
method in your code as part of the presentation or business logic. But we'll discuss that in
greater detail later.

Notice that Fat-Free understands array dot-notation. You can certainly use `@PARAMS['count']`
regular notation, which is prone to typo errors and unbalanced braces. The framework also
permits `@PARAMS.count` which is somehow similar to Javascript. This feature is limited to
arrays in F3 templates. Take note that `@foo.@bar` is a string concatenation,
whereas `@foo.bar` translates to `@foo['bar']`.

Here's another way to access tokens in a request pattern:

``` php
$f3->route('GET /brew/@count',
    function($f3,$params) {
        echo $params['count'].' bottles of beer on the wall.';
    }
);
```

You can use the asterisk (`*`) to accept any URL after the `/brew` route - if you don't really
care about the rest of the path:

``` php
$f3->route('GET /brew/*',
    function() {
        echo 'Enough beer! We always end up here.';
    }
);
```

An important point to consider: You will get Fat-Free (and yourself) confused if you have both
`GET /brew/@count` and `GET /brew/*` together in the same application. Use one or the other.
Another thing: Fat-Free sees `GET /brew` as separate and distinct from the route
`GET /brew/@count`. Each can have different route handlers.

## Dynamic Web Sites

Wait a second - in all the previous examples, we never really created any directory in our hard
drive to store these routes. The short answer: we don't have to. All F3 routes are virtual. They
don't mirror our hard disk folder structure. If you have programs or static files (images, CSS,
etc.) that do not use the framework - as long as the paths to these files do not conflict with
any route defined in your application - your Web server software will deliver them to the
user's browser, provided the server is configured properly.

### PHP 5.4's Built-In Web Server

PHP's latest stable version has its own built-in Web server. Start it up using the following
configuration:

```
php -S localhost:80 -t /var/www/
```

The above command will start routing all requests to the Web root `/var/www`. If an incoming
HTTP request for a file or folder is received, PHP will look for it inside the Web root and send
it over to the browser if found. Otherwise, PHP will load the default `index.php` (containing
your F3-enabled code).

### Sample Apache Configuration

If you're using Apache, make sure you activate the URL rewriting module (mod_rewrite) in your
apache.conf (or httpd.conf) file. You should also create a .htaccess file containing the following:

``` apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-l
RewriteRule .* index.php [L,QSA]
```

The script tells Apache that whenever an HTTP request arrives and if no physical file (`!-f`) or
path (`!-d`) or symbolic link (`!-l`) can be found, it should transfer control to `index.php`,
which contains our main/front controller, and which in turn, invokes the framework.

The `.htaccess file` containing the Apache directives stated above should always be in the same
folder as `index.php`.

You also need to set up Apache so it knows the physical location of `index.php` in your hard
drive. A typical configuration is:

``` apache
DocumentRoot "/var/www/html"
<Directory "/var/www/html">
    Options -Indexes FollowSymLinks Includes
    AllowOverride All
    Order allow,deny
    Allow from All
</Directory>
```

If you're developing several applications simultaneously, a virtual host configuration is easier
to manage:

``` apache
NameVirtualHost *
<VirtualHost *>
    ServerName site1.com
    DocumentRoot "/var/www/site1"
    <Directory "/var/www/site1">
        Options -Indexes FollowSymLinks Includes
        AllowOverride All
        Order allow,deny
        Allow from All
    </Directory>
</VirtualHost>
<VirtualHost *>
    ServerName site2.com
    DocumentRoot "/var/www/site2"
    <Directory "/var/www/site2">
        Options -Indexes FollowSymLinks Includes
        AllowOverride All
        Order allow,deny
        Allow from All
    </Directory>
</VirtualHost>
```

Each `ServerName` (`site1.com` and `site2.com` in our example) must be listed in your
`/etc/hosts` file. On Windows, you should edit `C:/WINDOWS/system32/drivers/etc/hosts`. A reboot
might be necessary to effect the changes. You can then point your Web browser to the address
`http://site1.com` or `http://site2.com`. Virtual hosts make your applications a lot easier to
deploy.

### Sample Nginx Configuration

For Nginx servers, here's the recommended configuration (replace ip_address:port with your
environment's FastCGI PHP settings):

``` nginx
server {
    root /var/www/html;
    location / {
        index index.php index.html index.htm;
        try_files $uri /index.php;
    }
    location ~ \.php$ {
        fastcgi_pass ip_address:port;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

### Sample Lighttpd Configuration

Lighttpd servers are configured in a similar manner:

```
$HTTP["host"] =~ "www\.example\.com$" {
    url.rewrite-once = ( "^/(.*?)(\?.+)?$"=>"/index.php/$1?$2" )
    server.error-handler-404 = "/index.php"
}
```

## Rerouting

So let's get back to coding. You can declare a page obsolete and redirect your visitors to
another site:

``` php
$f3->route('GET|HEAD /obsoletepage',
    function($f3) {
        $f3->reroute('/newpage');
    }
);
```

If someone tries to access the URL `http://www.example.com/obsoletepage` using either HTTP GET
or HEAD request, the framework redirects the user to the URL: `http://www.example.com/newpage`
as shown in the above example. You can also redirect the user to another site,
like `$f3->reroute('http://www.anotherexample.org/');`.

Rerouting can be particularly useful when you need to do some maintenance work on your site. You
can have a route handler that informs your visitors that your site is offline for a short period.

HTTP redirects are indispensable but they can also be expensive. As much as possible,
refrain from using `$f3->reroute()` to send a user to another page on the same Web site if you
can direct the flow of your application by invoking the function or method that handles the
target route. However, this approach will not change the URL on the address bar of the user's
Web browser. If this is not the behavior you want and you really need to send a user to another
page, in instances like successful submission of a form or after a user has been authenticated,
Fat-Free sends an `HTTP 303 See Other` header. For all other attempts to reroute to another
page or site, the framework sends an `HTTP 301 Moved Permanently` header.

## Triggering a 404

At runtime, Fat-Free automatically generates an HTTP 404 error whenever it sees that an incoming
HTTP request does not match any of the routes defined in your application. However,
there are instances when you need to trigger it yourself.

Take for instance a route defined as `GET /dogs/@breed`. Your application logic may involve
searching a database and attempting to retrieve the record corresponding to the value of
`@breed` in the incoming HTTP request. Since Fat-Free will accept any value after the `/dogs/`
prefix because of the presence of the `@breed` token, displaying an `HTTP 404 Not Found`
message programmatically becomes necessary when the program doesn't find any match in our
database. To do that, use the following command:

``` php
$f3->error(404);
```

## Representational State Transfer (ReST)

Fat-Free's architecture is based on the concept that HTTP URIs represent abstract Web resources
(not limited to HTML) and each resource can move from one application state to another. For this
reason, F3 does not have any restrictions on the way you structure your application. If you
prefer to use the [Model-View-Controller](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) 
pattern, F3 can help you compartmentalize your application components to stick to this paradigm.
On the other hand, the framework also supports the 
[Resource-Method-Representation](http://www.peej.co.uk/articles/rmr-architecture.html) pattern,
and implementing it is more straightforward.

Here's an example of a ReST interface:

``` php
class Item {
    function get() {}
    function post() {}
    function put() {}
    function delete() {}
}

$f3=require('lib/base.php');
$f3->map('/cart/@item','Item');
$f3->run();
```

Fat-Free's `$f3->map()` method provides a ReST interface by mapping HTTP methods in routes to
the equivalent methods of an object or a PHP class. If your application receives an incoming
HTTP request like `GET /cart/123`, Fat-Free will automatically transfer control to the object's
or class' `get()` method. On the other hand, a `POST /cart/123` request will be routed to the
`Item` class' `post()` method.

**Note:** Browsers do not implement the HTTP `PUT` and `DELETE` methods in regular HTML forms.
These and other ReST methods (`HEAD`, and `CONNECT`) are accessible only via AJAX calls to the
server.

If the framework receives an HTTP method that's not implemented by a class,
it generates an `HTTP 405 Method Not Allowed` error. F3 automatically responds with the
appropriate headers to HTTP `OPTIONS` method requests. The framework will not map this request
to a class.

## The F3 Autoloader

Fat-Free has a way of loading classes only at the time you need them,
so they don't gobble up more memory than a particular segment of your application needs. And
you don't have to write a long list of `include` or `require` statements just to load PHP
classes saved in different files and different locations. The framework can do this
automatically for you. Just save your files (one class per file) in a folder and tell the
framework to automatically load the appropriate file once you invoke a method in the class:

``` php
$f3->set('AUTOLOAD','autoload/');
```

You can assign a different location for your autoloaded classes by changing the value of the
`AUTOLOAD` global variable. You can also have multiple autoload paths. If you have your classes
organized and in different folders, you can instruct the framework to autoload the appropriate
class when a static method is called or when an object is instantiated. Modify the `AUTOLOAD`
variable this way:

``` php
$f3->set('AUTOLOAD','admin/autoload/; user/autoload/; default/');
```

**Important:** Except for the .php extension, the class name and file name must be identical,
for the framework to autoload your class properly. The basename of this file must be identical
to your class invocation, e.g. F3 will look for either `Foo/BarBaz.php` or `foo/barbaz.php` when
it detects a `new Foo\BarBaz` statement in your application.

## Working with Namespaces

`AUTOLOAD` allows class hierarchies to reside in similarly-named subfolders,
so if you want the framework to autoload a PHP 5.3 namespaced class that's invoked in the
following manner:

``` php
$f3->set('AUTOLOAD','autoload/');
$obj=new Gadgets\iPad;
```

You can create a folder hierarchy that follows the same structure. Assuming `/var/www/html/` is
your Web root, then F3 will look for the class in `/var/www/html/autoload/gadgets/ipad.php`. The
file `ipad.php` should have the following minimum code:

``` php
namespace Gadgets;
class iPad {}
```

Remember: All directory names in Fat-Free must end with a slash. You can assign a search path
for the autoloader as follows:

``` php
$f3->set('AUTOLOAD','main/;aux/');
```

## Routing to a Namespaced Class

F3, being a namespace-aware framework, allows you to use a method in namespaced class as a route
 handler, and there are several ways of doing it. To call a static method:

``` php
$f3->set('AUTOLOAD','classes/');
$f3->route('GET|POST /','Main\Home::show');
```

The above code will invoke the static `show()` method of the class `Home` within the `Main`
namespace. The `Home` class must be saved in the folder `classes/main/home.php` for it to be
loaded automatically.

If you prefer to work with objects:

``` php
$f3->route('GET|POST /','Main\Home->show');
```

will instantiate the `Home` class at runtime and call the `show()` method thereafter.

## Event Handlers

F3 has a couple of routing event listeners that might help you improve the flow and structure of
controller classes. Say you have a route defined as follows:

``` php
$f3->route('GET /','Main->home');
```

If the application receives an HTTP request matching the above route, F3 instantiates `Main`,
but before executing the `home()` method, the framework looks for a method in this class named
`beforeRoute()`. In case it's found, F3 runs the code contained in the `beforeRoute()` event
handler before transferring control to the `home()` method. Once this is accomplished,
the framework looks for an `afterRoute()` event handler. Like `beforeRoute()`,
the method gets executed if it's defined.

## Dynamic Route Handlers

Here's another F3 goodie:

``` php
$f3->route('GET /products/@action','Products->@action');
```

If your application receives a request for, say, `/products/itemize`,
F3 will extract the `'itemize'` string from the URL and pass it on to the `@action` token in the
route handler. F3 will then look for a class named `Products` and execute the `itemize()` method.

Dynamic route handlers may have various forms:

``` php
// static method
$f3->route('GET /public/@genre','Main::@genre');
// object mode
$f3->route('GET /public/@controller/@action','@controller->@action');
```

F3 triggers an `HTTP 404 Not Found` error at runtime if it cannot transfer control to the class
or method associated with the current route, i.e. an undefined class or method.

## AJAX and Synchronous Requests

Routing patterns may contain modifiers that direct the framework to base its routing decision on
the type of HTTP request:

``` php
$f3->route('GET /example [ajax]','Page->getFragment');
$f3->route('GET /example [sync]','Page->getFull');
```

The first statement will route the HTTP request to the `Page->getFragment()` callback only if an
`X-Requested-With: XMLHttpRequest` header (AJAX object) is received by the server. If an
ordinary (synchronous) request is detected, F3 will simply drop down to the next matching
pattern, and in this case it executes the `Page->getFull()` callback.

If no modifiers are defined in a routing pattern, then both AJAX and synchronous request types
are routed to the specified handler.

Route pattern modifiers are also recognized by `$f3->map()`.