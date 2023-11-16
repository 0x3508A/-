# Bootstrap Minimal Nix (and Nix Flakes)

This is the replication for [original article](https://github.com/rajivr/bootstrap-minimal-nix/blob/master/README.org).

This repository provides instructions for getting started with [Nix][1] on your existing Linux OS.

There are multiple ways to installing Nix.

In this repository, we document an approach that involves two specific aspects.

1. Using Nix Flakes in favor of Nix Channels
2. Installing Nix in single-user mode

Since Nix Flakes is still experimental, we need to get the installation tarball directly from Nix CI system ([Hydra][2]).

The instructions below uses evaluation [1618056][3].

1. Download and extract binary tarball

    ```sh
    mkdir /tmp/nix-install
    cd /tmp/nix-install

    curl -LO https://hydra.nixos.org/build/128347132/download/1/nix-3.0pre20201007_5257a25-x86_64-linux.tar.xz

    tar xvf nix-3.0pre20201007_5257a25-x86_64-linux.tar.xz
    ```

2. Install Nix
   We install Nix with the following options: `--no-daemon`, `--no-channel-add`, and `--no=modify-profile`.
   ```sh
   cd nix-3.0pre20201007_5257a25-x86_64-linux/

   ./install --no-daemon --no-channel-add --no-modify-profile
   ```
   Note: The User would *need permission to write* to `/nix` directory.

*Nix installation is now done*, but we still need to enable flakes support in Nix’s configuration file `nix.conf`.

By default Nix looks for its configuration in `/etc/nix/nix.conf`. Because we are doing a single-user installation, this file will not be present. Instead, we create a file `$HOME/.config/nix/nix.conf`, where we enable flake configuration by doing the following.

```sh
mkdir $HOME/.config/nix

echo "experimental-features = nix-command flakes" >> $HOME/.config/nix/nix.conf
```
With this, our **Nix setup is complete**.

## Environment Configuration

Even though Nix is installed and setup, *it is still completely isolated from our Linux OS environment*.

We can pull Nix into our environment by doing:

```sh
source $HOME/.nix-profile/etc/profile.d/nix.sh
```

With this, you are all set to start enjoying Nix and Nix Flakes goodness!

## Uninstalling and Upgrading Nix

Reproducibility is one of the key selling points of Nix. In your Nix learning journey, there will come a point where you would want to start from scratch.

If you installed Nix by following the above instruction, then you can uninstall your Nix setup by simply removing these files and directories.

- `/nix/`
- `$HOME/.config/nix/`
- `$HOME/.nix-defexpr/`
- `$HOME/.nix-profile`


  [1]:https://nixos.org/
  [2]:https://hydra.nixos.org/jobset/nix/master
  [3]:https://hydra.nixos.org/eval/1618056


----
<!-- Footer Begins Here -->
## Links

- [Back to Nix Language Hub](./README.md)
- [Back to Nix Manjaro Installation Article](../../Linux/ArchLinux/Manjaro-nix.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
