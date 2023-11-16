# V2 - the Second Attempt

A new Attempt at Installing Arch in February, 2021.

## 1 - Validating the Image

```sh
gpg --keyserver-options auto-key-retrieve \
 --keyserver pgp.mit.edu \
 --verify archlinux-2021.02.01-x86_64.iso.sig
```
Output:

```
gpg: assuming signed data in 'archlinux-2021.02.01-x86_64.iso'
gpg: Signature made Monday 01 February 2021 08:53:39 PM IST
gpg:                using RSA key 4AA4767BBC9C4B1D18AE28B77F2D434B9741E8AC
gpg: Good signature from "Pierre Schmitz <pierre@archlinux.de>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 4AA4 767B BC9C 4B1D 18AE  28B7 7F2D 434B 9741 E8AC

```

## 2 - Stage 1 of Install

### 2.1 - Set Terminal Font

```sh
setfont ter-120n
```

or

```sh
setfont sun12x22.psfu.gz
```

### 2.2 - Set KeyMap

```sh
loadley us
```

To check

```sh
localectl list-keymaps | grep us
```

### 2.3 - Setup Wireless (If needed)

```sh
iwctl
```

Scan for Networks:
```
[iwd]# station wlan0 scan
```

Display the scanned Networks

```
[iwd]# station wlan0 get-networks
```

Connect to the Access point:

```
[iwd]# station wlan0 connect <Access Point Name>
```

Check if the Connection was done:

```
[iwd]# station wlan0 show
```

Exit Program:
```
[iwd]# exit
```

### 2.4 - Check if we are online

```sh
ping -c 2 archlinux.org
```

Get IP address:

```sh
ip addr show
```

### 2.5 - Enable NTP (Optional)

```sh
timedatectl set-ntp true
```

Check:

```sh
timedatectl status
```

### 2.6 - Configure the Repository Mirror List

```sh
reflector \
 --country 'United States',Japan,India \
 --age 6 \
 --protocol https \
 --sort rate \
 --save /etc/pacman.d/mirrorlist
```

Update:

```sh
pacman -Syyy
```

Look at the List:

```sh
cat /etc/pacman.d/mirrorlist | less
```

### 2.7 - Partition Disk

Get Disks:

```sh
lsblk
```

For VM:
```sh
gdisk /dev/vda
```

For Normal:
```sh
gdisk /dev/sda
```

`n - for New Partition`

Partition Order for 8GB RAM:

| No . | Partition               | Size               | Code   |
| ---- | ----------------------- | ------------------ | ------ |
| 1    | `/boot`                 | `+512M` (511MByte) | `ef00` |
| 2    | Swap                    | `+9G`   (8.5GByte) | `8200` |
| 3    | `/dev/mapper/cryptroot` | Rest of Capacity   | `8300` |

`w - Write Changes to Disk`

Check:

```sh
lsblk
```

### 2.8 - Format Filesystems

Boot Filesystem:
```sh
mkfs.fat -F32 /dev/vda1
```

Swap Format:
```sh
mkswap /dev/vda2
```

Swap Turn On:
```sh
swapon /dev/vda2
```

Create Encryption for Root filesystem - type `YES`:
```sh
cryptsetup -y -v luksFormat /dev/vda3
```

Open the LUKS filesystem:
```sh
cryptsetup open /dev/vda3 cryptroot
```

Format the Mounted LUKS filesystem:
```sh
mkfs.ext4 /dev/mapper/cryptroot
```

### 2.9 - Mount the Respective Partitions

Root:
```sh
mount /dev/mapper/cryptroot /mnt
```

Create the required Directory:
```sh
mkdir /mnt/boot
```

Mount the EFI Boot Partition:
```sh
mount /dev/vda1 /mnt/boot
```

### 2.10 - Install Base Packages

```sh
pacstrap /mnt base linux-lts linux-firmware vim amd-ucode
```

### 2.11 - Generate the Filesystem Table

```sh
getfstab -U /mnt >> /mnt/etc/fstab
```

### 2.12 - Enter into the Installation

```sh
arch-chroot /mnt
```

## 3 - Stage 2 of Install

### 3.1 - Setup Timezone

```sh
ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
```

Sync the Hardware Clocks:
```sh
hwclock --systohc
```

Find :
```sh
timedatectl list-timezones | grep Kolkata
```

### 3.2 - Locale Generate

```sh
sed -i 's/#en_US.U/en_US.U/1' /etc/locale.gen
```
For Bharat also:
```sh
sed -i 's/#en_IN/en_IN/1' /etc/locale.gen
```

