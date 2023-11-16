# `rsync` Remote file synchronization command

The Best , Fastest - Copy & Synchronization Tool.

## Reference

- `rsync` Original Command Documentation

    <https://man7.org/linux/man-pages/man1/rsync.1.html>

- Good Detail on `rsync` Command options

    <https://www.computerhope.com/unix/rsync.htm>

## Tutorials

- How to Use the `rsync` Command | Linux Essentials Tutorial

    <https://www.youtube.com/watch?v=2PnAohLS-Q4>

- How to Use `rsync` to Reliably Copy Files Fast (many examples)

    <https://www.youtube.com/watch?v=Pygr_TpZRpM>

- Linux/Mac Terminal Tutorial: Sync Files Locally and Remotely

    <https://www.youtube.com/watch?v=qE77MbDnljA>

- `rsync` is a Based File Sync Program (& if you don't use it, you're wrong.)

    <https://www.youtube.com/watch?v=iTnWIKHtOnA>

## Command Format

```sh
# Command Format
rsync -{options} source destination
```

Here are the Parts:
- `-{options}` There can be multiple options here based on what and where we care copying or synchronizing data. All `options` can be clubbed together to form a single String.
- `soruce` This is the absolute path to the source directory.
- `destination` This the **one level above** the destination directory that needs to be synced. This is an **important caveat**. We can also **specify the absolute path** but **need to follow correct grammar**.

## Examples detailing the Use

Here are Few Example that Explain this:

```sh
## 1. Backup all the Log Files to a Storage
# Destination Directory = /mnt/backup/log
# (Storage Device mounted at /mnt )
# Soruce Directory = /var/log
sudo rsync -av /var/log /mnt/backup
# Note the destination 'log' Got created automatically
# Here the options:
# -v option = Verbose - Full output
# -a option = Archive Mode
#            - Copy all Subdirectories
#            - Retain the Permissions
#            - Matches Source and Destination and Meta Data
```

```sh
## 2. Simulating a Copy
sudo rsync -av --dry-run /var/log /mnt/backup
# --dry-run = Its simulation to show what would happen
```

```sh
## 3. Copy Data from one Linux machine to Another
# Note Make sure ~rsync~ is install on Both Servers
# Source Directory = /var/log
# Destination = myuser@139.159.1.250:/mnt/backup
#   Here we are connecting as `myuser` to machine IP
#   139.159.1.250 and Directory /mnt/backup
sudo rsync -av /var/log \
  myuser@139.159.1.250:/mnt/backup
# This would use an 'SSH' connection under the hood
# to copy the files to the remote Linux machine.
```

```sh
## 4. Remote Copy - Absolute Path Grammar !!!
sudo rsync --dry-run -avz /mnt/nexcloud-data/ \
  myuser@192.134.22.12:/mnt/backup-data/nextcloud/
# Note the Slashes at the End of the Source and Destination
#  These help to create Absolute Path reference and
#  is called *Absolution Path Grammer !!!*
#  This makes ure that the conent of the folder
#  is copied not the folder itself.
# -z option = Helps you to compress while sending
#     data to the other side.
```

```sh
## 5. For a Special Long time taking copy
sudo rsync --dry-run --progress -avz \
  /mnt/nexcloud-data/ \
  myuser@192.134.22.12:/mnt/backup-data/nextcloud/
# --progress option = helps to show a progress bar
#      for long running process.
```

```sh
## 6. Single File Copy
rsync myfiles/photos.zip subscribe
# Note that we dont need to specify name or / for the
#   destination folder.
```

```sh
## 7. Simple Recursive Copy
rsync -r myfiles/ subscribe/
# Note the '/' at the end of the Source and Destination
# This is the Absolute Path Grammar !!!
# Note this would not Preserve the Meta Data
# Hence it is recommend to use -a the Archive mode option
rsync -a myfiles/ subscribe/
```

```sh
## 8. Best Options to Sync Data
rsync -ac myfile/ subscribe/
# -c option = Would force Rsync to overwrite or skip
#    coping if the Destination file Checksum ==
#    matches Soruce file Checksum then Skip.
```

```sh
## 9. Make the Data a bit more readable
rsync -ach --progress myfile/ subscribe/
# -h option = This allows to make the data shown in human readable form
#   like MB or GB instead of bytes.
# --progress = This would show the Data-rate and copy times .etc.
```

