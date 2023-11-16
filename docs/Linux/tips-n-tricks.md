# Tips and Tricks

## Trim Trailing spaces in text file

```sh
find . -name "*.md" -type f -print0 | xargs -0 sed -i 's/[[:space:]]*$//'
```

Very essential for all `Markdown` files as shown.

## Find files with Trailing spaces in them

```bash
find . -type f -exec grep -E -l " +$" {} \;
```

## Fix auto-completion for `sudo` operation

Reference : <https://bbs.archlinux.org/viewtopic.php?id=45613>

Add the Following to your `.bashrc` or `.profile` configurations.

```sh
    complete -cf sudo
```

## Convert Windows line Endings to Unix line Endings

This is also known as `CRLF to LF` conversion.

```sh
tr -d '\r' < windows-file.txt > unix-file.txt
```

This can also be used with pipe output:
```sh
curl -L URL | tr -d '\r' > output-file
```

The above command would download a file and convert line-ending to Unix.

## Use direct `sudo` without password

Create this file
```sh
echo "$USER ALL=(ALL) NOPASSWORD: ALL" | sudo tee /etc/sudoers.d/$USER
```

In case you wish to do it for a specific user just replace `$USER` with the correct user name.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)


