# `veracrypt` multi platform Disk encryption tool

Disk Encryption Software based on **True Crypt** but a more secure version.

- Website

     <https://www.veracrypt.fr/en/Home.html>

- Downloads for `veracrypt`

     <https://www.veracrypt.fr/en/Downloads.html>

- Command-line Help (Very poor)

     <https://veracrypt.fr/en/Command%20Line%20Usage.html>

- How to Install `veracrypt` in Ubuntu like Linux distributions:

     <https://linuxhint.com/install-veracrypt-linux-mint/>

## Compatibility of `veracrypt`
> 
> **`veracrypt` also works on Windows and MAC**.
> and obviously on *Android* and *iPhone*.

### `veracrypt` For Android use : EDS Lite

<https://play.google.com/store/apps/details?id=com.sovworks.edslite>

This needs mandatory `AES256` and `SHA512` for `veracrypt` compatibility.

There is an even more powerful version **Paid** app.

<https://play.google.com/store/apps/details?id=com.sovworks.eds.android>

### `veracrypt` For iPhone use : Disk Decipher

<https://apps.apple.com/au/app/disk-decipher/id516538625>

It also supports most format since its a **Paid** app. Money well spent on this one.
It can also open files readonly if needed.

## To Get Help in Shell for `veracrypt`

```sh
veracrypt -h
```

This will display the help on the command line as well as show a window containing the help for easier copy paste.

## Mounting a Container as read-only in `veracrypt`

```sh
veracrypt -t -k "" -m ro --pim=0 \
	  --protect-hidden=no Container1 /mnt/veracrypt1
```

- The `-t` option provides the CLI or the Text only interface.
- The part `-m ro` is what makes the mount read-only.
- And `-k ""` disables the key-files option.

Typically a encrypted volume as only one way to open it.
However there may be many **PIM** slots used.
In a default case using `--pim=0` uses the first or only slot.

For this admin password as well as the encrypted volume password.
For easier operation just add `sudo` in front.

In this case we are using a file `Container1` is located in the current directory.
It can also be a volume or disk like `/dev/sda4` etc.

## Mounting a Container Normally to a Directory in `veracrypt`

```sh
sudo veracrypt -t -k "" --pim=0 --protect-hidden=no \
     /Backup/Container1 /home/user/secure
```

Here the file `/Backup/Container1` would get mounted to the **existing directory** in `user` home folder `secure`.

## Mounting a Container Using a Key-File in `veracrypt`

```sh
sudo veracrypt -t -k /Backup/secret.keyfile \
     --pim=0 --protect-hidden=no \
     /Backup/Container4 \
     ~/fixed
```

In the above command we are using the **Key-file** located at `/Backup/secret.keyfile`.
This is typically in a **Pen-Drive** or another secure location.
In this can no Password would be asked since we are providing the `-k <Key-Fil>` option.

## Some [comparison and other encryption topologies](./veracrypt-luks-compare1.md) than `veracrypt`

Here is the [document](./veracrypt-luks-compare1.md) on this one.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
