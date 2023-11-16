# `lsblk` Command

`lsblk` lists information about all available or the specified block devices.
This can be used to identify the storage devices connected on the system of various types.
Be it a FUSE partition, LUKS volume or a standard NTFS, the `lsblk` command lets you see it all. It also helps to find *mount points* for the currently mounted storage.
**Note:** This command requires `sudo` permissions.

There are multiple ways of using the `lsblk` command. These are defined by the options used while executing the command.

### References

- Command Manual <https://man7.org/linux/man-pages/man8/lsblk.8.html>
- <https://www.makeuseof.com/lsblk-command-in-linux/>
- <https://www.geeksforgeeks.org/lsblk-command-in-linux-with-examples/>

## Display block / Storage devices

```sh
sudo lsblk
```

This would display all normal volumes and *non-empty partitions*.

## Display All block / Storage devices including empty storage

```sh
sudo lsblk -a
```

## Display devices with Storage in Bytes or Human readable form

```sh
sudo lsblk -a -b
```

## Text Processing enabled Field select

```sh
sudo lsblk -a -b -o SIZE, NAME, MOUNTPOINT
```
This would display only the 3 columns of information for next stage text processing.

## Text Processing special hide column headings

```sh
sudo lsblk -a -b -o SIZE, NAME, MOUNTPOINT -dn
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)


