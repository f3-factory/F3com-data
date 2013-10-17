# Mongo

The Mongo class is a lightweight MongoDB wrapper. For full class details refer [class.mongodb.php @ php.net](http://php.net/manual/en/class.mongodb.php).

Namespace: `\DB` <br/>
File location: `lib/db/mongo.php`

---

## Constructor

```php
$db=new \DB\Mongo(string $dsn, string $dbname, [ array $options = NULL]);
```
For example, to connect to a MongoDB database, the syntax looks like:

``` php
$db = new \DB\Mongo('mongodb://localhost:27017','testdb');
```

In the `$options` parameter, you can define serveral connection settings. More details about that on the [PHP MongoClient Manual](http://www.php.net/manual/en/mongoclient.construct.php) site.


## Methods

### dsn

**Return data source name**

``` php
echo $db->dsn();
```

### uuid

**Returns unique connection UUID**

``` php
echo $db->uuid();
```

### log

**Return MongoDB profiler results**

```php
echo $db->log();
```

### drop

**Intercept native call to re-enable profiler**

```php
$db->drop(); int
```
This drops the database currently being used and re-enables its log profiler