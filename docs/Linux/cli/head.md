# `head` Command

## Details

The `head` command prints the first lines *(10 lines by default)* of one or more files or piped data to standard output.

### References

- <https://www.ibm.com/docs/en/aix/7.2?topic=files-displaying-first-lines-head-command>
- <https://linuxize.com/post/linux-head-command/>
- <https://superuser.com/a/470957> - Random Data file generation

## Command Syntax

```sh
head [OPTION]... [FILE]...
```

- `OPTION` - The most common options in the next sections.
- `FILE` - Zero or more input file names. If no `FILE` is specified, or when `FILE` is `-`, `head` will read the **standard input / pipe**.

## Options

### First 10 lines or Default - No options

```sh
head filename.txt
```

In its simplest form, when used without any option.
The `head` command displays the *first 10 lines*.

### Display a Specific Number of Lines

```sh
head -n <NUMBER> filename.txt
```
Use the `-n` (`--lines`) option followed by an integer `<NUMBER>` specifying the number of lines to be shown.

You can omit the letter `n` and use just the hyphen (`-`) and the number (with no space between them).

Example:

```sh
head -n 30 filename.txt
```

or

```sh
head -30 filename.txt
```

Both will print the first 30 lines from the file `filename.txt`.

### Display a Specific Number of Bytes

The `-c` (`--bytes`) option allows to print a specific number of bytes.

```sh
head -c <NUMBER> filename.txt
```

Example:

```sh
head -c 100 filename.txt
```

This would print the first 100 bytes from the file `filename.txt`

You can also use a multiplier suffix after the number to specify the number of bytes to be shown.
- `b` multiplies it by `512`
- `kB` multiplies it by `1000`
- `K` multiplies it by `1024`
- `MB` multiplies it by `1000000`
- `M` multiplies it by `1048576`, and so on.

Example using multiplier:

```sh
head -c 5k filename.txt
```

This would read *5 KiloByte* ( `5 x 1024 = 5120` ) bytes from the file `filename.txt`.

#### Generating Random output FILE

```sh
head -c 1M /dev/urandom > sample.bin
```

This is very useful way generate custom random block of Data.
It would write 1 MegaByte (1048576) of random data into `sample.bin`

## Displaying Multiple files

If multiple files are provided as input to the head command, it will display the *first 10 lines* from *each provided file*.

```sh
head filename1.txt filename2.txt
```

This example prints first 2 lines of each specified file:

```sh
head -n 2 filename1.txt filename2.txt
```

This can be especially useful in looking into code.

## Combining other commands with `head`

The `head` command can be used in combination with other commands by redirecting the standard output from/to other utilities using pipes.

The following command will hash the `$RANDOM` environment variable , display the first `32 bytes` and display `24 characters` random string:

```sh
echo $RANDOM | sha512sum | head -c 24 ; echo
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
