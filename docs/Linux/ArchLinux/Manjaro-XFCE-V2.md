# Manjaro XFCE - Installation Steps V2

## Preparation

Make sure to create the latest USB Disk for Manjaro.

<https://manjaro.org/download/>

!!! note "Without Internet Steps"
    Perform the **First 3** Steps **WITHOUT INTERNET**.

## 1. Correction to Fail to Lockout File

!!! warning "Do this **WITHOUT INTERNET**"

```shell
sudo nano /etc/security/faillock.conf
```
Need to change the `# deny = 3` to`deny = 5`.

It is in **Line 32** or so.

## 2. Configure Pacman Mirror Fetch

!!! warning "Do this **WITHOUT INTERNET**"

```shell
sudo nano /etc/pacman-mirrors.conf
```

Need to change **Line 22** to `Protocols = https`.

And **Line 27** or Last line to `SSLVerify = True`.

Quick Replace script:

```shell
# Switch the Protocol to HTTPS
sudo sed -i 's/# Protocols =/Protocols = https/g' /etc/pacman-mirrors.conf
# Enable SSL Certificate Verification
sudo sed -i 's/# SSLVerify/SSLVerify/g' /etc/pacman-mirrors.conf
```

## 3. Add Support for AUR and other Config options to `pamac`

!!! warning "Do this **WITHOUT INTERNET**"

Edit the *pamac config* file:

```shell
sudo nano /etc/pamac.conf
```

Edit the following:

```
## When removing a package, also remove those dependencies
## that are not required by other packages (recurse option):
RemoveUnrequiredDeps

## How often to check for updates, value in hours (0 to disable):
RefreshPeriod = 24

## When no update is available, hide the tray icon:
NoUpdateHideIcon

## When applying updates, enable packages downgrade:
EnableDowngrade

...

## Allow Pamac to search and install packages from AUR:
EnableAUR

...

## When AUR support is enabled check for updates from AUR:
CheckAURUpdates

...

## AUR build directory:
BuildDirectory = /var/tmp

## Number of versions of each package to keep when cleaning the packages cache:
KeepNumPackages = 1

...

## Maximum Parallel Downloads:
MaxParallelDownloads = 8

...
```

This would update both the GUI and the Command line.

## 4. Update the Mirrors:

### Using Command-line

```shell
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

```shell
cat /etc/pacman.d/mirrorlist
```

## 5. Fix User Permissions for Groups

```shell
# For Serial Port Access
sudo usermod -aG uucp $USER
sudo usermod -aG lock $USER
# For Non-sudo docker access
sudo usermod -aG docker $USER
# For virtmanger
sudo usermod -aG libvirt $USER
```

## 6. Mount the Backup-disk always

### Find the UUID for the backup disk

```shell
blkid

# Output
....
/dev/sda1: LABEL="Backup" UUID="12345c24-4837-4d31-b19c-12345a9b06cc" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="Backup-Linux" PARTUUID="8234fa2d-632b-4f82-a6ed-13d4f226f6fe"
....
```

### Edit the FSTAB file to Mount the Disk

```shell
# Create the Location to Mount
sudo mkdir /Backup
# Edit the fstab file
sudo nano /etc/fstab

