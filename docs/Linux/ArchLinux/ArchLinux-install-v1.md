# V1 First Install Attempt

The full installation can be divided into 4 main Phases.

1. Setup and partitioning which would include the boot-strapping
   the install of the Root file system.

2. Chroot environment during installation where basic packages are installed
   and the boot-loader is configured. This also includes the creation
   of a user.

3. This is the time when the other system packages are installed along with
   the desktop environment.

4. Finally tunning of the system and installation of the non-default drivers.

## Stage 1 commands

### 1. Set the Correct Font for the console so that its easier to see
Set a better font.
```sh
setfont sun12x22.psfu.gz
```

For even bigger fonts:
```sh
setfont solar24x32.psfu.gz
```

If `terminus-font` is already install like in the default **Arch-iso** you can use:
```sh
setfont ter-132n
```



### 2. Configure default Keymap
Search for US Keyboads:
```sh
localectl list-keymaps | grep us
```
Output:
```
...
us
us-acentos
us1
...
```
Set the Correct layout:
```sh
loadkeys us
```



### 3. Configure the Wireless
Enter the `iw` shell:
```sh
iwctl
```
Inside the `iw` shell Commands for `wlan0` WiFi Adapter
```sh
[iwd]# station wlan0 scan
[iwd]# station wlan0 get-networks
```
Now you will get the list of networks available.
In our case it is `wifiaccesspoint`. We connect to it.
```sh
[iwd]# station wlan0 connect wifiaccesspoint
```
You would be asked password for the selected network.
To check if the connection was made
```sh
[iwd]# station wlan0 show
```
Once you are satisfied, exit the `iw` shell.
```sh
[iwd]# exit
```



### 4. Check if we are Online
We try to Ping the `archlinux.org` server 2 times.
```sh
ping -c 2 archlinux.org
```
If you get the ok result then you have successfully connected to Internet.

In case you wish to further debug use the `ip` tool:
```sh
ip addr show
```
Or simply
```sh
ip a
```



### 5. (Optional) Setup NTP for Time and Date synchronization
This is optional but would help you in some later stages.
Enable NTP:
```sh
timedatectl set-ntp true
```
Check if the Correct time is being reported :
```sh
timedatectl status
```
**Note:** The time and date here would be reported in `UTC` time.
Hence you would need your phone or some other computer connected to
internet to verify the time shown.



### 6. Configure the Repository Mirrors
We would configure `https` mirrors for *3 countries* -
**India**, **United States** and **Japan**; with maximum update age of
*6 Hours* and sorted by their rate:
```sh
reflector --country 'United States',Japan,India --age 6 \
 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

Now to update the `pacman` repository:
```sh
pacman -Syy
```

To check if our mirror list indeed got updated:
```sh
cat /etc/pacman.d/mirrorlist
```



### 7. Partition the Disk

We would use 2 partitions - *Root* and *home*.

First we check the disk status:
```sh
lsblk
```
Find out the correct disk ID we are looking to work on.

In our case we have `/dev/sda`
Let's start the partition program:
```sh
cfdisk /dev/sda
```
Perform the Partition to obtain correct sizes of *Root* and *home*
partitions.

Confirm if the partitions are written to disk :
```sh
lsblk
```
In our case the configuration states that we have:
- *Root* is in `/dev/sda5`
- *home* is in `/dev/sda6`



### 8. Formatting the Partitions
Format the *Root* partition:
```sh
mkfs.ext4 /dev/sda5
```
Next Format the *home* partition:
```sh
mkfs.ext4 /dev/sda6
```
Swap is not needed since we have enough RAM to support us.



### 9. Mounting the Partitions correctly
Mount the Main *Root* partition:
```sh
mount /dev/sda5 /mnt
```
Create the directory for *boot* partition in our case a **UEFI** system and
mount the same:
```sh
mkdir /mnt/boot
mount /dev/sda2 /mnt/boot
```
Create the directory for *home* partitions and mount it:
```sh
mkdir /mnt/home
mount /dev/sda6 /mnt/home
```
Check the Mount points of the system to confirm if everything is alright:
```sh
lsblk
```



### 10. Bootstrap the Installation for Root File system
We use `pacstrap` to create the **Root** file system and install some
minimum things - including a editor like `vim`:
```sh
pacstrap /mnt base linux linux-firmware vim
```
### 11. Write the *FSTAB* file for correct mounting of drives
Check if we still have the correct mounting:
```sh
lsblk
```
Generate the *FSTAB* file:
```sh
genfstab -U /mnt >> /mnt/etc/fstab
```
### 12. Finally progress to Stage 2 by *CHROOT*
Enter the installation **Root** File System:
```sh
arch-chroot /mnt
```



## Stage 2 Commands

### 1. Set the Timezone correctly
Find the required timezone in our case `Kolkata`:
```sh
timedatectl list-timezones | grep Kolkata
```
Next set the correct zone:
```sh
ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
```
Now update the Hardware clocks:
```sh
hwclock --systohc
```
You can verify that the clocks are indeed set right:
```sh
date
```



### 2. Fix the Locale and keyboard
Configure the Locale for `UTF-8` with `US` and `IN`:
```sh
echo "## Edited" >> /etc/locale.gen
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "en_IN UTF-8" >> /etc/locale.gen
```
or edit `vim /etc/locale.gen` and add the lines
```
## Edited
en_US.UTF-8 UTF-8
en_IN UTF-8
```
Next Add the Configuration for Language `US` and Localization to `IN`.
```sh
vim /etc/locale.conf
```
Add the Lines:
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

Next Configure the Keymap to `IN` for English keyboard with *Rupee* symbol.
```sh
echo "KEYMAP=in" > /etc/vconsole.conf
```



### 3. Generate the Localization
Generate the Locale:
```sh
locale-gen
```
### 4. Configure the Host name
We chose the host name to be `arch1`.
```sh
echo "arch1" > /etc/hostname
```
Edit the `hosts` file :
```sh
vim /etc/hosts
```
Add the lines with *Tab* as separator:
```
127.0.0.1   localhost
::1         localhost
127.0.1.1   arch1.localdomain  arch1
```



### 5. Set Root User password
Create a password for the **Root** user:
```sh
passwd
```
Make sure to use 2 passwords one for **Root** and a different one for the
normal user which we create later.
### 6. Install the Boot requirements
Install the basic boot & Network utilities and services:
```sh
pacman -Syu grub efibootmgr os-prober ntfs-3g mtools dosfstools exfatprogs \
 networkmanager dialog base-devel linux-headers bash bash-completion nano \
 bluez bluez-tools pulseaudio-bluetooth cups git reflector \
 xdg-utils xdg-user-dirs openssh acpi wget
