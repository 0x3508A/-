# Ubuntu host key randomization and `cloud-init`

Ubuntu SSH Keys randomization & using cloud-init to create multiple VM
from the given template

```sh
$ sudo apt search cloud-init # Check if it is installed
$ sudo rm /etc/ssh/ssh_host_* # Remove SSH host Keys
$ sudo truncate -s 0 /etc/machine-id
$ ls -l /var/lib/dbus/machine-id # Find out if the Symlink is correct to
# /etc/machine-id that was emptied
$ sudo apt clean # Remove the db and cache
$ sudo apt autoremove
$
```

Regenerate Host keys
```sh
sudo rm ssh_host_*
sudo dpkg-reconfigure openssh-server
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Distro Hub](./Distro/README.md)
- [Back to Linux Hub](./README.md)
- [Back to Root Document](./README.md)

