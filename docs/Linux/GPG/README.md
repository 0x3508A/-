# `gpg` GNU Privacy Guard the Pretty Good Privacy(PGP) tool

Yes, both `gpg` and `pgp` are used in similar context but a different.
`gpg` is a tool however the `pgp` is a methodology of achieving good
privacy standards for communications.

Home Page : <https://gnupg.org/>

## Topics

- **[Encryption and Decryption using GPG Keys](./enc-dec.md)**
- **[SSH using GPG Keys](./ssh-gpg.md)**
- **[Easier way to Generate GPG Keys](./easy-way.md)**
- **[My way GPG key generation](./my-way.md)**
- **[`paperkey` Easy way to backup your keys on paper](../cli/paperkey.md)**
- **[My Best Secure GPG Configuration file](./README/gpg.conf)**

### References

- Main Website for GPG <https://gnupg.org/>
- **Un-Attended GPG key Generation** look into this:

    <https://www.gnupg.org/documentation/manuals/gnupg/Unattended-GPG-key-generation.html>

    **[PDF](./README/Unattended-GPG-key-generation.pdf)** Copy.

- Quick Cheat sheet of commands

    <https://guides.library.illinois.edu/data_encryption/gpgcheatsheet>

- GPG Command Options

    <https://www.gnupg.org/documentation/manuals/gnupg/GPG-Esoteric-Options.html>

- Primary Reference / Recommendations for Config

    <https://help.riseup.net/en/security/message-security/openpgp/best-practices>

- Complete **Default** `gpg.conf` file

    <https://raw.githubusercontent.com/ioerror/duraconf/master/configs/gnupg/gpg.conf>

    - Reference : [`ioerror-gnupg-gpg.conf`](./README/ioerror-gnupg-gpg.conf)

- Additional Options for Better Performance

    <http://wiki.yobi.be/wiki/GnuPG#gpg.conf>

- **Debian** Simple Generation video - **but wrong way to handle keys!!!**

    <https://www.youtube.com/watch?v=xGsixSh6sC4>

## Others

------

## Change the `gpg` Home Directory

We can change *Home location* of `gpg` for a trial key install
and verification.

This is some times needed to add some third party keys temporarily.
These keys might not be reasonable to be added into the main `gpg`
system level key-ring. They can be added to a temporary ring and then
after use be expunged.

This same technique can be used to generate new derived keys in
case the master keys have been preserved in some isolated location.

This method can also be used to change or add configuration to
existing keys for testing them. Since the configuration would be
temporary it would not effect the normal use.

There are two ways to achieve this:

1. Using the *Command argument* `--homedir dir` in the `gpg` Command
2. Using Environment Variable `GNUPGHOME`

Script `temp-gpg.sh`:

```sh
#!/bin/sh
mkdir $HOME/gnupghome
export GNUPGHOME=$HOME/gnupghome
```
The above script would create the new directory called `gnupghome`
under the user's `$HOME` directory. And then make sure that the current
environment inherits this change.

Typical use of the above script would be :

```sh
source./temp-gpg.sh
```

This would change the current environment with the custom
`gpg` directory.

## `gpg` Home Directory permissions

For Safe operation of `gpg` on the Linux system:

```sh
#!/bin/sh
chmod 0700 gnupghome
chown -R $USER gnupghome
```
!!! note ""
    Here `gnupghome` is the directory name. Replace it with the actual one as needed.

The above script would help us get the operating permissions correct
on the given `gpg` directory. Else the `gpg` tool would warn for
wrong permissions set.

## `gpg` File Permissions

```sh
#!/bin/sh

# File Permissions
chmod 600 gnupghome/*

# Take Ownership of the directory
chown -R $USER gnupghome
```

