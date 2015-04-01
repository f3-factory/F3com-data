# Session Handler

The framework contains some SESSION handlers as well.
To use them, just create a new instance of the certain class once. The plugins will register a new [session_set_save_handler](http://php.net/manual/en/function.session-set-save-handler.php "php.net :: function session_set_save_handler")
which syncs the frameworks [SESSION](quick-reference#cookie,-get,-post,-request,-session,-files,-server,-env) hive keys to the corresponding new session handler class.


---

## Engines

### Cache

Namespace: `\` <br>
File location: `lib/session.php`

The Session class provides a lightweight [Cache](cache)-based session handler.

Make sure you have [enabled the cache](quick-reference#cache) to make this work. Usage:

```php
// just create an object
new Session();

$f3->set('SESSION.test',123);
echo $f3->get('SESSION.test');
```

### SQL

This class provides a SQL-based session handler.

Namespace: `\DB\SQL` <br>
File location: `lib/db/sql/session.php`

Assuming that you have a working [SQL DB](sql) object in the `DB` hive key, use:

```php
$db = $f3->get('DB');
// just create an object
new \DB\SQL\Session($db);

$f3->set('SESSION.test',123);
echo $f3->get('SESSION.test');
```

It will automatically create the required table, if the `session` table does not exist. The table name can be controlled by the 2nd `$table` parameter of the constructor. A 3rd parameter is used to `$force` the table creation, if it's not existing.


### Mongo

This class provides a Mongo-based session handler.


Namespace: `\DB\Mongo` <br>
File location: `lib/db/mongo/session.php`

Assuming that you have a working [Mongo DB](mongo) object in the `DB` hive key, use:

```php
$db = $f3->get('DB');
// just create an object
new \DB\Mongo\Session($db);

$f3->set('SESSION.test',123);
echo $f3->get('SESSION.test');
```

The table name can be controlled by the 2nd `$table` parameter of the constructor.


### Jig

This class provides a JIG-based session handler.


Namespace: `\DB\Jig` <br>
File location: `lib/db/jig/session.php`

Assuming that you have a working [Jig DB](jig) object in the `DB` hive key, use:

```php
$db = $f3->get('DB');
// just create an object
new \DB\JIG\Session($db);

$f3->set('SESSION.test',123);
echo $f3->get('SESSION.test');
```

The table name can be controlled by the 2nd `$table` parameter of the constructor.


## Methods

### agent

**Return HTTP user agent**

```php
string|FALSE agent( )
```

This method returns the browser's user-agent that was used by the client when the current session was created.


### csrf

**Return anti-CSRF token**

```php
string|FALSE csrf( )
```

This method returns a CSRF token which is bound to the current active Session and the current request. Use this token to protect your application from CSRF attacks. The constructor of the Session classes already performs some checks to prevent Session hijacking, but it is recommended that you implement some extra validation to shield your app from URL-Spoofing and [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks.

```php
if ($f3->exists('SESSION.csrf',$csrf))
	echo $csrf; // old token from last request, use this to validate your requests and forms
$s = new Session();
$newToken = $s->csrf(); // token for current request
$f3->set('SESSION.csrf',$newToken); // use this in hidden form fields
```


### ip

**Return IP address**

```php
string|FALSE ip( )
```

This method returns the IP address that was used by the client when the current session was created.

### stamp

**Return Unix timestamp**

```php
string|FALSE stamp( )
```

This method returns the "last updated at" unix timestamp of the current session.
