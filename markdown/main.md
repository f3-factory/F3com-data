# Markdown
This is F3's own implementation of Markdown. Markdown is intended to write an easy-to-read / -write texts which can be converted to HTML afterwards. For more informations and a full documentation of the syntax, please have a look at the [project homepage](http://daringfireball.net/projects/markdown/).

Namespace: `\` <br/>
File location: `lib/markdown.php`

---

### instance

**Return class instance**

``` php
$md = \Markdown::instance();
```

The Markdown class uses the [Prefab](prefab) factory wrapper, so you are able to grab the same instance of that class at any point of your code.


### esc

** Convert characters to HTML entities **

```php
$md->esc( string $str ); string
```

This method also converts special character combinations like (tm), (r), (c) and '...' to their HTML entity.

```php
Markdown::instance()->esc('This <tag> will be converted'); // This &amp;lt;tag&amp;rt; will be converted
```

### snip
** Remove multiple line feeds from a text **

```php
$md->snip( string $str ); string
```

A new line is intended to split several paragraphs and is not used to format it. This method will remove every needless newline from the text.

```php
$text = "
This is a very long text with several needless newlines.
|
|
|
|
|
Which will be reduced to one new line betweet every paragraph.";

Markdown::instance()->snip($text);

/*
This is a very long text with several useless newlines.

Which will be reduced to one new line betweet every paragraph.
*/
```
### scan
**Scan line for convertible spans**

```php
$md->scan(string $str); string
```

### convert
** Render HTML equivalent of markdown **

<div class="alert alert-info">
	<strong>Notice:</strong> The <em>convert</em> method uses the above mentioned methods. It's not necessary to use them before / after using <em>convert</em>.
</div>

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



