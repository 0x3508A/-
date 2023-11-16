# `wkhtmltopdf` PDF Converter

Better PDF of Web Pages with `wkhtmltopdf`.

Source: <https://wkhtmltopdf.org/>

Src: <https://github.com/wkhtmltopdf>

## What is it?

Due to problems in printing websites with Firefox I found this tool. It would help print website into PDF with all links preserved.

`wkhtmltopdf` and `wkhtmltoimage` are open source (LGPLv3) command line tools to render HTML into PDF and various image formats using the Qt
WebKit rendering engine. These run entirely "headless" and do not require a display or display service.

There is also a C library, if you're into that kind of thing.

## How do I use it?

1.  Download a [precompiled binary](https://wkhtmltopdf.org/downloads.html) or build [from source](https://github.com/wkhtmltopdf/wkhtmltopdf)
2.  Create your HTML document that you want to turn into a PDF (or image)
3.  Run your HTML document through the tool. For example, if I really like the treatment Google has done to their logo today and want to capture it forever as a PDF:
    `wkhtmltopdf http://google.com google.pdf`

## Additional Command Line options

That's great, I've always wanted to turn Google's homepage into a PDF, but I want a table of contents as well.

There are plenty of command line options.
Check out the [auto-generated `wkhtmltopdf` manual](https://wkhtmltopdf.org/usage/wkhtmltopdf.txt).



----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
