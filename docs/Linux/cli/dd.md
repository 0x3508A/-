# `dd` Disk dup command

Most Feared "Disk Dump" or "Data Duplicator" Command

References:

- <https://linoxide.com/linux-dd-command-create-1gb-file/>
- <https://linuxhandbook.com/dd-command/>
- <https://www.cyberciti.biz/faq/unix-linux-dd-create-make-disk-image-commands/>

## Introduction

The `dd` command in Linux is a utility for copying and converting files and has many practical uses.

It has been suggested that the name is derivative of an older **IBM Job Control Language** function where `dd` stood for “Data Definition”.
In Linux, the abbreviation stands for “Data Duplicator” or “Disk Dump” or a variety of other alliterations depending on your source.

References:

- <https://linoxide.com/linux-dd-command-create-1gb-file/>
- <https://linuxhandbook.com/dd-command/>
- <https://www.cyberciti.biz/faq/unix-linux-dd-create-make-disk-image-commands/>

## Command Format

```sh
dd if=<filename> of=<filename> [options]
```

## Command to Store Disk Image

```sh
dd if=/dev/sda2 of=home_backup.img bs=1024
```

This command would store the `/dev/sda2` partition into a file called `home_backup.img`.
Transfer block size of disk is set at `1024`, this may need to changed based on Partition geometry.

## Command to Restore the Disk Image to Disk

```sh
dd if=/tmp/home_backup.img of=/dev/sda2 bs=1024
```

This command would **write** the `/dev/sda2` partition with the image `home_backup.img`.
Transfer block size of disk is set at `1024`, this may need to changed based on Partition geometry.

## Command to Write to CD-ROM or DVD

```sh
dd if=/dev/cdrom of=ubuntudvd.iso bs=2048 conv=noerror,sync
```

Since its CD/DVD so `bs=2048` is important.

The `conv=noerror` would ignore all writing errors.
The `conv=noerror,sync` would help to make sure any missing blocks of data will automatically be padded with null information.

It's important to make sure that your source and destination files have the same `bs` set for these operations, otherwise they will not have the intended results.

## Create Bootable USB drive

```sh
sudo mkfs.ext4 /dev/sdb
sudo dd if=someFile.iso of=/dev/sdb
```

Here we are first formatting the `/dev/sdb` as an `EXT4` partition.
Then we are writing a ISO image to the drive.
**Note:** The USB drive is bootable by the virtue of `someFile.iso`.

## Writing Raspberry Pi SD Card using `dd`

<https://raspi.debian.net/how-to-image/>

Here is the command:

```sh
xzcat 20210408_raspi_3_bullseye.xz | \
    sudo dd of=/dev/{YOUR_DEVICE} bs=64k oflag=dsync status=progress
```

The first part `xzcat` would unarchive the `lzma` archive of the Rasbian disk
image. The second part is the actual `dd` command that writes to your SD card.

## Create a Compressed Disk Image **WARNING!!**

!!! warning "**UNTESTED CODE!!!**"

```sh
dd if=/dev/vda conv=sync,noerror | gzip -c >/tmp/vdadisk.img.gz
# To increse compression use
dd if=/dev/vda conv=sync,noerror | gzip -c -9 >/tmp/vdadisk.img.gz
```

Because dd creates the exact content of an entire disk, it means that it takes too much size.
You can decide to compress the disk image with this command.

The pipe `|` operator makes the output on the left command become the **input** on the right command.
The `-c` option writes output on **standard output** and keeps original files unchanged.

Here is one more using **BZip2** with a slightly more compressed image:

!!! warning "**UNTESTED CODE!!!**"

```sh
dd if=/dev/vda conv=sync,noerror |\
    bzip2 -c --best > /tmp/vdadisk-raw.bz2
```

Reference = <https://serverfault.com/a/1081381>

Here is the **most** compressed one using **7zip**:

!!! warning "**UNTESTED CODE!!!**"

```sh
dd if=/dev/vda conv=sync,noerror bs=32M |\
    7z a -si > /tmp/vdadisk-raw.7z
```

In this the `bs=32M` helps to speed up the process.
Reference = <https://serverfault.com/a/1081381>

## Restoring from the Compressed Disk Image **WARNING!!**

!!! warning "**UNTESTED CODE!!!**"

```sh
gzip -dc /tmp/vdadisk.img.gz | dd of=/dev/vda
```

The `-d` option here is to uncompress.

Here is the **Bzip2** with Output to disk:

!!! warning "**UNTESTED CODE!!!**"

```sh
bzip2 -dc /tmp/vdadisk-raw.bz2 | dd of=/dev/vda
```

Here is the **7z** with Output to disk:

!!! warning "**UNTESTED CODE!!!**"

```sh
7z e -so /tmp/vdadisk-raw.7z | dd of=/dev/vda
```

## Dealing with block size in `dd` command

Before we get into the next example, let's talk about `BS`, or block size. If you've seen this used to specify a value with dd commands, you might wonder why it's there.

If your curiosity lead you to an internet search, then I'm willing to bet that you're probably still wondering why it's there.

I'll try my best to give a plain language explanation. Block devices are usually physical media with finite storage.

You can look up information on a medium like a disc by seeking a specific block of data. So for instance, the system can read a CD-ROM
and search for information starting at block 500 (an arbitrary number).
It can also be used to "bookend" information and maybe use info from block 500 to block 1500.

These blocks can be segmented in ways that make it efficient for the system to analyze. This may reflect the storage space of the medium, or standard system specs the medium is likely to be associated with.

I'll continue with the example of a CD-ROM which has its own defined block size (2048). Each block must have a maximum of 2048 bytes. Even if a block only contains 100 bytes of data, it will still take up the same 2048 bytes.

There are some cases where you may want to define the block size to make `dd` run faster or prevent data corruption. Going back to our CD-ROM example, creating blocks of a different size could cause anomalies when it's time for the data to be read.

If left undefined, dd will use a block size of `512`. This is the smallest block size that a typical hard drive can read.

If your medium is not confined to a certain block size, you're probably safe adjusting it for performance (write time). Let's look at a few
examples.

-   Performance with unspecified block size

```sh
[linuxhandbook@fedora ~]$ sudo dd if=/dev/sda of=home_backup.img
[sudo] password for linuxhandbook:
dd: writing to 'home_backup.img': No space left on device
31974953+0 records in
31974952+0 records out
16371175424 bytes (16 GB, 15 GiB) copied, 113.848 s, 144 MB/s
```

-   Performance with block size of 1024

```sh
[linuxhandbook@fedora ~]$ sudo dd if=/dev/sda of=home_backup.img bs=1024
[sudo] password for linuxhandbook:
dd: error writing 'home_backup.img': No space left on device
15987477+0 records in
15987476+0 records out
16371175424 bytes (16 GB, 15 GiB) copied, 75.4371 s, 217 MB/s
```

You can see that the process was performed at a faster speed. Another run with a block size of of `4096` was faster yet with a rate of `327 MB/s`.
System caching can also play a role in speed, but that is a topic for another day.

You may have noticed the variation in the number of records in and out.
This is because we are changing the size of each block and therefore the capacity of the individual blocks, despite the output file remaining the same size.
For this reason, adjusting the `bs` value can have unintended consequences. For example, it could lead to discrepancies when a checksum performed.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
