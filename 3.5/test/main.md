# Unit Test Kit
The Test class is a simple unit testing stack, where you can add multiple tests to and serve out their results. Check out the user guide section about [unit testing](unit-testing) to get a detailed walkthrough.

Namespace: `\` <br/>
File location: `lib/test.php`

---


### Instantiation

```php
$test = new Test($level);
```

The `$level` takes the following values: `Test::FLAG_False`, `Test::FLAG_True` or `Test::FLAG_Both`, which means that the testing stack only returns results for expections that resolved to `TRUE`, `FALSE` or BOTH (default).

### expect
**Evaluate condition and save test result**

```php
null expect ( bool $cond [, string $text = NULL ] )
```

### message
**Append message to test results**

```php
null message ( string $text )
```

### results
**Return test results**

```php
array results ()
```

The returned result array will contain additional assoc array elements of this structure:

- **status**: the boolean result of your test condition
- **text**: your test description
- **source**: the source of any occured error, if any (file and line)

### passed
**Return FALSE if at least one test case fails**

```php
null passed ()
```
