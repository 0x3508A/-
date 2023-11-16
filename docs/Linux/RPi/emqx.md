# EMQX - MQTT Broker

Homepage : <https://www.emqx.io/>

Video Tutorial : <https://www.youtube.com/watch?v=8azAVBCU-To>

Documentation of Steps to Install : <https://www.emqx.io/docs/en/v5.0/deploy/install-debian.html#debian>

They also offer **FREE MQTT broker service!!!**

!!! warning "64-bit Raspberry Pi OS"
    **IMPORTANT: This broker need 64-bit Raspberry Pi OS and Compatible Hardware!!!**

## Installation

### Download the EMQX repository

```sh
curl -s https://assets.emqx.com/scripts/install-emqx-deb.sh | sudo bash
```

!!! bug "Chinese software"
    **IMPORTANT WARNING : Chinese software directly running on your Internet connect hardware - what can go wrong !!!**

### Install EMQX

```sh
sudo apt-get install emqx
```

### Disable Telemetry

#### Using Startup

Disable telemetry in this boot via environment variables at startup:

`export EMQX_TELEMETRY__ENABLE=false`

Set this in your `.bashrc` or `.profile`.

#### Using Configuration file

The configuration for EMQX is stored in `/etc/emqx/emqx.conf`.

We can edit this:

```sh
sudo nano /etc/emqx/emqx.conf
```

Add this at beginning of the file after the comments:

```sh
telemetry.enable = false
```

### Start EMQX

```sh
sudo systemctl start emqx
```

### Start and Enable EMQX at startup

```sh
sudo systemctl enable --now emqx
```

## Dashboard

### URL location

Open the Dashboard for the Raspberry Pi EMQX installation through port `18083`.
Not sure why this special port.

Navigate to `http://your-rasperryPi.local:18083`

```
Yes the Dashboard Port is 18083
MQTT Ports : 1883 , 8883 (Secure)
Others : 8083 , 8084
```

### Default Password and Login

```
Default Login : admin
Default Password : public
```

You would be prompted to change this at the first login.

### Setup Authentication for MQTT Clients

Click on Authentication icon in the Dashboard.

Set the type to be `Password` based and should use `Internal Databse`.
It should be of type `Username` and Hashing set to `bcrypt` with `10` rounds.

Now the Database for storing Password authentication information would be created.

Click on Users and then `+` button to add users for the MQTT connection.

**Note** The `admin` user we assigned password [earlier](#default-password-and-login) was only for this Dashboard and not for MQTT.

### Reset Password for Dashboard

You can reset your Dashboard login password via the `admins` command.

```sh
./bin/emqx ctl admins passwd <Username> <Password>
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
