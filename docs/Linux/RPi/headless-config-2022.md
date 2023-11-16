# Headless Config - 2022

Reference:

 - <https://www.raspberrypi.com/documentation/computers/configuration.html#setting-up-a-headless-raspberry-pi>
 - <https://www.declarativesystems.com/2022/06/23/headless-rasperry-pi-setup.html>

This method would allow you to access the Raspberry Pi via SSH without a monitor.

There are 2 steps in the Process:

1. Network Configuration
2. Enable SSH
3. User Creation

For most of the work we would need to modify *boot* partition of the *SD Card* loaded with fresh *Raspbian OS*. Means *we can't do it with an existing installation*. If the initialization fails at some point then you would need *re-flashing of the SD Card again*, hence be careful.

## Network Configuration

We would need to create the `wpa_supplicant.conf` file in the *boot* partition of the *Fresh Raspberry Pi SD Card flashed*.

```sh
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
country=IN
update_config=1

network={
 scan_ssid=1
 ssid="<Name of your wireless LAN>"
 psk="<Password for your wireless LAN>"
 proto=RSN
 key_mgmt=WPA-PSK
 pairwise=CCMP
 auth_alg=OPEN
}
```

!!! note "Tip"
    A *pro-tip* Connect to the WiFi that is not connected to Internet just to be safe.

## Enable SSH

Create a blank file `ssh` in the *boot* partition of the *Fresh Raspberry Pi SD Card* flashed with *Raspbian OS*.

```bash
touch ssh
```

## Creating the User

To add the user we need to create `userconf.txt` in the *boot* partition of the *Fresh Raspberry Pi SD Card flashed*. with one line of text looking like:

```sh
username:password
```

Where `username` is your actual user name in the Raspberry Pi
when you login ( `labuser` ).

Remember we *should not use `pi` user* like earlier times, as *its a security risk*. The `password` part is an *encrypted form of the pass phrase* we would like to use for login.

To *Get the Encrypted pass phrase* we need to use the following command:

```bash
  openssl passwd -6
  # $6$46NGJBx4IGBEew0n$VqnPtf1EqKnxEZ7JcJAf5jn8w6fMytGysiCpeFxRPDlRjxS8szVMY1ZdvWy/2njEezIbB6IeXLt4IUCBydAC1/
  # for Raspberry - !! UNSECURE PASSWORD DO NOT USE !!
```

This command would ask for the password and show in the encrypted
form.

Hence the final `userconf.txt` would look like:

```sh
labuser:$6$46NGJBx4IGBEew0n$VqnPtf1EqKnxEZ7JcJAf5jn8w6fMytGysiCpeFxRPDlRjxS8szVMY1ZdvWy/2njEezIbB6IeXLt4IUCBydAC1/
```

Here is the commands that can ease the process creating  `userconf.txt` easy.

```bash
  echo -n "labuser:" | tee userconf.txt
  openssl passwd -6 | tee -a userconf.txt
```

Some times there is issues with `openssl` Generation hence we can try another way:

```sh
echo 'mypassword' | openssl passwd -6 -stdin
```

Or from Above:

```sh
echo 'maypassword' | openssl passwd -6 -stdin | tee -a userconf.txt
```

**But remember to clear it from you history !!**

## Reboot and Start

Once the *boot* partition has all the 3 parts done, safely un-mount the *SD Card*. Insert it into your *Raspberry Pi* and power it *On*. It would take approximately *3 to 5 Minutes* to get everything ready. So be patient.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
