# High Contrast Black and White (B/W) PDF Conversion

This also works with colored pdf and scanned documents.

References:

- <https://superuser.com/questions/622950/is-there-a-way-to-increase-the-contrast-of-a-pdf-that-was-created-by-scanning-a>
- <https://superuser.com/a/1186016>

## Process

If it has tons of pages, the easier tool is a command line one:
<http://www.imagemagick.org/script/download.php>

(ImageMagick is a very popular image manipulation library.)

!!! note
    The conversion process in **CPU Intensive** so don't worry if your *CPU Fans start buzzing*.

You will have to do three steps.

#### 1.  Convert PDF pages to individual image files.
See:
Convert PDF to image with high resolution or *Convert PDF to JPG* images with `ImageMagick` - how to `0-pad` file names?

`convert -density 600 your_pdf_filename.pdf  output-%02d.jpg`

#### 2.  Adjust image quality.
If you have only a few pages, *Photoshop* or *GIMP* will simply import each page as an image.
Update the contrast as you'd like and save.

For more info see *GIMP*:
how to remove background noise/artifacts and enhance handwritten text

or continue to use `ImageMagick`:
Batch-processing images of documents to look like a fax

`convert output*.jpg -normalize -threshold 80% final-%02d.jpg`

#### 3.  If you want a pdf back:

`convert final*.jpg my_new_highcontrast.pdf`

## Simple Script to Do all together

```bash
#!/bin/bash
convert -density 600 $1 output-%02d.jpg
convert output*.jpg -normalize -threshold 55% final-%02d.jpg
convert final*.jpg my_new_highcontrast.pdf
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
