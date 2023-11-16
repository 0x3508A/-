# `qcow2` format VM Disk Images

### Reference

- Main Reference : This works on Fedora / RedHat or Ubuntu
  <https://www.xmodulo.com/mount-qcow2-disk-image-linux.html>
- Arch Linux Package : `libguestfs`
  <https://archlinux.org/packages/community/x86_64/libguestfs/>
- Windows Guest VM Drives
  <https://libguestfs.org/guestfs-recipes.1.html#drive-letters-over-fuse>

## For Linux Guest VM use the `guestfish` Shell into Disk

<https://libguestfs.org/guestfish.1.html>
<https://libguestfs.org/guestfish.1.html#examples>

In case you want to use for listing:

```sh
><fs> command "/bin/ls -alh /"
><fs> quit
```

## Extracting the Guest Linux VM File System into an Image or `.gz` file

<https://libguestfs.org/guestfs-recipes.1.html#drive-letters-over-fuse#dump-raw-filesystem-content-from-inside-a-disk-image-or-vm>

## Copy out files from Guest File System

<https://libguestfs.org/guestfs-recipes.1.html#drive-letters-over-fuse#export-any-directory-from-a-vm>

## Listing out the File in the Linux Guest VM File System

<https://libguestfs.org/virt-ls.1.html>

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
