# Markdown
This is F3's own implementation of Markdown. Markdown is intended for writing human-readable text that can be converted to HTML afterwards. The goal of the Markdown formatting syntax is to make it as readable as possible. For more information, please refer to the project website and the [Markdown Syntax Documentation](http://daringfireball.net/projects/markdown/syntax "Markdown Syntax Documentation on the project website").

Namespace: `\` <br>
File location: `lib/markdown.php`

---

### Instantiation

**Return unique class instance**

```php
$md = \Markdown::instance();
```

The Markdown class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.


### convert
** Convert a given Markdown string to its equivalent HTML **

```php
$md->convert( string $txt ); // returns a HTML string
```

The convert method accept a string containing Markdown syntax:

```php
echo Markdown::instance()->convert('**Bold text**'); // <strong>Bold text</strong>
```

The string can be an entire file containing Markdown syntax:

```php
$file = F3::instance()->read('my_file_full_of_markdown.md');
$html = Markdown::instance()->convert($file);
```

### Syntax highlighting

Code blocks can be highlighted using language hinting as follows:

```
&#96;&#96;&#96;php
your PHP code
&#96;&#96;&#96;
```

Available languages are `php`, `apache`, `html`, `ini`.

NB: requires [HIGHLIGHT](quick-reference#highlight) to be enabled.
