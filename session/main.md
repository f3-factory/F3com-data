# Session Handler

The framework contains some SESSION handlers as well.
To use them, just create a new instance of the certain class once. The plugins will register a new [session_set_save_handler](http://php.net/manual/en/function.session-set-save-handler.php "php.net :: function session_set_save_handler")
which syncs the frameworks [SESSION](quick-reference#cookie,-get,-post,-request,-session,-files,-server,-env) hive keys to the corresponding new session handler class.
to the corresponding new session handler class.

---

### Cache

The Session class provides a lightweight [Cache](cache)-based session handler.

Namespace: `\` <br/>
File location: `lib/session.php`

Ensure that you have enabled the [cache](quick-reference#cache) to make this work. Usage:

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

It will automatically create the required table, if the `session` table does not exist. The table name can be controlled by the 2nd `$table` parameter of the constructor.


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
