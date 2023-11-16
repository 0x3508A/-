# `wget` downloading command

## Save as filename.html Or overwrite

```sh
wget http://www.google.com -O filename.html
```

## Check for internet connection

```sh
if wget -q --spider "http://archlinux.org"; then
    echo "Success, got internet connection!"
else
    echo "Error: No internet connection!"
fi
```

## Redirect output to standard output

```sh
url="https://archlinux.org/"
if wget -q "${url}" -O-| grep -qF 'File Not Found'; then
    echo "NO: ${url}"
else
    echo "YES: ${url}"
fi
```

## `wget` exit code

```
0: No problems occurred.
1: Generic error code.
2: Parse error—for instance, when parsing command-line options, the ‘.wgetrc’ or ‘.netrc’...
3: File I/O error.
4: Network failure.
5: SSL verification failure.
6: Username/password authentication failure.
7: Protocol errors.
8: Server issued an error response.
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)