!!! note ""
    The directory `gnupghome` is considered as the current
    [`gpg` Configuration Home Directory](#gpg-home-directory-permissions) permissions.
    These file permissions apply to the **files inside** this *GPG configuration Home directory*.

## Getting the Key ID or Fingerprint of `gpg` Keys

```sh
$ gpg --fingerprint --fingerprint
gpg: Note: trustdb not writable
/home/<user>/.gnupg/pubring.kbx
---------------------------
pub   rsa2048/0x5BD96CC4247B52CC 2012-05-04 [SC] [expired: 2022-05-02]
Key fingerprint = B466 3188 A692 DB1E 45A9  8EE9 5BD9 6CC4 247B 52CC
uid                   [ expired] Guillaume Benoit (Guinux) <guillaume@manjaro.org>
sub   rsa2048/0x14FA9A3D72CD35DE 2012-05-04 [E] [expired: 2022-05-02]
Key fingerprint = 96B2 D021 C483 81F7 D92B  2F51 14FA 9A3D 72CD 35DE

pub   rsa2048/0xCAA6A59611C7F07E 2012-05-05 [SC]
Key fingerprint = E4CD FE50 A2DA 85D5 8C8A  8C70 CAA6 A596 11C7 F07E
uid                   [ unknown] Philip Müller (Called Little) <philm@manjaro.org>
sub   rsa2048/0x320011450576724A 2012-05-05 [E]
Key fingerprint = CE14 BB1F 3850 6136 6B3C  1356 3200 1145 0576 724A
...
```

From the Output there 3 important parts we find out:

1. Who the Key belongs to Example:

    `[ unknown] Philip Müller (Called Little) <philm@manjaro.org>`

2. Public Key's **Key ID** or **Short 8-Hex digit Fingerprint**

    `rsa2048/0x5BD96CC4247B52CC` or `rsa2048/0xCAA6A59611C7F07E`

    Note the `pub` in front that denotes its a Public Key.
    The Sub Keys have a `sub` in front.

3. And finally the Validity and Expiry:

    `2012-05-04 [SC] [expired: 2022-05-02]`

    This one has already expired as of `2022-05-02`.

    `2012-05-05 [SC]`
    This one is still valid.

    Note that we always look at the validity of each key
    be it `pub` or `sub` types.

## `gpg` fingerprint of all keys

```sh
gpg --fingerprint --fingerprint
```

Also the `gpg.config` file setting does the same thing:

```
# List all keys with their fingerprints. This is the same output as --list-keys
# but with the additional output of a line with the fingerprint. If this
# command is given twice, the fingerprints of all secondary keys are listed too.
with-fingerprint
with-fingerprint
```

When this setting is added one does not need to repeat `--fingerprint`.

## Get `gpg` Master key or Secret Key fingerprint

```sh
gpg --list-secret-keys --fingerprint
```

One more additional `--fingerprint` in the above command would show the
**SubKey** fingerprint as well.

## Importing `gpg` Keys into a Key ring

Importing can be of two type of keys -

1. Public Key import
2. Private Key import
3. Public Key with Yubikey/Smartcard storage


### 1. Public Key Import

```sh
gpg --import public-key.txt
```

This command supports both **Armor ASCII format** and **Binary format**.

### 2. Private Key Import

```sh
gpg --import private-key.txt
```

This command supports both **Armor ASCII format** and **Binary format**.

!!! note ""
    You may be asked for a **Decryption** pass phrase in case the key is
    stored in an encrypted format.

### 3. Public Key with Yubikey / Smartcard Storage

```sh
gpg --import public-key.txt
```

You would only need to plug the **Yubikey/Smartcard storage** to verify the
key import using:

```sh
gpg --card-status
```

## Update the Trust in a Given Key

There are 2 types of Trust modifications that we encounter:

1.  Upgrade the Trust in **Public Key** from a known person.
2.  Upgrade Trust in your own **Private Key**

### 1. Trust your GPG Private Key

```sh
gpg --edit-key --expert <Key ID>
```

Here the `<Key ID>` needs to be replaced with the **Private Key** fingerprint
or short last *8-Hex-Digits*.

Next, in the GPG Shell Type:

```sh
gpg> trust
```

If this is your personal key make sure to make it **ultimate** trust.
Since in most cases the derived key is used.

### 2. Trust for someones GPG Public Key

We can edit the specific key using its signature:

```sh
gpg --edit-key 0x427F11FD0FAA4B080123F01CDDFA1A3E36879494
```

!!! note "Public Key"
    This is a Public Key here.

For the [Private keys](#1-trust-your-gpg-private-key) the process is different.

Next, in the GPG Shell Type:

```sh
gpg> trust
pub  4096R/36879494  created: 2010-04-01  expires: never       usage: SC
		     trust: unknown       validity: unknown
[ unknown] (1). Qubes Master Signing Key

Please decide how far you trust this user to correctly verify other users' keys
(by looking at passports, checking fingerprints from different sources, etc.)

   1 = I don't know or won't say
   2 = I do NOT trust
   3 = I trust marginally
   4 = I trust fully
   5 = I trust ultimately
   m = back to the main menu

Your decision? 5
Do you really want to set this key to ultimate trust? (y/N) y

pub  4096R/36879494  created: 2010-04-01  expires: never       usage: SC
		     trust: ultimate      validity: unknown
[ unknown] (1). Qubes Master Signing Key
Please note that the shown key validity is not necessarily correct
unless you restart the program.

gpg> q
```

The `trust` command invokes the edit on the trust parameters of a given key.
Next, the selection `5` assures that we trust the key fully.
Finally we need to write the changes so `q` to quit and write the trust.

#### Info Related to My setup

My [archive of docs on GPG](./README/gnupg-docs-zv2.7z) after a long time.

## Exporting GPG Keys

GPG Keys can be In Multiple Formats for various types Keys
(including *Private Master Key*).
There can be various kind of keys and export techniques.

There are namely 2 types of *export formats* that we would be looking into:

1.  Text Key or **ASCII Armor** Key output :
    This one is mainly used for key distribution, specially **public key**.
2.  Binary Key output :
    This one is used mainly for backup purposes -
    Specifically for **private keys**.

More Details:

<https://www.gnupg.org/documentation/manuals/gnupg/GPG-Input-and-Output.html>

*Types of Keys* and their Export:

1.  Secret Key Export
2.  Secret Sub-Key Export
3.  Public Key Export
4.  Secret keys to Key-Card loading

The above are the most common export types that are needed, when we are
on-boarding a *GPG Key infrastructure* for a new person. Here new person
can be any one who is just getting started with **GPG**.

### 1. Export GPG Master Private Key

In order to export the ****Master Private Key**** one needs the *Password*
if used during creation of the key.
Also the same key should be part of current GPG Key ring.

#### a. Export GPG Master Private Key In Binary Format

```sh
gpg --export-secret-keys [Last 8Hex Digits of MasterKey]\
> export/private-master-key
```

Here `[Last 8Hex Digits of MasterKey]` are actually the **key ID** or key
**Fingerprint**. They are [explained here](#getting-the-key-id-or-fingerprint-of-gpg-keys).

#### b. Export GPG Master Private Key In ASCII Armor Format

```sh
gpg --export-secret-keys -a [Last 8Hex Digits of MasterKey]\
> export/private-master-key.txt
```

!!! note "ASCII Armor"
    The additional `-a` in the command actually helps to generate
    the **ASCII Armor** Output format.

### 2. Export of GPG Public Key

This is needed to be shared with any one planning to communicate or sign.
Also needed where ever verification of GPG Identity is needed.
This also includes the derived Sub-Keys that would actually be used for a
more secure setup.

#### a. Export GPG Public Key In Binary Format

```sh
gpg --export [Last 8Hex Digits of MasterKey]\
> export/private-master-key
```

his is a binary output and can be used for **Paper key recovery** process.
Typically a **Text output** of the public key is preferred as its easier to share.

#### b. Export GPG Public Key In ASCII Armor Format

```sh
gpg --export -a [Last 8Hex Digits of MasterKey]\
> export/private-master-key.txt
```

This can be distributed or exported to key server. It would help people to
securely communicate and validate ones identity.

### 3. Export of GPG Private Sub-Key

We typically don't use the Primary keys for signing .etc.
Its the **derived keys** that we normally use.
They are somewhat temporary can can easily be replaced in case problems.
This more of a **security** guideline that one needs to use these **derived**
or **sub-keys**.
Also, we only need to back them up just in case we delete them by mistake -
hence only **Armor ASCII format** is used to export them.

There are typically 3 types of **Sub-keys**:

1.  Authentication - used for GPG based SSH and Login.
2.  Signing - used for GIT commit signing or Message signing .etc.
3.  Encryption - used for encryption and decryption of data.

```sh
gpg --export-secret-subkeys -a <Last 8Hex Digits of SignKey>\
    > export/private-sign-subkey.txt
gpg --export-secret-subkeys -a <Last 8Hex Digits of AuthKey>\
    > export/private-auth-subkey.txt
gpg --export-secret-subkeys -a <Last 8Hex Digits of EncKey>\
    > export/private-enc-subkey.txt
```

## Generating Key Revocation Certificates

!!! warning "DANGER of Loss"
    **!!DANGER!!** this is used to **Invalidate!! Private Master Key**.
    Keep it **safe and secure** in wrong hands
    **this can remove your identity** form services.

This certificate when imported by **GPG** revokes the particular key.
It removes both **trust** and **validity** of a key. The only
*way restore the key back* is by
[Deleting the Key](#deleting-or-removing-gpg-secret-keys) and
[Importing](#importing-gpg-keys-into-a-key-ring) it back again.
And that too if it still has Validity (means not expired yet).

This is also used for revoking keys stores in any **GPG Key Servers**.

```sh
gpg --output export/revoke_master-key.txt \
--gen-revoke <Last 8Hex Digits of MasterKey>
```

This would ask for reasons, typically a **blank** or **No Reason** is enough.
You would also need to type in the **Passphrase** for the master key.

## Deleting or Removing GPG Secret Keys

```sh
# Don't forget to remove the spaces in the digits of HEX Fingerprint
gpg --delete-secret-keys <Last 8Hex Digits of MasterKey>
Delete this key from the keyring? (y/N) y
This is a secret key! - really delete? (y/N) y
# Confirmation dialogs might show up for deletion of sub-keys.
```

!!! note "Master key Hex Digits"
    The `<Last 8Hex Digits of MasterKey>` needs to be replaced with the
    *Fingerprint* of the **Master Key**.

## Deleting or Removing GPG Public Keys

```sh
gpg --delete-keys <Last 8Hex Digits of Key>
...
Delete this key from the keyring? (y/N) y
...
```

With this you can remove some of the un-needed public keys.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
- References
    - Un-Attended GPG key Generation look into this: <https://www.gnupg.org/documentation/manuals/gnupg/Unattended-GPG-key-generation.html>
    - Quick Cheat sheet of commands <https://guides.library.illinois.edu/data_encryption/gpgcheatsheet>
    - GPG Command Options <https://www.gnupg.org/documentation/manuals/gnupg/GPG-Esoteric-Options.html>
    - Primary Reference / Recommendations for Config <https://help.riseup.net/en/security/message-security/openpgp/best-practices>
    - Complete **Default** `gpg.conf` file <https://raw.githubusercontent.com/ioerror/duraconf/master/configs/gnupg/gpg.conf>
        - Local File : [`ioerror-gnupg-gpg.conf`](./README/ioerror-gnupg-gpg.conf)
    - Additional Options for Better Performance <http://wiki.yobi.be/wiki/GnuPG#gpg.conf>
    - **Debian** Simple Generation video - but wrong way to handle keys
        - <https://www.youtube.com/watch?v=xGsixSh6sC4>