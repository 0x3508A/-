# Manjaro XFCE Steps after Install V3

## Preparation

Make sure to create the latest USB Disk for Manjaro.

<https://manjaro.org/download/>

!!! note "Without Internet Steps"
    Perform the **First 3** Steps **WITHOUT INTERNET**.

## 1. Correction to Fail to Lockout File

!!! warning "Do this **WITHOUT INTERNET**"

```sh
sudo nano /etc/security/faillock.conf
```

Need to change the `# deny = 3` to`deny = 5`.
It is in **Line 32** or so.

## 2. Configure Pacman Mirror Fetch

!!! warning "Do this **WITHOUT INTERNET**"

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

## 3. Add Support for AUR and other Config options to `pamac`

!!! warning "Do this **WITHOUT INTERNET**"

Edit the *pamac config* file:
```sh
sudo nano /etc/pamac.conf
```

Edit the following:
```sh
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

### Command-line

```sh
# Update the Mirror List with max timeout for search 5 seconds
sudo pacman-mirrors --timeout 5 --country Singapore South_Korea India United_States Japan
# Update all Packages from these updated mirrors
sudo pacman -Syyu
```

### Check the Generated Mirror List

```sh
cat /etc/pacman.d/mirrorlist
```

## 5. Fix User Permissions for Groups

Get various access groups:
```sh
# For Serial Port Access
sudo usermod -aG uucp $USER
sudo usermod -aG lock $USER
```

### Optional Without Password `sudo`
First make sure to remove the nagging multiple password prompts:
```sh
echo "${USER} ALL=(ALL:ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/20-user
sudo chmod 440 /etc/sudoers.d/20-user
```

## 6. Install Essentials Software

### Security, Password Manager and Secure Vault

```sh
sudo pacman -Syu keepassxc veracrypt bleachbit paperkey patch \
	yubikey-personalization-gui rkhunter seahorse
```

### Ucode for CPU

```sh
sudo pacman -Syu intel-ucode
```

Or

```sh
sudo pacman -Syu amd-ucode
```

### Cleanup items Network items

```sh
sudo pamac remove -u stoken openconnect networkmanager-openconnect
```

### Flatpak Support

```sh
# Flatpak Support in Manjaro
sudo pamac install flatpak libpamac-flatpak-plugin
# Flatpak Search
sudo pamac install gnome-software
```

### Essiential Flatpak Control Tools

```sh
# For Rights Management on Flatpack Software
flatpak install flathub com.github.tchx84.Flatseal

# For Cleanup on Things Left behind
flatpak install flathub io.github.giantpinkrobots.flatsweep
```

### Disk Utility and Backup

```sh
sudo pacman -Syu gnome-disk-utility smartmontools hddtemp baobab \
	restic grsync ifuse
```

Flatpak Install:

```sh
flatpak install flathub org.freefilesync.FreeFileSync
```

Other Optional Packages:

```sh
## Optional
sudo pamac install mkinitcpio-firmware # Make This Optional

```

### Remove backup and utilities not needed

```sh
sudo pamac remove -u timeshift-autosnap-manjaro timeshift
```

### Firewall

```sh
# Uninstall
sudo pacman -R ufw gufw
# Reinstall
sudo pacman -Syu ufw
# Enable
sudo ufw enable
sudo systemctl enable --now ufw.service
```

Setup the Rules:

```sh
sudo ufw limit 22/tcp comment "SSH"
sudo ufw allow 5353 comment "avahi"
sudo ufw deny to 224.0.0.1 comment "Block multicast packages"
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Remove Email

```sh
sudo pamac remove -u thunderbird
```

### Basic Utilities

```sh
sudo pacman -Syu geany geany-plugins simple-scan cheese evince \
	speedcrunch usbview zbar catfish \
	htop bluez bluez-libs blueman \
	print-manager usbutils

# (optional) dpkg
# (optional) clipit qalculate-gtk
# (optional) bluez-tools bluez-utils
sudo pamac install zsa-wally-cli
```

Optional :

- **AUR**`bluez-firmware`
- **AUR** `zramd` ( Swap on to Zram )

### Development