Set Locale format Configuration:

```sh
vim /etc/locale.conf
```
Add Lines:
```
LANG=en_US.UTF-8
LC_ADDRESS=en_IN
LC_IDENTIFICATION=en_IN
LC_MEASUREMENT=en_IN
LC_MONETARY=en_IN
LC_NAME=en_IN
LC_NUMERIC=en_IN
LC_PAPER=en_IN
LC_TELEPHONE=en_IN
LC_TIME=en_IN
```

Keyboard in Console:

```sh
echo "KEYMAP=in" > /etc/vconsole.conf
```

Generate the Locale:
```sh
locale-gen
```

### 3.3 - Configure hostname

```sh
echo -n "arch" > /etc/hostname
```

Setup Hosts:
```sh
export HOST=$(cat /etc/hostname)
echo -e "\t127.0.0.1\tlocalhost" >> /etc/hosts
echo -e "\t::1\t\tlocalhost" >> /etc/hosts
echo -e "\t127.0.1.1\t$HOST.localdomain $HOST" >> /etc/hosts
```

### 3.4 - Password for Root User

Password for Root User:
```sh
passwd
```

### 3.5 - Install Packages for Basic System

```sh
pacman -S grub efibootmgr reflector \
    ntfs-3g mtools dosfstools exfatprogs bash-completion \
    networkmanager network-manager-applet dialog wpa_supplicant \
    base-devel linux-headers git wget \
    bluez bluez-utils \
    xdg-utils xdg-user-dirs \
    cups \
    alsa-utils pulseaudio pulseaudio-bluetooth
```

### 3.6 - Edit startup for Encryption

```sh
vim /etc/mkinitcpio.conf
```

Change the `HOOKS` line like
```
HOOKS=(base udev autodetect keyboard keymap modconf block encrypt filesystem fsck)
```

Recreate the image with new settings:
```sh
mkinitcpio -P
```

### 3.7 - Install Grub

```sh
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCHLINUX
```

Generate Configuration File:
```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

Get Block ID:
```sh
blkid > uuid
```

Get the Line with BlockID for crypt root with `y + y` command:
```sh
vim uuid
```

Edit the Grub Config file:
```sh
vim /etc/default/grub
```
Modify Line:
```
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<UUID Number>:cryptroot root=/dev/mapper/cryptroot"
```

Re-Generate grub:
```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

### 3.8 - Enable Services

```sh
systemctl enable NetworkManager
systemctl enable bluetooth
systemctl enable cups
```

### 3.9 - Create default User

```sh
useradd -m -g users -G wheel,storage,rfkill,power,lp,lock,uucp -s /bin/bash archer
```

Password for User:
```sh
passwd archer
```

### 3.10 - Enable `wheel` privileges

```sh
EDITOR=vim visudo
```
Uncomment the Line:
```
%wheel ALL=(ALL) ALL
```

### 3.11 - Exit the Install

```sh
exit # Exits the chroot shell
umount -a # Auto unmount the drives
shutdown -h "now" # Turn off machine
```

Remove the CD or USB media after this.

## 4 - Stage 3 of Install

### 4.1 - Setup Network

```sh
sudo nmtui
```

### 4.2 - Setup the Terminal

Install `terminus` font:
```sh
sudo pacman -S terminus-font
```

Set the Font:
```sh
sudo setfont ter-132n
```

or

```sh
sudo setfont sun12x22.psfu.gz
```

### 4.3a - Enable More Services

NTP Setting:
```sh
timedatectl set-ntp true
```

Synchronize the Hardware and system clock:
```sh
sudo hwclock --systohc
```

Update Mirror List:
```sh
sudo reflector \
 --country 'United States',Japan,India \
 --age 6 \
 --protocol https \
 --sort rate \
 --save /etc/pacman.d/mirrorlist
```

Refresh Servers:
```sh
sudo pacman -Syy
```

Enable Reflector Timer:
```sh
sudo systemctl enable --now reflector.timer
```

For SSD:
```sh
sudo systemctl enable --now fstrim.timer
```

### 4.3b - Fix Bootloader sizing in VM (Optional)

```sh
sudo vim /etc/default/grub
```
Edit the Line:
```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet video=1920x1080"
```

