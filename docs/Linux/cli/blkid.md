# `blkid` command

### Reference

- Manual <https://man7.org/linux/man-pages/man8/blkid.8.html>
- <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/s2-sysinfo-filesystems-blkid>
- <https://www.thegeekdiary.com/blkid-command-examples-in-linux/>

## Basic Usage

It can determine the type of content (e.g., `filesystem` or `swap`) that a block device holds, and also the attributes (`tokens`, `NAME=value pairs`) from the content metadata (e.g., `LABEL` or `UUID` fields).

It's best use is to find the UUID.
Other than that `lsblk` fares much better with much more info.

```sh
sudo blkid
/dev/vda1: UUID="7fa9c421-0054-4555-b0ca-b470a97a3d84" TYPE="ext4"
/dev/vda2: UUID="7IvYzk-TnnK-oPjf-ipdD-cofz-DXaJ-gPdgBW" TYPE="LVM2_member"
/dev/mapper/vg_kvm-lv_root: UUID="a07b967c-71a0-4925-ab02-aebcad2ae824" TYPE="ext4"
/dev/mapper/vg_kvm-lv_swap: UUID="d7ef54ca-9c41-4de4-ac1b-4193b0c1ddb6" TYPE="swap"
```

The above example shows the basic use.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
