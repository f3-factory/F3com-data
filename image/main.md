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

### invert
**Invert image**

``` php
$img->invert();
```

### brightness
**Adjust brightness**

``` php
$img->brightness( int $level );
```

`$level` range: -255 to 255

### contrast
**Adjust contrast**

``` php
$img->contrast( int $level );
```

`$level` range: -100 to 100


### grayscale
**Convert to grayscale**

``` php
$img->grayscale();
```


### smooth
**Adjust smoothness**

``` php
$img->smooth( int $level);
```
`$level` range: -8 to 8, but as its been used for a matrix operation, greater values are applyable too, but may lead to unusable results.

### emboss
**Emboss the image**

``` php
$img->emboss();
```

### sepia
**Apply sepia effect**

``` php
$img->sepia();
```

### pixelate
**Pixelate the image**

``` php
$img->pixelate( int $size );
```

`$size` is the block size of a pixel, usually range: 0 - 100

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
### rgb
**Convert RGB hex triad to array**

``` php
$img->rgb( 0xFF0033 ); // returns array( 255, 0, 51 );
```


## Output
### render
### dump

## History
### save
### restore
### undo