# `efibootmgr` UEFI Boot editor

Linux User Space EFI Boot Manager for UEFI systemd.

## Source

<https://github.com/rhboot/efibootmgr>

## Reference

<https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface#efibootmgr>

## Usage

```sh
usage: efibootmgr [options]
      -a | --active          sets bootnum active
      -A | --inactive        sets bootnum inactive
      -b | --bootnum XXXX    modify BootXXXX (hex)
      -B | --delete-bootnum  delete bootnum
      -c | --create          create new variable bootnum and add to bootorder
      -d | --disk disk       (defaults to /dev/sda) containing loader
      -e | --edd [1|3|-1]    force EDD 1.0 or 3.0 creation variables, or guess
      -E | --device num      EDD 1.0 device number (defaults to 0x80)
      -f | --reconnect       Re-connect devices after driver is loaded
      -F | --no-reconnect    Do not re-connect devices after driver is loaded
      -g | --gpt             force disk w/ invalid PMBR to be treated as GPT
      -i | --iface name      create a netboot entry for the named interface
      -l | --loader name     (defaults to \elilo.efi)
      -L | --label label     Boot manager display label (defaults to "Linux")
      -n | --bootnext XXXX   set BootNext to XXXX (hex)
      -N | --delete-bootnext delete BootNext
      -o | --bootorder XXXX,YYYY,ZZZZ,...     explicitly set BootOrder (hex)
      -O | --delete-bootorder   delete BootOrder
      -p | --part part          (defaults to 1) containing loader
      -q | --quiet              be quiet
      -t | --timeout seconds    Boot manager timeout
      -T | --delete-timeout     delete Timeout value
      -u | --unicode | --UCS-2  pass extra args as UCS-2 (default is ASCII)
      -v | --verbose            print additional information
      -V | --version            return version and exit
      -w | --write-signature    write unique sig to MBR if needed
      -@ | --append-binary-args append extra variable args from
        file (use - to read from stdin).
```

## Listing all EFI Boot or UEFI Entries from system BIOS

```sh
$ efibootmgr
...
...
BootCurrent: 0004
BootNext: 0003
BootOrder: 0004,0000,0001,0002,0003
Timeout: 30 seconds
Boot0000* Diskette Drive(device:0)
Boot0001* CD-ROM Drive(device:FF)
Boot0002* Hard Drive(Device:80)/HD(Part1,Sig00112233)
Boot0003* PXE Boot: MAC(00D0B7C15D91)
Boot0004* Linux
...
```

### Deleting a EFI boot or UEFI Entry

```sh
$ efibootmgr -b 0004 -B
```

Here the `0004` it the hexadecimal boot entry from the **listing** command.

### Change Boot Order of EFI boot or UEFI entries

```sh
$ efibootmgr -o 0002,0001,0000,0003
```

This would change the order to boot from `Hard Disk` first and
then `CD-ROM` and finally `Diskette`.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
