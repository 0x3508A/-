# Expand Memory on LinkIt Smart 7688

Its Inspired by the Article :

- <https://www.hackster.io/akashravichandran/expand-the-memory-in-linkit-7688-e37aa5>
- Here is the **[PDF version](./expand-memory-linkit/Expand-the-Memory-in-Linkit-7688-Hackster.pdf)**

## 1. Serial Connection

This same as that mentioned in the [Main document](./linkit-smart-7688.md#steps-to-connect-the-serial-terminal).

This would help us to enter the **OpenWrt** Console.

## 2. SD Card Preparation

**Format** the *SD card* on a *PC* with at least `1GB` or so.

Though the tutorial uses `8GB` card, we have used an `1GB` card for *legacy sake*.

After formatting it would have a normal *DOS partition table* with *Single partition* possibly `FAT32`.

Finally insert into the `microSD` card slot of **LinkIt Smart 7688** module.

## 3. Install the packages

Press *Enter* in the *Serial Console* this would get you to the command prompt.

```sh
# 1. Update the repositories
opkg update
# 2. Instal the needed packages
opkg install block-mount kmod-fs-ext4 kmod-usb-storage-extras e2fsprogs fdisk
```

## 4. Format the SD Card to ext4

Since, we already have *one partition* on the SD card we can view the same using the `fdisk` command

```sh
# Open the the SD card with fdisk
fdisk /dev/mmcblk0
# Issue the Print command:
p
# Thos would print something like
Disk /dev/mmcblk0: 922 MiB, 966787072 bytes, 1888256 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x0ff12348

Device         Boot Start     End Sectors  Size Id Type
/dev/mmcblk0p1       2048 1888255 1886208  921M 83 FAT32
# Press q to Quit
q
```

We can now format this partition to `ext4` :

```sh
mkfs.ext4 /dev/mmcblk0p1

mke2fs 1.46.5 (30-Dec-2021)
Discarding device blocks: done
Creating filesystem with 235776 4k blocks and 59008 inodes
Filesystem UUID: bf4c527c-123a-456b-a289-0f63d7407e86
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

```

## 5. Copy the overlay file system

We need to first copy the current `root` file system.
For this we would first mount the SD card partition and then initiate a copy.

```sh
# Mount the SD Card
mount /dev/mmcblk0p1 /mnt
# Compress copy the whole overlay file system to the SD card mount point
tar -C /overlay -cvf - . | tar -C /mnt -xf -
# Un mount the SD card
umount /mnt
```

## 6. Configure the SD Card to be mounted automatically

We would need the newly created overlay file system to be mounted when the system boots.

```sh
# Transfer the mount description to `fstab`
block detect > /etc/config/fstab
# Edit the `fstab` to call the SD card as root
vi /etc/config/fstab
```

Now you would need to operate the `vi` editor here are few helpful short cuts:
1. the `vi` editor is by default in *NORMAL* mode to enter this mode keep pressing `Esc` key
2. To edit the file in `vi` editor you need to press `Insert` key to enter *INSERT* mode
3. To save the file go *NORMAL* mode and then press `Shift + :` to get *COMMAND* mode, enter `wq`.

Initially the `fstab` file might look like this.

```sh
config 'global'
        option  anon_swap       '0'
        option  anon_mount      '0'
        option  auto_swap       '1'
        option  auto_mount      '1'
        option  delay_root      '5'
        option  check_fs        '0'

config 'mount'
        option target '/mnt/mmcblk0p1'
        option uuid '123e4567-e89b-12d3-a456-426614174000'
        option enabled '0'
```

We would change the lines `option target` and `option enabled` under `config 'mount'`.

We would need to set `enabled '1'` the mounting and reset the mount to be actually `/overlay`

Here is how the modified file should look like:

```sh
...
config 'mount'
        option target '/overlay'
        option uuid '123e4567-e89b-12d3-a456-426614174000'
        option enabled '1'
```

Finally **save the file** using the shortcuts listed above.

Reboot the module using `reboot` command.

## 7. Verify that we are using the SD Card

Use the command `df -h` and carefully look at the `/overlay` part.

```sh
df -h

Filesystem                Size      Used Available Use% Mounted on
/dev/root                 3.8M      3.8M         0 100% /rom
tmpfs                    59.8M     88.0K     59.7M   0% /tmp
/dev/mmcblk0p1          888.2M      4.1M    822.1M   0% /overlay
overlayfs:/overlay      888.2M      4.1M    822.1M   0% /
tmpfs                   512.0K         0    512.0K   0% /dev

```

**In our case we have big `888MB` *SD card file system* !!.**

----
<!-- Footer Begins Here -->
## Links

- [Back to main LinkIt Article](./linkit-smart-7688.md)
- [Back to OpenWrt and MT7688 Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
