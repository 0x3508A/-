# `tlp` Power Saving Service

Command to apply laptop power saving settings.

[`tlp` Command](http://linrunner.de/tlp/)

**`tlp`** is a feature-rich command line utility for Linux, saving **laptop battery power** without the need to delve deeper into technical details.

`tlp` *default settings* are already **optimized for battery life** and implement **Powertop's recommendations** out of the box. So you may just install and forget it.

Nevertheless `tlp` is highly customize-able to fulfill your specific requirements.

**Note** Its installed by default in #Manjaro even on #Desktop.

Reference:

- Project: <http://linrunner.de/tlp/>
- Arch Linux Specific <https://wiki.archlinux.org/title/TLP>
- Disable USB Auto-Suspend <https://wiki.archlinux.org/title/TLP#USB_autosuspend>

## Disable USB Auto-Suspend

Tags: USB Auto-Suspend Power-Management Suspend

Check the USB Configuration in `/etc/tlp.conf` :

```sh
# Do not suspend USB devices
USB_AUTOSUSPEND=0
```

On a Desktop Linux this is very important other wise things would keep getting disconnected.

## Check `tlp` Service

Verify the service using `systemd` - `systemctl` command :
```sh
sudo systemctl status tlp
```

## See the Current status of `tlp` Service
```sh
sudo tlp-stat
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)

