# Encryption & Decryption with GPG

## Encryption & Decryption using GPG Public Key

### Introduction

In this configuration your own Private Key designated for Encryption and the
Public key of the other party can be used.

The Cipher algorithm is decided by the preferences stored in the
`gpg.conf` file.

Here is [my Configuration file](./README/gpg.conf).

### GPG cipher preferences

Its important to have the correct GPG cipher preferences in the `gpg.conf` file.

```
personal-digest-preferences SHA512 SHA384 SHA256 SHA224
personal-cipher-preferences AES256 AES192 AES CAST5 CAMELLIA192 BLOWFISH TWOFISH CAMELLIA128 3DES
personal-compress-preferences ZLIB BZIP2 ZIP
```

### Preparations

Important Points to note for Encryption & Decryption to work:

1.  You must have your [GPG Cipher preferences](#gpg-cipher-preferences) setup
    correctly in `gpg.conf`.
2.  You must have your one private key installed in the system or the
    Yubikey attached.
3.  You must have the ***Public key of the other party*** and their
    correct ID for that key. In case you want to keep for yourself then its
    already on the system where you have the private key loaded.

    Also make sure that other party's Key has been updated in terms of
    ***trust*** of the key.

4.  In case you wish to be able to decrypt the items you are sending add your
    own ID to the list of the recipients.

## Encrypt Files using GPG Public Key

Command Format for files:

```sh
gpg -e [-a] [-s] -r <Recipient1> [-r <Recipient2> ...] filename
```

Here is the explanation of the options:

- `-e` = For Encrypt Operation
- `-a` = For ASCII Armour for the output
- `-s` = Sign the Output
- `-r <Recipient1> [-r <Recipient2> ...]` = Each of the recipient IDs
    can be specified in chain.

    At least one is needed.
- `filename` = File to be encrypted , the resulting output would be
    `filename.gpg` for non-armor and `filename.asc` for ASCII armored output.

For Example:

```sh
gpg -eas -r build@manjaro.org -r user@email.com test.txt
```

This would Sign, Encrypt and Armour the `test.txt` for both parties.
And a new file `test.txt.asc` would be produced.

Another modification of the standard command can be used to produce a
desired output file name:

```sh
gpg -e [-a] [-s] -r <Recipient1> [-r <Recipient2> ...] -o output-file filename
```

Here the `-o output-file` specifies the output file instead of the default
`filename.gpg` or `filename.asc`.

### Decrypt Files using GPG Private Key

Command Format for files:

```sh
gpg -o output-file -d filename
```

Here is the explanation of the options:

- `-o output-file` = This specifies the output file for decryption operation.
    Here this is mandatory since GPG does not know what is the file or the
    data type. So we specifically tell GPG on what to generate.
    Without this option file content would be printed on the screen along
    with the sender details.
- `-d filename` = This provides the decryption command with the input filename.

For Example:

```sh
gpg -o test.txt -d test-enc
```

This would decrypt the `test-enc` file and generate the `test.txt` which
would contain the decrypted contents.

### Encryption of Piped Data using GPG Public Key

Command Format for Piped Data:

```sh
... | gpg -e [-a] [-s] -r <Recipient1> [-r <Recipient2> ...] | ...
```

This command can be *inserted between* <span class="underline">in a pipe</span>
and *generate an output*, that can gain be **piped**.

The *output* can then be either *printed to `stdout`,* or directed to file or
to the next command in the chain.

For Example:
```sh
echo "Life is Great" | gpg -eas -r user@email.com | tee test.enc
```

This command would encrypt the message and sign it for the desired recipients
and then write it to a file. This command would also show the encrypted data
on the screen (`tee`).

### Decryption of Piped Data using GPG Private Key

```sh
... | gpg -d | ...
```

This would just decrypt the incoming data from the previous command.

!!! note "Private Key Selection"
    This would the default **Private Key** in case you have
    *multiple Private keys* you would need add `--try-all-secrets` option.

    `... | gpg -d --try-all-secrets | ...`.

For Example:

```sh
cat test.enc | gpg -d
```

This command would decrypt the previous stage and shoe the decrypted
output on the `stdout`.

## En/Decryption using GPG with Symmetric Key / Pre-Shared Secret

### Introduction

This is doing Symmetric key encryption using GPG.
One only needs a pre-shared secret to perform the encryption & decryption.
This does not use the pre-installed local GPG keys.

For safer operation create a new GPG Home Directory.

### Encryption with GPG using Symmetric Key / Pre-Shared Secret

Command Format:

```sh
gpg [-a] [--cipher-algo AES256] -o output-filename -c input-filename
```
Here is the explanation of the options:

- `-a` = ASCII Armour for the output file.
- `--cipher-algo AES256` = Specify the encryption algorithm.
    Typically this is specified `gpg.conf` file.
- `-o output-filename` = Use to Specify the encrypted file output.
- `-c input-filename` Perform **symmetric key encryption** on input file.

For Example:

```sh
gpg -a -o test.enc -c test.txt
```

This would encrypt the `test.txt` file and generate `test.enc` with ASCII Armour.
The __Password__ would be prompted for *2 times entry*.

### Decryption with GPG using Symmetric Key / Pre-Shared Secret

Command Format:

```sh
gpg -o output-filename -d input-filename
```

Here is the explanation of the options:

- `-o output-filename` = Use to Specify the encrypted file output.
- `-c input-filename` Perform symmetric key encryption on input file.

!!! note ""
    GPG would try the default encryption algorithms specified in the
    `gpg.conf` file. If any *encryption beyond that list* is used, then the
    <span class="underline">decryption would fail</span>.

For Example:

```sh
gpg -o test.txt -d test.enc
```

This command would decrypt the `test.enc` to `test.txt`.

It would *request for password* of the Sender.

!!! note ""
    If you use the same GPG Home Directory then your password is already
    recorded and hence it will automatically decrypt without password.


----
<!-- Footer Begins Here -->
## Links

- [Back to GPG Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)

