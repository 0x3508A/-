# XFCE Desktop Environment Notes

## Keyboard Shortcut for Opening a Terminal Program

Combo Command for **XFCE4 Terminal**

<https://www.systutorials.com/docs/linux/man/1-xfce4-terminal/>

### Option 1
This would open the **XFCE4 Terminal** in `108x27 mode` with `ranger` as active
tab and the Tab would be named "Ranger".

With press on `q` on Keyboard this would exit.

```sh
xfce4-terminal --active-tab -T "Ranger" --geometry 108x27 --command="ranger"
```

### Option 2
This would open the **XFCE4 Terminal** with 2 Tabs in the second tab it would
be named as "Vim" and would open `vim`.

```sh
xfce4-terminal --active-tab -T "Ranger" --geometry 108x27 --command="ranger" --tab -T Vim --command=vim
```

## Fix a Frozen (a.k.a hanged) XFCE Session

Reference:

- Video:  <https://www.youtube.com/watch?v=zQx5NH4rYIA>
- Site: <https://www.addictivetips.com/ubuntu-linux-tips/fix-a-frozen-xfce-linux-desktop/>

There are multiple types of failures possible in XFCE:

1.  The XFCE panel does not respond
2.  The whole desktop is frozen

### Fix XFCE Panel by Refreshing

Here are commands and comments details the process:

```sh
# 1. Obtain the Process ID of the XFCE4 Panel
pidof xfce4-panel
# 2. Kill the Panel using the PID obtained
kill -9 <PID>
```

This would immediately **restart** the XFCE Panel. Alternatively, you can completely restart the panel:
```sh
# 1. Destroy all Process of XFCE4 Panel
killall xfce4-panel
# 2. Restart the XFCE4 Panel (Background)
xfce4-panel &
# 3. Finally release from current terminal session
disown
```

### Fix the Window Manager
We would need to refresh the window manger:
```sh
# 1. Run the start-up command with '--replace' option
xfwm4 --replace &
# 2. Release the process from the current terminal
disown
```

## Better XFCE restart script
If you are like most and forget things easily its better to create a script for this purpose.

`xfce4-restart` Script file:
```sh
#!/bin/sh
killall xfce4-panel
xfce4-panel
xfwm4 --replace &
```

## `clipman` Clipboard Manager for XFCE

Video Showing how to use `clipman`

<https://www.youtube.com/watch?v=F6QRjRfBFcQ>

### XFCE Official Documentation

<https://docs.xfce.org/panel-plugins/xfce4-clipman-plugin/start>

### For Installation

```sh
sudo pacman-S xfce4-clipman-plugin
```

<https://archlinux.org/packages/extra/x86_64/xfce4-clipman-plugin/>

## XFCE docklike plugin

Github Source: <
### Fix the Window Manager
We would need to refresh the window manger:
```sh
# 1. Run the start-up command with '--replace' option
xfwm4 --replace &
# 2. Release the process from the current terminal
disown
```

## Better XFCE restart script
If you are like most and forget things easily its better to create a script for this purpose.

`xfce4-restart` Script file:
```sh
#!/bin/sh
killall xfce4-panel
xfce4-panel
xfwm4 --replace &
```

## `clipman` Clipboard Manager for XFCE

Video Showing how to use `clipman`

<https://www.youtube.com/watch?v=F6QRjRfBFcQ>

### XFCE Official Documentation

<https://docs.xfce.org/panel-plugins/xfce4-clipman-plugin/start>

### For Installation

```sh
sudo pacman-S xfce4-clipman-plugin
```

<https://archlinux.org/packages/extra/x86_64/xfce4-clipman-plugin/>

## XFCE docklike plugin

Github Source: <https://github.com/nsz32/docklike-plugin>

AUR Install: <https://aur.archlinux.org/packages/xfce4-docklike-plugin-git>

PPA for Debian: <https://launchpad.net/~xubuntu-dev/+archive/ubuntu/extras>

Explanation: <https://www.linuxuprising.com/2020/12/docklike-plugin-xfce-panel-icon-only.html>

```sh
sudo add-apt-repository ppa:xubuntu-dev/extras

sudo apt install xfce4-docklike-plugin
```

In case of Arch:

```sh
sudo pamac install xfce4-docklike-plugin
```



----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
