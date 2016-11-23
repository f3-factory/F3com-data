# Log
This is the F3 custom Logger class.


Namespace: `\` <br>
File location: `lib/log.php`

---

### Instantiation

You can create a new Logger object and specify a log file like this:

```php
$logger = new Log('error.log');
```

The path the Logger will use for log files is defined in the global F3 [LOGS](quick-reference#logs) var.
If the file and folder does not exist, the Logger tries to create them.

### write
**Write specified text to log file**

```php
$logger->write( string $text [, string $format = 'r' ] );
```

The `$format` argument defines the date format that is going to be added to the log line. The default value `r` specifies a RFC 2822 compliant date format that looks like `Thu, 21 Dec 2000 16:01:07 +0200`.  The remote address is also added to the log entry.


### erase
**erases the whole log file**

```php
$logger->erase();
```
