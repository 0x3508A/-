# `find` Command

*Official manual*: <https://www.gnu.org/software/findutils/manual/html_mono/find.html>

## Execute commands on Found files

Here is how to execute a single command on **Per file** basis:

```sh
find . -type f -exec echo "Name" '{}' \;
```

Here is how to **combine the list** and then run the Command:

```sh
find . -type f -exec echo "Name" '{}' +;
```

Here there are **two commands** being executed on the found files:

```sh
find . -type f -exec patchelf --shrink-rpath '{}' \; -exec strip '{}' \; 2>/dev/null
```
- First `patchelf` removing the `rpath` for shrinking run-time dependencies.
- Second `strip` that removes the bulk of debug information.

## Fix executable permission while copying from Windows

When we copy stuff over from Windows partitions such as `NTFS` or `FAT32` to Linux, the file permissions are completely out of wack. Due to this we face multiple problems in the Linux side.

Here are a few commands to fix this using `find` command:

```sh
# For Files:
find . -type f -exec chmod 640 {} \;
# For Directories:
find . -type d -exec chmod 0750 {} \;
```

- First command For Files:
    - The above would remove the executable permission from all the files under the given directory.
    - It would also prevent others from reading into your directory.
- Second command for Directories:
    - This would reset the directory permissions correctly.
    - It would also prevent others from listing your directories.

## A single `sed` command for each file found is executed

```sh
find . -type f -name '*.txt' -exec sed -i 's/SEARCH/REPLACE/g' {} \;
```

## All files found are given as parameters to `sed` all at once

```sh
find . -type f -name '*.txt' -exec sed -i 's/SEARCH/REPLACE/g' {} +
```

## Find and execute multiple commands on files

```sh
find /usr/local/bin/ -name '*.sh' -type f \
    -exedir echo "Executing {}" \; -execdir chmod +x {} \;
```

## Find and execute multiple commands with success condition

```sh
find "${base_dir}" -type f -name '*.url' \
    -exec grep "${search_term}" {} \; -exec echo {} \;
```

## Find using multiple filename patterns

```sh
find . -type f -o -iname '*.asf' -o -iname '*.avi'
```

## Find files with size 0, greater than 1MB, greater than 500K

```sh
find . -type f -size 0
find . -type f -size +1M
du -d 0 -t 500K -h /bin/*
```

## Find empty directories

```sh
find . -type d -empty
```

## Find directories in directory only not going any deeper

```sh
find . -maxdepth 1 -mindepth 1 -type d
```

## Find all images from current directory

Very slow because it reads the file content

```sh
find . -type f -exec file {} \; | grep -o -P '^.+: \w+ image'
```

## Find and delete file marked as `*.xmp`

```sh
find . -type f -name '*.xmp' | sed 's/\.xmp//' | tr '\n' '\0' | xargs -0 rm -f "{}"
```

## Find and print size

For `printf` format, see <http://man7.org/linux/man-pages/man1/find.1.html>

```sh
find . -type f -name 'all.txt' -printf '%s %p\n' | \
    numfmt --field=1 --to=iec-i  --format='%10.3f'
find . -type f -name 'all.txt' -printf '%s %k %p\n'
```

## Trim Trailing spaces in text file

```bash
find . -name "*.md" -type f -print0 | xargs -0 sed -i 's/[[:space:]]*$//'
```

## Find files with Trailing spaces in them

```bash
find . -type f -exec egrep -l " +$" {} \;
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)

