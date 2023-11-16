# Easier Way to Generate GPG Keys

From Video <https://www.youtube.com/watch?v=xGsixSh6sC4>

## Begin

```sh
gpg --full-gen-key
```

Choices and Selections:

```
(1)rsa and rsa
...
4096 # As big as possible but supported by Yubikey
...
0 = Key does not expire
y
...
Real Name: Agantuk Manav
Email address: user@example.com
Comment:
...
O # Okay
# Setup the Passphrase
...
```

## Check the keys

```sh
gpg --figerprint --fingerprint
```

For easily using the Key ID - pint to file:

```sh
echo $Key_ID_here > keyid
```

## Edit the key

```sh
gpg --edit-key --expert $(cat keyid)
```

Choices and Selections:

```sh
gpg> adduid
Real name: Agantuk Manav
Email: user@email.com
Comment:
...
O # Okay
# Answer the Passphrase that you setup earlier
...
gpg> adduid
Real name: Agantuk Manav
Email: user@email-site.com
Comment:
...
O # Okay
# Answer the Passphrase that you setup earlier
####
# Add all other UID like this
```

Adding a New Authentication Key:

```sh
gpg> addkey
...
(8) RSA (set your own capabilities)
...
# By Default it is - Sign Encrypt
(S) Toggle the sign capability
...
(A) Toggle the authenticate capability
...
(E) Toggle the encrypt capability
...
(Q) Finished
# Key now has only Authenticate Capability enabled
...
4096 # Size for the Yubikey
...
365d # Key to Expire in 1 Year
...
y # Yes confirm the choice
y # Yes really create
# Answer the Passphrase that you setup earlier
```

Adding a New Sign Key:

```sh
gpg> addkey
...
(8) RSA (set your own capabilities)
...
# By Default it is - Sign Encrypt
(E) Toggle the encrypt capability
...
(Q) Finished
# Key now has only Sign Capability enabled
...
4096 # Size for the Yubikey
...
365d # Key to Expire in 1 Year
...
y # Yes confirm the choice
y # Yes really create
# Answer the Passphrase that you setup earlier
```

Adding a New Encrypt Key:

```sh
gpg> addkey
...
(8) RSA (set your own capabilities)
...
# By Default it is - Sign Encrypt
(S) Toggle the sign capability
...
(Q) Finished
# Key now has only Encrypt Capability enabled
...
4096 # Size for the Yubikey
...
365d # Key to Expire in 1 Year
...
y # Yes confirm the choice
y # Yes really create
# Answer the Passphrase that you setup earlier
```

Finally Save the modifications and Quit:

```sh
gpg> save
```

## Key Export to Disk Files

!!! note ""
    All **KeyID** are actually the last *8 hex data* from finger print.

```sh
mkdir export
gpg --export-secret-subkeys -a $AuthSubKeyID > \
 export/user_$AuthSubKeyID.private-auth-subkey.txt
# Answer the Passphrase that you setup earlier
gpg --export-secret-subkeys -a $SignSubKeyID > \
 export/user_$SignSubKeyID.private-sign-subkey.txt
# Answer the Passphrase that you setup earlier
gpg --export-secret-subkeys -a $EncSubKeyID > \
 export/user_$EncSubKeyID.private-enc-subkey.txt
# Answer the Passphrase that you setup earlier
# Save Master Key in Ascii format
gpg --export-secret-keys -a $(cat keyid) > \
 export/user_$MasterKeyID.private-master-key.txt
# Answer the Passphrase that you setup earlier
# Save Master key in Binary Format
gpg --export-secret-keys $(cat keyid) > \
 export/user_$MasterKeyID.private-master-key
# Save the Master Encryption Key
gpg --export-secret-subkeys -a $MasterEncKeyID > \
 export/user_$MasterEncKeyID.private-master-encrypt.txt
# Answer the Passphrase that you setup earlier
# Export the Public Key
gpg --export -a $(cat keyid) > \
 export/user_$MasterKeyID.public-key.txt
```