```sh
## 10. Copying Big files over un-reliable connections
#    Speically bad USB cables and hanging drives
rsync -ach --progress --prartial \
   myfile/ subscribe/
# --partial = copyies in partial chunks so if the copy
#     process gets interrupted by some network or power issue
#     the partial copied file would remain. And the command
#     would resume copy from the parts after the exisiting,
#     reducing the time to copy the files.
rsync -achP myfile/ subscribe/
# -P option = Combines the --progress --prartial together.
```

```sh
## 11. Identical Synchronization of Directories
#     Means that all files from source would be same as in the destinaiton.
#     In case the Destination has a few files extra they would
#     automatically be deleted.
rsync -achP --delete myfile/ subscribe/
# --delete = This would remove any files on the Destiation location
#    if they don't exist in the source location.
#   This is helpful in keeping 2 folders in absolute sync.
#   Very good for .git kind of folders.
```

```sh
## 12. Copy files and Delete the Source
rsync -achP --remove-source-files myfile/ subscribe/ &&\
  find myfile -type d -empty -delete
# --remove-source-files = Removes the Soruce files recursively
#    But it leaves the empty directories.
# Hence the find command removes them too.
```

```sh
## 13. Copy from Remote Server to Local
rsync -achPz root@20.232.22.3:/root/subscribe/ myfile/
# -z option = Added here to support compression while transporting file
# Note that on the Remote you need to have the rsync installed.
```

```sh
## 14. SSH Options in Rsysnc
rsync -achPz -e 'ssh -p 2212 -i ~/.ssh/id_other ' \
  root@20.232.22.3:/root/subscribe/ myfile/
# -e option = This lets us specify the SSH options using
#   a format of SSH command.
# Here the SSH commad uses the followitng variation:
# -p option = Custom port used for SSH
# -i option = Custom Identity file used for connecting
#    to the server.
```

```sh
## 15. Better File Diffrences before Move
rsync -achPi --delete --dry-run myfile/ subscribe/
# -i option = This shows a specific Itemized list of Changes
#   that would be made to the Destination.
```

## Backup Script

```bash
#!/bin/bash
DATE=$(date '+%F')
sudo rsync -achPz \
     /mnt/nexcloud-data/ \
     "myuser@192.134.22.12:/mnt/backup-data/nextcloud/${DATE}/"
```

## `rsync` Options as Aliases in the Shell

```bash
# Copy with progress Bar
alias cp-verbose='rsync -ah --info=progress2'
alias cp-archive='rsync -achP --info=progress2' # Default Rsync Options
alias cp-test-archive='rsysnc -achPi --info=progress2 --dry-run'
alias cp-mirror='rsync -achP --info=progress2 --delete --force --cc=xxh128'
alias cp-test-mirror='rsync -achPi --info=progress2 --delete --force --cc=xxh128 --dry-run'
```

## `rsync` To find difference between two directories

Compare and find the differences:

```sh
rsync -avcnhPi --delete --dry-run --exclude .git ./DIR-A/ ./DIR-B/
```

In the result here is the interpretation:

```
*Deleted - Means this would be deleted on the Target Location
>f++++++ - Means that file would be copied to the Target
```

Mirror `DIR-A` to `DIR-B` excluding the `.git` directory:

```sh
rsync -avchPi --delete --exclude .git ./DIR-A/ ./DIR-B/
```

## Installing `rsync` on Windows 10 or Windows 11

Reference: <https://www.youtube.com/watch?v=qJN9mb8fjDM>

We would need to enable **WSL** *Windows Subsystem for Linux*.

1.  Create a Ubuntu Instance:

    ```powershell
    wsl --install -d Ubuntu
    ```

2.  Login to our new Ubuntu Instance

    ```powershell
    wsl
    ```

3.  Check if `rsync` is available:

    ```sh
    which rsync
    # if its not available then
    sudo apt install rysnc
    ```

Now you are ready to use `rsync` on windows directly.

## `rsync` to only copy files not directories

Initial Example: <https://superuser.com/a/1353062>

```sh
rsync -achP --exclude='*/' --safe-links /home/ab/ /BackupC/cache/
```

The `--safe-links` helps to copy only relocatable links else they are not copied.

## Better way to avoid wrong Sym-links

<https://superuser.com/questions/799354/rsync-and-symbolic-links>

Even Better: <https://serverfault.com/a/233682>

```sh
rsync -achP --exclude='*/' --no-l /home/ab/ /BackupC/cache/
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)

