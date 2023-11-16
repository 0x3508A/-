# Nodejs on Raspberry Pi

This took a while to get ready but was worth the extra efforts.

## 1. Remove the Repository install Nodejs and add Build packages

```sh
# Remove old version
sudo apt remove nodejs
# Clean-up
sudo apt autoremove && sudo apt autoclean
```

Some development dependencies:

```sh
sudo apt install build-essential git curl
```

## 2. Install Keys

```sh
# Set the Keyring Name in the Shell
KEYRING=/usr/share/keyrings/nodesource.gpg
# Download the Keys to a Specific filename
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | \
 gpg --dearmor | sudo tee "$KEYRING" >/dev/null
```

### See if the Key is installed or not

```sh
gpg --no-default-keyring --keyring "$KEYRING" --list-keys
### This should show the Key ID 9FD3B784BC1C6FC31A8A0A1C1655A0AB68576280
```

## 3. Install Script for LTS Version

```sh
# Change to Root User as needed by the Script
sudo su
# Download an Run the Script
curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
```

This should be ready now.

## 4. Try out the

```sh
$ node -v
v16.13.2
```

## [`node-red` IoT Control Logic](./node-red.md)

This is the popular [IoT Visual programming](./node-red.md) interface.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)