Generate `paperkey`:

```sh
paperkey --secret-key \
 export/user_$MasterKeyID.private-master-key \
 --output export/user_$MasterKeyID.private-paper-key.txt
```

## Yubikey Export of Keys

!!! note "Dependency"
    Install `scdeamon` package for Yubikey processing.

```sh
# Check the Yubikey for keys
gpg --card-status
```

Place the Keys into Slots of Yubikey:

```sh
gpg --edit-key $(cat keyid)
...
gpg> key 2 # Select the Authentication Key
...
gpg> keytoard
...
(3) Authentication Key
# Answer the Passphrase that you setup earlier
# Admin Pin of the Yubikey would be Asked
## By default it is 12345678
# Answer the Admin Pin again for confirmation
...
gpg> key 2 # De-select the key
...
gpg> key 3 # To Select the Sign Key
...
gpg> keytocard
...
(1) Signature Key
# Answer the Passphrase that you setup earlier
# Admin Pin for Yubikey
...
gpg> key 3 # De-select the key
...
gpg> key 4 # To Select the Encryption Key
...
gpg> keytocard
...
(2) Encryption Key
# Answer the Passphrase that you setup earlier
# Admin Pin for Yubikey
gpg> quit
# Not save since we don't want private keys removed yet
...
n # Not Save Changes
y # Quit without saving changes
```

## Export the Whole GPG Configuration

```sh
cp -auxf .gnupg/ export/
```

## Remove the Master Key

**!!! Danger !!!**

```sh
gpg --delete-secret-keys $(cat keyid)
# Remove All keys
y # Yes remove the Key
y # Yes sure delete the Secret Key
# Keep Pressing 'Delete Key' button to delete all the keys.
```

## Configure the Yubikey PIN

Show the Keys in the Card:

```sh
gpg --card-edit
# Shows the all keys
gpg/card> admin
# Start the ADMIN mode
gpg/card> help # List all possible Commands
...
gpg/card> passwd
...
3 - change Admin PIN
# Enter the Default / old Admin PIN
# Enter a NEW Admin PIN
## (twice - Don't need to 8 digits it can be chars)
...
1 - change PIN
# Enter the default / old Normal PIN (default - 123456)
# Enter a NEW Normal PIN
## (twice - Don't need to 6 digits it can be chars)
...
q # Quit changing of PIN
...
gpg/card> quit # Exit
```

## Check if GPG really works

Check GPG Key Signing:

```sh
gpg --clearsign filename
# Type the Normal Yubikey password
cat filename.asc
```

## Force Signing Pin all the time for Yubikey

```sh
gpg --card-edit
...
gpg/card> admin # Set Admin Mode
...
gpg/card> forcesig
...
gpg/card> quit
```

## Update the Expiration Date on Keys

!!! note "Master key Location"
    This would only work if you have the **Master Key available in the PC**.

```sh
gpg --edit-key $KEYID # This is inside the Yubikey
# Set the Correct Key
gpg> key 2
...
gpg> expire
```

## Publish you keys (OPTIONAL)

```sh
gpg --send-keys $KEYID --keyserver $KEYSERVER
```

## SSH Authentication using GPG Keys

Add the Configuration GPG based SSH:

```sh
# Edit the Agent file
vim ~/.gnupg/gpg-agent.conf
```

Edit content:

```
enable-ssh-support
```

Disable the SSH Agent:

```sh
ps axuf | grep agent
...
kill -9 $ssh_agent_PID
kill -9 $polkit_agent_PID
```

Add Authentication config to `.bashrc`:

```sh
export SSH_AUTH_SOCK=/var/run/user/1001/gnupg/S.gpg-agent.ssh
```

Make sure to check the path `/var/run/user/1001/` for current user.
Logout+Login or Restart PC to Activate the configuration.


----
<!-- Footer Begins Here -->
## Links

- [Back to GPG Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
