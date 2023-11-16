# SSH Server `sshd`

## Installing `sshd` on Raspberry Pi

Typically SSH comes preinstalled if not then use the below:

```bash
sudo apt install ssh
sudo systemctl enable --now sshd.service
```

## Basic Secure Configuration of `sshd` on Raspberry Pi

This is a set of edits that would secure the SSH so that we can copy our identity.

Edit the `/etc/ssh/sshd_config` file :

```bash
# Create Backup
sudo cp /etc/ssh/sshd_config /etc/ssh/backup_sshd_config
# Edit the file
sudo nano /etc/ssh/sshd_config
```

Keeping a backup of the original file `/etc/ssh/sshd_config` is
a good idea in case something goes wrong.

Change the Following lines in `/etc/ssh/sshd_config` file:

```
...
PermitRootLogin no
...
AuthorizedKeysFile      .ssh/authorized_keys
...
```
Uncomment the lines and modify as shown.

## Copy an SSH Identity to Raspberry Pi

This step we copy our public key to the remote system.
Before doing this ensure that *normal ssh login* is working.

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub user@IPADDR
```

This would install the `id_rsa.pub` identity public key
to the remote machine.

!!! note
    For this command to work you need `.ssh` directory to be writable on the Host.

If you are ok to enable Password authentication and SSH Key
based authentication you are done here.

Just need to *Restart* the `sshd.service` to get the new
configuration Loaded.

## Enable SSH Key based Authentication on Raspberry Pi

We are now going to disable password based authentication and enable.

!!! warning
    Keep an active SSH login before proceeding.
    This would help in case something goes wrong.

Change the Following lines in `/etc/ssh/sshd_config` file:

```
...
PubkeyAuthentication yes
...
PasswordAuthentication no
...
```
This would disable the password authentication.
Any one wanting to access would need to have the SSH Keys for access.

Finally we need to restart the `ssh` service:

```bash
sudo systemctl restart sshd.service
```

!!! warning
    Don't close the the current SSH session yet.
    This might come in handy if something went wrong and you want to go back.

## Testing the SSH Connection to Raspberry Pi

Test the SSH Connection:

```bash
ssh -i ~/.ssh/id_rsa user@IPADDR
```

This would force the use the specific SSH key `~/.ssh/id_rsa` for authenticating.

If everything works then you have a `sshd` configured securely.
Else you would need to remove the changes in the `/etc/ssh/sshd_config` and
re-do the process.

## SSH Trouble shooting on Host

If you get the message:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
......
......
```

Its possible that you have some other **Host configuration** for the
same IP or Raspberry Pi host name.

- Delete the `known_hosts`  and `known_hosts.old` file in the `.ssh` directory.
- This would remove the older host config.

!!! note
    These are in your **HOST PC** or the PC you are trying to connect the **Raspberry Pi** from.


----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
