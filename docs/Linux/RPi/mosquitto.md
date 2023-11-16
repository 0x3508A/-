# `mosquitto` - MQTT Broker

## Raspberry Pi Installation and Configuration of `mosquitto`

Source: <https://mosquitto.org/blog/2013/01/mosquitto-debian-repository/>

References :

- <https://mpolinowski.github.io/docs/Development/Javascript/2021-06-02--mqtt-cheat-sheet/2021-06-02/>
    - **[PDF Copy of the Article](./mosquitto/Mosquitto-MQTT-Cheat-Sheet-Mike-Polinowski.pdf)**
- <https://blog.jaimyn.dev/mqtt-use-acls-multiple-user-accounts/>
    - **[PDF Copy of the Article](./mosquitto/MQTT-How-to-use-ACLs-and-multiple-user-accounts-Jaimyns-Blog.pdf)**
- ACL Rules <https://mosquitto.org/man/mosquitto-conf-5.html#idm44>
- Dynamic Security Plugin <https://mosquitto.org/documentation/dynamic-security/>

`mosquitto` - is the most common MQTT server / Broker for Raspberry Pi.

Tutorials:

- Introduction to MQTT

    <https://www.baldengineer.com/mqtt-introduction.html>

- MQTT Tutorial for Raspberry Pi, Arduino, and ESP8266

    <https://www.baldengineer.com/mqtt-tutorial.html>

- Digital Ocean Setting up MQTT on Ubuntu

    <https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-the-mosquitto-mqtt-messaging-broker-on-ubuntu-16-04>

- Paho Go - MQTT Client Library Encyclopedia

    <https://www.hivemq.com/blog/mqtt-client-library-encyclopedia-golang/>

- MQTT Mosquitto broker – Client Authentication and Client Certificates

    <https://primalcortex.wordpress.com/2016/11/08/mqtt-mosquitto-broker-client-authentication-and-client-certificates/>

- Easily Create CA Certificates

    <https://github.com/fcgdam/easy-ca>


Access Control List and Plugins:

-  Authorization - MQTT Security Fundamentals

    <https://www.hivemq.com/blog/mqtt-security-fundamentals-authorization/>

- `mosquitto.conf`- man page

    <https://mosquitto.org/man/mosquitto-conf-5.html>

- Plugins
    - <https://github.com/jpmens/mosquitto-auth-plug>
    - <https://github.com/hadleyrich/mosquitto-auth-plugin-http>
    - <https://github.com/mbachry/mosquitto_pyauth>



## Installation of `mosquitto`

```sh
sudo apt update
sudo apt install mosquitto
```

## Configuration of `mosquitto`

### 1. Edit configuration File

We would need to edit the `/etc/mosquitto/mosquitto.conf` file.

```sh
sudo nano /etc/mosquitto/mosquitto.conf
```

Modify the following line:
```sh
persistence false
```

This would disable Storage of messages and unnecessary use of SD card on Raspberry Pi.

Add these lines at the end:

```sh
# This line is Enough for default - We can change the default MQTT port here !!
listener 1883
allow_anonymous true # Ommit this when using password file.

# These 2 lines for add Password File based Security
allow_anonymous false
password_file /etc/mosquitto/pw

# This line would add the ACL control file
acl_file /etc/mosquitto/acl_file
```

This would disable anonymous login completely. In **`mosquitto` 2.0** by default its true.

The next line `password_file` enables user authentication.

The last line `acl_file` defines the **ACL** which will later be enabled in **Dynamic Authentication plugin**. Comment out this line if you only have a blank file. Since, all publications would be blocked if this `acl_file` specified is empty.

### 2. Create the Password file and Add Users

References :

- <https://mpolinowski.github.io/docs/Development/Javascript/2021-06-02--mqtt-cheat-sheet/2021-06-02/>
   - [PDF Copy of the Article](./mosquitto/Mosquitto-MQTT-Cheat-Sheet-Mike-Polinowski.pdf)
- <https://blog.jaimyn.dev/mqtt-use-acls-multiple-user-accounts/>
   - [PDF Copy of the Article](./mosquitto/MQTT-How-to-use-ACLs-and-multiple-user-accounts-Jaimyns-Blog.pdf)

