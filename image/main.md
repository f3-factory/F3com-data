# Image
The Image class offers a bunch of image processing features.

Namespace: `\` <br/>
File location: `lib/image.php`

---

## Instatiation

You can create a new Image object and load an existing image file like this:

``` php
$img = new Image('path/to/your/image.jpp');
```

To create a new empty image, i.e. for creating a captcha, just leave out the first `$file` argument:

``` php
$img = new Image();
```

The constructor also has a 2nd argument, the `$flag` option. Setting it to `TRUE` enables the file history, which can save additional states for the current file.

## Processing

### rgb
### invert
### brightness
### contrast
### grayscale
### smooth
### emboss
### sepia
### pixelate
### blur
### sketch
### hflip
### vflip
### crop
### resize
### rotate
### overlay

## Rendering
### identicon
### captcha

## Info
### width
### height

## Output
### render
### dump

## History
### save
### restore
### undo