# Veracryp-LUKS comparisons and More

## Details about Encryption from ArchLinux Wiki

<https://wiki.archlinux.org/title/Data-at-rest_encryption>

## Unlocking LUKS2 volumes with TPM2, FIDO2, PKCS#11 Security Hardware on systemd 248

<https://0pointer.net/blog/unlocking-luks2-volumes-with-tpm2-fido2-pkcs11-security-hardware-on-systemd-248.html>

## `systemd-cryptenroll` — Enroll PKCS#11, FIDO2, TPM2 token/devices to LUKS2 encrypted volumes

<https://www.freedesktop.org/software/systemd/man/systemd-cryptenroll.html>

## How resilient are VeraCrypt and LUKS encrypted volumes against data corruption

<https://newbedev.com/how-resilient-are-veracrypt-and-luks-encrypted-volumes-against-data-corruption>

## `gocryptfs` - simple. secure. fast.

<https://nuetzlich.net/gocryptfs/>

## `eCryptfs`

<https://www.ecryptfs.org/>

## Tomb

<https://www.dyne.org/software/tomb/>

<https://news.ycombinator.com/item?id=7586796>

## Encryption with VeraCrypt

<https://www.linux-magazine.com/Issues/2016/188/VeraCrypt>

## A quick-start guide to OpenZFS native encryption

<https://arstechnica.com/gadgets/2021/06/a-quick-start-guide-to-openzfs-native-encryption/>

## Breaking LUKS Encryption

<https://blog.elcomsoft.com/2020/08/breaking-luks-encryption/>

## Details on LUKS Creations and Use

<https://www.cyberciti.biz/security/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/>

## Resizing LUKS Parition

<https://www.golinuxcloud.com/resize-luks-partition-shrink-extend-decrypt/>

## Enable TRIM support on mounted SSD for LUKS paritions

<https://wiki.archlinux.org/title/Dm-crypt/Specialties#Discard/TRIM_support_for_solid_state_drives_(SSD)>

## How to add Keys to a existing LUKS Partition

<https://access.redhat.com/solutions/230993>

<https://www.thegeekstuff.com/2016/03/cryptsetup-lukskey/>

LUKS-formatted dm-crypt volumes have 8 key slots

```sh
  # Adding Password
  [root ~]# cryptsetup luksAddKey /dev/sda3
  Enter any existing passphrase: Existing passphrase which can be used to open DEV
  Enter new passphrase for key slot: New passphrase to add to DEV
  [root ~]#
  # To Find Block ID for LUKS Partitions
  blkid -t TYPE=crypto_LUKS
  # Create A random Key File
  dd if=/dev/urandom bs=32 count=1 of=/root/random_data_keyfile1
  # Adding Keyfile to the LUKS Partition
  [root ~]# cryptsetup luksAddKey /dev/sda3 /root/random_data_keyfile1
  Enter any passphrase: Existing passphrase which can be used to open DEV
  [root ~]#
  # Own
  sudo cryptsetup open \
       /dev/disk/by-uuid/fd6a893a-cc63-abcd-95e4-123f6123806\
       luks-fd6a893a-abcd-4dae-abcd-b43f6345b806\
       --type luks\
       --key-file ~/secure/mykey1.keyfile
  sudo mount /dev/mapper/luks-fd6a893a-abcd-4dae-abcd-b43f6345b806 /mnt
  sudo umount /mnt
  sudo cryptsetup close luks-fd6a893a-abcd-4dae-abcd-b43f6345b806
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Main Veracrypt Aricle](./veracrypt.md)
- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
