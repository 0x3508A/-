# SSH using GPG Keys

References:

- <https://opensource.com/article/19/4/gpg-subkeys-ssh>
- <https://www.ssh.com/academy/ssh/authorized-keys-file>

## 1. Enable SSH using GPG Keys

```sh
echo "enable-ssh-support" > ~/.gnupg/gpg-agent.conf
chmod 600 ~/.gnupg/gpg-agent.conf
```

The first command enables the **GPG-Agent** to allow **SSH**. The second
line configures the permissions for the file.

## 2. Next we need to **Add the Keys for SSH Use**

```sh
echo KEY-GRIP-Number >> ~/.gnupg/sshcontrol
```

This would allow the key with the respective `KEY-GRIP-Number` to be
used as a default key for SSH. The `KEY-GRIP-Number` can be obtained
from the command:

```sh
gpg -K --with-keygrip
```

!!! note "Authentication Permission"
    The Key selected for the SSH over GPG must have **Authentication or (A)**
    Attribute set for the Key.

## 3. Start GPG-AGENT on login

Next We need to add the following Lines to our `.bashrc` file:

```bash
...
export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)
gpgconf --launch gpg-agent
...
```

These line tell **SSH** how to access the `gpg-agent`. This is done by
changing the value of the `SSH_AUTH_SOCK` environment variable.

## 4. Obtain Public Keys for SSH

Next to get the Public Keys to be installed for SSH Access:

```sh
ssh-add -L
```

This command would list all the **SSH Public Keys**.
You can copy the respective blocks and store into the
`~/ssh/authorized_keys` file.

## Check if GPG Keys can be used for SSH
```sh
gpg -K --with-keygrip
```

This command would display the install Private keys that can be used for
***GPG encryption,decryption,signing and authentication via SSH***.


----
<!-- Footer Begins Here -->
## Links

- [Back to GPG Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
