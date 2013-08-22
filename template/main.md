# Template

F3's own lightning fast and extendable template engine gives you all the flexibility you need for modern and clean templating.
Make sure you have read the user guide section about [templating](views-and-templates#a-quick-look-at-the-f3-template-language).

Namespace: `\` <br/>
File location: `lib/template.php`

---

### instance

**Return class instance**

``` php
$template = \Template::instance();
```

The Template class uses the [Prefab](prefab-registry) factory wrapper, so you can grab the same instance of that class at any point of your code.


### token
** Convert token to variable **

```php
$template->token( string $str ); string
```

Example:

```php
echo $template->token('My {{@color}} car looks nice.');
// returns: My $color car looks nice.
```


### extend
** Extend template with custom tag **

```php
$template->extend( string $tag, callback $func ); void
```

This ability lets you add an own custom function to render a html tag as you like.

Have a look at a simple example, so see how it basically works:

```php
class ImageViewHelper {

    static public function render($args) {
        $attr = $args['@attrib']; // contains all tag attributes
        $html = (isset($args[0])) ? $args[0] : ''; // contains the inner html

        $path = $attr['src'];
        $imgObj = new \Image($path);
        $imgObj->resize(
            $attr['width'],
            $attr['height'],
            ((isset($attr['crop']) && $attr['crop']=='true') ? true : false)
        );
        $file_data = $imgObj->dump('png');
        /** @var Base $f3 */
        $f3 = \Base::instance();
        $file_name = $f3->hash($path.$attr['width'].$attr['height']).'.png';
        $file_path = $f3->get('TEMP').$file_name;
        $f3->write($file_path, $file_data);
        return '<img src="'.$file_path.'" />';
    }
}

// register the tag renderer in your index.php or view controller
\Template::instance()->extend('image','ImageViewHelper::render');
```

This is a basic snippet that takes all html tags that looks like

```
<image src="images/south-park.jpg" width="60" height="60" crop="true" />
```

scales the image from `src` to your desired dimensions, copies the new file to a temp folder and
 creates an output like `<img src="tmp/0dgsnl2kmnb.png" />` in your template.

 Templates are all pre-rendered and cached. This way our Image Tag Renderer will only process the file once and not on every request.

Combine your tag processing by using the [token](template#token) method to add some support for dynamic values in your tag attributes and write some php calls to the result to generate fully flexible view helper.



### render
** Render a template **

```php
$template->render( string $file, [ string $mime = 'text/html' ], [ array $hive = NULL ]); string
```

This render method loads your specified template file and builds a php based view file which is cache in F3's `TEMP` directory.
F3 is smart enough to recognize any made changes on the template file and renders the php view again if required.

The `$file` argument expects a file path that is within any defined directories in F3's [UI](quick-reference#ui) var.
After rendering, F3 sets the appropriate header Content-Type. If you like to render and return a JSON, XML or an E-Mail response, just change `$mime` for your needs.
The `$hive` argument can be set to use an explicit array of variables in that template. The default way is to allow the template to have access to the whole F3 hive with all it's variables.

Example:

```php
echo $template->render('templates/layout.html');
```
