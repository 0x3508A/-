# `lstopo` Command

Show System Topology - All **RAM** **PCI** **CPU** **Hardware** !!!

Sometimes we need to know what is our system configuration like (CPU, RAM, network interfaces, etc...), but we don't want to open computer case to look what's inside or even don't have such possibility, especially when we are connected to remote server.

We can use `lstopo` command in Linux command line to quickly display system architecture.

References:

- <http://www.tuxfixer.com/display-hardware-topology-in-linux/>
- <https://manpages.ubuntu.com/manpages/trusty/man1/lstopo.1.html>
- <https://linux.die.net/man/1/lstopo>

!!! note
    You need to run this command as `root` else it would not work.
    Hence `sudo` is essential when running these.

Install:
```sh
# For Arch Linux
sudo pacman -S hwloc-libs hwloc-gui
# Or for Manjaro
sudo pacman -S hwloc
```

There are actually 3 commands part of the same package:

- `lstopo` = Shows a GUI displaying the Topology
- `lstopo-no-graphics` = Same as above but without GUI in CLI directly.
- `hwloc-ls` = Even More detailed hardware info in CLI


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)