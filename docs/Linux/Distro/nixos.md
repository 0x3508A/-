# NixOS

## Videos & Some Snippets - 2023-October run

- You Should Use Flakes Right Away in NixOS!

    <https://www.youtube.com/watch?v=ACybVzRvDhs>

- Vimjoyer - Easy to Understand NixOS Videos

    - Channel <https://www.youtube.com/@vimjoyer>
    - Nix tutorials Playlist

        <https://www.youtube.com/playlist?list=PLko9chwSoP-15ZtZxu64k_CuTzXrFpxPE>

    - Nix flakes explained

        <https://www.youtube.com/watch?v=S3VBi6kHw5c&list=PLko9chwSoP-15ZtZxu64k_CuTzXrFpxPE&index=2>

- Enable Nix Flakes <https://nixos.wiki/wiki/Flakes>

    This page has some good base details about how to begin using `nix --flake` in commands.

    - Enable Nix Flakes from the `nix.conf` file add the Line:

        ```sh
        # In ~/.config/nix/nix.conf or /etc/nix/nix.conf config File
        experimental-features = nix-command flakes
        ```

    - Enable Nix Flakes from within `configuration.nix` file add the Line:

        ```nix
        nix.settings.experimental-features = [ "nix-command" "flakes" ];
        ```

    - Finally also with `home-manager` add this:

        ```nix
        nix = {
          package = pkgs.nix;
          settings.experimental-features = [ "nix-command" "flakes" ];
        };
        ```
- Good NixOS Flakes + Confighuration Resource <https://mynixos.com/>
- Some NixOS Configurations with Feature Comparison

    <https://nixos.wiki/wiki/Comparison_of_NixOS_setups>

- Full Collection of NixOS Configurations

    <https://nixos.wiki/wiki/Configuration_Collection>

- Finding Old Nix Packages or Nix Package Versions the correct way

    <https://www.nixhub.io/>

------

## References

- Manual <https://nixos.org/manual/nixos/stable/>
- Unstable Version Manual <https://nixos.org/manual/nixos/unstable/>
- Download <https://nixos.org/download.html>
- Forum for Help <https://discourse.nixos.org/>
- Sample Configuration in *Enc 72*
    - [`configuration.nix` - System Wide Configuration at `nixos/system/configuration.nix`](./nixos/nixos-72.7z)
    - [`home.nix` - Home Manager Configuration at `nixos/home/home.nix` ](./nixos/nixos-72.7z)

- NixOS Videos
    - NixOS Setup Guide - Configuration / Home-Manager / Flakes

        <https://www.youtube.com/watch?v=AGVXJ-TIv3Y>

    - Reproducible NixOS installation w/ flakes

        <https://www.youtube.com/watch?v=_j8kDS6GLJs&list=PLZmotIJq3yOKew30oT8aEbPUOEKBmNpY1>

- `nixpkgs` Repository
    - Main <https://github.com/NixOS/nixpkgs>
    - For Configuration like `networking`, `i18n` .etc.

        <https://github.com/NixOS/nixpkgs/tree/master/nixos/modules/config>

    - For Services

        <https://github.com/NixOS/nixpkgs/tree/master/nixos/modules/services>

- Rajiv Nix Channel Pinning
    <https://github.com/rajivr/nix-channels>

- Rajiv NixOS Configuration Pre-flakes

    See `channels` and `machines` directory as on 12th April 2021.

    In *Enc 72*:

    [nixos-config-pre-flake.tar.gz at `nixos/Rajiv/nixos-config-pre-flake.tar.gz`](./nixos/nixos-72.7z)

## NixOS Install on VM using SSH

Nix OS Minimal Init for VM based install :

1.  Configure SSH by setting root password `sudo passwd root`
2.  Find out the VM IP address - `ip -c a` .
3.  Login to the Machine using a terminal  `ssh root@ip-address` .

## NixOS WiFi Configuration during Install

<https://wiki.archlinux.org/title/Wpa_supplicant>

On the minimal ISO, or if you are more familiar with `wpa_supplicant` then you can also run

1.  `wpa_passphrase ESSID | sudo tee /etc/wpa_supplicant.conf`
2.  then enter your password and
3.  `systemctl restart wpa_supplicant`

## NixOS Disk Partitioning during Install

1.  Make sure to have the **EFI system Partition** of `512MiB` and labeled as `boot`

    `mkfs.fat -F32 -n BOOT /dev/vda1`

2.  Make sure that the root file system should have `nixos` label

    `mkfs.ext4 -L nixos /dev/vda2`

3.  To check the created Partition details:

    ```sh
    lsblk -l  # Displays partition sizes
    lsblk -f # also displays UUID
    ```

## NixOS Install Experience

