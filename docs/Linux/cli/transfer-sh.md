# `transfer.sh` Command-line File Sharing

Sources: <https://github.com/dutchcoders/transfer.sh/>

<https://transfer.sh/> *Don't Visit its not a Website but just API*

Examples Document: <https://github.com/dutchcoders/transfer.sh/blob/main/examples.md>

Blog article where I found this tool : <https://www.tecmint.com/file-sharing-from-linux-commandline/>

## Uploading and Downloading

### Uploading with wget

```bash
$ wget --method PUT --body-file=/tmp/file.tar \
    https://transfer.sh/file.tar -O - -nv
```

### Uploading with PowerShell

```powershell
PS H:\> invoke-webrequest -method put -infile .\file.txt https://transfer.sh/file.txt
```

### Upload using HTTPie

```bash
$ http https://transfer.sh/ -vv < /tmp/test.log
```

### Uploading a filtered text file

```bash
$ grep 'pound' /var/log/syslog | curl \
    --upload-file - https://transfer.sh/pound.log
```

### Downloading with curl

```bash
$ curl https://transfer.sh/1lDau/test.txt -o test.txt
```

### Downloading with wget

```bash
$ wget https://transfer.sh/1lDau/test.txt
```

## Archiving and backups

### Backup, encrypt and transfer a MySQL dump

```bash
$ mysqldump --all-databases | gzip | \
    gpg -ac -o- | curl -X PUT --upload-file \
    "-" https://transfer.sh/test.txt
```

### Archive and upload directory

```bash
$ tar -czf - /var/log/journal | \
    curl --upload-file - https://transfer.sh/journal.tar.gz
```

### Uploading multiple files at once

```bash
$ curl -i -F filedata=@/tmp/hello.txt \
    -F filedata=@/tmp/hello2.txt https://transfer.sh/
```

### Combining downloads as zip or tar.gz archive

```bash
$ curl https://transfer.sh/(15HKz/hello.txt,15HKz/hello.txt).tar.gz
$ curl https://transfer.sh/(15HKz/hello.txt,15HKz/hello.txt).zip
```

### Transfer and send email with link (using an alias)

```bash
$ transfer /tmp/hello.txt | mail \
    -s "Hello World" user@yourmaildomain.com
```
## Encrypting and decrypting

### Encrypting files with password using gpg

```bash
$ cat /tmp/hello.txt | gpg -ac -o- | curl \
    -X PUT --upload-file "-" https://transfer.sh/test.txt
```

### Downloading and decrypting

```bash
$ curl https://transfer.sh/1lDau/test.txt | gpg -o- > /tmp/hello.txt
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