# Add the Line to End of the File
UUID=12345c24-4837-4d31-b19c-12345a9b06cc             /Backup        ext4    defaults,noatime 0 0
```

## 7. Remove `Snapd` and `flatpak` from `pamac` (Optional)

Some times these can cause issues :

```shell
pamac remove snapd
pamac remove flatpak
```

## 8. Install Essentials Software

Do this using sing `pamac` GUI.

### Security, Password Manager and Secure Vault etc

- `keepassxc`
- `veracrypt`
- `bleachbit`
- `arch-audit`
- `gnome-keyring` (Optional) If you need Gnome Keyring support
- `seahorse` (Optional) If you need Gnome Keyring support
- `paperkey`
- `patch`
- `yubikey-personalization-gui`
- **AUR** `zenity-light` For `Webkit2gtk` less implementation
- `amd-ucode` or `intel-ucode` - For Processor Related issues
- **REMOVE** `stoken` (You may need to Remove the *VPN related Packages*)
- **REMOVE** `openconnect`
- **REMOVE** `networkmanager-openconnect`
- `rkhunter` = Rootkit Hunter for Linux

### Disk Utility and Backup

- `gnome-disk-utility`
- `smartmontools`
- `hddtemp`
- `baobab`
- `restic`
- `grsync`
- (Optional) **AUR** `woeusb` (To create Windows Bootables)
- **REMOVE** `timeshift-autosnap-manjaro`
- **REMOVE** `timeshift`
- (Optional) **AUR** `f3` to check the Flash capacity and genuine checks
- `ifuse`
- (Optional) **AUR** `mkinitcpio-firmware` Optional fixes the missing things in `mkinitcpio`.

### Basic Utilities

- **REMOVE** `yad`
- **REMOVE** `gufw`
- `ufw` = Firewall Needed
- **REMOVE** `yelp` ( `yelp-tools` and `yelp-xsl` are ok )
- **REMOVE** `zenity`
- **REMOVE** `webkit2gtk`
- **REMOVE** `zensu`
- `simple-scan`
- `cheese`
- `evince` (Should be Installed by the OS)
- `speedcrunch`
- `usbview`
- `zbar` (Should be Installed by the OS)
- `catfish` (Should be Installed By OS)
- `htop` (Should be Installed by the OS)
- (Optional) **AUR** `bluez-firmware`
- `bluez-libs`
- `bluez-plugins`
- `bluez-tools`
- `bluez-utils`
- (Optional) `clipit` (XFCE Clipman alternative)
- (optional) `dpkg`
- (optional) `ipw2100-fw`
- (optional) `ipw2200-fw`
- `print-manager`
- **REMOVE** `zensu`
- `usbutils`
- **AUR** `zramd` ( Swap on to `ZRAM` )
- **AUR** `zsa-wally-cli` ( For Moonlander ZSA )

### Development

- `binutils` (Should be Installed by the OS)
- `gvim`
- `vim-airline`
- `vim-spell-en`
- `gedit`
- `gedit-plugins`
- **REMOVE** `mousepad`
- `emacs`
- `exa`
- `tmux`
- `tree`
- `bat`
- `gitg`
- `stow`
- `screen`
- `nano-syntax-highlighting` (Should be Installed by the OS)
- `meld`
- `ctags`
- `make`
- `cmake`
- `patch` (Installed in earlier Step)
- `diffutils` (Should be Installed by the OS)
- `fossil` (Git Replacement Source Control Utility)
- `entr`
- `shellcheck`
- `sqlitebrowser`
- `xclip` (Installed in earlier Step)
- `go`
- (Optional) `hashcat` (Password Recovery)
- `python-pyserial`
- `python-pygments`
- `python-pip`
- `python-virtualenv`
- `scons`
- `the_silver_searcher`
- `tk` (Also install Python Counterpart)
- `upx`
- `flameshot`
- `screenkey`
- `dialog`
- **AUR** `gforth`
- `bash-completion`
- Direct Python Install:

    `sudo python -m pip install pyautogui pyinstaller pyperclip thonny tk`

### Online Software
 - **REMOVE** `hexchat`
 - **REMOVE** `pidgin`
 - **REMOVE** `modemmanager`
 - `wget` (Should be Installed by the OS)
 - `youtube-dl` or `yt-dlp`
 - `telegram-desktop`
 - `signal-desktop`
 - `persepolis`
 - `jq`
 - `gping`
 - `curlie`
 - `xh`
 - `nmap`
 - **AUR** `google-chrome`
 - `filezilla`
 - `remmina`
 - **AUR** `mqttfx-bin`
 - **REMOVE** `steam-manjaro`
 - `httrack` - For Downloading Websites

### DTP, Multimedia and Graphics
- `libreoffice-fresh`
- `pdfarranger`
- `img2pdf`
- `pdftricks`
- `cherrytree`
- `viewnior`
- `imagemagick`
- `aspell`
- `aspell-en`
- `hyphen`
- `hyphen-en`
- `system-config-printer`(Installed in earlier Step)
- `gimp` (Should be Installed by the OS)
- `inkscape`
- `mythes-en`
- `languagetool`
- `audacity`
- `simplescreenrecorder`
- `okular` (optional) Specialized PDF Reader for `.mobi` and `.epub`
- `openscad`
- `freecad`
- `xchm` (optional) Special read for windows .chm document
- `cuda` (optional) Only if mandated by any software
- `vidcutter`
- `mpv`
- `rhythmbox` ( ipod-nano and ipod-touch connection )
- `pandoc` (Also Install `texlive-bin` `texlive-core` `texlive-pictures`)
- `ffmpeg`
- `rofi`
- `unicode-emoji`
- `wallpapers-2018`
- `wallpapers-juhraya`
- `xournalpp` = `Xournal++` ( For PDF Annotation )
- `caffeine-ng`

### FONTS

#### Find All Fonts installed in a system
```bash
pamac list -i | grep 'ttf\|font\|otf\|woff' |\
cut -d " " -f 1 | sed 's/.*/, \0/g' |\
tr -d '\n' && echo
```

#### Font Editors & Libs

- `fontconfig`
- `fontforge`
- `woff2`
- `xorg-fonts-encodings`
- `xorg-mkfontscale`

#### Fonts for Install
```sh
sudo pacman -S adobe-source-code-pro-fonts, adobe-source-han-sans-cn-fonts, \
    adobe-source-han-sans-jp-fonts, adobe-source-han-sans-kr-fonts, \
    adobe-source-sans-fonts, adobe-source-serif-fonts, \
    awesome-terminal-fonts, cantarell-fonts, dina-font, gentium-plus-font, \
    gnu-free-fonts, gsfonts, inter-font, lib32-fontconfig, libertinus-font, \
    nerd-fonts-noto-sans-mono, noto-fonts, noto-fonts-cjk, noto-fonts-compat, \
    noto-fonts-emoji, noto-fonts-extra, otf-cascadia-code, otf-cormorant, \
    otf-crimson, otf-font-awesome, otf-hermit, otf-latin-modern, otf-overpass, \
    powerline-fonts, terminus-font, texlive-fontsextra, ttf-anonymous-pro, \
    ttf-bitstream-vera, ttf-caladea, ttf-cascadia-code, ttf-comfortaa, \
    ttf-cormorant, ttf-crimson, ttf-crimson-pro, ttf-crimson-pro-variable, \
    ttf-croscore, ttf-dejavu, ttf-droid, ttf-eurof, ttf-fantasque-sans-mono, \
    ttf-fira-code, ttf-fira-mono, ttf-fira-sans, ttf-font-awesome, \
    ttf-font-icons, ttf-font-logos, ttf-hanazono, ttf-hannom, ttf-ibm-plex, \
    ttf-icomoon-icons, ttf-inconsolata, ttf-indic-otf, ttf-jetbrains-mono, \
    ttf-joypixels, ttf-lato, ttf-liberation, ttf-linux-libertine, \
    ttf-linux-libertine-g, ttf-material-design-icons-webfont, \
    ttf-meslo-nerd-font-powerlevel10k, ttf-monofur, ttf-monoid, ttf-montserrat, \
    ttf-opensans, ttf-overpass, ttf-roboto, ttf-roboto-mono, ttf-roboto-slab, \
    ttf-sarasa-gothic, ttf-shadow-fonts, ttf-ubuntu-font-family, woff-fira-code, \
    woff2-cascadia-code, woff2-fira-code
