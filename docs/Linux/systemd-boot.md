# `systemd-boot` alternate Bootloader

Default Boot packaged with `systemd`.

<https://wiki.archlinux.org/title/Systemd-boot>

Here are steps for a typical ArchLinux install:

```sh
# Arch Install
# - ...Source Update & Update roots
# - ...ntp
# - ...Partition (No Swap)
# - ...Format
# Mount
mount /dev/vda2 /mnt
mkdir /mnt/boot
mount /dev/vda1 /mnt/boot
pacstrap /mnt base linux linux-firmware nano git amd-ucode
genfstb -U /mnt >> /mnt/etc/fstab
# Enter The install
arch-chroot /mnt
# Create the Swap File for 2GB
fallocate -l 2GB /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
# Add Swapfile to fstab
nano /etc/fstab
## .. Go to the end of the file and add
/swapfile  none  swap  defaults  0 0
## .. Save the Install
# - ...Configure Timezone
# - ...Generate Locale
# - ...Hostname and Hosts file
# - ...RootUser password `change`
# - ...Install Basic Tools
pacman -S efibootmgr networkmanager network-manager-applet wireless_tools \
  wpa_supplicant dialog os-prober mtools dosfstools base-devel linux-headers \
  xdg-user-dirs
# SystemD Boot
bootctl --path=/boot install
# Configuration of Boot - loader
nano /boot/loader/loader.conf
## .. Uncomment The timeout - Or the System directly boots-up
timeout 3
## .. Change the default line
default arch-*
## .. Save the File and exit
# Configuration of Boot - entries
nano /boot/loader/entries/arch.conf
## .. Title of the Machine
title    Arch Linux
linux    /vmlinuz-linux
initrd   /initramfs-linux.img
options  root=/dev/vda2 rw
## .. Save the File and Exit
# - ...Enable Services
systemctl enable NetworkManager
# - ...Add Local User
groupadd ab
useradd -m -g ab -G wheel -s /bin/bash ab
passwd ab
# - ...Add sudo Privileges
echo "ab ALL=(ALL) ALL" | tee /etc/sudoers.d/ab
# - ...Umount all Partitions
umount -a
reboot #or PowerOff
#####
# - ...Install SSH
sudo pacman -S openssh
sudo systemctl start sshd
# - ...add other parts
sudo pacman -S xf86-video-qxl # for VM
# - ...KDE Installation
sudo pacman -S xorg sddm plasma kde-applications-meta packagekit-qt5
sudo systemctl enable sddm
```

```sh
# Arch Install 2
timedatectl set-ntp true
# - ...Partition
# - ...Format the Partitions
# - ...Mount the Disks
# pacstrap
pacstrap /mnt base base-devel linux-lts linux-firmware vim git amd-ucode \
  man-db man-pages texinfo linux-lts-headers \
  inetutils netctl dhcpcd \
  sof-firmware s-nail nano
genfstab -U /mnt >> /etc/fstab
# Enter the Install
arch-chroot /mnt
# Stage 1

ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
hwclock --systohc

sed -i 's/#en_US.UTF-8/en_US.UTF-8/g' /etc/locale.gen
sed -i 's/#en_IN/en_IN/g' /etc/locale.gen
echo "LANG=en_US.UTF-8" | tee /etc/locale.conf
echo "KEYMAP=us" | tee /etc/vconsole.conf
locale-gen

echo "vm1" | tee /etc/hostname
echo -e "127.0.0.1\tlocalhost
::1\t\tlocalhost" | tee -a /etc/hosts
echo -e "127.0.1.1\t$(cat /etc/hostname).localdomain $(cat /etc/hostname)" | \
  tee -a /etc/hosts

mkinitcpio -p linux-lts

passwd

bootctl --path=/boot install

sed -i '/default \c/default arch-*' /boot/loader/loader.conf

echo -e "title\tArch Linux
linux\t/vmlinux-linux
initrd\t/amd-ucode.img
initrd\t/initramfs-linux.img
options root=UUID=" | tee /boot/loader/entries/arch.conf

echo "$(lsblk -nld -o UUID /dev/vda3) rw" | tee -a /boot/loader/entries/arch.conf

bootctl update

```

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)

