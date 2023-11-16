# QEMU `qcow2` Disk Image

This the default image format used in KVM domain.

## Mounting QEMU `qcow2` Disk Image

Source: <https://gist.github.com/shamil/62935d9b456a6f9877b5>

Local **[PDF](./qmu-disk-image/How-to-mount-a-qcow2-disk-image-GitHub.pdf)** Copy.

This is a quick guide to mounting a `qcow2` disk images on your host server.
This is useful to reset passwords, edit files, or recover something without
the virtual machine running.

We would be using the **Network Block Device** in **QEMU** for this.

### Step 1 - Enable NBD on the Host

```sh
sudo modprobe nbd max_part=8
```

### Step 2 - Connect the QCOW2 as network block device

```sh
sudo qemu-nbd --connect=/dev/nbd0 /var/lib/vz/images/100/vm-100-disk-1.qcow2
```

### Step 3 - Find The Virtual Machine Partitions

```sh
sudo fdisk /dev/nbd0 -l
```

### Step 4 - Mount the partition from the VM

```sh
sudo mount /dev/nbd0p1 /mnt/somepoint/
```

Note the partition names with have the following format :
 - For device `/dev/nbd0`
 - First Partition `/dev/nbd0p1`
 - Second Partition `/dev/nbd0p2` .. etc.

### Step 5 - After you done, `unmount` and disconnect

```sh
sudo umount /mnt/somepoint/
sudo qemu-nbd --disconnect /dev/nbd0
sudo rmmod nbd
```

## Extending Size of VM Guest Image in `qcow2` Format

### Information about VM Disk Image

```sh
sudo qemu-img info win10-HW.qcow2
```

Displays information about `win10-HW.qcow2` Disk image.

### Resize VM Disk Image

Find out the path / location of the **VM Disk image** used in a particular **VM**.
Make sure that the **VM** in question is ***switched OFF***.
Next open terminal to the same directory as that of the **VM Disk image**.

Issue the Expansion command:
 ```sh
 sudo qemu-img resize win10-HW.qcow2 +25G
 ```
Here the above command would expand the `win10-HW.qcow2` by another `25GiB`.

Remember to extend the same *inside* the **Guest VM** as well.

### Reference

- <https://blog.khmersite.net/2020/11/how-to-expand-qcow2/>

----
<!-- Footer Begins Here -->
## Links

- [Back to KVM Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
