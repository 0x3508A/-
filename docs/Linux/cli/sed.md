# `sed` Stream EDitor Command

Stream EDitor for filtering and text transforming Tool.

## `sed` Online Evaluation Tool

<https://sed.js.org/>

## Replace all Wiki links with Markdown

```sh
# Wiki Link with Name
echo "[[Nix Modules|lang.nix.nix-modules]]" |\
    sed  's/\[\[\(.*\)|\(.*\)\]\]/[\1](\2.md)/;s/\[\[\(.*[^<>].*\)]\]/[\1](\1.md)/'
# Wiki Link Without Name
echo "[[lang.nix.nix-modules]]" |\
    sed  's/\[\[\(.*\)|\(.*\)\]\]/[\1](\2.md)/;s/\[\[\(.*[^<>].*\)]\]/[\1](\1.md)/'
```

`sed` Pattern:
`'s/\[\[\(.*\)|\(.*\)\]\]/[\1](\2.md)/;s/\[\[\(.*[^<>].*\)]\]/[\1](\1.md)/'`

## Uncomment a Line in a file

```sh
sudo sed -i 's/#en_US.U/en_US.U/1' /etc/locale.gen
```

This modify the file `/etc/locale.gen` and uncomment the line `#en_US.UTF-8 UTF-8`.
This is useful in **Arch Linux** Installation.

## Replace a line with a given pattern

```sh
sudo sed -i '/GRUB_DEFAULT=/c\GRUB_DEFAULT=saved' /etc/default/grub
```

This would replace all occurrences of `GRUB_DEFAULT=` in a line with `GRUB_DEFAULT=saved` in the `/etc/default/grub` file.
The above helps to save new **Grub** Menu choice for next boot as **default**.

## Modifying a file with an Environment Variable in Pattern

```sh
# Defined somwhere initially
export FSTYPE='ext4'
...
# Use of the Env Variable
sed -i "s/MODULES=(/MODULES=($FSTYPE /g" /etc/mkinitcpio.conf
```

This would add the `ext4` to the **Modules** list that would be loaded at boot-up using `/etc/mkinitcpio.conf`.

## Delete empty line

```sh
sed -e "/^$/d" 1x.txt > 2x.txt
```

## Delete all newlines

Concatenate each line of `1x.txt` into **1 line of string**.

```sh
sed ":a;N;$!ba;s/\n//g" 1x.txt > 2x.txt
```
Reference <http://sed.sourceforge.net/sedfaq5.html#s5.10>

## Print line without 'code'

```sh
sed -n "/code/!p" 1x > 2x.txt
```
In bash shell, use single quote

## Delete line 2 up to line 10 inclusively

```sh
sed -e "2,10d" 1x.txt > 2x.txt
```

## Skip the 3rd line(Replace 'a' with 'b' for all lines except the 3rd line)

```sh
sed -e "3n; s/a/b/g" 1x.txt > 2x.txt
```

## Insert LINE at line 3.

If no number before i is defined, then `sed` will insert LINE before every line in `1x.txt`.
```sh
sed -i "3i\LINE" 1x.txt
```

## Append WORD at the end of file

```sh
sed -i "$a\WORD" 1x.txt
```

## Use semi-colon(;) as OR operator
Print line containing expression1 or expression2 or etc.

```sh
sed -n "/exp1/p; /exp2/p" 1x.txt
```

## Number each line of a file

```sh
sed = filename.txt | sed "N;s/\n/\t:/" > 1x.txt
```

## Replace empty lines with previous non-empty line

```sh
sed -e "/[^ ]/{h}" -e g input.txt
```
Reference <http://www.computing.net/unix/wwwboard/forum/7768.html>

## Copy pattern found multiple times using ampersand(&)

```sh
echo 123 abc | sed "s/123/& | & | &/"
```

## Copy pattern found multiple times using parentheses.

```sh
echo 123 abc | sed -e "s/\(123\)\(.*\)/ \1 \1 \2 \1 \2 /"
```

## Process every 4th line

```sh
sed -e "0~4s/$/ReplaceString/g" input.txt > output.txt
```

## Rename Files with unique pattern using `sed`

First lets look at the problem.

We need to rename the following type of files:

```sh
$ for f in cli.nix.*; do echo "$f"; done
cli.nix.learning.bootstrap-minimal-nix-and-nix-flakes.md
cli.nix.learning.journal.2021.04.12.md
cli.nix.learning.journal.2021.04.13.md
cli.nix.learning.journal.2021.04.14.configurationnix.md
cli.nix.learning.journal.2021.04.14.md
cli.nix.learning.journal.2021.05.18.md
cli.nix.learning.md
cli.nix.learning.nix-on-manjaro.md
cli.nix.md
cli.nix.repl.md
```

The `cli` needs to get replaced by `tools`

### Practice replace with `sed`

```sh
$ echo "cli.nix.md" | sed "s/cli/tools/"
tools.nix.md
```

This would help us to produce the required name for the `mv` command.

Lets bring everything together

### Solution for Renaming using `sed`

```sh
for f in cli.nix.*; do mv "$f" $(echo "$f" | sed "s/cli/tools/"); done
```

That's it nimble and compact rename.

```sh
$ ls tools.*
tools.nix.learning.bootstrap-minimal-nix-and-nix-flakes.md
tools.nix.learning.journal.2021.04.12.md
tools.nix.learning.journal.2021.04.13.md
tools.nix.learning.journal.2021.04.14.configurationnix.md
tools.nix.learning.journal.2021.04.14.md
tools.nix.learning.journal.2021.05.18.md
tools.nix.learning.md
tools.nix.learning.nix-on-manjaro.md
tools.nix.md
tools.nix.repl.md

```

Done !

## Strip first few lines in File and Print it

```sh
# Remove the First 10 lines from the test.txt file while printing the rest
sed 1,10d test.txt

# Remove only the first line from the test.txt and print
sed 1,1d test.txt
```

The above would not remove the line from the actual file but just alter the printing of the files.

### Reference

Search - `rename a bunch of files in linux with specific pattern`

Links

- <https://stackoverflow.com/questions/6840332/rename-multiple-files-by-replacing-a-particular-pattern-in-the-filenames-using-a>


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