```

### Embedded and Electronics
- `openocd`
- `avr-binutils`
- `avr-gcc`
- `avr-libc`
- `avrdude`
- `arduino`
- (optional) `kicad`
- (optional) `kicad-library-3d`
- `moserial` (`lrzsz` - for xmodem and y-modem)
- `stlink`
- `picocom`
- `geda-gaf` (Optional)
- `ngspice` (Optional)
- `octave` (Optional)
- `gqrx` ( For SDR)
- `rtl-sdr` ( For SDR)
- `gnuradio` (For SDR)
- `gnuradio-companion` (For SDR)
- `airspy` (For AirspySDR)
- `android-tools` (Basic Android Platform tools)
- `android-udev` (Udev rules for Android)
- `sdcc`
- `pulseview`
- **AUR** `thonny` (Or use python to load)
- `arm-none-eabi-binutils`
- `arm-none-eabi-gcc`
- `arm-none-eabi-gdb`
- `arm-none-eabi-newlib`
- **AUR** `nodemcu-pyflasher`
- **AUR** `saleae-logic2`

### VM
- **REMOVE** `modemmanager`
- **REMOVE** `brltty`
- **Allow-Override** `iptables => iptables-nft`
- **AUR** `brltty-dummy` This replaces the original one removed.
- `virt-viewer`
- `virt-manager`
- `qemu-full`
- `qemu-arch-extra` -> `qemu-emulators-full`
- `edk2-ovmf`
- `bridge-utils`
- `dnsmasq`
- `vde2`
- `openbsd-netcat`
- `ebtables => iptables-nft`
- `wine`
- `wine-gecko`
- `wine-mono`
- `winetricks` (For Wine)
- `nmap` (Optional)

#### Fix the Configuration

```shell
sudo nano -cl /etc/libvirt/libvirtd.conf
```
Here are the **Lines to Edit** below "UNIX socket access controls":
1. Uncomment the line 81 or so:
```shell
unix_sock_group = "libvirt"
```
2. Uncomment the line 91 or so:
```shell
unix_sock_rw_perms = "0770"
```

#### Networking fix

```xml linenums="1" title="br10.xml"
<network>
  <name>br10</name>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='br10' stp='on' delay='0'/>
  <ip address='192.168.72.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.72.50' end='192.168.72.200'/>
    </dhcp>
  </ip>