```



### 7. Install the Boot-loader and Configure
We would be using `GRUB` and its auto-generated configuration:
```sh
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```
Next run the command to generate the configuration:
```sh
grub-mkconfig -o /boot/grub/grub.cfg
```
Make sure to check if all the operating systems are detected here.



### 8. Enable Essential Services
We use `systemd` to enable the required services:
```sh
systemctl enable NetworkManager
systemctl enable bluetooth
systemctl enable org.cups.cupsd
```

### 9. Create assign groups to the Normal user
We have chosen the name `archer` for our user:
```sh
useradd -m -g users -G wheel,storage,power,lp,lock,uucp -s /bin/bash archer
```
Next we set a password for the user:
```sh
passwd archer
```



### 10. Give the `wheel` group `sudo` permissions
We need to open the permissions in editor:
```sh
EDITOR=vim visudo
```
Just scroll and un-comment the line `%wheel ALL=(ALL) ALL`.
### 11. Move to the Next stage
We come to the end of the 2nd stage of installation.
First we move out the **chroot** environment.
```sh
exit
```
Next we unmount all the volumes.
```sh
umount -a
```
Finally we reboot the computer.
```sh
reboot
```


## Stage 3 Commands
After we reboot, we can select `Arch Linux` in the `GRUB` menu.
This would drop us to the text login prompt.
Where we can login using `archer` user name and the password we
set earlier.

### 1. Connect to the Internet
We would have to use a different tool now `NetworkManager`:
```sh
nmtui
```
This is text based gui driven, so should be easy to connect.
Next, we verify if we are indeed connected.
```sh
ping -c 2 archlinux.org
```



### 2. Verify that we still have our secure Repositories
Check the list of mirrors:
```sh
cat /etc/pacman.d/mirrorlist
```
If we still find it incorrect then run the `reflector` command again:
```sh
reflector --country 'United States',Japan,India --age 6 \
 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```



### 3. (Optional) Fix the console font
This is same way we did earlier:
```sh
setfont sun12x22.psfu.gz
```



### 4. Install the XFCE Desktop Environment Packages
First we need to update `pacman`:
```sh
sudo pacman -Syu
```
Install the Display driver for Basic **Intel** mother board and
**NVIDIA** graphics card:
```sh
sudo pacman -S xf86-video-intel xf86-video-nouveau
```
In case you wish to have the non-free version of the **NVIDIA** drivers
use packages `nvidia` and `nvidia-utils`.
For other type of GPUs use the following - `xf86-video-amdgpu`,
`xf86-video-ati`, `xf86-video-vesa` ; these correspond to AMD CPU+GPU combo,
ATI Graphics cards and Older VESA supporting cards, respectively.

Next we install the Desktop Environment and its utilities:
```sh
sudo pacman -S xorg xf86-input-libinput libinput xfce4 xfce4-goodies \
 xfce4-sensors-plugin archlinux-xdg-menu lightdm lightdm-gtk-greeter \
 pavucontrol gvfs firefox udisks2 udiskie udevil xarchiver p7zip usbutils \
 cups-pdf libcups system-config-printer blueman ufw ipset ebtables \
 p7zip network-manager-applet youtube-dl
