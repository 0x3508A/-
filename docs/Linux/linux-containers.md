# `lxc` , `lxd` - Linux Containers

- Important Video Reference: <https://www.youtube.com/watch?v=aIwgPKkVj8s>
    - Blog Post for the video: <https://www.learnlinux.tv/getting-started-with-lxd-containerization/>

- Arch Linux Wiki Article: <https://wiki.archlinux.org/title/LXD>

## Installation

```sh
sudo pacman -Syu lxd
```

For Ubuntu snap is a better way:

```sh
sudo apt install snapd
sudo snap install lxd
```

### Adding User to the `lxd` Group

```sh
sudo usermod -aG lxd $USER
```

### Initialize the Container daemon `lxd`

```sh
sudo lxd init
```

This would ask us to *Enable Clustering* - need to **decline** this.

## Definitions

`lxc` - Primary command for working with **Linux Containers**.

`lxd` - Demon running the **Linux Containers** seldom used.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
