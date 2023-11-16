# `paperkey` OpenPGP key to Paper

GPG Paper Backup Tool.

Home Page: <https://www.jabberwocky.com/software/paperkey/>

A reasonable way to achieve a long term backup of OpenPGP (GnuPG, PGP, etc) keys is to print them out on paper. Paper and ink have amazingly long retention qualities - far longer than the magnetic or optical means that are generally used to back up computer data.

Repository: <https://github.com/dmshaw/paperkey/>

## Install

```sh
sudo pacman -S paperkey
```

## Creating `paperkey` for GPG Private Key

```sh
paperkey --secret-key private-master-key --output private-paper-key.txt
```

Here `private-master-key` is the binary export of the GPG Key. This would generate the `private-paper-key.txt` that can be printed. Printing can also be done using **Large QR-Code** or in **OCR** Fonts. That would help to read back the paper key.

## Restoring GPG Private Key from `paperkey`

```sh
paperkey --pubring public.key --secrets paper-key.txt --output private-key
```

Here `public.key` is a Binary export of GPG Key that we are trying to recover. The `paper-key.txt` is the scanned form of the *`paperkey`* printed earlier. Finally `private-key` will be the binary GPG Private Key that can be imported.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
