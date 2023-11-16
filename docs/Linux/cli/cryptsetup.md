# `cryptsetup` LUKS Command

Linux Unified Key Setup or `LUKS` provides disk encryption in Linux.

Works on a block device, rather than on a file system.
Makes full disk Encryption possible as well as file based encrypted storage.

It only works under Linux and is not supported in any other OS.

GRUB's `cryptodisk` feature:
- #GRUB asks for a passphrase
- then unlocks the device on its own
- then looks for config, kernel, initramfs, etc.

The primary Command to `LUKS` is `cryptsetyp`.

The `cryptsetup` defaults:
- `LUKS2` for encryption
- New , different on-disk format
- Not supported by GRUB2

How To Use Linux LUKS Full Disk Encryption For Internal / External / Boot Drives

<https://www.youtube.com/watch?v=5rlZtasM-Pk>

Good Video Detailing all the Ways needed for **Password** based LUKS encryption.

Archlinux `dm-crypt/Device` :

<https://wiki.archlinux.org/title/Dm-crypt/Device_encryption>

Archlinux `dm-crypt` Full :

<https://wiki.archlinux.org/title/Dm-crypt>



----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)

