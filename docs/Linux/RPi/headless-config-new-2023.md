# Headless Config - 2023

!!! bug "DOES NOT WORK"
    This is not working at all and is kept here only for documentation purpose.

    **This does not work Verified `2023-02-22 19:02.40-285` and Limited only to Raspberry Pi Lite OS**

After using the `Raspberry Pi Imager` we realized that it has lot of security issues.
First It steals the WiFi password from PC.
Though it does not store it correctly.

And we wanted a correct alternative for other older variety of images etc.

Hence this tutorial. The *secret is* we first created a SD card using `Raspberry Pi Imager`.

Then inspected it to find out how on earth on a **Windows 10** a **Linux** image was getting modified.

Well to our surprise it was rather elegant.
The key is to modify the `cmdline.txt` = this determines how the Raspberry Pi boots.

By adding a **startup script** into the way the Raspberry Pi boots the `Raspberry Pi Imager` added all the necessary info.

And then ***like a virus*** erase both the **Startup Script** = `firstrun.sh` and `cmdline.txt` modification.

Very neat but can't hide from our curiosity.

Lets do this step by step and look at the requirements for typical Raspberry Pi installs:

1. Set the Host name of the Raspberry Pi.
2. Add **SSH credentials** to the __default User__ that comes with the **Linux Image**. This means we have a way to **enabling SSH** as well  as installing our **Key**. Apart from this all the basic SSH security modifications needs to  added as well.
3. Make sure of the User name and their password as selected
4. Setup the WiFi connection if needed.
5. Setup time-zone and Key maps
6. Finally make sure we log all the steps into a log file before exiting.

These are also kind of TODO till we validate all the steps are working correctly.

## Here is how the `cmdline.txt` would look like

Typically it would be like:

```sh
console=serial0,115200 console=tty1 root=PARTUUID=b1214a26-02 rootfstype=ext4 fsck.repair=yes rootwait quiet init=/usr/lib/raspberrypi-sys-mods/firstboot
```

You need to replace it with:

```sh
console=serial0,115200 console=tty1 root=PARTUUID=b1214a26-02 rootfstype=ext4 fsck.repair=yes rootwait quiet init=/usr/lib/raspberrypi-sys-mods/firstboot systemd.run=/boot/firstrun.sh systemd.run_success_action=reboot systemd.unit=kernel-command-line.target
```

Later as the `firstrun.sh` would fix this file.

Note: Need to find a way to fix the `root=PARTUUID=b1214a26-02` else things would fail.

## Here is how the `firstrun.sh` would look like

We would add the pieces in step by step.

### 0. Initial things in the script

Add the following at the beginning:

```sh
#!/bin/bash

set +e
# Disable Errors notification

LOGFILE="/boot/firstboot.log"

echo "" > $LOGFILE
echo "Script Started on " >> $LOGFILE

```

### 1. Add the Host name of the Raspberry Pi

We need to modify this in 2 steps:
1. Create the `hostname` file = System Name
2. Fix the `hosts` file = Network name and configuration

Add the following :

```sh
HOST_NAME="rpi"
CURRENT_HOSTNAME=`cat /etc/hostname | tr -d " \t\n\r"`
echo "$HOST_NAME" >/etc/hostname
sed -i "s/127.0.1.1.*$CURRENT_HOSTNAME/127.0.1.1\t$HOST_NAME/g" /etc/hosts
```

### 2. Add SSH Credentials and Enable the same

We would need to do the following:
1. Setup the `~/.ssh` directory in the current user's Home
2. Add the `SSH Public Key` to current user's Home in `~/.ssh/authorized_keys`
3. Disable password based SSH authentication in configuration
4. Enable `SSH` service on the Raspberry Pi

Add the following:
```sh
SSH_KEY="ssh-rsa ....== user@email"
FIRSTUSER=`getent passwd 1000 | cut -d: -f1`
FIRSTUSERHOME=`getent passwd 1000 | cut -d: -f6`
install -o "$FIRSTUSER" -m 700 -d "$FIRSTUSERHOME/.ssh"
install -o "$FIRSTUSER" -m 600 <(printf "$SSH_KEY") "$FIRSTUSERHOME/.ssh/authorized_keys"
echo 'PasswordAuthentication no' >>/etc/ssh/sshd_config
systemctl enable ssh
```

### 3. Create the User as per our Specification

We would need to do the following:
1. Add the **Hashed Password** to the current user
2. Check if the current username is correct else fix it.

First we need to generate the **Hashed Password**:
```sh
openssl passwd -5 -salt $(head -c18 /dev/urandom | openssl base64)
```
This would generate the hash using `SHA256` algorithm.
For even more secure we can use `SHA512` using:
```sh
openssl passwd -6 -salt $(head -c18 /dev/urandom | openssl base64)
```

Once you have your correct **Hashed Password** using the above method place in the following:
```sh
USER_NAME="ab"
HASHED_PASSWORD="$5$....."
echo "$FIRSTUSER:$HASHED_PASSWORD" | chpasswd -e
if [ "$FIRSTUSER" != "$USER_NAME" ]; then
      usermod -l "$USER_NAME" "$FIRSTUSER"
      usermod -m -d "/home/$USER_NAME" "$USER_NAME"
      groupmod -n "$USER_NAME" "$FIRSTUSER"
fi
```
Note: the `$FIRSTUSER` is from the previous step.

### 4. Setup WiFi Connection

Here we need to do the following:
1. Write the configuration for WiFi in the `/etc/wpa_supplicant/wpa_supplicant.conf` file
2. Enable the configuration

For WiFi again we would need **Hashed Password**
```sh
wpa_passphrase YOUR_SSID YOUR_PASSWORD
```
Note: This can only be done in a Linux machine with `wap_passphrase` installed.

You need add the following:
```sh
cat >/etc/wpa_supplicant/wpa_supplicant.conf <<'WPAEOF'
country=IN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
ap_scan=1

update_config=1
network={
	ssid="YOUR_SSID"
	psk=PSK_HASH_VALUE
}

WPAEOF
chmod 600 /etc/wpa_supplicant/wpa_supplicant.conf
rfkill unblock wifi
for filename in /var/lib/systemd/rfkill/*:wlan ; do
     echo 0 > $filename
done
```

### 5. Setup time-zone, Key-Maps and Locale

You need to add the following:
```sh
rm -f /etc/localtime
echo "Asia/Calcutta" >/etc/timezone
dpkg-reconfigure -f noninteractive tzdata

cat >/etc/default/keyboard <<'KBEOF'
XKBMODEL="pc105"
XKBLAYOUT="us"
XKBVARIANT=""
XKBOPTIONS=""

KBEOF
dpkg-reconfigure -f noninteractive keyboard-configuration

perl -pi -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
locale-gen en_US.UTF-8
update-locale en_US.UTF-8
```

### 6. Complete the Log file
```sh
echo "Done !" >> $LOG_FILE
```

### 7. Clean Up like a Virus

We can do the following:
1. Remove the `firstrun.sh`
2. Fix the `cmdline.txt` file so that it does not run the script again.

Add the following at the end of the `firstrun.sh` file:

```sh
rm -f /boot/firstrun.sh
sed -i 's| systemd.run.*||g' /boot/cmdline.txt
exit 0
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
