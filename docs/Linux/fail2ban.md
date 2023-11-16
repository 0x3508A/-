# `fail2ban` - Security Service

Homepage <http://www.fail2ban.org/>

Reference Video: <https://www.youtube.com/watch?v=ukHcTCdOKrc>

Fail2ban, written in Python, is a scanner that examines the log files produced by the Linux System, and checks them for suspicious activity. It catches things like *multiple brute-force attempts to log in*, and can inform any installed firewall to stop further login attempts from suspicious IP addresses.

If you’re exposing port `22` for `SSH`, it’s recommended to install `fail2ban`. If you’re simply using `OpenVPN` via `PiVPN`, then `fail2ban` isn’t required.

## Installation of `fail2ban`

```sh
sudo apt install fail2ban
```

#### Configuration of `fail2ban` for `sshd` and `ufw`

If you are using `fail2ban` to safeguard `ssh` and you have `ufw` firewall install then use this.

```sh
sudo nano /etc/fail2ban/jail.local
```

Add the following text:

```ini
[DEFAULT]
bantime = 1h
banaction = ufw

[sshd]
enabled = true
```

Make sure to save the file.
This tells `fail2ban` use `ufw` to block the attempts on `sshd` a.k.a **openSSH Server**.

#### Enable `fail2ban` and restart it

```sh
# Start `fail2ban` now
sudo systemctl enable --now fail2ban
```

Restart it in case you have added the `jail.local` file:

```sh
sudo systemctl restart fail2ban
```

Finally restart the `sshd` a.k.a **openSSH Server**.

```sh
sudo systemctl restart sshd
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
