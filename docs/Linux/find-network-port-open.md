# Network Ports open in Linux

This can be an handy tool to figure out which ports are open on your local Linux PC or Raspberry Pi.

## Using `netstat` command

```sh
sudo netstat -tulpn | grep LISTEN
```

#### Installing `netstat`

```sh
sudo apt install net-tools
```

Or for Arch:

```sh
sudo pacman -S net-tools
```

## Using `lsof` command

Alternatively using the [`lsof` command](./cli/lsof.md):

```sh
sudo lsof -i -P -n | grep LISTEN
```

#### Installing `lsof`

This is default on Arch Linux:

```sh
sudo pacman -S lsof
```

On Debian Variants:

```sh
sudo apt install lsof
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
