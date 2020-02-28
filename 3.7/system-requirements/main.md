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
apt-get install php7.2 php7.2-common php7.2-mysql php7.2-cgi php7.2-cli php7.2-fpm php7.2-mbstring php7.2-curl php7.2-xmlrpc php7.2-gd php7.2-zip php7.2-pdo-sqlite php7.2-xml
```
