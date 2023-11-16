# `grep` Content Search Command

## Reverse a `grep` search

- Means **report if not found**

```sh
export MKI=/etc/mkinitcpio.conf
[ -z "$(grep '^HOOKS=(.*encrypt.*)' $MKI)" ] && echo "not found"
```

This query would check the file `/etc/mkinitcpio.conf`.
If the Regex `"^MODULES=(.*ext4.*)"` is found then it would report error.
If *NOT FOUND* then it would return TRUE and print `not found`.

## Grep tab using Perl regex

```sh
grep -P "\t"
```

## Select multiple text patterns(e.g "this" or "that")

```sh
grep -E "this|that"
```

## Grep either pattern1 or pattern2.

```sh
grep -e '(pattern1)|(pattern2)'
grep '\(pattern1\)\|\(pattern2\)'
```

## Searches in files recursively through directories for pattern

while *ignoring filename* containing `ignoreString`

```sh
grep pattern $(find . -type f | grep -v 'ignoreString')
grep pattern $(find . -name '*.txt' -or -name '*.java' | grep -v 'ignoreString')
grep -E -rn "Your search string" /location/
```

## Show 15 lines before and after

```sh
grep -C 15
```

## Show row number

```sh
echo -e "one\ntwo"| grep -n ^
```

## Show all but highlight pattern found

```sh
echo -e "one\ntwo\nthree"| grep -E 'e|$'
```

## Show all lines and highlight pattern.

```sh
grep -E '^|pattern' file
```

## Color match term

```sh
grep --color=auto
```

## Grep white spaces up to pound key(#)

```sh
grep -v "^[[:space:]]*#"
```

## Grep 3 digits

```sh
echo 1459mmmm34mm | grep -E [0-9]{3}
echo 2786mmmm34mm | grep -E [[:digit:]]{3}
```

## Grep dash / hyphen

```sh
echo -e "--x---t--\nas" | grep -E '[\-]{2}'
```

## Count number of occurrence found

```sh
grep -c ^processor /proc/cpuinfo
```
Number of CPUs in this case.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
