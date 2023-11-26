# `rclone` remote sync and cloud backup tool

Sync & Encryption for Files to Cloud **Golang**.

Website <https://rclone.org/>

!!! warning "Does not support 2FA"
    Any online cloud storage that has `2FA` then it does not work.


!!! bug "Synchronization Errors"
    It has only one party synchronization. Does not handle well when multiple simultaneous updates are done.

## Configuration for Encrypted Dropbox Storage

```sh
$ rclone config
....
e/n/d/r/c/s/q>n
....
name > Dropbox
....
Storage> 13 # For Dropbox service
...
client_id> <Blank>
...
client_secret> <Blank>
...
y/n> n # Advance Config
...
y/n> y # Auto Config
...
y/e/d> y # Approve the Permission from Drop Box
...
e/n/d/r/c/s/q> n
...
name>DropboxEncrypt
...
Storage> 14 # Encrypt/Decrypt a remote
...
remote> Dropbox:m1 # Here m1 is the PC name
#Normally should contain a ':' and a path, e.g. "myremote:path/to/dir",
# "myremote:bucket" or maybe "myremote:" (not recommended).
....
filename_encryption> 1 # Encrypt File Names
....
directory_name_encryption> 1 # Encrypt Directories
....
y/g> g # Generate Password - For Already assigned use Enter My Password
....
Bits> 256
.......... # Keep a Note for this Password 1 if generated
...
y/n> y
...
y/g/n> g # Password 2 for salt - For Already assigned use Enter My Password
...
Bits> 256
...
......... # Keep a Note for this Password 2 if generated
...
y/n> n # Advanced Config
...
y/e/d> y # Confirm Remote Configuration for Encrypted Location
...
q> q # Quit the Program
```

## Local Folder to Sync to Cloud - Command
```sh
rclone sync ./ToSyncFolder/ DropboxEncrypt:ToSyncFol -v
```

`ToSyncFol` is the name of the storage folder in the cloud.
The `-v` option describes whats uploaded to cloud.

## List Full Directories on Cloud - Command
```sh
rclone lsf DropboxEncrypt:
```

Shows contents of the directory. The `:` here is essential to give details about all *containers* under the `DropBoxEncrypt` connection.

## Sync Local Folder from Cloud - Command
```sh
rclone sync DropboxEncrypt:ToSyncFol ./ToSyncFolder/ -v
```

The `-v` option describes whats downloaded from cloud.

## Delete a Container Directory - Command
```sh
rclone pruge DropboxEncrypt:mq
```

Here `mq` is the location that is described `lsf` command([List Full Directories](#list-full-directories-on-cloud---command)).

## `rclone` for Android

<https://github.com/x0b/rcx>

## `rclone` for Windows

We would need to install the Following:

```batch
winget install -e --id Rclone.Rclone
winget install -e --id kapitainsky.RcloneBrowser
```

These two would ensure you have both the
command line `rclone` as well as the
GUI support of `RcloneBrowser`.

Finally if you wish to **mount the `rclone` volumes** then:

```batch
winget install -e --id WinFsp.WinFsp
```

!!! note "Windows File System Proxy"
	WinFsp is system software that provides runtime and development
	support for custom file systems on Windows computers.
	In this sense it is similar to FUSE (Filesystem in Userspace),
	which provides the same functionality on UNIX-like computers.

	Homepage : <https://winfsp.dev/>

## RcloneBrowser a GUI tool for `rclone`

This is available both for Linux and Windows.

Current Version Website:

<https://kapitainsky.github.io/RcloneBrowser/>

Sources of Current Version:

<https://github.com/kapitainsky/RcloneBrowser>

There are other version to but that one is not actively maintained.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