Regenerate the Grub configuration:
```sh
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### 4.4a - Install Packages for Graphical desktop - XFCE

Video Driver:

- `xf86-video-intel` = Intel Video
- `xf86-video-amdgpu` = AMD Builtin Video
- `nvidia` + `nvidia-utils` = Latest nVidia
- `xf86-video-ati` - For ATI Cards
- `xf86-video-noveau` - For Opensource nVidia cards
- `xf86-video-qxl` - For QXL driver of VM (not needed always)

```sh
sudo pacman -S <Video Driver> xorg xorg-xinit xf86-input-libinput libinput \
    xfce4 thunar-archive-plugin thunar-volman thunar-media-tags-plugin \
    xfce4-notifyd xfce4-pulseaudio-plugin xfce4-sensors-plugin \
    xfce4-wavelan-plugin xfce4-whiskermenu-plugin xfce4-screenshooter \
    xfce4-mount-plugin xfce4-taskmanager speedcrunch gedit gthumb \
    gvfs-mtp gvfs-afc gvfs-google gvfs-gphoto2 gvfs-goa gvfs-nfs gvfs-smb \
    udiskie udisks2 usbutils engrampa karchive p7zip spice-gtk \
    gparted blueman ufw gufw firefox keepassxc qpdfview simple-scan \
    alsa-firmware alsa-oss pulseaudio-alsa pavucontrol pamixer \
    pulseaudio-equalizer pulseaudio-jack pulseaudio-zeroconf \
    audacious bleachbit gnome-disk-utility nm-connection-editor \
    system-config-printer veracrypt vlc moserial audacity avahi \
    puzzles code sqlitebrowser gitg meld rednotebook \
    telegram-desktop thunderbird xchm bat hddtemp smartmontools tree patch \
    tmux arch-audit hyphen-en hunspell pdfarranger zbar mythes-en \
    hunspell-en_US libreoffice-fresh languagetool restic \
    nano-syntax-highlighting openocd picocom stlink python-pyserial \
    simplescreenrecorder youtube-dl \
    wireguard-dkms wireguard-tools make go nodejs cmake npm
```

Install Fonts and Themes:
```sh
sudo pacman -S materia-gtk-theme papirus-icon-theme \
    archlinux-wallpapers ttf-dejavu ttf-liberation noto-fonts \
    cantarell-fonts gsfonts noto-fonts-cjk noto-fonts-emoji \
    ttf-font-awesome ttf-bitstream-vera ttf-inconsolata \
    ttf-indic-otf ttf-droid
```

Enable Services:
```sh
sudo systemctl enable --now ufw
```

Config `xinitrc`:
```sh
cp /etc/X11/xinit/xinitrc ~/.xinitrc
```
Edit the file: `vim ~/.xinitrc`
Comment out line Lines from `twm &` till `exec xterm -geometry 80x66+0+0 -name login`.
Add Lines:
```
setxkbmap -layout in -model pc105 -variant eng &
exec dbus-launch --sh-syntax xfce4-session
```

### 4.4b - Install Remaining Package for Graphical desktop - BSPWM

More Info refere to **EF - Linux Made Simple** Channel on Youtube:

Part 1 <https://www.youtube.com/watch?v=PLBm0C5Gv58>

Part 2 <https://www.youtube.com/watch?v=7lLa4IoVdCo>

Video Driver:

- `xf86-video-intel` = Intel Video
- `xf86-video-amdgpu` = AMD Builtin Video
- `nvidia` + `nvidia-utils` = Latest nVidia
- `xf86-video-ati` - For ATI Cards
- `xf86-video-noveau` - For Opensource nVidia cards
- `xf86-video-qxl` - For QXL driver of VM (not needed always)

```sh
sudo pacman -S <Video Driver> xorg xorg-xinit bspwm \
    sxhkd dmenu nitrogen picom xfce4-terminal firefox arandr \
    keepassxc qpdfview \
    ttf-dejavu ttf-droid ttf-liberation noto-fonts
```

Create the Configuration:
```sh
mkdir ~/.config/bspwm ~/.config/sxhkd
cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/
cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/
```

Modify the Config for Keybindings:
```sh
vim ~/.config/sxhkd/sxhkdrc
```
Modify the line for `terminal emulator` instead of `urxvt`:
```
    xfce4-terminal