```sh
sudo pacman -S binutils vim-airline vim-spell-en gvim \
	emacs exa tmux tree bat gitg stow screen nano-syntax-highlighting \
	meld ctags make cmake diffutils entr shellcheck sqlitebrowser \
	xclip go hashcat python-pyserial python-pygments python-pip \
	python-virtualenv scons the_silver_searcher tk upx flameshot \
	screenkey dialog bash-completion lua nim code terminus-font
```

- Can't use `gedit` with `Firejail`.

#### Fix the Keyboard Selection error in `vconsole`

We would need to edit the `vconsole.conf` file:

```sh
sudo nano /etc/vconsole.conf
```

Make sure that the content is like this:

```sh
KEYMAP=us
FONT=ter-132n
FONT_MAP=
```

This would help to update the terminal font as well as keymap.

Next we need to load this into the system:

```sh
sudo mkinitcpio -p linux61
```

After typing `-p` if tab completion is used the correct option would automatically appear.

### XFCE Docklike Plugin

```sh
sudo pamac xfce4-docklike-plugin
```

<https://aur.archlinux.org/packages/xfce4-docklike-plugin-git>

This would replace the default XFCE Window bar and become more dock like.

### Python Specific Items

```sh
sudo pacman -S python-pyperclip
sudo pamac install thonny
sudo pamac install pyinstaller
sudo pamac install python-pyautogui
```

- Could not get `tk` to work yet.

### Online Software

```sh
sudo pamac remove -u hexchat pidgin modemmanager
sudo pacman -Syu wget yt-dlp telegram-desktop signal-desktop \
	jq gping curlie xh nmap filezilla remmina \
	httrack	syncthing
sudo pamac install anydesk-bin
sudo pamac install zoom
sudo pamac install ungoogled-chromium-bin
sudo pamac install brave-bin
```

- **AUR** `google-chrome`
- **AUR** `mqttfx-bin`

### DTP, Multimedia and Graphics

```sh
sudo pacman -Syu pdfarranger img2pdf pdftricks cherrytree \
	viewnior imagemagick inkscape caffeine-ng \
	system-config-printer aspell aspell-en hyphen hyphen-en mythes-en \
	audacity vidcutter mpv rhythmbox ffmpeg
sudo pamac remove -u audacious
sudo pamac remove -u thunderbird

# Optional ( Only if installed )
sudo pamac remove -u thunderbird-i18n-en-us
```

### Embedded and Electronics

```sh
sudo pacman -Syu avr-binutils avr-gcc avr-libc arduino-avr-core avrdude \
	arduino openocd android-tools android-udev picocom moserial \
	lrzsz pulseview arm-none-eabi-binutils arm-none-eabi-gcc \
	arm-none-eabi-gdb arm-none-eabi-newlib
sudo pamac install saleae-logic2
```

Others:
```
 - (optional) kicad
 - (optional) kicad-library-3d
 - (optional) stlink
 - geda-gaf (Optional)
 - ngspice (Optional)
 - octave (Optional)
 - gqrx ( For SDR)
 - rtl-sdr ( For SDR)
 - gnuradio (For SDR)
 - gnuradio-companion (For SDR)
 - airspy (For AirspySDR)
 - (optional) sdcc
 - **AUR** nodemcu-pyflasher
 - **AUR** saleae-logic2
```

### VM and Windows Compatiblity

```sh
sudo pacman -Syu wine winetricks wine-gecko wine-mono wine-nine
```

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

## 7. Firejail Setup

### Install Firejail

```sh
sudo pacman -Syu firejail firetools
sudo apparmor_parser -r /etc/apparmor.d/firejail-default
sudo pamac install firejail-pacman-hook
sudo firecfg --add-users $USER
sudo firecfg

# Local Profile Directory
mkdir ~/.config/firejail

# Alternate Media Directory needed for Profile Enforcement
sudo mkdir /media
```

### Add a Shortcut to Run `Firejail Net Tracer` (NOT WORKING)

```sh
sudo firejail --nettrace
```

More Info on this from this Video : <https://odysee.com/@netblue30:9/networking:5>


### Fix for `FreeFileSync-bin` Package under Firejail

