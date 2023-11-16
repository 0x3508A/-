# `sudo` privileged execution command

To execute command as other user or Root.

## Add `sudo` Permission for a given user
```sh
root@server:~$ usermod -aG sudo <user-name>
```

**Note:** For the above command to work you must have **root** access.

## Check if you have as `sudo` user
```sh
sudo -l
```

Here are the Possible results:

- `(ALL) ALL` You can do everything but you would need to *provide a Password*.
    Probably your user permissions look like: `root ALL=(ALL : ALL) ALL`
- `(NOPASSWD) ALL` You can do everything **without Password**.
    Your settings look like: `%wheel ALL=(ALL:ALL) NOPASSWD: ALL`
    This is for the Whole `wheel` user group.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