```


### 5. Enable Important Services
Configure the GUI environment again using `systemd`:
```sh
sudo systemctl enable ufw
sudo systemctl enable lightdm
```
If you are using **SSD** for the install then you also need to enable
the `fstrim`:
```sh
sudo systemctl enable fstrim.service
sudo systemctl enable fstrim.timer
```
In case you don't find the `fstrim` service just install
`sudo pacman -S util-linux`.



### 6. Adding some Essential XFCE utilities
Here is the sundry list of utilities:
```sh
sudo pacman -S firefox lm_sensors keepassxc qpdfview pdfarranger zbar qtqr \
 xsane featherpad notepadqq telegram-desktop nomacs vlc libreoffice hunspell \
 hunspell-en_US languagetool keychain gnome-keyring seahorse \
 hddtemp smartmontools tree patch tmux veracrypt htop hyphen-en libmythes \
 mythes-en gitg meld arch-audit gufw
```

Additionally we would need some good fonts packages *(Nearly 1.5GB)*:
```sh
sudo pacman -S ttf-droid gnu-free-fonts noto-fonts noto-fonts-extra \
 ttf-caladea ttf-carlito ttf-cascadia-code ttf-cormorant ttf-croscore \
 ttf-dejavu ttf-eurof ttf-fira-code ttf-fira-mono ttf-fira-sans \
 ttf-hack ttf-inconsolata ttf-indic-otf ttf-ionicons ttf-jetbrains-mono \
 ttf-junicode ttf-khmer ttf-lato ttf-joypixels ttf-ibm-plex ttf-hannom \
 ttf-hanazono ttf-liberation ttf-linux-libertine ttf-linux-libertine-g \
 ttf-monofur ttf-nerd-fonts-symbols ttf-opensans ttf-proggy-clean \
 ttf-roboto ttf-roboto-mono ttf-sazanami ttf-tibetan-machine \
 ttf-ubuntu-font-family ttf-font-awesome
```



### 7. Remove some packages from XFCE group
There are some utilies that overlap hence need to be removed.
```sh
sudo pacman -R mousepad
sudo pacman -R ristretto
```
### 8. Configure the AUR download tool
We would be using `paru` tool.
Here are its dependencies:
```sh
sudo pacman -S --needed base-devel
```
We would build it in the temporary location:
```sh
cd /tmp
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```
You would be asked permission and `sudo` password entry for installation.



### 9. Let's go GUI
We are done with the things for this stage.
```sh
reboot
```


## Stage 4 Commands
Now you would be greeted in `lightdm` environment.
You can login with the same `archer` user.
Open a Terminal for the next set of commands.

### 1. (Optional) Network connection Check
In the Terminal:
```sh
nmtui
```



### 2. Install the AUR Packages
We need a few essential packages from AUR. We would be installing
them using `paru` tool that we installed earlier.
```sh
paru xfce4-kbdleds-plugin
paru caffeine-ng
sudo ufw enable
```
Additionally install the `languagetool` extension for LibreOffice
from `https://languagetool.org/#libreoffice`.

Also make sure to add `nm-applet` to the `Session and Startup`
else Wifi Icon, would not be visible.

**IMPORTANT**: Also make Enable `Launch GNOME services on startup` in `Advance` Tab
of the `Session and Startup`. This would allow the `ssh` and `gpg` services
to have a nice Password GUI and maintain credentials over the session.



### 3. Fix Bluetooth tools
We need to remove a few things first:
```sh
sudo pacman -R bluez-utils
sudo pacman -R bluez-hcitool
sudo pacman -R bluez-rfcomm
```
Next we install the AUR packages for the fixes:
```sh
paru bluez-rfcomm
paru bluez-hcitool
paru bluez-utils-compat
```



