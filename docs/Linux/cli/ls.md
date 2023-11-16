# `ls` Filesystem List Command

## List Only Files no Directories
```sh
ls -1ap | grep -v /
```

The first list a single line `-1` with `-p` to help identify directories. The `-a` is optional, it helps to bring even hidden files.
Then the `grep` eliminates all the directories.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