#### Create the Password file

```sh
sudo mosquitto_passwd -c /etc/mosquitto/pw <UserName>
```

This command would both create the `/etc/mosquitto/pw` in `-c` and add hashed password for `<UserName>`. Note that it would **Overwrite** the older file, so use it **Carefully**.
**IMPORTANT** This only needs to be used once for creating the password file `/etc/mosquitto/pw`

#### Add Users into the Password File

```sh
sudo mosquitto_passwd /etc/mosquitto/pw <UserName>
```

This can be used multiple times.

#### Deleting Users from Password file

```sh
mosquitto_passwd -D /etc/mosquitto/pw <UserName>
```

### 3. Create am empty ACL file

References :

- ACL Rules <https://mosquitto.org/man/mosquitto-conf-5.html#idm44>
- Dynamic Security Plugin <https://mosquitto.org/documentation/dynamic-security/>

#### Create the Empty File

```sh
sudo touch /etc/mosquitto/acl_file
```

#### Add some Rules into the file

Here is an example of the sample `/etc/mosquitto/acl_file` :

```sh
# This affects access control for clients with no username.
topic read $SYS/#

# This only affects clients with username "roger".
user roger
topic foo/bar

# This affects all clients.
pattern write $SYS/broker/connection/%c/state
```

We might need to add further data such as :

```sh
user admin
topic readwrite cameras/#
topic read $SYS/broker/#
topic write hello/world
```

We are using `#` globing but we can also specify a particular topic.
Or provide a multi selector like `stat/+/POWER`.

### 4. Allow MQTT Port on Firewall (OPTIONAL)

```sh
# Open the Port on Firewall for MQTT
sudo ufw allow 1883
```

Change the port in case you have customized it in the `/etc/mosquitto/mosquitto.conf`.

### 5. Restart the `mosquitto` MQTT service

```sh
sudo systemctl restart mosquitto.service
```

### 6. Check status of `mosquitto` MQTT service

```sh
sudo systemctl status mosquitto.service
```

## `mosquito-clients` Tools for the MQTT

Here is an easy to understand example:

```sh
mosquitto_sub -h localhost -t /hello/world
mosquitto_pub -h localhost -t /hello/world -m 'welcome!'
```

In case we have user's defined:

```sh
mosquitto_sub -h localhost -t /hello/world -p 1885 -u admin -P instar
mosquitto_pub -h localhost -t /hello/world -m 'welcome!' -p 1885
```

Here `-p` is used to customize the MQTT port.
The `-P` option specifies the Password.

### `mosquitto_pub` Flags

```
-r : Sets retain flag
-n : Sends Null message useful for clearing retain message.
-p : Set Port number Default is 1883
-u : Provide a username
-P : Provide a password
-i : Provide client name
-I : Provide a client id prefix- Used when testing client restrictions using prefix security.
-q : QoS Specify the quality of service to use for the message, from 0, 1 and 2. Defaults to 0
```

### `mosquitto_sub` Flags

```
-p : Set Port number Default is 1883
-u : Provide a username
-P : Provide a password
-i : Provide client name
-I : Provide a client id prefix- Used when testing client restrictions using prefix security
```

### Debugging the Communications

We would use the `-d` flag

```sh
mosquitto_pub -h localhost -t /hello/world -m 'welcome!' -d

Client (null) sending CONNECT
Client (null) received CONNACK (0)
Client (null) sending PUBLISH (d0, q0, r0, m1, '/hello/world', ... (8 bytes))
Client (null) sending DISCONNECT
```

```sh
mosquitto_sub -h localhost -t /hello/world -d

Client (null) sending CONNECT
Client (null) received CONNACK (0)
Client (null) sending SUBSCRIBE (Mid: 1, Topic: /hello/world, QoS: 0, Options: 0x00)
Client (null) received SUBACK
Subscribed (mid: 1): 0
Client (null) received PUBLISH (d0, q0, r0, m0, '/hello/world', ... (8 bytes))
welcome!
```

For client ID add the option `-i 'myClientID'`.

## ACL File Format

Reference:

- <https://mosquitto.org/man/mosquitto-conf-5.html#idm44>
- Simple tips for the ACL File <http://www.steves-internet-guide.com/topic-restriction-mosquitto-configuration/>

