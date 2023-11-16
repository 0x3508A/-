# `ssh` / `openssh`

Secure Shell Client & **OpenSSH** Server.


## Configuration Notes for OpenSSH Server on Raspberry Pi

Improving SSH Security a tutorial video
<https://www.youtube.com/watch?v=xVW1fGRlRkE>


## Getting Started with OpenSSH Server a Full Guide

A deeper guide video
OpenSSH Full Guide
<https://www.youtube.com/watch?v=YS5Zh7KExvE>

-   Everything you need to get started!

## Edit OpenSSH Server Configuration in `/etc/ssh/sshd_config`

```sh
sudo vim /etc/ssh/sshd_config
```

## Disable Root Login on OpenSSH Server Configuration

In the `/etc/ssh/sshd_config` file:

```sh
...
PremitRootLogin no
...
```

## Restart OpenSSH Server Service

```sh
sudo systemctl restart sshd
```

## Create a SSH Key for VM or Server

```sh
ssh-keygen
...
# Set the Location to /home/user/.ssh/id_rsa_remoteMachine
...
# Add the Password for the Key
...
```

!!! note
    For the Above Command to work you must have the `.ssh` directory in the host PC as writable. Later reconfigure it with secure permissions.

## Copy the SSH Public Key to remote machine

```sh
ssh-copy-id -i ~/.ssh/id_rsa_remoteMachine.pub user@remoteIP
```

Copy the ssh Key if only public key is available with `-f` option:

```sh
ssh-copy-id -f -i ~/.ssh/id_rsa_remoteMachine.pub user@remoteIP
```

!!! warning
    The key used here should always be a ***Public Key ONLY***.

## Disable Password authentication for OpenSSH Server

!!! warning
    You must copy your SSH before doing this.

In the `/etc/ssh/sshd_config` file:

```sh
...
PubkeyAuthentication yes
...
PasswordAuthentication no
...
```

