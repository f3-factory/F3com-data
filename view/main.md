# View 

View is the class responsible for rendering PHP views.

Namespace: `\` <br/>
File location: `lib/base.php`

---

## What is a view?

A view is a representation of your project data. It is considered as a best practice to separate the code generating the data from the code generating the representation.

For example, let's consider how you could generate a website sitemap:

``` php
// somewhere in the beginning of the code, we generate the list of URLs
$urls=array('http://domain.tld/home','http://domain.tld/contact','http://domain.tld/about');

// somewhere in the end of the code, we display the list
// as an HTML view:
header('Content-Type: text/html; charset=UTF-8');
echo "<html><table><tr><td>$urls[0]</td></tr><tr><td>$urls[1]</td></tr><tr><td>$urls[2]</td></tr></table></html>";
// or as a XML view:
header('Content-Type: text/xml; charset=UTF-8');
echo "<?xml><urlset><url><loc>$urls[0]</loc></url><url><loc>$urls[1]</loc></url><url><loc>$urls[2]</loc></url></urlset>";
// or as a CSV view:
header('Content-Type: text/csv; charset=UTF-8');
echo implode("\r\n",$urls);
```

## Render a view

The View class makes it easy to separate the two steps defined above, since it requires for rendering a filename and a data hive:

``` php
$view->render(string $file , [ string $mime = 'text/html' ], [ array $hive = NULL ]); string
```

<div class="alert alert-info">
    NB1: The content-type header is automatically generated.
    <br>
    NB2: If no data hive is provided, the global F3 hive is used.
</div>

Here's how to use it in your route handler:

``` php
$urls=array('http://domain.tld/home','http://domain.tld/contact','http://domain.tld/about');
$view=\View::instance();
echo $view->render('myview.html','text/html',array('urls'=>$urls));
// or
echo $view->render('myview.xml','text/xml',array('urls'=>$urls));
// or
echo $view->render('myview.csv','text/csv',array('urls'=>$urls));
```

myview.html

``` html
<html>
<head>
...
</head>
<body>
<table>
<?php foreach($urls as $url):?>
  <tr>
    <td>
      <?php echo $url;?>
    </td>
  </tr>
<?php endforeach;?>
</body>
</html>
```

myview.xml

``` html
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<?php foreach($urls as $url):?>
  <url>
    <loc>
      <?php echo $url;?>
    </loc>
  </url>
<?php endforeach;?>
</urlset>
```

myview.csv

``` html 
<?php foreach($urls as $url):?>
<?php echo $url;?>
<?php endforeach;?>
```
