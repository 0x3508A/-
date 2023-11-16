
# W5 Pro Stick PC Install

## Enable SSH on Stick PC

On Target PC:

```sh
# Set Keymap
loadkeys us
```
WiFi Configuration

```
We would use the 'iw' Shell
 iwctl <- Command is used to activate this shell

For the default 'wlan0' adapter we would use the following commands
 [iwd]# station wlan0 scan
 [iwd]# station wlan0 get-networks
 [iwd]# station wlan0 connect <your-desired-accesspoint-name>
 [iwd]# station wlan0 show
 [iwd]# exit
```

Set Password for root user:

```sh
passwd
```

Once this is set, you have default SSH enabled so this Target PC is ready for connection.

Find out your IP address

```sh
ip -c a
```
Here `-c` will just add the color the IP address shown.

## Update Repositories

```sh
# Setup NTP
timedatectl set-ntp true
# Reflector update
reflector \
	--country India,Singapore,Japan,'South Korea' \
	--age 6 \
	--protocol https \
	--sort rate \
	--save /etc/pacman.d/mirrorlist
# System Update
pacman -Syyy
```

## Drive Format

We had an internal EMMC and SD card both of 32GB.
So decided the following structure:

| Partition           | Type | Size                   | Mount Location  |
| ------------------- | ---- | ---------------------- | --------------- |
| EMMC partition 1    | UEFI | +200M                  | `/mnt/boot/efi` |
| EMMC partition 2    | Root | with rest of the space | `/mnt`          |
| SD Card partition 1 | Swap | +9.4GB                 | `swap`          |
| SD Card partition 2 | Home | with rest of the space | `/mnt/home`     |

## Begin Install

```sh
pacstrap /mnt base linux-lts linux-firmware vim intel-ucode bash-completion
```
It had an Intel CPU hence `intel-ucode` and for easier typing `bash-completion`.

```sh
genfstab -U /mnt >> /mnt/etc/fstab
```

Creating the Filesystem mount table.

Now enter the new system:

```sh
arch-chroot /mnt
```

Setup the basic things:

```sh
# Setup NTP
timedatectl set-ntp true
# Set the Correct time Zone
ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
# Synchronize the clock
hwclock --systohc
```

Locale settings:
```sh
# Edit current locale - Uncomment the line en_US.UTF-8
vim /etc/locale.gen
# Generate
locale-gen
# Edit Locale config
vim /etc/locale.conf
# Add the line `LANG=en_US.UTF-8`
```

Set Host name and Networking:
```sh
echo -n 'arch-iot' > /etc/hostname
echo "\n127.0.0.1\tlocalhost" >> /etc/hosts
echo "::1\t\tlocalhost" >> /etc/hosts
echo "127.0.1.1\tarch-iot.localdomain\tarch-iot" >> /etc/hosts
```

Set the Root password of the system:
```sh
passwd
```

Install remaining utilities:
```sh
pacman -S reflector grub efibootmgr networkmanager network-manager-applet \
 mtools dosfstools base-devel linux-headers git xdg-utils xdg-user-dirs \
 ufw nano nano-syntax-highlighting vim-airline
```

Fix the Boot sequence for headless booting:

`vim /etc/mkinitcpio.conf`

Find the `HOOK=` and make it looks like this:

`HOOKS=(base udev block keyboard autodetect modconf kms keymap consolefont filesystems fsck)`

Finally regenerate the system:

`mkinitcpio -p linux-lts`

Next, install `grub`:

```sh
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB --recheck

# Generate Grub configuration
grub-mkconfig -o /boot/grub/grub.cfg
```

## Enable Services

```sh
systemctl enable NetworkManager
# Don't enable ufw for now till the reboot
```

## Create the user for the System

```sh
useradd -m -g users \
	-G wheel,storage,rfkill,power,lp,lock,uucp \
	-s /bin/bash \
	<UserName>
passwd <UserName>
```

Add the User permission as `sudo`:
```sh
vim /etc/sudoers.d/<UserName>
# Add the Line
# <UserName> ALL=(ALL) ALL
# For No password once logged in you can have
# <UserName> ALL=(ALL) NOPASSWORD: ALL
```

Fix `wheel` Group Permissions :
```sh
EDITOR=vim visudo
# Uncomment the line
# "%wheel ALL=(ALL:ALL) ALL"
```

## End

```sh
# Exit the Chroot
exit
# Unmount all drives
umount -a
```

Reboot the system to enter into the new Arch installation.

----
<!-- Footer Begins Here -->
## Links

- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)