```

Config `xinitrc`:
```sh
cp /etc/X11/xinit/xinitrc ~/.xinitrc
```
Edit the file:
```sh
vim ~/.xinitrc
```
Delete Lines from `twm &` till `exec xterm -geometry 80x66+0+0 -name login`.
Add Lines:
```
setxkbmap -layout in -model pc105 -variant eng &
nitrogen --restore &
xsetroot -cursor_name left_ptr
picom -f &
exec bspwm
```

Run the **bspwm** :
`startx`

Shortcuts:

- `Start + Alt + q` to **Terminate BSPWM** and back to command prompt
- `Start + Alt + r` to **Restart BSPWM**
- `Start + w` to Close any Application / Window
- `Start + enter` for Terminal Window
- `Start + SpaceBar` for dmenu
- `Start + c` to Switch between Windows
- `Start + <desktop no>` to Switch to another Desktop
- `Start + Shift + <desktop no>` to move Window to other Desktop
- `Start + m` to switch between Monocle mode (Full screen) no border mode
- `Start + Alt + {h,j,k,l}` For Increasing size of window
    Left(h)|Right(l)|Down(j)|Up(k)
- `Start + Alt + Shift + {h,j,k,l}` For Decreasing size of window
    Left(h)|Right(l)|Down(j)|Up(k)

#### Dispaly Fix for VM

Edit the Compositor `picom` settings for VM (Optional):
```sh
sudo vim /etc/xdg/picom.conf
```
Find the `vsync` option:
```
vsync = false
#vsync = true
```

#### Add Wallpaper

Arch Linux WallPaper (Optional):
```sh
sudo pacman -S archlinux-wallpaper
```
Set this using `nitrogen` for folder `/usr/share/backgrounds/archlinux`.
Also change the sizing preference form `Automatic` to `Zoomed Fill`

#### MultiMonitor Setup

Find monitors: `xrandr`

`vim ~/.config/bspwm/bspwmrc`

Example for DP-1 (7,8,9,10) and DP-0(1,2,3,4,5,6):
```
bspc monitor DP-0 -d I II III IV V VI
bspc monitor DP-1 -d VII VIII IX X
```

`bspc rule -a Chromium desktop='^2'` Means that **chrome** will always
launch in the `2`nd Desktop.

To find the Class name or Application name for the `bspwmrc` file:
`xprop` and click on the desired window.
Now use the `WM_CLASS(STRING)` to specify the Program name.
For Example `bspc rule -a firefox desktop='^2'` for Firefox

#### Display Resolution Fix for VM

Edit Screen Settings `Start + SpaceBar` select - `arandr` for VM (Optional):
Change display settings to 1920x1080 resolution.
Dont forget to __save the configuration__ to a script file using **save button**.
Add a line to execute this script in `.xinitrc` file - before `picom` line:
`vim ~/.xinitrc`
```
$HOME/.screenlayout/display.sh
```

### 4.4c - Install Remaining Package for Graphical desktop - i3

Video Driver:

- `xf86-video-intel` = Intel Video
- `xf86-video-amdgpu` = AMD Builtin Video
- `nvidia` + `nvidia-utils` = Latest nVidia
- `xf86-video-ati` - For ATI Cards
- `xf86-video-noveau` - For Opensource nVidia cards
- `xf86-video-qxl` - For QXL driver of VM

```sh
sudo pacman -S <Video Driver> xorg i3 dmenu \
    lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings \
    ttf-dejavu ttf-liberation noto-fonts \
    firefox nitrogen picom lxappearance pcmanfm \
    materia-gtk-theme papirus-icon-theme \
    xfce4-terminal keepassxc qpdfview \
    noto-fonts-emoji unicode-emoji
```

Extra Setting for VM (optional):
```sh
sudo vim /etc/lightdm/lightdm.conf
```
```
# Uncomment and Modify the Line
display-setup-scripts=xrandr --output Virtual-1 --mode 1920x1080
```


Enable Login greeter:
```sh
sudo systemctl enable lightdm
```

Arch Linux WallPaper (Optional):
```sh
sudo pacman -S archlinux-wallpaper
```

Fix for `picom` transparency on VM (Optional):
```sh
sudo vim /etc/xdg/picom.conf
```
Comment Out (make vsync=false)
```
vsync = false
# vsync = true
```

Settings Fix in **i3** config:

```sh
vim ~/.config/i3/config
```

Add these Lines at the End of the file:

```sh
# Gaps Configuration
# Include lines from https://github.com/Airblader/i3/wiki/Example-Configuration

# Bharat Rupee Keyboard Config
exec setxkbmap -layout in -model pc105 -variant eng &

# Nitrogen
exec nitrogen --restore

# Picom
exec picom -f
```

----
<!-- Footer Begins Here -->
## Links

- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
