# `udevadm` USB device management Daemon

`udev` stand of #USB Device Rules. It controls the run-time behavior of `systemd-udevd`, requests kernel events, manages the event queue, and provides simple debugging mechanisms.


## Reload USB Rules or USB connection Rules

```sh
sudo udevadm control --reload-rules && sudo udevadm trigger
```

## Check USB Auto Suspend #State

```bash
for d in /sys/bus/usb/devices/[0-9]* ;\
    do if [[ -e $d/product ]] ;\
       then echo -e "`basename $d`\t`cat $d/power/control`\t`cat $d/speed`\t`cat $d/product`" ;\
       fi ;\
    done
```

This would print if the Power Control / Suspend is enabled on various USB devices.

## Disable #auto-suspend on #USB

### Method 1 = Using `tlp` configuration

Explained [here](./tlp.md#disable-usb-auto-suspend), this method really **WORKS**.

### Method 2 = Using `udev` Rules

An example: `/etc/udev/rules.d/91-local.rules`
(note that `91` is higher than the `42` in the filename of
`/usr/lib/udev/rules.d/42-usb-hid-pm.rules`, so gets executed *after*)

```
ACTION=="add", SUBSYSTEM=="usb", ATTR{product}=="Razer Abyssus", ATTR{power/control}="on"
```

This switches the **`control`** parameter from **`auto`** (i.e. `auto-suspend` is allowed) to **`on`** (`auto-suspend` is **not allowed**).

First, make the new rule take effect:
```sh
sudo udevadm control --reload
```

Reference : <https://bbs.archlinux.org/viewtopic.php?pid=1178070#p1178070>

## Prevent **MicroPython** from Auto-Suspend or USB Suspend the Device

First we need to find the device using [`lsusb` Command](./cli/lsusb.md):
```sh
$ lsusb
...
Bus 005 Device 013: ID 2e8a:0005 MicroPython Board in FS mode
...
```

Hence the New rule for this becomes `12-micropython.rules`:
```
ATTRS{idProduct}=="511b", ATTRS{idVendor}=="0416", MODE="0666", GROUP="plugdev"
ACTION=="add", SUBSYSTEM=="usb", \
ATTR{idProduct}=="511b" , ATTRS{idVendor}=="0416", ATTR{power/control}="on"
```

!!! note
    Permission for `12-micropython.rules` must be `644` and should be created as `root`.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