And finally [Restart the SSH service](#restart-openssh-server-service).

## Change the Default Port on OpenSSH Server

In the `/etc/ssh/sshd_config` file:

```sh
...
Port 2038
...
```

And finally [Restart the SSH service](#restart-openssh-server-service).

This helps to ignore port scans of `22` port by default.

Typical command for `SSH` into remote machine now changes:

```sh
ssh -p 2038 user@remoteIP
```

For `scp` command it would become:

```sh
scp -P 2038 ...
```

## Find the `sshd` OpenSSH Server port on PC

```sh
sudo netstat -tulpn | grep ssh
```

## Allow only specific users to login in OpenSSH Server

In the `/etc/ssh/sshd_config` file:

```sh
...
Port 2038
AllowUsers user user2
...
```

!!! note
    This only works for **PC based Linux OS** not *Embedded Linux* targets like Raspberry Pi.

## SSH tunnelling for fun and profit

<https://www.everythingcli.org/ssh-tunnelling-for-fun-and-profit-ssh-config/#gfm-10>

## SSH Client Configuration file `~/.ssh/config`

Correct Permission for security [PermissionCalculator](http://permissions-calculator.org/)

Where exactly is the configuration stored : `~/.ssh/config`

What should be Right Permissions for the folder:

```sh
chmod 0700 ~/.ssh
```

What should be Permissions for files in the `~/ssh` folder:

```sh
chmod 600 ~/.ssh/*
```

The above would ensure basic safety of your *configuration* and **SSH directory**.

## Various vulnerability list on SSH client configuration

causing **SSH Tunneling HACK**.

<https://www.everythingcli.org/ssh-tunnelling-for-fun-and-profit-ssh-config/#gfm-10>


## Hostname Alias in SSH

Reference :

- Configuration to add Alias for hosts

    <https://friendo.monster/log/ssh_config.html>

`~/.ssh/config` lets you make what we can call as *ssh aliases*. You can
give connections short names and specify various connection settings ‒
pretty much anything you can pass to `ssh`'s CLI can be configured here.

Here's a simple example:

```sh
Host nas
    Hostname 192.168.1.10

Host laptop
    Hostname 192.168.1.11
    User laptop-me

Host someserver
    Hostname 203.0.113.1
    User seriousthings
    Port 10100
    PubkeyAuthentication yes
    Identityfile ~/.ssh/id_rsa_seriousthings

Host *
    PubkeyAuthentication no
```

Then you are able to do, for example:

```sh
ssh nas
```

These settings apply from top to bottom, cumulatively. So you want to
structure this file with specific rules above general rules, otherwise
the general rules will override the specific, which is not what you
want.

One nice benefit is that you'll get autocompletion of hosts defined in
`~/.ssh/config` when using ssh under `bash` or `zsh` (at least).

`~/.ssh/config` can also be used to shore up some of the leaky parts of
sshing, as described at the link below ‒ the whole page, and other
articles in the series (linked at the top) are worth a read if you use
ssh a decent amount.

### Structure of SSH Client Config

This in the `~/.ssh/config` file:

```sh
Host c1
    HostName 192.168.0.1

Host c2
    HostName 192.168.0.2

Host c*
    User cytopia
    Port 10022
    PubkeyAuthentication yes
    IdentityFile ~/.ssh/id_rsa__c_cytopia@cytopia-macbook

# For github usage
Host github.com
    HostName github.com
    User git
    PubkeyAuthentication yes
    IdentityFile ~/.ssh/id_github

Host *
    PubkeyAuthentication no
    IdentitiesOnly yes
    HashKnownHosts yes
    ServerAliveInterval 30
    UseRoaming no
```

### Aliases for the IP Addresses

A Section of the `~/.ssh/config` file:

```sh
Host c1
    HostName 192.168.0.1

Host c2
    HostName 192.168.0.2
...
```

The first 2 sections are **Alias** defined.
Now this means you use `ssh` like this:

```sh
ssh c1
ssh c2
...
```

Internally this becomes:

```sh
ssh cyptopia@192.168.0.1 -p 10022 \
    -i ~/.ssh/id_rsa__c_cytopia@cytopia-macbook \
    -o PubkeyAuthentication=yes \
    -o ServerAliveInterval=30
# Similar to
ssh c1
```

### Rules for Writing the `~/.ssh/config` file

You can basically categorize `ssh blocks` into three stages:

1.  Most specific (without any wildcards)
2.  Some generalization (with wildcard definitions)
3.  General section (which applies to all).

> So always remember:
>
> 1.  Specific definitions at the top
> 2.  General definitions at the bottom
> 3.  Whenever a specific value has been found, it cannot be overwritten by
>     values defined below.

!!! note
    Regarding the `~/.ssh/config` it is important to keep track of is the `Host` section (aligned to the left). Notice here that the general definitions are at the very top and more wildcard definitions (using the asterisk `*`) are followed below.

If you want to ssh connect to `c1` (`ssh c1`), the file is read as follows:

1.  Find section `Host c1` and use its corresponding `HostName`
    (`192.168.0.1`)
2.  Find more general section `Host c*` and use their values (User, Port,
    etc).
3.  Find most general section `Host *`
    1.  Don't use `User` as it has already been defined for this
        connection in `c*`
    2.  Don't use `Port` as it has already been defined for this
        connection in `c*`
    3.  Don't use `PubkeyAuthentication` as it has already been defined
        for this connection in `c*`
    4.  Use `ServerAliveInterval` as there is no previous definition.

### Autocompletion in SSH Client

You will have autocompletion (at least under `bash` or `zsh`) for every
host and **every alias defined** in `~/.ssh/config` file.
This is true for hosts and even IP addresses.

When you type `ssh 1` and hit tab:

```sh
$ ssh 1
192.168.0.1     192.168.0.2     192.168.0.3     192.168.0.4
```

Note: We have replaced the IP addresses with internal once.

Here is another Example with **alias**:

```sh
$ ssh c
c1     c2
```

### To fix Identity leak problem via SSH keys

Add these to your `~/.ssh/config`:

```sh
...
# Turn off pubkey auth for all hosts
Host *
    PubkeyAuthentication no
    IdentitiesOnly yes
    ServerAliveInterval 30
...
```

!!! warning
    This would **not allow** you to login SSH Servers **without public Keys installed**.

It is **Safer** to have it as:

```sh
...
# Generic Config
Host *
    #PubkeyAuthentication no
    #IdentitiesOnly yes
    HashKnownHosts yes
    ServerAliveInterval 30
    UseRoaming no
...
```

### Protect `known_host` SSH against Harvesting Attack

```sh
...
# Generic Config
Host *
    ...
    HashKnownHosts yes
...
```

This would enable unique hash for the Host entries.
**Note:** Make sure to **remove the existing** `~/.ssh/known_hosts` file.

1.  Known Problems with `HashKnownHosts yes` setting

    !!! warning
        Some times when the Host configuration changes this would **block the connection**.

    If you get the message:

    ```
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that a host key has just been changed.
    ......
    ......
    ```

    **In the above Example:**
    Installing the public Key into a *particular host* that was *earlier accessed* using **username password**.

    **Solution**:

    - Delete the `~/.ssh/known_hosts` file or
    - remove the *particular host* entry in the file.

    It would fix this problem.

## Copying SSH Public Key to Remote Host

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub user@remoteip
```

Replace the `id_rsa.pub` with your **public key file**.

## Detailed SSH Client Command with options

```sh
ssh cyptopia@192.168.0.1 -p 10022 \
    -i ~/.ssh/id_rsa__c_cytopia@cytopia-macbook \
    -o PubkeyAuthentication=yes \
    -o ServerAliveInterval=30
```

This command uses a **specific key** with option for **public key auth**.
And **timeout of 30seconds**.

## Restarting `ssh-agent` Process for SSH Client Config to take Effect

After changing the **SSH Config** one needs to restart the `ssh-agent`:

```sh
killall ssh-agent; eval `ssh-agent`
```

## SSH Client Login Error Due to existing Host in `known_hosts`

If you get the message:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
......
......
```

Its possible that you have some other Host configuration for the same IP
or Raspberry Pi host name.

-   Delete the `known_hosts` and `known_hosts.old` file in your *user
    home* `.ssh` directory.
-   This would remove the older host config for the given host name.

## SSH Client Config Test for Github

In order to test out the *Github SSH keys* for Identity configured
correctly use the following command:

```sh
ssh -T git@github.com
```

**Output:**

```
Hi <github-user-name>! You've successfully authenticated, but GitHub does not provide shell access.
```

!!! note
    You need to have your `ssh-identity` registered using `ssh-add` command.

## Check the Logs for SSH Login Attempts

```sh
sudo tail -n 100 /var/log/auth.log
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)