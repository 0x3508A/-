# Manjaro XFCE - Installation Steps V1

## 1. Correction to Fail to Lockout File

```sh
sudo nano /etc/security/faillock.conf
```

Need to change the `deny = 5`

Instead of `# deny = 3`.

It is in **Line 32** or so.

## 2. Configure Pacman Mirror Fetch

```sh
sudo nano /etc/pacman-mirrors.conf
```

Need to change **Line 22** to `Protocols = https`

And **Line 27** or Last line to `SSLVerify = True`

Quick Replace script:

```sh
# Switch the Protocol to HTTPS
sudo sed -i 's/# Protocols =/Protocols = https/g' /etc/pacman-mirrors.conf
# Enable SSL Certificate Verification
sudo sed -i 's/# SSLVerify/SSLVerify/g' /etc/pacman-mirrors.conf
```

## 3. Update the Mirrors:

### Using Command-line

```sh
# Update the Mirror List with max timeout for search 5 seconds
sudo pacman-mirrors --timeout 5 --country United_States Japan
# Update all Packages from these updated mirrors
sudo pacman -Syyu
```

### Using `pamac` GUI:

1. Open `pamac` User interface `Add/Remove Software`
2. In the 3 dot side menu , click on `Preferences` - Authenticate
3. Go to `Official Repositories`
4. Set the Desired Country from drop-down
5. Click on `Refresh Mirror List`
6. After the Refresh is complete - close the dialog
7. Use the same menu to `Refresh Database`

### Check the Generated Mirror List

```sh
cat /etc/pacman.d/mirrorlist
```

## 4. Remove `Snapd` and `flatpak` from `pamac`

```sh
pamac remove snapd
pamac remove flatpak
```

## 5. Install Essentials Software

```sh
# Password Manager and Secure Vault
sudo pacman -S keepassxc veracrypt
# Popular Disk manager and Cleaner
sudo pacman -S gnome-disk-utility bleachbit
# Backup Software for Projects
sudo pacman -S restic
# Basic Editor for Text - gedit
sudo pacman -S gedit
# Remove the Other editors
pamac remove mousepad
# Command line Utilities
sudo pacman -S tree tmux arch-audit
# Hard Disk management tools
sudo pacman -S smartmontools hddtemp
# Docker
sudo pacman -S docker
# Source Manipulation and Much needed programming languages
sudo pacman -S code gitg python-pygments go nodejs npm \
 nano-syntax-highlighting meld make patch
# For Concatenating PDF files
sudo pacman -S pdfarranger
# Embedded Essentials
sudo pacman -S openocd moserial lrzsz picocom stlink python-pyserial thonny
# Telegram
sudo pacman -S telegram-desktop
# Multimedia essentials
sudo pacman -S audacity simplescreenrecorder youtube-dl gthumb
# File Recovery Software - PhotoRec + testdisk
sudo pacman -S testdisk
# Cleaner Software
sudo pacman -S bleachbit
# Remove the default Image Viewer
pamac remove viewnior
# For Linux Keyring (optional)
sudo pacman -S seahorse
# For Scanning Files (optional)
sudo pacman -S --needed simple-scan
# For Writing Journals
sudo pacman -S --needed rednotebook
# A Good Calculator (optional)
sudo pacman -S --needed speedcrunch
# For STM32CubeMX IDE
pamac install ncurses5-compat-libs
# CHM File viewer
sudo pacman -S --needed xchm
# Install Linux Headers For Kernel - Select the correct Kernel Version
sudo pacman -S --needed linux-headers make patch
# Wireguard Kernel Module - For VPN
sudo pacman -S --needed wireguard-dkms wireguard-tools
```

### Install [`nb`](https://github.com/xwmx/nb)
Make sure to have `$HOME/bin` in the **`PATH`**.
```sh
wget https://raw.github.com/xwmx/nb/master/nb -O $HOME/bin/nb && \
chmod 744 $HOME/bin/nb
# Install the Code Completion
sudo nb completions install --download
```

Install other Dependencies:

```sh
sudo pacman -S --needed bat pandoc ripgrep tig w3m xclip
```

- [`bat`][link-bat] = Alternative to `cat` and syntax-highlighting
- [`ncat`][link-ncat] (optional) = This is a feature-packed networking utility which
  reads and writes data across networks from the command line (TCP / UDP).
  Its part of the `nmap` package.
- [`pandoc`][link-pandoc] = For generating documents in different formats like **Markdown to PDF**.
- [`rg`][link-rg] = **`ripgrep`** is a command-line Regex directory search tool better than `find` and written in **Rust**.
- [`tig`][link-tig] = Text based UI interface for Git.
- [`w3m`][link-w3m] = Famous command-line Web Browser for Bookmarks.
- [`xclip`][link-xclip] = Native utility to X Windows Server for pasting content from Clipboard

## 6. Fix User Permissions for Groups

```sh
# For Serial Port Access
sudo usermod -aG uucp $USER
sudo usermod -aG lock $USER
# For Non-sudo docker access
sudo usermod -aG docker $USER
```

## 7. Mount the Backup-disk always

### Find the UUID for the backup disk

```sh
blkid

# Output
....
/dev/sda1: LABEL="Backup" UUID="12345c24-4837-4d31-b19c-12345a9b06cc" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="Backup-Linux" PARTUUID="8234fa2d-632b-4f82-a6ed-13d4f226f6fe"
....
```

### Edit the FSTAB file to Mount the Disk

```sh
# Create the Location to Mount
sudo mkdir /Backup
# Edit the fstab file
sudo nano /etc/fstab

# Add the Line to End of the File
UUID=12345c24-4837-4d31-b19c-12345a9b06cc             /Backup        ext4    defaults,noatime 0 0
```

## 8. Personalization

Run the `./install-profile -f` Command to finish the personalization
process for the shell.


  [link-bat]:https://github.com/sharkdp/bat
  [link-ncat]:https://nmap.org/ncat/
  [link-pandoc]:https://pandoc.org/
  [link-rg]:https://github.com/BurntSushi/ripgrep
  [link-tig]:https://github.com/jonas/tig
  [link-w3m]:https://en.wikipedia.org/wiki/W3m
  [link-xclip]:https://github.com/astrand/xclip

----
<!-- Footer Begins Here -->
## Links

- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
