# `btrfs` File System

A few commands.

## Create a RAID1 mirror. metadata(-m) and data(-d) are mirrored
    mkfs.btrfs -L MyMirrorDisks -m raid1 -d raid1 /dev/sda /dev/sdb

## Show status.
    btrfs fi show
    btrfs filesystem show

## Add label
    btrfs filesystem label [<device>|<mount_point>] [<newlabel>]

## Show RAID profile
    btrfs filesystem df <mount_point>

## Create a filesystem across four drives (metadata mirrored, linear data allocation)
    mkfs.btrfs -d single /dev/sdb /dev/sdc /dev/sdd /dev/sde

## Stripe the data without mirroring, metadata are mirrored
    mkfs.btrfs -d raid0 /dev/sdb /dev/sdc

## Use raid10 for both data and metadata
    mkfs.btrfs -m raid10 -d raid10 /dev/sdb /dev/sdc /dev/sdd /dev/sde

## Don't duplicate metadata on a single drive (default on single SSDs)
    mkfs.btrfs -m single /dev/sdb

## Use full capacity of multiple drives with different sizes (metadata mirrored, data not mirrored and not striped)
    mkfs.btrfs -d single /dev/sdb /dev/sdc

## Scan all devices
    btrfs device scan

## Scan a single device
    btrfs device scan /dev/sdb

## RAID10 striped mirror
    mkfs.btrfs -m RAID10 -d RAID10 /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1

## Mounting `btrfs` file systems on other Linux PC

Let us assume that `/dev/sda2` contains the `btrfs` file system.
It has 2 volumes `@` and `@home` like a typical Linux install.

```sh
sudo mkdir /mnt/bm
sudo mount -o subvol=@ /dev/sda2 /mnt/bm
# Home directory would already exist in the root `@`
sudo mount -o subvol=@home /dev/sda2 /mnt/bm/home
```

This would mount both the volumes and the file system would can also
be `chroot` into.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Hub](./cli/README.md)
- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
