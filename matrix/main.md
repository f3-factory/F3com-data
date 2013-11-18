# Matrix
The matrix class offers some generic array utilities

Namespace: `\` <br/>
File location: `lib/matrix.php`

---

### instance

**Return class instance**

``` php
$matrix = \Matrix::instance();
```

The Matrix class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.

### calendar
**Return month calendar of specified date, with optional setting for first day of week (0 for Sunday)**

``` php
$matrix->calendar(string $date, [ int $first = 0 ]); array
```

example:

``` php
$cal = $matrix->calendar('2001-09-11');
/*
array(
  array(6=>1),
  array(0=>2,1=>3,2=>4,3=>5,4=>6,5=>7,6=>8),
  array(0=>9,1=>10,2=>11,3=>12,4=>13,5=>14,6=>15),
  array(0=>16,1=>17,2=>18,3=>19,4=>20,5=>21,6=>22),
  array(0=>23,1=>24,2=>25,3=>26,4=>27,5=>28,6=>29),
  array(0=>30)
) */
```


### changekey

**Change the key of a two-dimensional array element**

``` php
$matrix->changekey(array &$var, string $old, string $new); null
```

example:

``` php
$array=array(
  'id'=>array(456,123,345,234),
  'name'=>array('ringo','paul','george','john'),
  'sales'=>array(0.13,0.35,0.57,0.79)
);
$matrix->changekey($array,'sales','percent');
/* $array is now:
array(
  'id'=>array(456,123,345,234),
  'name'=>array('ringo','paul','george','john'),
  'percent'=>array(0.13,0.35,0.57,0.79)
)*/
```

### pick

**Retrieve values from a specified column of a multi-dimensional array variable**

``` php
$matrix->pick(array $var, mixed $col); array
```

example:

``` php
$array=array(
  array('id'=>123,'name'=>'paul','sales'=>0.35),
  array('id'=>456,'name'=>'ringo','sales'=>0.13),
  array('id'=>345,'name'=>'george','sales'=>0.57),
  array('id'=>234,'name'=>'john','sales'=>0.79)
);

$result = $matrix->pick($array,'name');
/* returns:
array('ringo','paul','george','john')
*/
```

### sort

**Sort a multi-dimensional array variable on a specified column**

``` php
$matrix->sort(array &$var, mixed $col, [ int $order=SORT_ASC ]); bool
```

example:

``` php
$array=array(
  array('id'=>123,'name'=>'paul','sales'=>0.35),
  array('id'=>456,'name'=>'ringo','sales'=>0.13),
  array('id'=>345,'name'=>'george','sales'=>0.57),
  array('id'=>234,'name'=>'john','sales'=>0.79)
);

$matrix->sort($array,'sales');

/* $array is now:
array(
  array('id'=>456,'name'=>'ringo','sales'=>0.13),
  array('id'=>123,'name'=>'paul','sales'=>0.35),
  array('id'=>345,'name'=>'george','sales'=>0.57),
  array('id'=>234,'name'=>'john','sales'=>0.79)
)
*/
```

### transpose

**Rotate a two-dimensional array variable**

``` php
$matrix->transpose(array &$var); null
```

example:

``` php
$array=array(
  array('id'=>123,'name'=>'paul','sales'=>0.35),
  array('id'=>456,'name'=>'ringo','sales'=>0.13),
  array('id'=>345,'name'=>'george','sales'=>0.57),
  array('id'=>234,'name'=>'john','sales'=>0.79)
);

$matrix->transpose($array);
/* $array is now:
array(
  'id'=>array(456,123,345,234),
  'name'=>array('ringo','paul','george','john'),
  'sales'=>array(0.13,0.35,0.57,0.79)
)*/
```