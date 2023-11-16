# Kernel Virtual Machine - `libvirt`

**Virtualization** in ***Native Linux Kernel*** style.

## Topics

- **[Arch Linux/Manjaro Installation Steps](./manjaro-install.md)**.
- **[Remote Connection to Display of VM](#remote-connection-to-display-of-vm)**.
- **[Multiple Monitors for VM](#multiple-monitors-for-vm)**.
- **[Adding more USB devices to VM](#adding-more-usb-devices-to-vm)**.
- **[CPU Pinning And My CPU Pinning Configuration](./cpu-pinning.md)**.
- **[Windows Guest in KVM](./windows.md)** - Copy & Paste Clipboard Functionality.
- **[Linux Guest in KVM](./linux.md)** - Copy & Paste Clipboard Functionality and Windows 11.
- **[QEMU Disk Image Format `qcow2`](./qmu-disk-image.md)** - More insight into the KVM Disk Images.
    - **[Extending Size](./qmu-disk-image.md#extending-size-of-vm-guest-image-in-qcow2-format)** of VM Guest Image in `qcow2` format.
    - **[Mounting KVM Images](./qmu-disk-image.md#mounting-qemu-qcow2-disk-image)** `qcow2` disk Image.
- **[Sharing Files](./share-files.md)** between Guest Windows VM or Linux VM.
- **[PCI Pass Through](#pci-pass-through-links)** - Some links related to this.

## Others

## Remote Connection to Display of VM

Typically the VM's in **KVM** are destined to run like *head-less* unit.
Hence the *remote viewer* = **`virt-viewer`** is needed.

In order to connect to VM use the address

```sh
spice://127.0.0.1:5900
```

That's the default port on which the `virt-manager` hosts the VM display output.
If there are more than one VM active then try `5901` and so on.

## Multiple Monitors for VM

This is possible by adding few instances of `Video QXL`.

This can be done in the configuration mode of the VM's
(a.k.a `i` or *show hardware details* after opening the VM).

The new `spice` address would increment after `5900` port.

First additional one being `5901` and the respective address being in `virt-viewer`:

```sh
spice://127.0.0.1:5901
```

## Adding More USB Devices to VM

Some times we need to mount more than 1 USB device to our Guest OS.
This can be easily achieved by adding more `USB Redirector` to the VM.

This can be done in the configuration mode of the VM's
(a.k.a `i` or *show hardware details* after opening the VM).

## PCI Pass Through Links

- From Arch Linux Wiki

    <https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF>

- QEMU #4: How to add a USB controller to a QEMU/KVM virtual machine

    <https://www.youtube.com/watch?v=IYOmuPzrdXk>

- Linux Qemu/KVM GPU, USB, PCI passthrough!

    <https://www.youtube.com/watch?v=c8fTAIieUwU>

- IOMMU Viewer

    <https://github.com/pavolelsig/IOMMU-viewer/blob/master/iommu_viewer.sh>

- Intro to IOMMU groups for KVM virtual machines

    <https://www.youtube.com/watch?v=UgIUPZWBu9c>

- Easy ACS kernel patch guide for Ubuntu 20.04

    <https://www.youtube.com/watch?v=JBEzshbGPhQ>

- Adding USB controllers and devices with Virtual Machine Manager

    <https://www.youtube.com/watch?v=SSQxrgE_rjg>



----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)