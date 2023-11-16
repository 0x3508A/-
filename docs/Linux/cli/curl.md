# `curl` Client URL Command

## Download files

```sh
curl -LO http://ftp.gnu.org/gnu/hello/hello-2.10.tar.gz
```

The `-LO` option helps to output the downloaded data to disk.

## Download file but only Display on screen or redirect

```sh
curl -L https://en.wikipedia.org/wiki/CURL
# Here is a example of redirection
curl -L https://en.wikipedia.org/wiki/CURL|grep Website
```

This would print the file to the Screen.
The **output** of `curl` can be *redirected to another command*.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
