# ArchLinux Hub

All about **ArchLinux** and its derivatives like *Manjaro* .etc.

## Topics

- **[Manjaro `vconsole.conf` fix for `systemd` degraded status](./manjaro-vconsole-conf-fix-for-systemd.md)** - Fix the system status after installing Fresh Manjaro Linux.
- [Enabling Emoji support in Terminal](./Manjaro-emoji-terminal-support.md) - UNICODE in Terminal.
- **[Learning Nix under Manjaro](./Manjaro-nix.md)** - Helpful way to install Nix with its full feature set.

### Manjaro Installation

- [Steps V1](./Manjaro-XFCE-V1.md) - Steps to take after installing Manjaro XFCE Edition.
- [Steps V2](./Manjaro-XFCE-V2.md) - Second version
- **[Steps V3 Latest](./Manjaro-XFCE-V3.md)** - Latest steps for configuration.

### Arch Linux Installation *Attempts*

Multiple attempts but no success yet.

- [V1](./ArchLinux-install-v1.md)
- [V2](./ArchLinux-install-v2.md)
- [V3](./ArchLinux-install-v3.md)
- [W5 Pro Stick PC - Command Line only](./ArchLinux-install-W5-Pro.md)
- **[Latest attempt in Repository `arch-base`](https://github.com/0x3508A/arch-base)**
- **[Desktop IoT](./desktop-iot.md)** - Creating a IoT server using ArchLinux.

## Others

### `pacman` List all installed packages

```sh
pacman -Qq
```

### Update Manjaro Linux Repository using `pamac`
```sh
pamac update --force-refresh
```

### How to install Proton VPN on Archlinux and Manjaro

```sh
pamac build protonvpn
```

#### References

- <https://protonvpn.com/support/official-linux-client-arch/>

#### Adding Public Key for Repository

- <https://protonvpn.com/support/official-linux-client-arch/#note4>
- Click the link <https://repo.protonvpn.com/debian/public_key.asc> to download.
- `pacman-key --add /path/to/downloaded/public_key.asc`

### Fix Bluetooth Audio

Reinstall `blueman`

```
sudo pacman -Sy blueman
```

### Fix for `webkit2gtk` vulnerability issue and Pin Entry

`zenity-light 3.41.0-1`

Display graphical dialog boxes from shell scripts. Light version without WekKit2
<https://aur.archlinux.org/packages/zenity-light>

Uninstall the normal `zenity` and `webkit2gtk` packages

### iPod Nano Install on Manjaro Linux

Reference: <https://wiki.archlinux.org/title/IOS>

1. Setting Up the software

    `sudo pacaman -S ifuse rhythmbox libmtp libgpod`

2. Check the `usbmuxd` Service has or Manually start it.

    This is needed for the plugged in Ipod Mini / Nano to work.

    `sudo systemctl status usbmuxd.service`

    If its not started then:

    `sudo systemctl start usbmuxd.service`

3. Mount the Player: Use `thunar` to mount the Player.
4. Open `rhythmbox` to browse the Ipod.

    <https://software.manjaro.org/package/rhythmbox>


### Creating USB Drive for any ArchLinux Variant

1. Find all disks on your system
	`sudo fdisk -l`
   Also can use another command:
	`lsblk -f`
2. If the device is mounted then unmount the same Else ignore this.
	`sudo umount /dev/sdX`
3. Write the ISO image using `dd` : <br />
	```sh
    sudo dd bs=4M if=/path/to/endeavouros-x86_64.iso of=/dev/sdX \
    		conv=fsync oflag=direct status=progress
    ```

### Theme Packages for XFCE

Some **Manjaro** style Themes:

```sh
sudo pacman -S papirus-icon-theme materia-gtk-theme
```

Some other Themes:

```sh
sudo pacman -S arc-gtk-theme arc-icon-theme
```

### ArchLinux Wallpapers

```sh
sudo pacman -S archlinux-wallpaper
```

### User management and Photo Association Packages

```sh
yay -S mugshot # Helps to add photo for the user
yay -S gnome-system-tools # Help to add User settings.
```

### Get rid of `mkinitcpio` Firmware errors

```sh
yay -S mkinitcpio-firmware
```

This is combo package that installs all needed firmware sub-packages:

<https://aur.archlinux.org/packages/mkinitcpio-firmware>


### Downgrading packages in Manjaro

Install the downgrade capability in Manajro:

```sh
sudo pamac install manjaro-downgrade
```

Next Downgrade the package:

```sh
sudo manjaro-downgrade emacs
```

## Install Arch in 2 Minutes (Chris Titus)

Here is the video like that starts all this:

<https://www.youtube.com/watch?v=hKpxMWm5l7w>

And the Accompanying web-page Link:

<https://christitus.com/installing-arch-in-2-minutes/>

Basically the command:

```sh
bash <(curl -L christitus.com/archtitus)
```

Is a link to a repository code:

<https://github.com/ChrisTitusTech/ArchTitus/blob/main/scripts/curl-install.sh>

Here is the **[Raw version](https://github.com/ChrisTitusTech/ArchTitus/raw/main/scripts/curl-install.sh)**.

The source code is in the following Repositories:

- The **[Original repository](https://github.com/ChrisTitusTech/ArchTitus)** of the code by Chris Titus.
- The [developer version](https://github.com/khaneliman/ArchInstaller) by Austin Horstman.

Both have an inspiring things behind.
One thing that catches people off-guard is the problem with `git`.
Typically in `Arch` boot the install of `git` is difficult.

One needs to do this to mitigate the same:

```sh
pacman -Sy glibc git reflector
```

These would install the required packages.


----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
