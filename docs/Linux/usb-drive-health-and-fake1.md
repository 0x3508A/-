# USB Disk Health and Fake detection

This process can be used in case on *SD Cards* and *other memory types* also.

It helps to *detect and root out fake capacity* reported by some cheap **USB drives** and **SD Cards**.

References:

- <https://www.cyberciti.biz/faq/linux-check-the-physical-health-of-a-usb-stick-flash-drive/>

## First Option: Using `badblocks`

!!! warning "Permanent Damage"

    This method though being **destructive** is very thorough.

    In this method your **disk will be made blank**.

    Means the **Filesystem and Partitions are over written**.

    **Be CAREFUL !!**.

This method would not detect if your USB Disk or Pen-drive is fake,

It would only ensure that the physical health is good.

And since this is a **Destructive** test make sure to **backup**.

Here is how you go about.

### 1. First we need to make sure that the following is installed

```sh
# This includes the `badblocks`
sudo pacman -S e2fsprogs
```

Typically this is already available installed in **Manjaro**.

[Links to `e2fsprogs` ](https://archlinux.org/packages/core/x86_64/e2fsprogs/)

for `badblocks` utility : <https://archlinux.org/packages/core/x86_64/e2fsprogs/>

Insight into `badblocks` command:
<https://wiki.archlinux.org/title/Badblocks>

### 2. To check we *insert* the **USB drive** and identify its disk.

!!! warning
    **Do not mount** the **USB drive** at this point !!!

```sh
# We are creating a 'error.log' file for any errors in the
# write process.
sudo badblocks -w -s -o error.log /dev/sdX
```

Here `/dev/sdX` is a placeholder replace it with the actual disk.

And to find out the disks mounted in your system use [`lsblk` command](./cli/lsblk.md).

This would take a while to complete.

### 3. Format Disk

Finally after the test is over you can **format** the `/dev/sdX` to a **FAT32** partition disk.

## Second Option: using `f3` Program to detect fakes

This option is better in case you would like to only check the capacity.

Its to confirm if the USB Drive or Pen-drive is Fake or real.

Here is the [project home-page](https://github.com/AltraMayor/f3/):

<https://github.com/AltraMayor/f3/>

Also this method is *non-destructive* but needs a **empty & formatted** USB Disk/Pen-drive.

Here are the steps to do the full test:

### 1. We would fist need to install the dependency for `f3`

```sh
sudo yay -S f3
```
In Arch Linux its only available as a **AUR** package:

<https://aur.archlinux.org/packages/f3>

Use you own `AUR` helper of choice to install.

### 2. First mount the disk to a suitable location on the PC

```sh
sudo mount /dev/sdX /mnt
```

### 3. Test the writing to the disk

```sh
f3write /mnt
```

### 4. Test the reading of the disk

```sh
f3read /mnt
```

### 5. Test the Probing of the disk

This is thorough but takes more time.
This would test out the capacity as well as health of the disk.

```sh
sudo f3probe --destructive --time-ops /dev/sdX
```

!!! warning "Permanent Damage !!"

    This command will **delete all data** on USB Disk or Pen-Drive.

    **It would take very long to execute.**

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