Topic access is added with lines of the format:

    topic [read|write|readwrite|deny] <topic>

The access type is controlled using `read`, `write`, `readwrite` or `deny`.

This parameter is optional (unless `<topic>` includes a space character) - if not given then the access is read/write. `<topic>` can contain the `+` or `#` wildcards as in subscriptions.

The `deny` option can used to explicitly deny access to a topic that would otherwise be granted by a broader `read`/`write`/`readwrite` statement.

Any `deny` topics are handled before topics that grant `read`/`write` access.

The first set of topics are applied to anonymous clients, assuming `allow_anonymous` is `true`.

User specific topic ACLs are added after a user line as follows:

```
user <username>
topic [read|write|readwrite|deny] <topic>
```

The username referred to here is the same as in `password_file`. It is not the `clientid`.
We can any number of `topic` to given `user` and separate multiple users using a blank line.

It is also possible to define ACLs based on pattern substitution within the topic. The form is the same as for the topic keyword, but using pattern as the keyword.

    pattern [read|write|readwrite|deny] <topic>

The patterns available for substitution are:

    %c to match the client id of the client

    %u to match the username of the client

The substitution pattern must be the only text for that level of hierarchy.

Pattern ACLs apply to all users even if the `user` keyword has previously been given.

### Example

    pattern write sensor/%u/data

Allow access for bridge connection messages:

    pattern write $SYS/broker/connection/%c/state

If the first character of a line of the ACL file is a `#` it is treated as a comment.

If `per_listener_settings` is `true`, this option applies to the current listener being configured only.

If `per_listener_settings` is `false`, this option applies to all listeners.

Reloaded on reload signal.

The currently loaded ACLs will be freed and reloaded.

Existing subscriptions will be affected after the reload.

## Dynamic Security Plugin

Dynamic Security Plugin <https://mosquitto.org/documentation/dynamic-security/>

Youtube Video: <https://www.youtube.com/watch?v=QvRBtRH2mN0>

### 1. Installation in Raspberry Pi

Documented: <https://mosquitto.org/documentation/dynamic-security/#installation>

#### 1.a. Edit the Configuration File

Edit the main `mosquitto` configuration :

```sh
sudo nano /etc/mosquitto/mosquitto.conf
```

Add these lines just under the comments at the top of the file:

```sh
listener 1883
allow_anonymous false
per_listener_settings false

plugin /usr/lib/aarch64-linux-gnu/mosquitto_dynamic_security.so
plugin_opt_config_file /var/lib/mosquitto/dynamic-security.json

```

!!! note "Important"
    While using **Dynamic Security** remove `password_file` and `acl_file` options.
S   ince, both would be dealt with the **Dynamic Security** plugin.

These above likes would configure the plugin to load with `mosquitto`
and also the location of **Dynamic Security Plugin** data.

#### 1.b. Special for Linux PC

!!! note
    For a normal Linux PC we would need a different plugin line:

`plugin /usr/lib/x86_64-linux-gnu/mosquitto_dynamic_security.s`

This is detailed in <https://mosquitto.org/documentation/dynamic-security/#installation>

#### 1.c. Dynamic Security Plugin Storage

All settings for the plugin are stored in `/var/lib/mosquitto/dynamic-security.json`.

This is not specifically in the `/etc` since it is changed at runtime.

#### 1.d. Restart the `mosquitto` service

```sh
sudo systemctl restart mosquitto.service
```

### 2. Configuration

#### 2.a. Generate the initial Configuration

The `dynamic-security.json` file is where the plugin configuration will be stored.

This file will be updated each time you make `client`/`group`/`role` changes, during normal operation the configuration stays in memory.

To generate an initial file, use the `mosquitto_ctrl` utility.

```sh
mosquitto_ctrl dynsec init path/to/dynamic-security.json <admin-username>
```

Choose `<admin-username>` name that can be used to configure the **Dynamic Security Plugin**.

This is the **Admin User** who can

- Create and Assign **Roles**
- Create and Manage **Users** and **Clients**
- Manage **Rules** and **Access Rights**

This command would ask for password of the **Admin User**.
Make sure to set this carefully.

#### 2.b. Change User Permissions

