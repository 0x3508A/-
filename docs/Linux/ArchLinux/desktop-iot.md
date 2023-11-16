# Desktop IoT

This is example of converting a desktop **Arch Linux** into an *IoT* capable
server unit.

This follows the standard **ArchLinux** Installation as a *Command line only* OS.

## Installing Firewall and Mosquitto for MQTT

`sudo pacman -S mosquitto ufw`

This would install the `mosquitto` **MQTT Broker** and the `ufw` firewall.
**Note:** The `ufw` firewall runs on top of `iptables` package.

## Configuring Mosquitto MQTT Broker

### Edit the Configuration for MQTT

We would need to Edit the `/etc/mosquitto/mosquitto.conf` file:
```sh
sudo vim /etc/mosquitto/mosquitto.conf
```

Add add the Content:
```
listener 1883
allow_anonymous true
```

This would configure the `mosquitto` broker to listen on `0.0.0.0:1883`.
It would allow local as well as remote connections to be accepted by
the **broker**.

The last part would allow `anonymous` means *without password* entry.
Though this is **insecure** its good starting point.

### Restart the MQTT Broker

Finally restart the `mosquitto` **Broker**:

```sh
sudo systemctl restart mosquitto.service
```

### Stop the Auto-start of `mosquitto` broker

This is optional in case you wish to have better control:

```sh
sudo systemctl disable mosquitto.service
```

So when you need you can restart it by:

```sh
sudo systemctl start mosquitto.service
```

## Configure the Firewall

```sh
sudo ufw limit 22/tcp comment "SSH"  # Remote session
sudo ufw allow 1883 comment "MQTT-Open" # non-Encrypted connection
sudo ufw limit 1880 comment "Node-Red"
sudo ufw allow 5353 comment "avahi" # avahi Discovery port
sudo ufw deny to 224.0.0.1 comment "block multicast packets" # Multi-cast Disable
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable
sudo systemctl enable --now ufw
```

This would open `1883` port for the **MQTT Broker** to accept outside connection.
It would also enable the `ufw` firewall to start every time the PC rebooted.

## Install and Configure node-red

### First Install Nodejs

```sh
sudo pacman -S nodejs
```

### Installing Node-Red on Arch Linux

We would need to install it from **AUR**.

<https://aur.archlinux.org/packages/nodejs-node-red>

If you have `pamac` or any other `AUR` helper tool you can directly install.

Else follow the `git` technique.

```sh
cd /tmp
git clone https://aur.archlinux.org/nodejs-node-red
cd nodejs-node-red
makepkg -si
cd ..
rm -rf nodejs-node-red
cd
```

### Starting Node-Red as a service

There are 2 options to using the `Node-Red`:

1.  Use as a *System Service* via `systemd`
2.  Use as user process in terminal for local work

In our case we would like to use the First one.

Since, it needs to be system wide and run as a daemon.

```sh
sudo systemctl start nodejs-node-red
```

This would enable **Node-Red** and it should be available at:

<http://localhost:1880>

The port `1880` is the default port for **Node-Red**.

### Generate Password for Node-Red Admin

```sh
sudo su
node-red admin hash-pw | tee -a /var/lib/nodejs-node-red/.node-red/settings.js
```

This adds the password to the end settings file.
You can add multiple users using the same above command.

**Settings File:** `/var/lib/nodejs-node-red/.node-red/settings.js`

This is location **Node-Red** settings and other files, when installed as as *Service*.

### Enabling Password for Node-Red Admin

Since the `node-red admin hash-pw` only adds the Hashes of password, you would
need to format them into correct `json` settings.

```sh
sudo su
vim /var/lib/nodejs-node-red/.node-red/settings.js
```

Add the following content format:
```json
    adminAuth: {
      type: "credentials",
      users: [
        {
            //username: "admin",
            //password: "$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN.",
            username: "node2520",
            password: "$2b$08$Qh8fe1om5/Mix/vNVeZqUeA8sg2fEn53vA7rrbR6IGQ2.J7i576jW",
            permissions: "*"
        }
      ]
    }
```

In the above value we have also change the *Administrator* user name.
This provides additional security but not mandatory.

### Finally Restart the Node-Red Service

```sh
sudo systemctl restart nodejs-node-red
```

----
<!-- Footer Begins Here -->
## Links

- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