</network>
```
Save this as `br10.xml` to define the Bridge.

Temporarily start the `libvirtd` service to create the bridge.

```shell
# Start the Service Right now
sudo systemctl start libvirtd.service
# Create the Network Bridge
sudo virsh net-define br10.xml
```

####  Add Current User to the 'libvirtd' group

```shell
 # For Libvirt access
sudo usermod -aG libvirt $USER
```

Use the `startv` command to enable the services and network bridge.

### Others

```shell
    # File Recovery Software - PhotoRec + testdisk
    sudo pacman -S testdisk
    # For Writing Journals (Optional)
    # sudo pacman -S --needed rednotebook
    # For STM32CubeMX IDE
    pamac install ncurses5-compat-libs
    # Program to Convert the Web Pages into PDF
    sudo pacman -S --needed wkhtmltopdf
```

### Install [`nb`](https://github.com/xwmx/nb)

Make sure to have `$HOME/bin` in the **`PATH`**.

```shell
wget https://raw.github.com/xwmx/nb/master/nb -O $HOME/bin/nb && \
chmod 744 $HOME/bin/nb
# Install the Code Completion
sudo nb completions install --download
```

Install other Dependencies:
```shell
sudo pacman -S --needed bat pandoc ripgrep tig w3m xclip
## For nCat = Needed in Browse
sudo pacman -S --needed nmap
```

- [`bat`][link-bat] = Alternative to `cat` and syntax-highlighting
- [`ncat`][link-ncat] (optional) = This is a feature-packed networking utility which
  reads and writes data across networks from the command line (TCP / UDP).
  Its part of the `nmap` package. This is needed for browse.
- [`pandoc`][link-pandoc] = For generating documents in different formats like **Markdown to PDF**.
- [`rg`][link-rg] = **`ripgrep`** is a command-line Regex directory search tool better than `find` and written in **Rust**.
- [`tig`][link-tig] = Text based UI interface for Git.
- [`w3m`][link-w3m] = Famous command-line Web Browser for Bookmarks.
- [`xclip`][link-xclip] = Native utility to X Windows Server for pasting content from Clipboard

### Sublime Text Install

Install the GPG key:

```shell
curl -O https://download.sublimetext.com/sublimehq-pub.gpg && sudo pacman-key --add sublimehq-pub.gpg && sudo pacman-key --lsign-key 8A8F901A && rm sublimehq-pub.gpg
```

Select the channel to use:
**Stable x86_64**

```shell
echo -e "\n[sublime-text]\nServer = https://download.sublimetext.com/arch/stable/x86_64" | sudo tee -a /etc/pacman.conf
```

Update pacman and install Sublime Text

```shell
sudo pacman -Syu sublime-text
```

## 9. Personalization

Run the `./install-profile -f` Command to finish the personalization
process for the shell.

## 10. Keyboard Configuration

**Note :** **DO NOT !!!!** Press **Reset to Defaults**.

It would corrupt your complete XFCE.

*Don't Panic* all shortcuts are given below.

- `Super + c` = `subl` (Sublime)
- `Super + d` = `gnome-disks`
- `Super + Shift + d` = `xkill`
- `Super + e` = `thunar`
- `Super + f` = `catfish`
- `Super + g` = `gedit`
- `Super + h` = `gtkhash`
- `Super + i` = `inkscape`
- `Super + l` = `xflock4`
- `Super + n` = `cherrytree`
- `Super + p` = `xfce4-display-settings --minimal`
- `Super + q` = `xkill`
- `Super + r` = `xfce4-terminal --active-tab -T "Ranger" --geometry 108x27 --command="ranger"`
- `Super + s` = `seahorse`
- `Super + t` = `Path to Timer App`
- `Super + v` = `veracrypt`
- `Super + x` = `keepassxc`
- `Super + z` = `code` (VS Code is back).
- `Calculator` = `speedcrunch`
- `Home` = `thunar`
- `Ctrl+Alt+M` = `xfce4-taskmanager`
- `Ctrl+Alt+T` = `xfce4-terminal`
- `Shift+Print` = `xfce4-screenshooter -wd 1`
- `Ctr+Print` = `xfce4-screenshooter -r`
- `Print` = `xfce4-screenshooter -fd1`
- `Alt+F1` = `xfce4-popup-whiskermenu`
- `Alt+F2` = `xfce4-appfinder --collapsed`
- `Ctrl+Alt+P` = `xfce4-display-settings --minimal`
- `Ctrl+Esc` = `xfdesktop --menu`
- `Ctrl+Alt+Del` = `xflock4`
- `Ctrl+Alt+X` = `xkill`

### Date Time ShortCuts
- `Super + Shift + t` = `bash -c "sleep .5 && echo -n \"$(date)\" | xclip -i"` = Full Date with IST Time like `date` command.
- `Super + Shift + j` = `bash -c "sleep .5 && echo -n \"Journal.$(date +'%Y.%m.%d')\" | xclip -i"` = Special Journal Timestamp
- `Super + Shift + s` = `bash -c "sleep .5 && TZ=UTC echo -n \"Scratch.$(date -u +'%Y.%m.%d.%s')\" | xclip -i"` = Special Scratch Timestamp

## 11. Install udev Rules

There files in `usb-rules` copy them to the system:

```shell
 sudo cp -arTf ./usb-rules /etc/udev/rules.d
 sudo chmod 644 /etc/udev/rules.d/*.rules
 sudo chown -R root /etc/udev/rules.d/*.rules
 # Reload the UDEV daemon
 sudo udevadm control --reload-rules && sudo udevadm trigger
```

## 12. Limit log File size

<https://wiki.manjaro.org/index.php?title=Limit_the_size_of_.log_files_%26_the_journal>

Edit the file `/etc/systemd/journald.conf`

```config
...
[Journal]
...
SystemMaxUse=250M
...
SystemMaxFileSize=50M
...
```

## 13. Fix Secure Folders - ssh Linking

This `.ssh` directory is inside some secure storage mounted to your `$HOME`.
So the path would be `$HOME/<secure-dir>/.ssh`.

```shell
cd $HOME/<secure-dir>/.ssh
stow -t ~/.ssh .ssh
```
Here using the `-t` option we are changing the TARGET location.

## 14. Install Reference Folders and Files

**Note:** Make sure that the `stow` package is installed.

### Base Folders

```shell
# Flush the directories from Home
cd ~
# !!!! DANGEROUS COMMAND !!!!! - BE CAREFULL
rm -rf Arduino bin Documents go Music Pictures Videos
# Into the Actual Version
cd /Backup/01_LiveManjaro
# Insert them to Home
stow -t ~ .
```

### Dotfiles

```shell
# Flush the directories from Home
cd ~
# !!!! DANGEROUS COMMAND !!!!! - BE CAREFULL
rm -rf .bashrc .profile .vimrc .zshrc
# Into the Actual Version
cd /Backup/01_LiveManjaroDotfiles
# Insert them to Home
stow -t ~ .
```

## 15. Fix Firmware Warnings - New in March 2022

`==> WARNING: Possibly missing firmware for module: bfa`

<https://aur.archlinux.org/packages/aic94xx-firmware>

This is intern fixed by the `manjaro-firmware` package.

`==> WARNING: Possibly missing firmware for module: qed`

<https://archlinux.org/packages/core/any/linux-firmware-qlogic/>

This is intern fixed by the `manjaro-firmware` package.

`==> WARNING: Possibly missing firmware for module: qla1280`

- This warning some times does not appear

`==> WARNING: Possibly missing firmware for module: qla2xxx`

- This warning some times does not appear

### USB 4-Port PCI-E Card with NEC upd720201 Chipset

<https://aur.archlinux.org/packages/upd72020x-fw>

This is needed for PCIE Card with the Virtualization Pass Through capable card.

This attempt failed due to IOMMU allocation.


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
