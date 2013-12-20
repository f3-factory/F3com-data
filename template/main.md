# Template

F3's own lightning fast and extendable template engine gives you all the flexibility you need for modern and clean templating.
Make sure you have read the user guide section about [templating](views-and-templates#a-quick-look-at-the-f3-template-language).

Namespace: `\` <br>
File location: `lib/template.php`

---

### instance

**Return unique class instance**

```php
$template = \Template::instance();
```

The Template class uses the [Prefab](prefab-registry) factory wrapper, so you can, and you should, grab the same instance of that Template class at any point of your code.


### token
** Convert token to variable **

```php
string token ( string $str )
```

Example:

```php
echo $template->token ('My {{@color}} car looks nice');

// returns the string  'My $color car looks nice'
```

### render
** Render and return a template given by its filename **

```php
string render ( string $filename [, string $mime = 'text/html' [, array $hive = NULL [, int $ttl = 0 ]]] )
```

The `$filename` argument expects a file path that is within any defined directories by F3's [UI](quick-reference#ui) system variables. Remember, for your convenience, [UI](quick-reference#ui) can be multiple paths.

Actually, the render method first try to load your specified template from the temporary folder [TEMP](quick-reference#temp) acting as a file cache for compiled templates. The specified template file is only load if needed: F3 is smart enough to detect if a newer version of the template file has been saved and then it builds a php based view file and only then renders the php view again.

Once rendered, F3 escape the content according to the [ESCAPE](quick-reference#escape) system variable, and sets the appropriate `Content-Type:` value of the HTTP header. If you'd like to get the response returned as JSON, XML or as E-Mail content, just change the `$mime` type parameter accordingly to your needs. (While the charset of the content is defined by the [ENCODING](quick-reference#encoding) system variable)

The `$hive` argument allows you to use an explicit array of variables for that template. By default the rendered template has access to the whole F3 hive with all it's variables.

The `$ttl` argument specifies, in seconds, the Time To Live in the F3 cache for the php based compiled view file. When a `$ttl` is given, F3 will store the compiled view in the cache. On the next request, if the time specified by `$ttl` has passed, the compiled view will be rebuild from the template and stored again in the cache for another cycle.

Examples:

```php
echo $template->render('layout.html'); // assumes layout.html is in the UI folder
```

```php
$flow = $template->render('widgets/tweeter-feeds.html', 'application/json', NULL, 300 ); // cache for 5 minutes
```

### resolve
** Render template string **

```php
string resolve ( string $str, array $hive = NULL )
```

### extend
** Extend F3 template engine with a custom tag **

```php
void extend ( string $tag, callback $func )
```

The `extend` function allows you to create your own custom template tag. It can be seen as a hook to the F3 template engine. 

`$tag` is the name of your custom tag <small>(without the `<` and `>` indeed)</small>. Don't name your tag after an existing [F3 template directive](quick-reference#include) as your tag handler won't be called. <small>(Read: To date, you can't use `extend` to override an existing F3 template directive)</small>

`$func` is the name of your custom function that F3 will callback when it finds your custom `$tag` during the rendering of a template.

So, to define a new custom tag & register/hook its custom handler, it gives:

```php
\Template::instance()->extend('my_new_special_tag','MyImageViewHelper::my_tag_renderer');
```

But let's have a look at a functionnal example to see how it basically works:

The idea is to create a new HTML `<image>` tag that would have the ability to resize images on the fly according to the `width` and `height` attributes found in the markup.

Let's do that now:

```php
class ImageViewHelper {

    static public function render($args) {
        // retrieve the attributes of the template tag, as found in the template
        // in our case, we expect 'src', 'width' and 'height', and optionally 'crop'
        $attr = $args['@attrib']; // provided by the F3 template engine
        // retrieve the inner html, as found in the template
        $html = (isset($args[0])) ? $args[0] : '';

        $imagepath = $attr['src'];
        $imgObj = new \Image($imagepath);
        $imgObj->resize(
            $attr['width'],
            $attr['height'],
            ((isset($attr['crop']) && $attr['crop']=='true') ? true : false)
        );
        $f3 = \Base::instance();
        // to avoid clash, build an unique name for the new generated image
        $file_name = $f3->hash($imagepath.$attr['width'].$attr['height']).'.png';
        // save it for example in TEMP
        $imagepath = $f3->get('TEMP').$file_name;
        // convert it to PNG and save it to a file
        $f3->write($imagepath, $imgObj->dump('png'));
        // done! return the HTML markup
        return 
            sprintf ('<img src="/%s" width="%u" height="%u" />', 
                        $imagepath,$attr['width'],$attr['height']);
    }
}

// register the tag renderer either in index.php or in your view controller
\Template::instance()->extend('image','ImageViewHelper::render');
```

The basic snippet above takes all html tags that look like this:

```html
<image src="images/south-park.jpg" width="60" height="60" crop="true" />
```

and scales the image given by the `src` attribute to the specified `width="60" height="60"` dimensions, then copies the new scaled image to the `TEMP` folder and finaly generates an HTML output similar to `<img src="/tmp/0dgsnl2kmnb.png" />` in your template.

Knowing that F3 templates are all pre-rendered and cached. This way our Image Tag Renderer will only process the file once and not on every request.

Combine your tag processing by using the [token](template#token) method to add some support for dynamic values in your tag attributes and write some php calls to the result to generate fully flexible view helper.
