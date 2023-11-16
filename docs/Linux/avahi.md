# Avahi - Zero Configuration networking

Reference: <https://wiki.archlinux.org/title/Avahi>

[Avahi](https://avahi.org/) is a free Zero-configuration networking (zeroconf) implementation, including a system for multicast DNS/DNS-SD service discovery. It allows programs to publish and discover services and hosts running on a local network with no specific configuration. For example you can plug into a network and instantly find printers to print to, files to look at and people to talk to. It is licensed under the GNU Lesser General Public License (LGPL).

Basically it would allow the `raspberrypi.local` or more generically `<hostname>.local` way of finding the PC.

## Installation

```sh
sudo pacman -S avahi nss-mdns
```

Typically the `avahi` package comes pre-installed in most Linux distros but isn't activated.

## Activating the Service

We need to disable the `systemd-resolved` service before running or activating `avahi`,

```sh
sudo systemctl disable systemd-resolved.service
```

Finally we can now start the `avahi` service.

```sh
sudo systemctl start avahi-daemon.service

# Obtain IP address to mDNS name link
sudo avahi-autoipd -D
```

You can also `enable --now` to activate and enable for boot.

## Firewall Configuration

```sh
sudo ufw allow 5353
# or
sudo ufw allow 5353/udp
```

This would allow the `mDNS` service to work in the local network.

## Adding services to `avahi`

```sh
# Enable SSH to discover using mDNS
sudo cp /usr/share/doc/avahi/ssh.service /etc/avahi/services/
# SFTP service
sudo cp /usr/share/doc/avahi/sftp.service /etc/avahi/services/
```

This would allow `ssh` and `sftp` clients to discover this host over `mDNS`.

!!! warning
    Adding services like SSH to `avahi` can be a serious security threat if not configured properly.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
