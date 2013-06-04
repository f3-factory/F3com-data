# Markdown
This is F3's own implementation of Markdown. Markdown is intended for writing human-readable text that can be converted to HTML afterwards. For more information and a full documentation of the syntax, please have a look at the [project homepage](http://daringfireball.net/projects/markdown/).

Namespace: `\` <br/>
File location: `lib/markdown.php`

---

### instance

**Return class instance**

``` php
$md = \Markdown::instance();
```

The Markdown class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.


### convert
** Render HTML equivalent of markdown **

```php
$md->convert( string $txt ); string
```

You can pass single lines of Markdown to the method.

```php
Markdown::instance()->convert('**Bold text**'); // <strong>Bold text</strong>
```

Or a file with containing Markdown syntax

```php
$md = Markdown::instance();
$content = F3::instance()->read('my_file.md'); // returns the content as string

$md->convert($content);
```



