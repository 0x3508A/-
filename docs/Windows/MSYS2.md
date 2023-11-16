# MSYS2

Home Page: <https://www.msys2.org/>

## Installation

`winget` installation command (Administrator Command Prompt):

```batch
winget install --id MSYS2.MSYS2 -e
```

## MSYS2 Configuration Windows

### System Upgrade

```sh
# Full Update and System Upgrade
pacman -Syyuu
```

### Update

```sh
# Normal Update and Upgrade
pacman -Syyu
```

### Basic Tools

```sh
# Packages to install for normal operations
pacman -Syu aspell aspell6-en diffutils git \
  patch wget rsync p7zip
```

### More spell checkers
```sh
# To get Hunspell
pacman -Syu mingw64/mingw-w64-x86_64-hunspell \
  mingw64/mingw-w64-x86_64-hunspell-en
```

### Emacs specific spell checker
```sh
# Better aspell
pacman -S mingw64/mingw-w64-x86_64-aspell
pacman -S mingw64/mingw-w64-x86_64-aspell-en
## Source https://emacs.stackexchange.com/a/45752
```

### Windows Input helper

```sh
pacman -Syu winpty
#  mingw64/mingw-w64-x86_64-youtube-dl \
#  mingw64/mingw-w64-x86_64-python-pyserial \
#  mingw64/mingw-w64-x86_64-python-pip
```

## Add Full Windows PATH to the MSYS2 terminal

This feature allows us to use the whole Windows install programs inside MSYS2 terminal.
A very important fix.

Reference <https://stackoverflow.com/a/50992294>

Run `msys2_shell.cmd -use-full-path`

This is typically needed  for the Right Click menu or where we are invoking from scripts.

For MINGW64 `msys2_shell.cmd -mingw64 -use-full-path`
or

un-comment `MSYS2_PATH_TYPE=inherit` in `msys2.ini`

(can be found in the installation directory of MSYS2 - `msys64`).

## Auto-launching `ssh-agent` on MSYS2

Reference: <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows>

You can run `ssh-agent` automatically when you open bash or Git shell. Copy the following lines and paste them into your `~/.profile` or `~/.bashrc` file in Git shell:

```sh
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

!!! warning "Windows OpenSSH"
    Its better to use the Windows SSH since its integrated into the OS well
    and also works with *Git for windows*.

## All MSYS2 Packages repository for additional packages on Git-Bash-Windows

<https://repo.msys2.org/msys/x86_64/>

This can be used to add commands to GIT Bash for Windows

Reference: <https://superuser.com/a/1464552>

----
<!-- Footer Begins Here -->
## Links

- [Back to Windows Hub](./README.md)
- [Back to Root Document](../README.md)