```sh
sudo chown mosquitto:mosquitto /var/lib/mosquitto/dynamic-security.json
```

This would transfer ownership of the configuration file to `mosquitto` user.
It would make this easier to configure things and not cause issues.

Finally, make the file writable for other users in the group:

```sh
sudo chmod 664 /var/lib/mosquitto/dynamic-security.json
```

#### 2.c. Restart the Service

Now we would need to [restart the `mosquitto` service](#5-restart-the-mosquitto-mqtt-service) again for these to take effect.

## Clear "Retained" Messages

Many times there are needs where we publish *retained* message. These messages remain on Broker. These are delivered when ever a subscriber connects.

Here are the Steps:

1. List out all the Topics that have retained messages. This should not be the wild card but the exact topics.
2. Send **Null** or **Zero length** messages to these topics.

!!! note
    **A blank message** from any MQTT client should suffice to *remov*e the particular topic **retained** message.

Typically using `mosquito` in Linux:

```sh
mosquitto_pub -h hostname -t the/topic -n -r -d
```

Add any authentication parameters that might be needed in the above command.

```
-n = Send a null (zero length) message
-r = Retain the message as a “last known good” value on the broker
-d = Enable debug messages
```

## Securing MQTT communication Bridges with Outside worlds

Reference:

<https://www.justinribeiro.com/chronicle/2012/11/08/securing-mqtt-communication-between-ardruino-and-mosquitto/?static=true>

Securing communications between my Arduino's and the outside world using MQTT and `Mosquitto`.

### Building the Bridge at Home

We would need to read [documentation](http://mosquitto.org/man/mosquitto-conf-5.html) on `mosquitto.conf` file.

Here are the specific modifications to `mosquitto.conf`:

```bash
#... previous things

address ec2-somethingsomething.us-west-1.compute.amazonaws.com:8883
connection BridgeIt

topic devices/# out "" home
topic cmds/# in remote ""

username SomeUser
password SomePassword

bridge_cafile /etc/keys/myCA.crt
bridge_certfile /etc/keys/myServer.crt
bridge_keyfile /etc/keys/myServer.key

#... other things
```

Breaking down the `config`,

- we’re securing the **bridge connection** for using SSL (to see how to generate the certs and keys, have a look at the `mosquitto-tls` [documentation](http://mosquitto.org/man/mosquitto-tls-7.html))
- we’re using a `username/password`
- we’re defining which **topics** we’re sending out and listening for.

### Locking Down the Remote Side

This the **AWS** instance that we would need to process.

```bash
#... previous things

acl_file /etc/mosquitto/accesscontrols
allow_anonymous false
password_file /etc/mosquitto/users

bind_address 127.0.0.1
port 8883

cafile /etc/keys/ca.crt
certfile /etc/keys/server.crt
keyfile /etc/keys/server.key

#... other things
```

To create the password file, you can use [`mosquitto_passwd`](http://mosquitto.org/man/mosquitto_passwd-1.html).

The [access control file](#acl-file-format) will differ based on your information design.

## Old Installation

### ~~1. Install GPG Key~~ -- No Longer Needed

```sh
# Download
wget http://repo.mosquitto.org/debian/mosquitto-repo.gpg.key
# Add the Key to the APT
sudo apt-key add mosquitto-repo.gpg.key
# Remove the Key from Disk
rm -rf mosquitto-repo.gpg.key
```

Its part of the Main Repo.

### ~~2. Add Repository~~ -- No Longer Needed

```sh
# Go to the Sources directory
cd /etc/apt/sources.list.d/
# Download the PPA
sudo wget http://repo.mosquitto.org/debian/mosquitto-bullseye.list
```

For older `buster` use:
```sh
sudo wget http://repo.mosquitto.org/debian/mosquitto-buster.list
```

For older `jessie` use:
```sh
sudo wget http://repo.mosquitto.org/debian/mosquitto-jessie.list
```

For older `stretch` use:
```sh
sudo wget http://repo.mosquitto.org/debian/mosquitto-stretch.list
```

Finding the **Debian** Version:
```sh
lsb_release -a
...
Codename:	buster
...
```

### 3. Update and Install

```sh
sudo apt update
sudo apt install mosquitto
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
