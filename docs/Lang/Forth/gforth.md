#  `gforth` - Notes

## Starting `gforth` from Windows `CMD`

```batch
gforth -p "c:\Program Files (x86)\gforth"
```
By default **`gforth`** is installed on **Windows** at `C:\Program Files (x86)\gforth`.
Hence we include the same to make sure that we have all the start-up things in place.

**Note:** This needs the `gforth` install directory to be in **Windows PATH**.

### File extensions Supported by `gforth`

| File Extension | Details                                                                                                                   |
| -------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `*.fs`         | Forth stream source file (include with `include <file>` from within `gforth`, or start with `gforth <file1> <file2> ...`) |
| `*.fi`         | Forth image files (start with `gforth -i <image file>`)                                                                   |
| `*.fb`         | Forth blocks file (load with `use <block file> 1 load`)                                                                   |
| `*.i`          | C include files                                                                                                           |
| `*.ds`         | documentation source                                                                                                      |
| `*TAGS`        | `etags` files                                                                                                             |

## How to Include files with Forth Words in `Gforth`

```forth
s" d:\poor-mans-case.forth" included
```

This would include or execute the words in the file `d:\poor-mans-case.forth`.



----
<!-- Footer Begins Here -->
## Links

- [Back to Forth Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
