# Auth
The Auth class is used to authenticate against several storages.

Namespace: `\` <br/>
File location: `lib/auth.php`

---

## Instantiation
``` php
$auth = new \Auth(string $storage, [ array $args = NULL ]);
```

You can map the class internal keys `id` and `pw` with your current storage structure. Given an example for F3's Jig:

``` php
$db = new \DB\Jig('data/');
$db_mapper = new \DB\Jig\Mapper($db, 'users');
$auth = new \Auth($db_mapper,  array('id' => 'username', 'pw' => 'password'));
```

Currently available authentication storages are:

* Jig
* SQL
* MongoDB
* LDAP
* SMTP

## Authenticating

### Login
**Used to authenticate credentials against an authentication storage**

``` php
$auth->login(string $id, string $pw, [ string $realm = NULL ]);
```

The `login()` is the core method of the Auth class. By passing login credentials to it, it will authenticate the user against the given authentication storage.

Example:

``` php
$db = new \DB\Jig('data/');
$user = new \DB\Jig\Mapper($db, 'users');
$auth = new \Auth($user, array('id'=>'user_id', 'pw'=>'password'));
$auth->login('admin','secret'); // returns true on successful login
```

### Basic
**Use basic authentication to login**

``` php
$auth->basic([ string $func = NULL ], [ bool $halt = TRUE ]);
```

The `basic()` method provides an alternative way to authenticate users. In this case the browser will display a login prompt to authenticate the user. 

You can manipulate the password by passing a method to `$func` before it's being compared with the password stored in the storage.

By setting `$halt` to `FALSE` you suppress the `401 Unauthorized` error on unsuccessful login and the method will just return `FALSE`.

``` php
$db = new \DB\SQL('mysql:host=localhost;dbname=project1', 'root', 'topsecret123');
$user = new \DB\SQL\Mapper($db, 'users');
$auth = new \Auth($user, array('id'=>'user_id', 'pw'=>'password'));
$auth->basic(); // a prompt will open up to authenticate the user
```

