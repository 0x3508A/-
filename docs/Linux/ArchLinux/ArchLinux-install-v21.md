# Arch Linux Installation Version 21

Yes, that many incomplete and failed attempts.

Following the Instructions from

<https://www.learnlinux.tv/arch-linux-full-installation-guide/>

Video

<https://www.youtube.com/watch?v=DPLnBPM4DhI>

Strangely this install finished without a problem.
Though the final GUI was crude but functional.

Most important there was a sense of understanding of what was needed to fix and add more functionality.

Installed started on `00:30 30-October-2023`.
Literally middle of the night for us here.

And completed around `01:15 30-October-2023`.
Not bad for the 21st attempt.

## Boot from Arch Media

### Internet

Using the `iw` Shell `iwctl` Command.

For the default `wlan0` adapter commands:

```sh
 [iwd]# station wlan0 scan
 [iwd]# station wlan0 get-networks
 [iwd]# station wlan0 connect <your-desired-accesspoint-name>
 [iwd]# station wlan0 show
 [iwd]# exit
```

### Time Configuration

```sh
timedatectl set-ntp true
timedatectl set-timezone "Asia\Kolkata"
```

### Partition setup

List the Disks

```sh
fdisk -l
```

Layout:

1. `EFI 512MiB`
2. `Root 120GiB` LVM Partition
3. No Swap as would use a File for the same

More details about [LVM are documented here](../lvm.md).

Setup of LVM:

```sh
 pvcreate --dataalignment 1m /dev/<DEVICE NAME>
 vgcreate volgroup0 /dev/<DEVICE NAME>
 lvcreate -L 60GB volgroup0 -n lv_root
 lvcreate -l 100%FREE volgroup0 -n lv_home 
 modprobe dm_mod
 vgscan
 vgchange -ay
```

We will mount the Boot partition later only format it.

```sh
mkfs.fat -F32 /dev/<EFI-DEVICE>
```

Format & Mount the Partitions:

```sh
mkfs.ext4 /dev/volgroup0/lv_root
mount /dev/volgroup0/lv_root /mnt
mkfs.ext4 /dev/volgroup0/lv_home
mkdir /mnt/home
mount /dev/volgroup0/lv_home /mnt/home
```

### Create `fstab`

Here we have a new way to create `fstab` file even before the system is available.

```sh
mkdir /mnt/etc
genfstab -U -p /mnt >> /mnt/etc/fstab
```

Check It:

```sh
cat /mnt/etc/fstab
```

### Begin Installation of Arch

No more update to package list needed.

```sh
pacstrap -i /mnt base
```

Enter the System:

```sh
arch-chroot /mnt
```

Install Kernel and headers:

```sh
pacman -S linux linux-headers linux-lts linux-lts-headers
pacman -S nano bash-completion terminus-font
pacman -S base-devel openssh
systemctl enable sshd
```

We install both Kernels and their headers.

We have added a few more ancillary packages to help us with the work.

Some more networking packages:

```sh
pacman -S networkmanager wpa_supplicant wireless_tools netctl
pacman -S dialog
systemctl enable NetworkManager
```

Install LVM Support:

```sh
pacman -S lvm2
```

We would need to add the LVM to load at boot:

```sh
nano /etc/mkinitcpio.conf
```

Add `lvm2` after `block` in `HOOKS=` line.

Rebuild the configs:

```sh
mkinitcpio -p linux
mkinitcpio -p linux-lts
```

Since we installed both we need to build for both.

Finally its time for editing the Locale:

```sh
nano /etc/locale.gen
```

Uncomment `en_US.UTF-8` line.

Generate the Locale:

```sh
locale-gen
```

We create password for `root` user and add a PC user as well. This would be helpful later.

```sh
passwd
useradd -m -g users -G wheel <USERNAME>
passwd <USERNAME>
```

Edit the `sudo` permissions:

```sh
EDITOR=nano visudo
```
Uncomment the `%wheel ALL=(ALL...` line.

Finally installing bootloader stuff:

```sh
pacman -S grub efibootmgr dosfstools os-prober mtools
```

Let's mound our Boot partition now:

