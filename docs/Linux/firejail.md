# `firejail` The SUID Sandbox program

Github: <https://github.com/netblue30/firejail>

Home Page: <https://firejail.wordpress.com/>

ArchLinux Wiki: <https://wiki.archlinux.org/title/Firejail>

> My main motivation behind studying `firejail` was to **block the telemetry** 
> from [`VS Code`](../Windows/vscode.md) and `FreeFileSync` like programs.
> The purpose of `firejail` being able to block **Network access** on 
> applications.
>
> This was a *very complex way* to achieve the goal while `flatpak` actually 
> did this much simply.
>

## What is `firejail` ?

`firejail` is a **SUID sandbox program** that reduces the risk of security breaches by restricting the running environment of untrusted applications using *`Linux namespaces`* and `seccomp-bpf`. It includes security profiles for over 1000 common Linux applications such as *Firefox*, *Chromium*, *Transmission*, *VLC* etc.

### Magic of `firejail`

You can do lots of useful things like:

- Disable access to Internet for selected programs.
- Control which directories are accessible to programs.
- Use different configuration of the same program for different use-cases.
- Keep telemetry sending programs in check.
- All this in the classical way of the Linux command line!

## Install Steps for ArchLinux

Video for this : <https://odysee.com/@netblue30:9/archlinux:d2e>

The steps are as follows:

1. Install `Firejail` software using `pacman` package manager:
	`$ sudo pacman -S firejail`
2. Configure the sandbox - this step integrates the sandbox with your desktop manager.
    `$ sudo firecfg`
3. Enable `AppArmor`. Starting with` version 0.9.62`, `AppArmor` is enabled automatically during configuration (step 2 above).
    `$ sudo apparmor_parser -r /etc/apparmor.d/firejail-default`

Another nice piece that comes with `firejail` is **`FireTools`**.
They make it easier to configure apps with special security Settings.

Here is a video showing the **`FireTools`** :
<https://odysee.com/@netblue30:9/firetools:6>

## AppImage Special

Applying `firejail` for `AppImage` files.

`AppImage` is a Linux distribution agnostic format used by many software developers to package their programs. This shows you how to handle `appimages` in `firejail` security sandbox.

For more information, check <https://firejail.wordpress.com/documentation-2/appimage-support/>

We would create a shortcut for opening the specific `AppImage` from desktop.

Desktop shortcut of `kdenlive` program`:
```sh
$ cat ~/Desktop/kdenlive.desktop
[Desktop Entry]
Name=kdenlive
Exec=/usr/bin/firejail --net=none --appimage /opt/kdenlive-20.12.2-x86_64.appimage
Icon=/home/netblue/config/kdenlive.svg
Terminal=false
Type=Application
```

*No Internet for `kdenlive` now !!*

## Create Custom Profile

```sh
# Create te Config Directory
mkdir -p ~/.config/firejail

# Start the Profile Builder
firejail --build=app-name.profile app-name
# Replace "app-name" with your programs

# Use the program for some time and close

# Update the Profile and integrate it into desktop
sudo firecfg
```

## Creating your own VPN on Digital Ocean & Secure Firefox

Yes, you can really lockout things.

Use DNS of HTTPS:

```
https://doh.applied-privacy.net/query
```

Here is a video explaining the details:
<https://odysee.com/@netblue30:9/sshvpn-final:b>

SSH Tunnel on port `8888`:

```sh
ssh -TND 8888 root@134.209.114.250
```

Set Tunnel on Proxy Settings to:

`SOCKS V5 Host: 127.0.0.1   @  Port : 8888`

No Proxy Field:

`localhost, 127.0.0.1`

- [X] Proxy DNS wen using SOCKS v5

Here is even better Advance Browser security Video:

<https://odysee.com/@netblue30:9/firefox:c>

## Security Profiles and you can go Fusion !!

Building Firejail Security Profiles:
<https://odysee.com/@netblue30:9/profiles:5>

*Cool Kids play `firejail` !!*

----
<!-- Footer Begins Here -->
## Links

- Videos
    - [`firejail` Security Sandbox Channel](https://odysee.com/@netblue30:9) over Odysee
- [Back to Command Line Hub](./cli/README.md)
- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
