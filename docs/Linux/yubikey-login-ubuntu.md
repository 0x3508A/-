# Yubikey to Login in  Ubuntu

Reference Video <https://www.youtube.com/watch?v=pfVhAtJt5_o>

## Installation

```sh
sudo apt install libpam-u2f
mkdir -p ~/.config/Yubico
# insert the Yubikey
pamu2cfg > ~/.config/Yubico/u2f_keys # Enable the Yubikey Auth
# Press the Yubikey button when it flashes
```

## Enable Yubikey for Sudo

```sh
sudo nano /etc/pam.d/sudo
```
Look for the line `@include common-auth` Below it:

```
auth <TAB> required <TAB> pam_u2f.so
```

## Login Manager to Require Yubikey:

```sh
sudo nano /etc/pam.d/gdm-password
or
sudo nano /etc/pam.d/lightdm
```

Look for the line `@include common-auth` Below it:

```
auth <TAB> required <TAB> pam_u2f.so
```

## Add Yubikey needed for TTY login

```sh
sudo nano /etc/pam.d/login
```

Look for the line `@include common-auth` Below it:

```
auth <TAB> required <TAB> pam_u2f.so
```

## SSH Login

Reference Video <https://www.youtube.com/watch?v=xVW1fGRlRkE>

```sh
ssh <cred>
```

On the Remote Machine:

```sh
sudo add-apt-repository ppa:yubico/stable
...
sudo apt install libpam-yubico # For Yubikey Auth
```

Edit the config in Remote Machine:

```sh
sudo nano /etc/ssh/authorized_yubikeys
...
# Add a Line
<userID>:<first12chars-short-press-yubikey>
# Save the file after this
```
Generate API Key:

<https://upgrade.yubico.com/getapikey>

Enter you Email Address. In YubiKey OTP short press the button to enter text.
Check the Box for terms and press *Get API Key*.

It would give the `<CLIENT ID>` and `<Secret Key>`.

Edit the SSH Config on Remote Machine:

```sh
sudo nano /etc/pam.d/sshd
...
# Add a line at the Opt of the file
auth required pam_yubico.so id=<CLIENT ID> KEY=<Secret Key> authfile=/etc/ssh/authorized_yubikeys
## Save tje File with this
```

Edit the SSHD config in Remote Machine:

```sh
sudo nano /etc/ssh/sshd_config
...
PasswordAuthentication no
...
ChallengeResponseAuthentication yes
...
UsePAM yes
## Save the File
```
Restart the SSH Service on the Remote Machine:
```sh
sudo systemctl restart ssh
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
