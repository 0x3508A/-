# Unattended Upgrades

This is an automatic upgrades application that takes control of `apt` to perform the common `update` and `upgrade` like command at a periodic basis.

### References

- <https://linuxhint.com/manage-raspberry-pi-automatic-updates/>
- Repository detailing the install process<br /><https://gist.github.com/coopbri/0cae1253904f491706618d42236cd094>
- Basic install even for **Debian** and **Ubuntu** systems:<br /><https://pimylifeup.com/unattended-upgrades-debian-ubuntu/>
- Configuration Examples: <br /><https://gist.github.com/anatolebeuzon/98ff195f3375e8ca621e7df3955b4f23>
- Video <https://www.youtube.com/watch?v=ukHcTCdOKrc>


## Installation

```sh
sudo apt install unattended-upgrades
```

You would be presented with `ncurses` based prompt press yes to confirm.

!!! note "Occasional Warning message"
    Don't worry if you see some warning messages during the installation. Its common and we would complete the correct configuration.

!!! info "Internet Connection"
    We do need a active internet connection with the PC or Raspberry Pi that we are configuring this. Else we would not be able to test out the configuration.

## Configuration

We would need to edit the `/etc/apt/apt.conf.d/50unattended-upgrades` file:

```sh
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

Uncomment and Add the lines :

```sh
...
      "origin=Debian,codename=${distro_codename}-updates";
      "origin=Debian,codename=${distro_codename}-proposed-updates";
	  "origin=Raspbian,codename=${distro_codename},label=Raspbian";
      "origin=Raspberry Pi Foundation,codename=${distro_codename},label=Raspberry Pi Foundation";
...
```
**Note:** Last 2 lines are only needed for **Raspberry Pi**.
Make sure to save the file.

Finally, we change the tasks periodicity by editing the file `/etc/apt/apt.conf.d/20auto-upgrades`:

```sh
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::Verbose "1";
APT::Periodic::AutocleanInterval "7";
```

Make sure that the file (`/etc/apt/apt.conf.d/20auto-upgrades`) has the above lines.
Remember to save the file.

## Load the New Configuration

```sh
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

You would be presented with `ncurses` based prompt, press yes again.

## Check the status of `unattended-upgrades` service

```sh
sudo systemctl status unattended-upgrades.service
```

## Test out the `unattended-upgrades` service

```sh
sudo unattended-upgrade -d -v --dry-run
```

If there are any upgrades then it would start the same.
Else it would be an normal output with lot of text.

## Alternative technique using `crontab` service

Edit the `crontab`:

```sh
sudo su
crontab -e
```

Add the line:

```sh
0 0 * * 0 sudo apt-get update && sudo apt-get dist-upgrade -y && sudo apt-get autoclean
```

This would execute every day to perform the upgrade.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
