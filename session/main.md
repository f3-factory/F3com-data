# Session Handler

The framework contains some SESSION handlers as well.
To use them, just create a new instance of the certain class once. The plugins will register a new [session_set_save_handler](http://us1.php.net/manual/en/function.session-set-save-handler.php)
which syncs the frameworks [SESSION](quick-reference#cookie,-get,-post,-request,-session,-files,-server,-env) hive key
to the corresponding new session handler class.

---

### Cache

The Session class provides a lightweight [Cache](cache)-based session handler.

Namespace: `\` <br/>
File location: `lib/session.php`

Ensure that you enabled the [CACHE](quick-reference#cache) to make this work. Usage:

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

Assuming that you have a working [SQL DB](sql) object in `DB` hive key, use:

```php
$db = $f3->get('DB');
// just create an object
new \DB\SQL\Session($db);

$f3->set('SESSION.test',123);
echo $f3->get('SESSION.test');
```

It will automatically create the needed table, if the `session` table is not existing. The table name can be controlled by the 2nd `$table` parameter.


### Mongo

This class provides a Mongo-based session handler.


Namespace: `\DB\Mongo` <br>
File location: `lib/db/mongo/session.php`

Assuming that you have a working [Mongo DB](mongo) object in `DB` hive key, use:

```php
$db = $f3->get('DB');
// just create an object
new \DB\Mongo\Session($db);

$f3->set('SESSION.test',123);
echo $f3->get('SESSION.test');
```

The table name can be controlled by the 2nd `$table` parameter in constructor.


### Jig

This class provides a JIG-based session handler.


Namespace: `\DB\Jig` <br>
File location: `lib/db/jig/session.php`

Assuming that you have a working [Jig DB](jig) object in `DB` hive key, use:

```php
$db = $f3->get('DB');
// just create an object
new \DB\JIG\Session($db);

$f3->set('SESSION.test',123);
echo $f3->get('SESSION.test');
```

The table name can be controlled by the 2nd `$table` parameter in constructor.
