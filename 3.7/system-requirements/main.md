# System Requirements

F3 needs at least the following server configuration:

**APACHE Webserver**

* PHP 5.4 or higher
* PCRE 8.02 or higher (usually shipped with PHP package, but needs to be additionally updated on CentOS or Red Hat systems)
* mod_rewrite and mod_headers enabled
* GD libary (for Image plugin)
* cURL, sockets or stream extension (for Web plugin)

Nginx and Lighttpd configurations are also possible.


**PHP Modules**

If you have a custom server and want to have installed everything you could probably need, here is a debian 1-liner for you (cache module not included):

```
apt-get install php8.1 php8.1-common php8.1-mysql php8.1-cgi php8.1-cli php8.1-fpm php8.1-mbstring php8.1-curl php8.1-xmlrpc php8.1-gd php8.1-zip php8.1-pdo-sqlite php8.1-xml
```