```sh
mkdir /boot/EFI
mount /dev/<EFI-DEVICE> /boot/EFI
```

It's late but much better this way.

Install the bootloader:

```sh
grub-install --target=x86_64-efi --bootloader-id=GRUB_UEFI --recheck
```

If this does not work check for locale directory `ls -l /boo/grub`.

Else create it `mkdir /boo/grub/locale`.

Fix the locale:

```sh
cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
```
Don't worry about the strange `/en\@quot/` its correct !

Finally generate the Grub config.

```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

Exit the `chroot` and power off to remove the boot media:

```sh
exit
umount -a
poweroff
```

Now **Restart** the computer.

## Boot into fresh Arch Linux OS for First time

Login using the `<USERNAME>` we created earlier.

Don't worry about internet as its not needed immediately.

Become the super user, here where your `root` password is needed.

```sh
su
```

We first need the SWAP `2GiB` file:

```sh
dd if=/dev/zero of=/swapfile bs=1M count=2048 status=progress
chmod 600 /swapfile
mkswap /swapfile
```

Let's add this to the `fstab` safely:

```sh
cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab
mount -a
swapon -a
```

Now to check SWAP:

```sh
free -m
```

It should show the new `2GiB` swap added there.

Now let's work on the time:

```sh
timedatectl set-timezone "Asia/Kolkata"
timedatectl set-ntp true
systemctl enable systemd-timesyncd
```

Name your Computer:

```sh
hostnamectl set-hostname <SYSTEM-NAME>
```

Let's fix the `host` file for networking:

```sh
nano /etc/hosts
```

Edit the file so that it looks like this:

```sh
# Static table lookup for hostnames.
# See hosts(5) for details.
127.0.0.1 localhost
127.0.1.1 <SYSTEM-NAME>
```

Finally reboot the computer again.

## Second Boot into the System

Login using the `<USERNAME>` we created earlier.

Become the `root` user:

```sh
su
```

Setup the network this time.

```sh
nmtui
```

Install essential UCODE and firmware:

```sh
pacman -S intel-ucode
pacman -S linux-firmware sof-firmware
```

For AMD chips use `amd-ucode`.

Rebuild the configuration:

```sh
mkinitcpio -p linux
mkinitcpio -p linux-lts
```

Finally reboot again for the configuration to take effect.

## Third Boot into the System

Login using the `<USERNAME>` we created earlier.

Become the `root` user:

```sh
su
```

This time the internet should work properly.

Secure the `sshd` using its configuration file:

```sh
nano /etc/ssh/sshd_config
```

Restart the `sshd`:

```sh
systemctl restart sshd
```

Install another editor `vim`:

```sh
pacman -S bash-completion vim vim-airline
```

Finally install the X interface:

```sh
pacman -S xorg-server
```

We need to install the correct graphics driver:

- This is for processor built In AMD or Intel GPU

    ```sh
    pacman -S mesa
    ```
- For NVIDIA

    ```sh
    pacman -S nvidia nvidia-lts
    ```

- For VirtualBox VM

    ```sh
    pacman -S virtualbox-guest-utils xf86-video-vmware
    systemctl enable vboxservice
    ```

    Don't worry the `vmware` part is ok.

Finally reboot again for the graphics drivers to take effect.

## Fourth Boot into the System

Login using the `<USERNAME>` we created earlier.

Become the `root` user:

```sh
su
```

We fix the Console fonts:

```sh
vim /etc/vconsole.conf
```

Add the line `FONT=ter-132b`.
This would enable proper boot fonts.

Finally reboot the computer again to check.

## Fifth Boot into the System

Login using the `<USERNAME>` we created earlier.

Become the `root` user:

```sh
su
```

Finally we install the `XFCE`:

```sh
pacman -S xfce4 xfce4-goodies
pacman -S lightdm lightdm-gtk-greeter
systemctl enable lightdm
```

This is the last time to reboot.

You will next be greeted by the `light-dm` login screen.

There is a need to reorganize this but it was nice that the whole thing worked without much of troubles.

----
<!-- Footer Begins Here -->
## Links

- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)



