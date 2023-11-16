# My Way of Generating GPG keys

## Preparation

Here is a step by step guide to help with generation of the GPG Keys.

Make sure to use a ***Isolated PC = NO INTERNET****.

Typically [Tails Linux](https://tails.boum.org/) with **Networking disabled**
option makes an Ideal workstation.

Also as per *Rajiv* **do it under a blanket** for added later of security!!!

## 1. Configuring the Storage

Its important to make sure which PC you are using.

Even more **important** is where you store.

Typically a **USB stick** with
**[LUKS](https://gitlab.com/cryptsetup/cryptsetup/)** or
**[VeraCrypt](https://veracrypt.fr/en/Home.html)** Volume
configured would be a nice for storage during **Key generation**.

Here are the Steps:

1. Create a folder under which you would be generating the Keys.
    Let's name it as `gpghome`.

    ```sh
    mkdir gpghome
    ```

2. Set correct permissions on this directory.

    This is important for safety reasons.

    ```sh
    chmod 0700 gpghome
    ```

    This would make sure that only you have
    access to this directory or than exception
    of `root`.

    In our case we use [Tails Linux](https://tails.boum.org/) hence
    there is no `root`.

3. Create the directory for **GPG Keyring** this is  typically `.gnupg`.
    And while we are making this let's also create
    `export` directory where we can export our keys for backup.

    ```sh
    mkdir -p gpghome/.gnupg
    chmod 0700 gpghome/.gnupg
    mkdir -p gpghome/export
    chmod 0700 gpghome/export
    ```

4. Now we would enter this directory.

    ```sh
    cd gpghome
    ```

    Its important to know the location of `gpghome` - meaning the absolute path.
    Let's assume it to be `/mnt/gpghome` for now.
    This would later become very useful while defining the environment.

## 2. Configure GPG options

We need to make sure that our GPG key generation follows the essential
safety guidelines.

For the same we have a [Secure version of `gpg.conf`](./README/gpg.conf) file.

Let's edit our Configuration:

```sh
nano ./.gnupg/gpg.conf
```

Type in the secure config into this file.
Finally we would set correct permission for the above fine.

```sh
chmod 0600 ./.gnupg/gpg.conf
```

!!! note ""
    Above commands assume you are already in the `gpghome` directory.

## 3. Setup the Environment

Environment setup actually means setting up variables in the
shell, we would use for generating keys.

```sh
export GNUPGHOME=/mnt/gpghome/.gnupg
```

This would set our [GPG Home Directory](./README.md#change-the-gpg-home-directory).

Remember the mount path from **Step 1 Point 4** - its the absolute
path for our `gpghome`.

In order to see if the GPG is working in the correct way we need to issue
a **Key listing** command.

```sh
gpg --list-keys
```

This should automatically create two files under `.gnupg` directory.
Namely `pubring.kbx` `trustdb.gpg`.
Notice the path in the output of the above command.
It should be in our set `GNUPGHOME` location.
If not then something is wrong in the config or the environment setup.

## 4. Generate the GPG Master Key
```sh
gpg --expert --full-gen-key
```

The special `--expert` option helps us to generate the **ECC** keys that we need.

```
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 9
```

This would be `(9) ECC and ECC` which means that we want primary key
to use **ECC** and we also want a **ECC** key for Encryption.

```
Please select which elliptic curve you want:
   (1) Curve 25519
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection? 5
```

We choose the `(5) NIST P-521` option which is nearly equal to **RSA8192**.

```
...
Key is valid for? (0) 0
...
```

This means that our key **does not expire**.

```
Is this correct? (y/N) y
```

Confirm that yes we want the key not to expire.

```
Real Name: Agantuk Manav
Email: user@example.com
Comment:
```

Enter your Name and email. **Make sure Comment Is Blank**.
This is **Important** for security.

```
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
```

Confirm that you are sure.

**PassPhrase**: This is the first time you are asked to set the pass phrase.
This passphrase would be use to encrypt the storage of your Private Key.
So you can choose a memorable phrase make sure its secure.
If the passphrase is very small it would complain.

Not a good idea to have a small passphrase - click on *Take it any way*.
Its better to choose a **Strong Passphrase** and
use **Password manager** to store it.

Finally your GPG Master Key is ready and installed in the current environment.

**If you Don't need Passphrase - !!!DANGER!!!**
**Key without Passphrase can cause IRREPARABLE DAMAGE !!!**

You have been **WARNED !!!**

To have a **Blank** password:

- Press **OK** in the Password assignment box during key generation.
- Next press **Yes, protection is not needed**
- Again in the Password assignment box Press **OK**
- Next press **Yes, protection is not needed**

After this you would not need any passphrase for your GPG key.

This is **DANGEROUS !! DO NOT DO THIS!!**.

## 5. Check the Key Generation

```sh
gpg --list-keys
```

You should get the Fingerprint now. Its a `40` Character long
`10-digit 16-bit HEX number` string.

The `16-bit` groups are separated by spaces.
It would be very useful to have this Fingerprint handy for later uses.

```sh
echo {complete-masterkey-fingerprint} > export/keyid
```

Replace the `{complete-masterkey-fingerprint}` with the long
HEX number in the fingerprint.

This command would store the fingerprint in a file `export/keyid`.
This would be useful to execute GPG commands.

## 6. Add IDs For Primary Key

Now while creating the GPG key we have already added the primary ID.
However we would need to add more IDs based on what email address and
credentials we would like to validate/authenticate.

```sh
gpg --edit-key --expert $(cat export/keyid)
```

Here is the quick use of our `export/keyid` file we created in the [Step 5](#5-check-the-key-generation).

```
gpg> adduid
...
Real Name: Agantuk Manav
Email Address: user@email.com
Comment:
...
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
```

Type the **Passphrase** to set this new ID.
Repeat the above process again to add another ID.
Once ready. We need to activate the master ID again to be the primary.

```
gpg> uid 1
...
gpg> primary
```

This would set the ID that we want to use by default as primary.
This might not be a big deal but some sites and use-cases need this
to be correct.

```
gpg> save
```

This would save our new IDs into the GPG Master Key and quit the GPG terminal.
```sh
gpg --list-keys
```

This should show all your IDs added.

!!! note "Multiple IDs"
    If you add more than 2 Additional IDs then the order of the addition of
    the ID would determine the final order in the key's ID list.

## 7. Generate the Sub-keys for our GPG Master key

As stated earlier for better security we use the Sub-Keys.
Typically we never load the primary key any where. Its safely stored away
as a **`paperkey`** or some safe in a **USB-Stick**.

```sh
gpg --edit-key --expert $(cat export/keyid)
```

### 7.1 Authentication Sub-Key

We would first generate the Authentication key. This would help in
understanding how we set the options. To have desired characteristics
for the particular key.

```
gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 8
```

We would select the `(8) RSA (set your own capabilities)` since we can
load `RSA-4096` in most **Yubikey** and we really want to customize
the key options.

```
Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Sign Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? A

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Sign Encrypt Authenticate

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? S

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Encrypt Authenticate

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? E

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Authenticate

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? Q
```

We make sure that the Key only has **Authenticate** Set.

```
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
```

We set the Bits to **4096** as discussed earlier for **Yubikey**.

```
...
Key is valid for?(0) 365d
...
Is this correct? (y/N) y
Really create? (y/N) y
```

We need only **1 Year** hence **365d** is entered. You need to enter the
**Passphrase** to generate the key. After that it list the update.
Look out for the key with an `expires:` detail. This should be the
new **Authentication Sub-Key** we just created.

### 7.2 Sign Sub-Key

```
gpg> addkey # For Sign Key
...
Your Selection? 8 # For RSA (set your own capabilities)
...
Your Selection? e # For Disabling Encrypt - Toggle the encrypt capability
...
Your Selection? q # for Quit
...
What keysize do you want? (3072) 4096
...
Key is valid for? (0) 365d # Only 1 Year
...
Is this correct? (y/N) y
Really create? (y/N) y
```

You would need to type the **Passphrase** again.

### 7.3 Encryption Sub-Key

```
gpg> addkey # For Encrypt Key
...
Your Selection? 8 # For RSA (set your own capabilities)
...
Your Selection? s # For Disabling Sign - Toggle the sign capability
...
Your Selection? q # for Quit
...
What keysize do you want? (3072) 4096
...
Key is valid for? (0) 365d # Only 1 Year
...
Is this correct? (y/N) y
Really create? (y/N) y
```

You would need to type the **Passphrase** again.

### 7.4 Save the Keys

```
gpg> save
```

Save all the Sub-keys and quit the GPG terminal.
Next we can validate the generation.

```sh
gpg --list-keys
```

## 8. Export Keys for Storage

We would keep all copies of the keys. At the end we would load the Sub-keys
into the **Yubikey** for use.

```sh
gpg --export-secret-subkeys -a <AuthKeyFP> > export/private-auth-subkey.txt
# Type Passphrase
gpg --export-secret-subkeys -a <SignKeyFP> > export/private-sign-subkey.txt
# Type Passphrase
gpg --export-secret-subkeys -a <EncKeyFP> > export/private-enc-subkey.txt
# Type Passphrase
gpg --export-secret-keys -a <MasterKeyFP> > export/private-master-key.txt
# Type Passphrase
gpg --export-secret-keys <MasterKeyFP> > export/private-master-key
# Type Passphrase
gpg --export -a <MasterKeyFP> > export/public-key.txt
gpg --export <MasterKeyFP> > export/public-key
```

!!! note "Replace correctly"
    Replace the `<AuthKeyFP>`, `<SignKeyFP>`, `<EncKeyFP>` and `<MasterKeyFP>`
    with the respective fingerprints we read at the end of the [Step 7](#7-generate-the-sub-keys-for-our-gpg-master-key).

## 9. Produce the `paperkey`

Typically `paperkey` does not come pre-installed in **Tails** distribution.

Hence you would need to create a special **air-gapped PC** with this installed.
An VM with networking removed is also a good option.

```sh
paperkey --secret-key export/private-master-key \
 --output export/private-paper-key.txt
```

More details on [`paperkey` here](../cli/paperkey.md).

## 10. Revocation Certificate for Primary Key

If ever get your primary key compromised, then this would help to revoke
the trust of your key. Even, if you loose its password.

In our case it would rarely be required. Since we would not use the primary key.
Since as a good measure keeping it ready is not bad.
Just make sure to keep it safe and secure physically.

```sh
gpg --output export/revoke-master-key.txt \
  --gen-revoke <MasterKeyFP>
```

Replace `<MasterKeyFP>` with the fingerprint of the Private Master Key.
You would be prompted to type **Passphrase** again.

## 11. Keep the Key Details Handy

We are ready with all the pieces now for our next step to load the sub-keys to
the **Yubikey**. But, before that its a good Idea to keep a copy of all the
fingerprints in a handy file.

```sh
gpg --list-keys > export/keyDetails
```

This would list out all our fingerprints of sub-keys and primary key.
It would also have the ID we are using. A good reference in case we need
to re-create or modify the keys.

### 11.1 Storing Passphrase in Plain Text can cause IRREPARABLE Harm !!!

You have been **WARNED!!!**.

This is only needed if you don't use a good Password Manager that has
long support life.

Export your passphrase for key in plain text to the export directory.
```sh
echo <passphrase> > export/keypass
```

This might seem counter productive, but let me assure you its needed.
If you ever lost your key, you would need some way to recover.
And, without Passphrase of your key you can't recover it.

!!! note "Encrypted Partitions is best"
    We are already in an **Encrypted partition** its *kind of safe* to assume
    that this *passphrase stored in plain text* is fine.
    You would need to remember the password for this *Encrypted partition*
    at least.

## 12. Loading of Sub-Keys into Yubikey

Yes finally. You can load the keys into any number of **Yubikey**.
We would consider **3 Yubikey** to be a healthy number.
Well its up to your choice.

To check if our **Yubikey** is plugged in and recognized by GPG type the
following command.

```sh
gpg --card-status
```

The serial number printed should match the **Yubikey** you have.

```sh
gpg --edit-key --expert $(cat export/keyid)
```

We want to edit the key for loading only. But we would not save it.

```
gpg> key 2 # This selects the Auth key
...
gpg> keytocard
Please select where to store the key:
  (3) Authentication Key
Your selection? 3
# Enter the GPG Key Pasphrase
# Yubikey ADMIN PIN (default): 12345678
# For new Yubikey you would be asked again to enter this PIN
...
gpg> key 2
...
gpg> key 3 # Sign Key Selected
...
gpg> keytocard
Please select where to store the key:
  (1) Signature Key
  (3) Authentication Key
Your selection? 1
# Enter the GPG Key Pasphrase
# Yubikey ADMIN PIN
...
gpg> key 3
...
gpg> key 4 # Encrypt Key Selected
...
gpg> keytocard
Please select where to store the key:
  (2) Encryption Key
Your selection? 2
# Enter the GPG Key Pasphrase
# Yubikey ADMIN PIN
...
gpg> quit # Must Do this DO NOT SAVE
Save changes? (y/N) n
Quit without saving? (y/N) y
```

Be careful about the last part. You need to `quit`.
Don't save any changes else your private key would get erased.

Use the same process to Load other **Yubikey**.

## 13. Things some times don't go well

Distastes do happen. Many guides talks about creating keys, installing them.
May be using them as well. But they don't tell you if something goes wrong.
There can be any number of risk factors.

### 13.1 Loss of Yubikey

You lost your **Yubikey**. Well for us its nearly ₹5800/- and its
really valuable. So loosing it is not an option. Remember last time we
discussed about the **Healthy number** of **Yubikey** = **3**.
Yes, That's the reason. Even in worst case you loose one, you might
still have 2 in backup.

### 13.2 Deleted Key material during Yubikey loading

You accidentally **deleted your keys !!!.** Ya it happens on bad days.
Or your PC crashed. No problem reload the public key - remember we export
it earlier as `export/public-key`. And with **Yubikey** you are good to go.

```sh
gpg --import public-key
```

While loading the **Yubikey** with `gpg>` you saved by mistake.

```sh
gpg --delete-secret-keys <MasterKeyFP>
...
Delete this key from the keyring? (y/N) y
This is a secret key! - really delete? (y/N) y
# Confirmation dialogs might show up for deletion of sub-keys.
...
gpg --delete-keys <MasterKeyFP>
```

This would remove your Master key that was damaged due to key-material removal.
Here `<MasterKeyFP>` is the fingerprint of the master-key. Remember the
`export/keyid` file we created. This contains the whole fingerprint.
Thus you can use that with `$(cat export/keyid)` instead of `<MasterKeyFP>`.

```sh
gpg --import export/private-master-key.txt
# You would be prompted your passphrase
```

This would import the secret key material into the GPG environment.
Its important to keep the **passphrase** safe.
If you forget the passphrase of the key by now; then you would understand why
in *Step 11.1*', it was suggested to store passphrase to in Password Managers
or worst case to disk if nothing else.

### 13.3 Your Sub-Keys have expired

Well this happens every year round the same time. Since we have used 1 Year
time validity. You would need to adjust the validity and upgrade your keys.
You can import from your backup or if you have stored key material with
physical safety, then you are set.
In both ways you would need the passphrase of the primary key.

We would assume that you have *imported* or loaded your original keys that
have expired.

Lets first upgrade the validity of keys:

```sh
gpg --edit-key --expert $(cat export/keyid)
```

Remember this from the start steps. Yes the same command would be needed here.

```
...
gpg> key 2 # Select the Auth Key
...
gpg> expire # Alter Expiration
Changing expiration time for a subkey.
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 365d
...
Is this correct? (y/N) y
# prompted for Passphrase
...
gpg> key 2
...
gpg> key 3 # Select the Sign Key
...
gpg> expire # Alter Expiration
...
Key is valid for? (0) 365d
...
Is this correct? (y/N) y
# prompted for Passphrase
...
gpg> key 3
...
gpg> key 4 # Select the Encrypt Key
...
gpg> expire # Alter Expiration
...
Key is valid for? (0) 365d
...
Is this correct? (y/N) y
# prompted for Passphrase
...
gpg> save
```

Now all your keys should be updated with a fresh lease of 1 Year.
You can do this any time they would just count days up from the day
the operation is preformed.

Next like earlier we need to Export the keys -
Similar to **[Step 8](#6-add-ids-for-primary-key)**:

Finally load the new sub-keys into **Yubikey** like in
**[Step 12](#12-loading-of-sub-keys-into-yubikey)**.

You are all set.

----
<!-- Footer Begins Here -->
## Links

- [Back to GPG Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