From video playlist of <https://www.youtube.com/playlist?list=PL-saUBvIJzOkjAw_vOac75v-x6EzNzZq->

VM CPU Pin 1 (Windows) `kvm` `cpupinning` `qemu` :

```xml
  <cputune>
    <vcpupin vcpu="0" cpuset="12"/>
    <vcpupin vcpu="1" cpuset="13"/>
    <vcpupin vcpu="2" cpuset="14"/>
    <vcpupin vcpu="3" cpuset="15"/>
  </cputune>
```

VM CPU Pin 2 (Linux) `kvm` `cpupinning` `qemu` :

```xml
  <cputune>
    <vcpupin vcpu="0" cpuset="8"/>
    <vcpupin vcpu="1" cpuset="9"/>
    <vcpupin vcpu="2" cpuset="10"/>
    <vcpupin vcpu="3" cpuset="11"/>
  </cputune>

```

### Partition

```sh
sudo fdisk /dev/vda
-- g # GPT Partition
-- n | Parition No 1<CR> | Default First Sector <CR>|Size +512MiB
-- t | Parition No 1 Type : 1 # EFI System
-- n | Parition No 2<CR> | Default First Sector <CR> | Size Default End of Disk <CR>
-- w # Write to Disk
```

No Swap partitions needed.

### Format Partition

```sh
sudo mkfs.fat -F 32 /dev/vda1
sudo fatlabel /dev/vda1 NIXBOOT
sudo mkfs.ext4 /dev/vda2 -L NIXROOT
```

### Mount Partitions

```sh
sudo mount /dev/disk/by-label/NIXROOT /mnt
sudo mkdir /mnt/boot
sudo mount /dev/disk/by-label/NIXBOOT /mnt/boot
```
### Swap File = 2GB

```sh
sudo dd if=/dev/zero of=/mnt/.swapfile bs=1024 count=2097152
sudo chmod 600 /mnt/.swapfile
sudo mkswap /mnt/.swapfile
sudo swapon /mnt/.swapfile
```

### Generate the Configuration Files

```sh
sudo nixos-generate-config --root /mnt
cd /mnt/etc/nixos
```

### Edit the Configuration file

```sh
sudo /mnt/etc/nixos/configuration.nix
```

Editing the File contents:
```nix
...
networking.hostName = "nixvm"
networking.wireless.enable = true # For actual Systems
...
time.timeZone = "Asia/Kolkata"
...
services.xserver.enable = true;
services.xserver.displayManager.sddm.enable = true; # For KDE
services.xserver.desktopManager.plasma5.enable = true;
...
sound.enable = true;
hardware.pulseaudio.enable = true;
...
services.xserver.libinput.enable = true; # For Laptop with Touch Pad support
...
users.users.nixer = {
    isNormalUser = true;
    initialPassword = "Password"; # First Boot Password
    extraGroups = [ "wheel" ]; # Wheel to enable SUDO
};
...
environment.systemPackages = with pkgs; [
  wget vim
  firefox
];
...
```

### Edit Hardware Configuration file

```sh
sudo /mnt/etc/nixos/hardware-configuration.nix
```

Editing the File:

```nix
...
filesystems."/" =
  { device = "/dev/disk/by-label/NIXROOT";
    fsType = "ext4"
  };
filesystems."boot" =
 { device = "/dev/disk/by-label/NIXBOOT";
    fsType = "vfat"
 };
 swapDevices = [
  { device = "/.swapfile";
  }
 ];
...

```

### Start Installation

```sh
cd /mnt
sudo nixos-install
```

Set Root Password at the End.

And reboot into the New system greeted by `systemd-boot`

### Update the Password for the user

```sh
sudo passwd nixer
```

### Setup Home Manager

```sh
nix-channel --add https://github.com/nix-community/home-manager/archive/release-20.09.tar.gz home-manager
nix-channel --update
```

!!! note "VPN"
    For VM disable the VPN for faster downloads

You would need to Logout and Login back on the UI to enable to `home-manager`.

```sh
nix-shell '<home-manager>' -A install
```

### Configure Home Manager

```sh
vim ~/.config/nixpkgs/home.nix
```

Edit in to the File:

```nix
...
# Before the end of the function
home.packages = with pkgs; [
 alacritty
];
# Alacritty Config
home.file = {
 ".config/alacritty/alacritty.yaml".text = ''
   env:
     TERM: xterm-256color
 '';
};
...
```

Install :

```sh
home-manager switch
```

### Help for Configuration.nix file

```sh
man configuration.nix
```

### Help on Home Manager Nix file

```sh
man home-configuration.nix
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Nix Language Hub](../../Lang/Nix/README.md)
- [Back to Nix Package Manager Article](../cli/nix.md)
- [Back to Distro Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