Right click on the Shortcut for `FreeFileSync` Select *Edit Application*.

Change the Command to :

```sh
/bin/bash -c 'firejail --net=none /opt/freefilesync/FreeFileSync'
```

Similarly for `RealTimeSync` :

```sh
/bin/bash -c 'firejail --net=none /opt/freefilesync/RealTimeSync'
```

### Fix for `VS Code` to be Non-Network under Firejail

Creat the 2 Profiles :

```sh
echo "net none" > ~/.config/firejail/code.local
echo "net none" > ~/.config/firejail/code-oss.local
```

This disables network completely for `VS Code`.

### Bypass to Install Extensions in `VS Code` under Firejail

```sh
firejail --ignore=net code
```

This would bypass the restriction on Network.

### Fix to enable `Yubikey` + Chrome `WebUSB` under Firejail

We would need to run the respective browser with special configuration:

```sh
firejail --ignore=private-dev firefox
firejail --ignore=private-dev chromium
```

## 8. Software Configuration

### VeraCrypt

- In Volume Section : Un-check the `Never save history`
- Go to "Settings -> Preferences"
    - Security Tab:
        - Check - "Wipe cached passwords on exit" and "Preserve modification..."
    - Mount Options Tab:
        - Check - "Mount volumes as read-only"
    - Background Task Tab: All options enabled
- Click on OK to save

### KeePassXC

- In View Menu : Select Compact mode - This would restart the program
- In View Menu : Un-Check "Show Preview Panel"
- In Tools Menu : open Settings
- In Application Settings: General -> Basic Setting Tab
    - Change "Remember previously used databases" to 2 recent files
    - Un-Check "Load previously open databases on startup"
    - Under "File Management Section" Un-Check all options
    - Under "User Interface Section" Check - "Use monospaced font for notes"
- In Application Settings: General -> Auto-Type Tab
    - Set "Auto-Type start delay" to 5000mS
    - Set "Auto-Type typing delay" to 50mS
- In Application Settings: Security
    - Check and Set " Lock Database after inactivity of " 600 sec
    - Check and set " Clear search query after " 8 min
- In Application Settings: SSH Agent
    - Check Enable SSH Agent Integration

### Fix Application AutoStart

Open Setings *Session and Startup* option.

Remove the Following:

- clipit
- Clipman

Add the Following:

- Secret Storage Service (Gnome Keyring)
- SSH Key Agent (Gnome Keyring)

Now inorder to load the **Gnome Keyring** :

- Got to the *Advanced* Tab
- Check - Launch GNOME services on Startup
- Click Close to save the settings.

### Fix *LibreOffice* Locale settings

- Open the **`LibreOffice Calc`** Application
- Open *Tools* -> **Options ...**
- Go to `Language Settings -> Languages`
- Set *Locale Setting* : `English (India)`
- Set *Default Languages for Documents* - **Western**: `English (USA)`
- Click on `Apply` and then `ok` to save.
- Similarly Open **`LibreOffice Writer`** Application and do the same.

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
- ~~`Super + g` = `gedit`~~
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
- Calculator = `speedcrunch`
- Home = `thunar`
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
- --- Date Time ShortCuts
- `Super + Shift + t` = `bash -c "sleep .5 && echo -n \"$(date)\" | xclip -i"` = Full Date with IST Time like `date` command.
- `Super + Shift + j` = `bash -c "sleep .5 && echo -n \"Journal.$(date +'%Y.%m.%d')\" | xclip -i"` = Special Journal Timestamp
- `Super + Shift + s` = `bash -c "sleep .5 && TZ=UTC echo -n \"Scratch.$(date -u +'%Y.%m.%d.%s')\" | xclip -i"` = Special Scratch Timestamp

## 11. Install udev Rules

There are 23 files in the directory `usb-rules` copy them to the system:

```shell
sudo cp -arTf ./usb-rules /etc/udev/rules.d
sudo chmod 644 /etc/udev/rules.d/*.rules
sudo chown -R root:root /etc/udev/rules.d/*.rules
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

https://aur.archlinux.org/packages/upd72020x-fw

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
