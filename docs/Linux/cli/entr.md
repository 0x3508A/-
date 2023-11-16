# `entr` file change watch Utility

It *observes* *watches* *on-change* Executes commands.

References:

 - <https://github.com/clibs/entr>
 - <https://www.systutorials.com/docs/linux/man/1-entr/>

## Install

```sh
sudo pacman -S entr
```

## Run compile for Golang Programs as the source code changes

```sj
ls | entr -c bash -c "go run *.go"
```

This would run the Golang files upon changes in a shell.

## Run SQL on change in file

```sh
echo my.sql | entr -p psql -f /_
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)



