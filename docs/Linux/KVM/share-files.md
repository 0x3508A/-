# Sharing Files with KVM Guests

## In Guest VM

### Dependencies for Windows VM

- `spice-webdavd` = **Spice WebDAV daemon** Needs to be installed
- Download: <https://www.spice-space.org/download/windows/spice-webdavd/>
- Make sure to **Save Link As** the Latest file **`spice-webdavd-x64-latest.msi`**
    - Typically <https://www.spice-space.org/download/windows/spice-webdavd/spice-webdavd-x64-latest.msi>

This is apart from the **[Windows SPICE Guest Driver](./windows.md)**.

Make sure to *restart* the **Windows VM** after installing these.

### Dependencies for Linux VM

#### Ubuntu / Debian Specific

- `spice-webdavd` - Spice Driver for the WebDAV share

#### ArchLinux Specific

- There is no package as that of `spice-webdavd`
- Need to check on this TODO

#### Common Dependencies

- `davfs2` - Helps to Mount the WebDAV share like a local File System

Apart from the normal mandatory dependencies:
- `spice-vdagent` - Spice Agent for Guest VM
- `xserver-xorg-video-qxl` - Graphics Driver needed


Make sure that all these dependencies are installed and the Machine is restarted.

## In Host

### Configuring the VM in `virt-manager`

We would need to add a **Spice Channel** for `org.spice-space.webdav.0`.

This is done in the following steps explained with pictures:

#### 1. Click on the *Add Hardware* Button in the VM configuration

![Add Hardware Button](./share-files/kvm-libvirt_2021-04-27_10-50-21.png)

#### 2. Select *Channel* in the **Add New Virtual Hardware** Window and set the Name to `org.spice-space.webdav.0` selecting it from **drop down**.

![Selecting the Web DAV channel](./share-files/kvm-libvirt_2021-04-27_10-52-12.png)

#### 3. Make sure that the `spice-webdavd` is correctly installed.

##### Download for Windows:

![Website for Installing spice WebDAV](./share-files/kvm-libvirt_2021-04-27_10-57-50.png)

##### Save the Correct file in Windows:

![Correct Filename spice WebDAV](./share-files/kvm-libvirt_2021-04-27_10-59-40.png)

##### Make sure the Spice WebDAV service is active in Windows Task Manager

![Spice WebDAV Service](./share-files/kvm-libvirt_2021-04-27_11-02-25.png)

#### 4. Reboot the Guest VM to be sure

#### 5. Open the `virt-viewer` (a.k.a *Remote Viewer*)

![Remote Viewer for VM](./share-files/kvm-libvirt_2021-04-27_11-05-43.png)

##### Select the Correct `spice` server address. And connect to the VM.

#### 6. Setup the Folder Share

##### Select *Preferences* from the **File** menu in the *Remote Viewer*

![Preferences in File Menu](./share-files/kvm-libvirt_2021-04-27_11-06-29.png)

##### Observe the *Share Folder* option is now Active.

![Share Folder Option is active](./share-files/kvm-libvirt_2021-04-27_11-07-02.png)

##### Select the correct directory for sharing. Or use the *Other* option to find one.

![Find correct directory using Other options](./share-files/kvm-libvirt_2021-04-27_11-07-30.png)

##### Finally Activate the folder sharing

![Activate Folder Sharing](./share-files/kvm-libvirt_2021-04-27_11-08-03.png)

#### 7. Success - WebDAV drive should show up in the respective Guest VM

##### In #Windows Explorer

![Spice WebDAV in Windows Explorer](./share-files/kvm-libvirt_2021-04-27_11-10-23.png)

Make sure the refresh the drive using **F5** before any file action takes place.

##### In Ubuntu Nautilus - As part of *Other Locations* as *Spice client folder*

![Spice WebDAV in Ubuntu Nautilus](./share-files/kvm-libvirt_2021-04-27_14-13-41.png)

Some times the WebDAV does not mount. Just restart the PC or Logout/login to fix this.


----
<!-- Footer Begins Here -->
## Links

- [Back to KVM Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
