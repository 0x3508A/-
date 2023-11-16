# Nix Learning on Manjaro

It had been a difficult initially to get some way to test out [Nix Package Manager](https://nixos.org/download#nix-install-linux).
With help and guidance from Rajiv , I finally found a way around.

## New Method 2023 with Nix Flakes support

### **NOT CORRECT** following the Single User Nix install process

!!! warning "Incorrect way as per Rajiv"

<https://nixos.org/download>

Single-user installation :

<https://nixos.org/manual/nix/stable/installation/single-user>

```sh
sh <(curl -L https://nixos.org/nix/install) --no-daemon
```
Got:
```
Installation finished!  To ensure that the necessary environment
variables are set, either log in again, or type

. /home/user/.nix-profile/etc/profile.d/nix.sh

in your shell.
```

```sh
nix-channel --list
nixpkgs https://nixos.org/channels/nixpkgs-unstable
```
### Modded Rajiv's Nix bootstrap with new Nix Install Process

His original work:

<https://github.com/rajivr/bootstrap-minimal-nix>

More reference to that *[below](#tutorial-by-rajiv-on-getting-started-with-nix-environment)*.

It was concluded that the mistake in process shown in the NixOS website was:

Need to Use `--no-daemon --no-channel-add --no-modify-profile` instead of just `--no-daemon`.

Instead we would like to directly install `nix-2.18.1`:

<https://releases.nixos.org/?prefix=nix/nix-2.18.1/>

Here is how we go about it:

1. Getting Nix for Manjaro:

    ```sh
    mkdir /tmp/nix-install
    cd /tmp/nix-install
    curl -LO https://releases.nixos.org/nix/nix-2.18.1/nix-2.18.1-x86_64-linux.tar.xz
    tar xvf nix-2.18.1-x86_64-linux.tar.xz
    ```

2. Installation of Nix in Manjaro:

    ```sh
    cd /tmp/nix-install
    cd nix-2.18.1-x86_64-linux
    ./install --no-daemon --no-channel-add --no-modify-profile
    ```

    Output of the Installation Process:

    ```sh
    installing 'nix-2.18.1'
    building '/nix/store/hzx954rnda4k348rb4wvqbdzp9z0ynjy-user-environment.drv'...

    Installation finished!  To ensure that the necessary environment
    variables are set, please add the line

    . /home/user/.nix-profile/etc/profile.d/nix.sh

    to your shell profile (e.g. ~/.profile).

    ```

3. Cleanup after install is completed:

    ```sh
    cd ../..
    rm -rf nix-install
    ```

4. Next Enable flakes Feature:

    ```sh
    mkdir $HOME/.config/nix
    echo "experimental-features = nix-command flakes" >> $HOME/.config/nix/nix.conf
    ```

5. Now after Reboot activate the Environment:

    ```sh
    . /home/user/.nix-profile/etc/profile.d/nix.sh
    ```

Easy!! you now have Nix without channels and supporting the **Flakes** feature.

### Some Packages to Test with Nix

#### Moserial

<https://flathub.org/apps/org.gnome.moserial>

<https://wiki.gnome.org/action/show/Apps/Moserial>

#### Arduino Legacy v1.8.19

<https://www.arduino.cc/en/software#legacy-ide-18x>

## Old Method Pre-2022 for getting Nix on Manjaro

### 1. Create a User for Nix Trials on Manjaro

Now we chose to name this user `nix-user` keeping the nomenclature.

Use the **`Manjaro Settings Manager`** to create a **Standard user** with a set password.

### 2. Create the `/nix` directory and permissions

Since, the new user `nix-user` does not have any sudo permissions.
We need to create the root directories for them.

```sh
sudo mkdir /nix
```

Now initially it would have `root` only permissions.
We need to assign it to `nix-user` for our testing.

```sh
sudo chown -R nix-user /nix
```

Now we are ready to Jump in to our new user.

### Script for Automation

```sh
#!/bin/sh

mkdir /tmp/nix-install
cd /tmp/nix-install
curl -LO https://hydra.nixos.org/build/128347132/download/1/nix-3.0pre20201007_5257a25-x86_64-linux.tar.xz

tar xvf nix-3.0pre20201007_5257a25-x86_64-linux.tar.xz

cd nix-3.0pre20201007_5257a25-x86_64-linux/
./install --no-daemon --no-channel-add --no-modify-profile
cd ..
rm -rf nix*

cd


mkdir $HOME/.config/nix
echo "experimental-features = nix-command flakes" >> $HOME/.config/nix/nix.conf

curl -LO https://github.com/NixOS/nixpkgs/archive/refs/tags/21.05.tar.gz
git clone --depth 1 --branch release-21.05 https://github.com/nix-community/home-manager.git

tar -xf 21.05.tar.gz

echo "source $HOME/.nix-profile/etc/profile.d/nix.sh" >> ~/.bashrc
echo "export NIX_PATH=\"nixpkgs=${HOME}/nixpkgs-21.05:home-manager=${HOME}/home-manager\"" >> ~/.bashrc

echo
echo
echo " Make sure to Log-out and Login again after installation "
echo
```

Make sure to follow the Login - Logout cycle after this so that the new config gets activated.

### Install `home-manager`

Before running this make sure to Logout.
Also run this command in a TTY not a terminal application.
`Ctrl+Alt+F2...` .etc.

```sh
nix-shell '<home-manager>' -A install
```

----

## Tutorial by Rajiv on Getting started with Nix environment

<https://github.com/rajivr/bootstrap-minimal-nix>

Its a Hosted page so will link to it directly.

[Bootstrap Minimal Nix (and Nix Flakes)](../../Lang/Nix/bootstrap-minimal-nix-and-nix-flakes.md)

In order to Enter the User just use:

```sh
su nix-user
```

After typing the password you can switch to the `nix-user`

Also don't for get to set the environment.

## Creating Local `nixpkgs`

The Problem with #Rajiv 's tutorial is that its more local.
Hence we don't necessary have access to `nixpkgs`

In [`nix-pills`](https://nixos.org/guides/nix-pills/our-first-derivation.html) following *chapter 6*, we would get into trouble without this.

### Get the Current Version of `nixpkgs`

Get the Current Version of the Nix Channel build for `nixos-21.05`:

```sh
curl -Ls -H "Accept: application/vnd.github.v3.sha" "https://api.github.com/repos/NixOS/nixpkgs/commits/nixos-21.05"
```

Here the last part at `.../commits/nixos-21.05` is the place we need to add
**future** versions if needed.

This command would give the following output:
```
63ee5cd99a2e193d5e4c879feb9683ddec23fa03
```

This is the commit hash. Hence our download path becomes:
`https://github.com/NixOS/nixpkgs/archive/63ee5cd99a2e193d5e4c879feb9683ddec23fa03.tar.gz`

### Get the SHA256 Hash required by `nix-build`

To get the SH256 Hash Correctly:

```sh
nix-prefetch-url --unpack --print-path https://github.com/NixOS/nixpkgs/archive/63ee5cd99a2e193d5e4c879feb9683ddec23fa03.tar.gz
```

It would give the output
```
unpacking...
0avbsx5chbwr0y55shndkzf0ixx3bznbzq526p5nj8llryxa10af
/nix/store/k48nwi5676v70la1by8vhqb7p8551c9d-63ee5cd99a2e193d5e4c879feb9683ddec23fa03.tar.gz
```

The second line is the SHA256 Hash needed by nix build.

We might optionally **cleanup** the `nix-store` before we proceed:

```sh
nix-store --gc
```

### Create the `nixpkgs.nix` for System build

Now in order to have a location for `nixpkgs` we need to first create a `.nix` for `nix-build`.

```sh
cd ~
nano nixpkgs.nix
```

Note that this would be in our `nix-user` `$HOME` directory.

Here is whats should be written to `nixpkgs.nix`:

```nix
let
  nixpkgs = fetchTarball {
    url = "https://github.com/NixOS/nixpkgs/archive/63ee5cd99a2e193d5e4c879feb9683ddec23fa03.tar.gz";
    sha256 = "0avbsx5chbwr0y55shndkzf0ixx3bznbzq526p5nj8llryxa10af";
  };
  pkgs = import nixpkgs {};
in
  pkgs.runCommandNoCC "nixpkgs-stable" {} ''
    ln -s ${nixpkgs} $out
  ''
```

Note the *URL Path* and *SHA256 Hash* in the content. This would change with revisions.

Next we need to create a script to perform the `nix-build`.

This script would be called `sync.sh`:

```sh
#!/usr/bin/env bash

set -e
set -u
PS4=" $ "

unset NIX_PATH

set -x

nix-build "./nixpkgs.nix" --out-link "nixpkgs"
```

Make sure the get the `sync.sh` as *Executable*.
And run the same.

### Update `NIX_PATH` variable

We would need to edit our `.bashrc` to add this variable.

Add the line at the end of `.bashrc`:

```sh
source ~/.nix-profile/etc/profile.d/nix.sh

export NIX_PATH="nixpkgs=$HOME/nixpkgs"
```

These would always keep the `nix-user` ready in the `nix` space.


----
<!-- Footer Begins Here -->
## Links

- [Back to Nix Language Hub](../../Lang/Nix/README.md)
- [Back to NixOS Article](../Distro/nixos.md)
- [Back to Nix Package Manger](../cli/nix.md)
- [Back to ArchLinux Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
