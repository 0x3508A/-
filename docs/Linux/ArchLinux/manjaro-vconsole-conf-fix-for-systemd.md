# Manjaro `vconsole.conf` fix for `systemd`

Reference: <https://bbs.archlinux.org/viewtopic.php?id=251853>

After installing Manjaro Linux one might notice this:

```sh
$ systemctl status -l
...
    state: degraded
...
```

Well its not much of a warning but may be sometimes taken as an error.

Digging a little bit deeper:

```sh
[root@amd64-archlinux bernd_b]# systemctl status systemd-vconsole-setup
● systemd-vconsole-setup.service - Setup Virtual Console
     Loaded: loaded (/usr/lib/systemd/system/systemd-vconsole-setup.service; static; vendor preset: disabled)
     Active: inactive (dead)
       Docs: man:systemd-vconsole-setup.service(8)
             man:vconsole.conf(5)
```
This is the problem !!
A simple fix can help here.

We edit the console keymap file:

```sh
sudo nano /etc/vconsole.conf
```

Make it look like:

```sh
KEYMAP=us
FONT=
FONT_MAP=
```

Finally we reconfigure the system:

```sh
sudo mkinitcpio -p linux61
sudo systemctl restart systemd-vconsole-setup
```

This would make sure that the `systemctl` reports no errors.

Check again with `systemctl status -l`.

----
<!-- Footer Begins Here -->
## Links

- [Back to Main `systemd` article](../systemd.md)
- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
