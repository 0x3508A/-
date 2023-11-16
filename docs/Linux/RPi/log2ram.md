# `Log2Ram` Service

Save the Raspberry Pi SD card with `Log2Ram`.

Sources : <https://github.com/azlux/log2ram>

Write Up: <https://pimylifeup.com/raspberry-pi-log2ram/>

## Introduction

The `Log2Ram` utility helps to **reduce wear** on the **SD Card** of Raspberry Pi.

It moves the **default Logging** to the **RAM instead of SD Card**.
That's where it gets its name **`Log2Ram`**.

## Reference

- Source Repository: <https://github.com/azlux/log2ram>
- <https://pimylifeup.com/raspberry-pi-log2ram/>
- <https://mcuoneclipse.com/2019/04/01/log2ram-extending-sd-card-lifetime-for-raspberry-pi-lorawan-gateway/>

## Download and Install `Log2Ram`

Grab the link from the [Releases](https://github.com/azlux/log2ram/releases) tab on [Github](https://github.com/azlux/log2ram).

<https://github.com/azlux/log2ram/releases>

```bash
# Download the Release
wget https://github.com/azlux/log2ram/archive/refs/tags/1.6.1.tar.gz -O log2ram.tar.gz
# Un-Archive the file
tar xf log2ram.tar.gz
# Enter the Directory - Name is based on Release
cd log2ram-1.6.1
# Start the Installation
sudo ./install.sh
```

!!! note
    Do not reboot immediately we need to configure `Log2Ram`.

## Configure `Log2Ram`

For configuration we need to edit `/etc/log2ram.conf`:

```bash
sudo nano /etc/log2ram.conf
```

Edit the Links:

```
...
SIZE=40M
...
PATH_DISK="/var/log"
...
```

If these are already at the correct value no need to modify.
The Size of the logs `40M` can be increased up to `128M` depending on the model of your Raspberry Pi.
Finally after saving the file you can **reboot** the Raspberry Pi.

## Check if `Log2Ram` is running

Check the `df` **Disk Free** command:

```bash
df -h
# It should Show Something like:
# Filesystem      Size  Used Avail Use% Mounted on
# ...
# log2ram          40M   18M   43M  30% /var/log
# ...
```

This output shows that the `Log2Ram` is working since, `/var/log` is mounted to RAM.

## [Log2Ram Error 1071](./log2ram-issue.md)

[Error 1071](./log2ram-issue.md) detailed in this document.


----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
