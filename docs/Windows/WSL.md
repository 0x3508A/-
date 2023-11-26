# Windows Sub-System for Linux

## Resources

### Overview

- **[awesome-wsl](https://github.com/sirredbeard/Awesome-WSL)** - Hub for Most information on WSL.
- [D3D12 GPU Video acceleration](https://devblogs.microsoft.com/commandline/d3d12-gpu-video-acceleration-in-the-windows-subsystem-for-linux-now-available/)
- **[Details on WSL and X11 GUI](https://github.com/ikrima/gamedevguide/blob/master/docs/dev-notes/linux/wsl.md)**
    - Web Location for the same <https://ikrima.dev/dev-notes/linux/wsl/>

### Linux Desktop

- [X410 Guides](https://x410.dev/cookbook/)

    - [Enable systemd in WSL2 and have the best Ubuntu GUI desktop experience!](https://x410.dev/cookbook/wsl/enable-systemd-in-wsl2-and-have-the-best-ubuntu-gui-desktop-experience/)
    - [Using X410 with WSL2](https://x410.dev/cookbook/wsl/using-x410-with-wsl2/)

- [You can start Mate desktop in WSL 2 like this](https://old.reddit.com/r/bashonubuntuonwindows/comments/15jeuyu/you_can_start_mate_desktop_in_wsl_2_like_this/)

    - [xWSL.cmd (Version 1.5 / 20220718)](https://github.com/DesktopECHO/xWSL)

- [Ubuntu 20.04 Desktop GUI on WSL 2 on Surface Pro](https://www.most-useful.com/ubuntu-20-04-desktop-gui-on-wsl-2-on-surface-pro-4.html)

    - [KDE Plasma On WSL On Ubuntu 20.04 On Surface Pro 3](https://www.most-useful.com/kde-plasma-on-wsl.html)
    - [Fully Working KDE on Bash on Ubuntu 20.04](https://old.reddit.com/r/bashonubuntuonwindows/comments/j2i5ix/fully_working_kde_on_bash_on_ubuntu_2004/)
    - [WSL2 GUI Using VcXsrv: Complete Guide For Beginners](https://old.reddit.com/r/linux/comments/ii79t0/wsl2_gui_using_vcxsrv_complete_guide_for_beginners/)

- [wsl-install.sh | Windows 10 WSL Setup Guide](https://github.com/davecwright3/wsl-linux-de)

- [Install Desktop GUI for WSL | WSL Enable Desktop Guide](https://hub.tcno.co/windows/wsl/desktop-gui/)

- [What's the easiest way to run GUI apps on Windows Subsystem for Linux?](https://askubuntu.com/questions/993225/whats-the-easiest-way-to-run-gui-apps-on-windows-subsystem-for-linux)

- LXQt

    - [LXQt on WSLg](https://unix.stackexchange.com/questions/686691/lxqt-on-wslg-doesnt-fit-the-full-screen)
    - [Launch xfce4 or other desktop in Windows 11 WSLg Ubuntu distro](https://askubuntu.com/questions/1385703/launch-xfce4-or-other-desktop-in-windows-11-wslg-ubuntu-distro/1385747#1385747)

### Nvidia/CUDA

- <https://ubuntu.com/tutorials/enabling-gpu-acceleration-on-ubuntu-on-wsl2-with-the-nvidia-cuda-platform#3-install-nvidia-cuda-on-ubuntu>
- <https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local>

## Commands

| Commands                             | Details                                           |
| ------------------------------------ | ------------------------------------------------- |
| `wsl --list --online`                | list available Linux distributions                |
| `wsl --list --verbose`               | list installed Linux distributions                |
| `wsl --unregister [distro]`          | unregister and uninstall wsl distro               |
| `wsl --set-default-version <ver>`    | WSL base version `<ver> = 1 or 2`in Windows       |
| `wsl --update`                       | update WSL                                        |
| `wsl --status`                       | check WSL status                                  |
| `wsl --system`                       | launch system distro                              |
| `wsl -d [distro] --exec '...'`       | run command without using the default Linux shell |
| `wsl -d [distro] --shell-type <typ>` | Set Shell type `<typ>=standard or login`          |
| `wsl --debug-shell`                  | launch debug shell for diagnostics purposes       |
| `wsl hostname --all-ip-addresses`    | get all Host IP address                           |

### Special Commands

Find the current DNS settings:

```sh
grep -m1 nameserver /etc/resolv.conf|awk '{print $2}'
```

```sh
ip route|grep default
```

```sh
ifconfig eth0|grep inet
```

## `WSLg` = Windows Subsystem for Linux - Graphical

-   Source Repository: <https://github.com/microsoft/wslg>

-   WSL: Run Linux GUI Apps by **Microsoft Developer**

    <https://www.youtube.com/watch?v=kC3eWRPzeWw>

    - Run Linux GUI apps on the Windows Subsystem for Linux

        <https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps?WT.mc_id=windows-c9-niner>

    - Windows Command Line Website

        <https://devblogs.microsoft.com/commandline/?WT.mc_id=windows-c9-niner>

    - WSL Tips and Tricks

        <https://craigloewen-msft.github.io/WSLTipsAndTricks/>

- Install Linux GUI apps on Windows 10 by [Chris Titus Tech](https://www.youtube.com/c/ChrisTitusTech)

    <https://www.youtube.com/watch?v=KcIdAunRSSc>

- The Pros and Cons of Linux in Windows by Chris Titus Tech

    <https://www.youtube.com/watch?v=ysCD2u80niM>

- I Coded with WSL2 for a Week

    <https://www.youtube.com/watch?v=LktFP0Dpl-c>

-   Windows 11 runs Graphical Linux Apps out of the box with WSLg

    <https://www.youtube.com/watch?v=b1YBx1L8op4>

- How to make the ultimate Terminal Prompt on Windows 11 *long Video*

    <https://www.youtube.com/watch?v=VT2L1SXFq9U>

- Arch Linux + WSL (Portuguese Video)

    <https://www.youtube.com/watch?v=8Ag_VgDd0CQ>

## Flatpak issues on WSL

Flatpak cannot be used normally after the built-in Systemd support is enabled:

<https://github.com/microsoft/WSL/issues/9119>

Steps to run Flatpak on WSL2 assuming a Ubuntu VM:

```sh
sudo apt install flatpak
sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

Installation would need a `sudo`:

```sh
sudo flatpak install flathub org.mozilla.firefox
```

Finally to Run the program it would not need any `sudo`:

```sh
flatpak run org.mozilla.firefox
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Windows Hub](./README.md)
- [Back to Root Document](../README.md)

