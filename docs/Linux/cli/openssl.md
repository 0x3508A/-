# `openssl` Command

#### The Multi-platform Security command that Works!

## Generate Random Data from OpenSSL using Encryption
```sh
head -c 100m /dev/zero |\
    openssl enc -aes-128-cbc \
	    -pass pass:"$(head -c 20 /dev/urandom | base64)"  > my100MBfile
```

This command does the following steps:

1.  Generate the **100MB** of <span class="underline">Zero Byte Data</span>
2.  Generate a **Password of 20 characters using `/dev/urandom`** process it using `base64`
3.  Encrypt the data using **AES-128-CBC** to generate the file
4.  Store data into `my100MBfile`

The amount generated data can be varied using the `head` command.
The `100m` indicates *100 Mega in Bytes* of Data.
It can be `5m` for only **5MiB of data**.

**Note:** The end result is a binary output.

It can easily be converted to text if needed using:
```sh
head -c 100m /dev/zero |\
    openssl enc -aes-128-cbc \
	    -pass pass:"$(head -c 20 /dev/urandom | base64)" |\
    base64 > my100MBfile.txt
```

The last `base64` would convert it to a text data.

## Good `openssl` Command Docs

<https://www.madboa.com/geek/openssl/>

## Generate 128-bit Prime Numbers

```sh
openssl prime -generate -bits 128 -hex | python -c "import sys;\
    m=sys.stdin.read();\
    print('\nData : {:#0x}\n'.format( int(m,16) ));\
    print('Length: {:d}\n'.format( len(m.strip()) ))"
```

## Generate 256-bit Prime Numbers

```sh
openssl prime -generate -bits 256 -hex | python -c "import sys;\
    m=sys.stdin.read();\
    print('\nData : {:#0x}\n'.format( int(m,16) ));\
    print('Length: {:d}\n'.format( len(m.strip()) ))"
```

## Encrypt a File

```sh
openssl enc -e -aes-256-cbc -k passphrase -a -in  TestingFile.txt -out Enc.txt
```

Here the `-e` used for **encryption**.

!!! note
     `-a` option is used for **BASE64** encoding.

!!! note
    `passphrase` is the password used to perform the **encryption**.

If we want to print the **IV Key** and other Details add the `-p` option

```sh
openssl enc -e -aes-256-cbc -k passphrase -a -p -in  TestingFile.txt -out Enc.txt
```

If we want to allow the **user the enter the Password** we ca use the command like this:

```sh
openssl enc -e -aes-256-cbc -a -p -pbkdf2 -in TestingFile.txt -out Enc.txt
```

!!! note
    This also works in **Windows** on [MSYS2](../../Windows/MSYS2.md).

## Decrypt the File

```sh
openssl enc -d -aes-256-cbc -k passphrase -a -in Enc.txt -out Dec.txt
```

Here the `-d` used for **decryption**.

Again for accepting the **user Password** in command line:

```sh
openssl enc -d -aes-256-cbc -a -pbkdf2 -in Enc.txt -out Dec.txt
```

!!! note
    This also works in **Windows** on [MSYS2](../../Windows/MSYS2.md).

## Encrypt/Decrypt files for [`transfer.sh`](./transfer-sh.md)

```sh
# Encrypt files with password using openssl
$ cat /tmp/hello.txt|openssl aes-256-cbc -pbkdf2 -e| \
    curl -X PUT --upload-file "-" https://transfer.sh/test.txt
```

There are 3 parts in this command:

1. Send the file contents to `openssl`
2. Use `openssl` to encrypt using `aes-256-cbc` with `-pbkdf2` Key derivation
    function and `-e` to ask for password from user to **Encrypt**.
3. Finally upload the file to [`transfer.sh`](./transfer-sh.md) service.

The reverse is also easy:

```sh
# Download and decrypt
$ curl https://transfer.sh/bqsnEP9ik8/test.txt| \
    openssl aes-256-cbc -pbkdf2 -d > /tmp/hello.txt
```

Here we are using [`curl` command](./curl.md) output to `openssl` and
`-d` with **decrypt** by asking user password again.


!!! note
    This also works in **Windows** on [MSYS2](../../Windows/MSYS2.md).

## Some Notes on `xxd` and `openssl` encryption

```sh
openssl --help
xxd --help
echo 0: 63616e746765747468697332776f726b |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K 00000000000000000000000000000000 |\
    xxd -p
echo 0: 63616e746765747468697332776f726b |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K 00000000000000000000000000000000 |\
    xxd -p
echo 0: 55AA00 |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K 00000000000000000000000000000000 |\
    xxd -p
echo 0: 55AA0000000000000000000000000000 |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K 00000000000000000000000000000000 |\
    xxd -p
echo 0: 63616e746765747468697332776f726b |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K 00000000000000000000000000000000 |\
    xxd -p
echo 0: 00000000000000000000000000000000 |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K df12189258481ff8406bedbce2de442c |\
    xxd -p
echo 0: 55AA0000000000000000000000000000 |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K df12189258481ff8406bedbce2de442c |\
    xxd -p
echo 0: 55AA0000000000000000000000000000 |\
    xxd -r |\
    openssl enc -aes-128-ecb -nopad -K df12189258481ff8406bedbce2de442c |\
    xxd -p
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
