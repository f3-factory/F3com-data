# Jig 

Jig provides a simple way to store arrays of data into flat files. 

Namespace: `\DB` <br/>
File location: `lib/db/jig.php`

---

## Constructor

```php
$db=new \DB\Jig(string $dir, [ int $format = \DB\Jig::FORMAT_JSON ] );
```

`$dir` defines the storage directory name. It can be relative to the base path or absolute.

<div class="alert alert-info">
    NB1: The directory is automatically created if not already existing.
    <br>
    NB2: The directory name must have a trailing slash!
</div>

`$format` defines the storage format. Possible values are:

* `\DB\Jig::FORMAT_JSON` which stores data in JSON format.
* `\DB\Jig::FORMAT_Serialized` which serializes data. Depending on the [SERIALIZER](quick-reference#serializer) system variable, data is stored using the standard PHP serialization format, the igbinary format or ... JSON.

## Write

```php
$db->write(string $file, array $data); int
```

This method writes data to a given file and returns the number of written bytes. E.g:

```php
$db=new \DB\Jig('jig/',\DB\Jig::FORMAT_JSON);
$lineup=array(
  'Jimmy' => array('birth'=>44,'instr'=>'guitars'),
  'Robert' => array('birth'=>48,'instr'=>'vocals'),
  'John' => array('birth'=>48,'instr'=>'drums'),
  'John Paul' => array('birth'=>46,'instr'=>'keyboards'),
);
echo $db->write('lineup',$lineup); // outputs 160
```

## Read

```php
$db->read(string $file); array
```

This method retrieves the data stored in a given file. E.g: 

```php
$data=$db->read('lineup');
echo $data['Robert']['birth']; // outputs 48 (cf. above)
```

## Drop

This methods erases all the data stored in the current directory.

```php
$db=new \DB\Jig('jig/'); // jig/ is empty
$db->write('file1.dat',$data1); // jig/ contains file1.dat
$db->write('file2.dat',$data2); // jig/ contains file1.dat and file2.dat
$db->drop(); // jig/ is empty
```

