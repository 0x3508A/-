# Alpine Linux

Home Page : <https://alpinelinux.org/>

Images : <https://alpinelinux.org/downloads/>

## Preparation

!!! warning "Disk Limitation"
    Alpine Linux can only be installed in full disk. Means **no dual booting** is possible.

Use the *Standard Version* of the ISO.

Since, most of the other things are installed on need basis.

## Install

Start the Installation:

```sh
setup-alpine
```

Steps:

```sh
# Select the US keyboard
us
# Select the Layout
us
# Interface for Connection to Internet
eth0
# Network Address Mechanism
dhcp
# No need for manual changes
n
# Now Set the Root Password
<root-password>
# Set timezone
Asia/Kolkata
# No Proxy
none
# Alpine Mirror selection- Press space bar | https://mirrors.alpinelinux.org/ = 1
1
# Setup Normal Admin User
<username>
# Password for new User
<user-password>
# No need
none
# SSH Server
openssh
# Select the Disk Partition
sda
# Select the way to partition - Better to use cryptsys
cryptsys
# Password for Disk encryption
<disk-password>
# Password to open Disk
<disk-password>
# Begin formatting the disk
y
```

## Post install

```sh
df -kh # See Disk Usage
doas apk update
# Lock Root user out
doas passwd -l root
doas setup-desktop ## Setup desktop Environment
# Add Librewolf from Flatpak
doas apk add flatpak vim nano
doas flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
doas flatpak install flathub io.gitlab.librewolf-community

# Add AppArmour**
doas apk add apparmor apparmor-utils apparmor-profiles
cat /sys/kernel/security/lsm
capability,landlock,yama
## Configure AppArmour to Startup
doas vim /boot/extlinux.conf
echo "... APPEND .... =ext4 lsm=capability,landlock,yama,apparmor"
doas rc-service apparmor start
doas rc-update add apparmor boot

# Add Enforce AppArmor
doas aa-status
doas aa-enforce /etc/apparmor.d/*

# Static IP Config
# > https://www.cyberciti.biz/faq/how-to-configure-static-ip-address-on-alpine-linux/
doas nano /etc/network/interfaces
# add the below definition
cat <<EOF
auto eth0
iface eth0 inet static
        address 192.168.1.50/24
        gateway 192.168.1.1
        hostname alpine.home.net
EOF
```

----
<!-- Footer Begins Here -->
## Links

- Videos
  - Installing Alpine Linux as a Desktop OS
    - <https://www.youtube.com/watch?v=YNYtJ3jyMRs>
    - This also contains important info about **AppArmour**.
  - More of a Review on security features of an older version and **`lynis`** tool.
    - <https://www.youtube.com/watch?v=5tDEYkWD338>
- [Back to Distro Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../README.md)