### 4. Install Driver for TP-Link Archer T9UH V2 USB dongle WiFi adapter
We need DKMS for this driver.
```sh
sudo pacman -Syu
sudo pacman -S dkms
paru rtl8814au-aircrack-dkms-git
```
More information about this from -
`https://aur.archlinux.org/rtl8814au-aircrack-dkms-git.git`



### 5. Install Driver for Brother Printer HL-L2321D
Download the Linux DEB file driver from the Brother Support website.
It should be a file like `linux-brprinter-installer-2.2.2-1.gz`
Un-archive it into a folder.

Install this driver's dependencies and add current user to new group `lpadmin`:
```sh
sudo pacman -S dpkg lib32-glibc
sudo groupadd lpadmin
sudo usermod -aG lpadmin $USER
```

Change into the directory where the printer driver is extracted.
```sh
chmod +x linux-brprinter-installer-2.2.2-1
```
**Note** While performing the Installation make sure not to select the URI.

As per Brother driver Documentation:

> Step7. The driver installation will start. Follow the installation screen directions.
> When you see the message **"Will you specify the DeviceURI ?",**
>
> For USB Users: Choose N(No)
> For Network Users: Choose Y(Yes) and DeviceURI number.
>
> The install process may take some time. Please wait until it is complete.

Give command for installation:
```sh
sudo ./linux-brprinter-installer-2.2.2-1 HL-L2321D
```


-------

## Alternate Install - i3

```sh
# Window Manager - To start from command-line we need `xorg-xinit`
sudo pacman -S xorg xorg-xinit
# I3 packages
# * i3-wm = I3 window manager
# * i3lock = Improved screenlocker based upon XCB and PAM
# * i3status = Status bar at the Bottom of the screen
# * i3blocks = Define blocks for your i3bar status line
# * dmenu = For the Run Menu operation to select programs start
# * terminator = Terminal
sudo pacman -S i3-wm i3lock i3status i3blocks dmenu terminator firefox
```

Copy the `Xinit` file into home directory:
```sh
sudo cp /etc/X11/xinit/xinitrc ~/.xinitrc
# edit this file
sudo vim ~/.xinitrc
#
# Delete last few lines containing `-geometry`.
#
# Add the following line to end of the .xinitrc
#
# exec i3
#
# And save the file
```

Start I3 using:
```sh
startx
```

Press `Enter` if the Config is being created the first time.

- Set `<Win>` as the default modifier key and press Enter
- There might be an error after this step. Dont worry open the configuration for edit.
  Find the location for `dmenu` using search. There would be 2 options enabled for same `d` key.
  Try to change Comment the one with the bare `dmenu` command since it directly
  catches the command name. The other i3 based dmenu is better.

Command:

- Mod {Win} + Enter = Terminal prompt
- `xrandr --output Virtual-1 --mode 1920x1080` = This would make display in VM
  equal to full screen.
- Mod {Win} + Shift + q = Quit application
- Mod {Win} + v = For Vertical Tiling begining
- Mod {Win} + h = For Horizontal Tiling begin
- Mod {Win} + s = Stack Mode
- Mod {Win} + e = Tiling Mode
- Mod {Win} + w = Tab Mode
- Mod {Win} + r = resize Mode Use Left and Right arrow keys
- Mod {Win} = Use Arrow keys to move around windows
- Mod {Win} + Shift = Arrow keys can be used to move the windows left or right
- Mod {Win} + 1 to 9 = To switch the other workspaces
- Mod {Win} + Shift + 1 to 9 = Move window to other workspace
- Mod {Win} + d = For Dmenu to come up
- Mod {Win} + Shift + r = To resetart the I3 compositor
- Mod {Win} + Shift + e = To exit I3 and return to lightDM or commandline.

Config (end of the file):

```config
# Keybaord Layout - Always runs when the config reloads
exec_always setxkbmap -model pc105 -layout in -variant eng
# Display Resolution of VM
exec_always xrandr --output Virtual-1 --mode 1920x180
```

Keyboard Config for India:
```sh
$ cat /etc/X11/xorg.conf.d/00-keyboard.conf
# Read and parsed by systemd-localed. It's probably wise not to edit this file
# manually too freely.
Section "InputClass"
        Identifier "system-keyboard"
        MatchIsKeyboard "on"
        Option "XkbLayout" "in"
        Option "XkbModel" "pc105"
        Option "XkbVariant" "eng"
EndSection
```

-----

## Xmonad Installation for ArchLinux

<https://www.youtube.com/watch?v=JmPLbZQRgas>


----
<!-- Footer Begins Here -->
## Links

- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
