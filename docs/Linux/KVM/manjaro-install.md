# Installing KVM `libvirt` under Manjaro or Arch Linux

EF-Tech Made Simple video on KVM

<https://www.youtube.com/watch?v=itZf5FpDcV0>

## 1. QEMU Installation

 - `ovmf` helps to do the UEFI Bios stuff.
 - `bridge-utils` for network bridge needed for VMs
 - `vde2` for QEMU distributed Ethernet emulation
 - `dnsmasq` the DNS forwarder and DHCP server
 - `openbsd-netcat` network testing tool (Optional)
 - `ebtables` and `iptables` to create packet routing and firewalls

### Problem with `brltty`

If you are using **USB Serial** dongles you would end up with troubles.
Yes, while installing **QEMU** an unnecessary dependency crops up - `brltty`.

This is an *Accessibility* software for blind users. Though useful it actually interferes with all **USB Serial dongles**.

Another package that is already installed is `modemmanager`.

We need to fix the `brltty` by installing a dummy package `brltty-dummy` from **AUR**. This would fix the dependency.
Additionally we would need to disable the `brltty` service with masking it in our `systemd`.

#### Reference

- <https://archlinux.org/packages/extra/x86_64/brltty/>
- <https://archlinux.org/packages/extra/x86_64/modemmanager/>
- <https://aur.archlinux.org/packages/brltty-dummy>
- <https://wiki.archlinux.org/title/Install_Arch_Linux_with_accessibility_options>
- <https://forums.opensuse.org/showthread.php/534161-How-do-I-disable-brltty>
- <https://unix.stackexchange.com/questions/670636/unable-to-use-usb-dongle-based-on-usb-serial-converter-chip>

### Install Commands

#### 1. Preparation

!!! note "AUR Instal helper"
    You need either [`pamac`](https://wiki.manjaro.org/index.php/Pamac) or
    [`yay`](https://github.com/Jguer/yay) installed.

```sh
# Install brltty-dummy removing the original - NOTE needs AUR Enabled
sudo pacman -R brltty modemmanager
sudo pamac install brltty-dummy
# or
sudo yay -S brltty-dummy
```

#### 2. QEMU Install

```sh
# Install the QEMU Now
sudo pacman -S qemu qemu-arch-extra ovmf bridge-utils dnsmasq vde2 \
 openbsd-netcat ebtables iptables
```

## 2. Virt-Manager and `libvirtd` Service Install

```sh
# Virt-manager helps to create and organize the VM's.
# And virt-viewer is used to open remote window into the VM instance
sudo pacman -S virt-manager virt-viewer
```

Typically `virt-viewer` is called the **Remote Viewer** since it supports many protocols like RDP.

In order to connect to VM use the address

```
spice://127.0.0.1:5900
```

Thats the default port on which the `virt-manager` hosts the VM display output.

!!! note "Display for VM"
    If you have more than 1 VM open then use `5901` port for the second one,
    `5902` for the third one and so on.
    The corresponding address would be come `spice://127.0.0.1:5901` and
    `spice://127.0.0.1:5902` etc.

## 3. Enable Services

```sh
# Enable Auto-Start of the Service
sudo systemctl enable libvirtd.service
```

!!! note ""
    In order to reduce the system Load you can skip the Auto-start part.
    And do it every time you wish to use the service.

Also Start it:

```sh
# Start the Service Right now
sudo systemctl start libvirtd.service
```

To check its status:
```sh
sudo systemctl status libvirtd.service
```

## 4. Configure KVM

Open the `/etc/libvirt/libvirtd.conf` for editing

```sh
sudo nano -cl /etc/libvirt/libvirtd.conf
```

Here are the **Lines to Edit**:

1. Uncomment the line 81 or so:
   ```sh
   unix_sock_group = "libvirt"
   ```
2. Uncomment the line 98 or so:
   ```sh
   unix_sock_rw_perms = "0770"
   ```

Make sure to **save the file before you exit**.

## 5. Creating a New Network Bridge for VM's

### 5.1. Create a file like `br10.xml` in your home directory

```sh
sudo nano br10.xml
```

With Content of `br10.xml` as :

```xml
<network>
  <name>br10</name>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='br10' stp='on' delay='0'/>
  <ip address='192.168.72.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='192.168.72.50' end='192.168.72.200'/>
    </dhcp>
  </ip>
</network>
```

This would help to register a new network.

Here we are changing the default IP Address `192.168.72.X` and also
allowing the specific ports `1024-65535` to be forwarded over NAT.
This helps to easily communicate with the Guest OS in the VM's.

### 5.2. Register the Bridge Network:

```sh
sudo virsh net-define br10.xml
```

Now we have the network registered in the `libvirtd`.

### 5.3. Start the Bridge

In order to Start this network:

```sh
sudo virsh net-start br10
```

If you want to Permanently enable the network bridge, so that it auto starts:

```sh
sudo virsh net-autostart br10
```

!!! note "Startup Constrains for Network"
    Permanently enabling this network would take resources.
    Hence the above start command must be given every time before starting `virt-manager`.

## 6. Permissions for Current User

In-order to be able to use the `virt-manager` as normal user, we need to add the user to the `libvirt` group.

```sh
sudo usermod -a -G libvirt $(whoami)
```

**Reboot** the PC or Computer after changing the options.

----
<!-- Footer Begins Here -->
## Links

- [Back to KVM Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
