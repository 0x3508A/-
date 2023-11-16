# `nix` Package Manager

The functional [`nix` language](../../Lang/Nix/README.md) based package manger
for [NixOS](../Distro/nixos.md).

Some Good References:

- **Nix Pills** Main Learning Guide

    <https://nixos.org/guides/nix-pills/>

- Video Intro To Nix

    <https://www.youtube.com/watch?v=D5Gq2wkRXpU>

- Video Nix: How and Why it Works

    <https://www.youtube.com/watch?v=lxtHH838yko>

- Video Overview of Syntax and features

    <https://www.youtube.com/watch?v=m4sv2M9jRLg>

- **[Nix-Fundamentals Document (PDF)](./nix/Nix-Fundamentals.pdf)**
- Package Searching <https://search.nixos.org/packages>
- Nix Package Manager Guide <https://nixos.org/manual/nix/stable/>
- Nixpkgs Manual <https://nixos.org/manual/nixpkgs/stable/>
- Curated List of Nix (Awesome Nix) <https://github.com/nix-community/awesome-nix>

## Rajiv's Tutorial on Setting up Nix as single user

[Full Write-up Here](../../Lang/Nix/bootstrap-minimal-nix-and-nix-flakes.md)

## [Nix Learning on Manjaro](../ArchLinux/Manjaro-nix.md)

- [Setting Up Nix on Manjaro](../ArchLinux/Manjaro-nix.md)

### Nix Learning using NixOS ISO

```sh
# Add the Channels
nix-chnnels --add https://nixos.org/channels/nixos-20.09 nixpkgs
# Update Channel
nix-channel -v --update
## Add A basic package to get the Profile
nix-env -i -v file hello
## Execute to See if it was working
hello # Should print "Hello World!"
## ~/.nix-profile is ready now
# Check the Currently installed things
nix-env -q
```

## Nix REPL

To enter the shell:
```sh
nix repl
```

- To Quit type `:q`.

- For Help `:?`.

## Commands

- `nix` Combo Command

    - `repl` for direct nix *REPL* command-line. `:?` for help and `:q` to Quit.
    - `show-derivation` details a supplied derivation path in good format
        - Needs `--experimental-features nix-command`  in the newer version of Nix.

- `nix-store` = Nix Store management

    - Find Refs in particular package = `-q --references <path>`
    - Find which other packages refers to this package = `-q --referrers <path>`
    - Display all package dependencies recursively in a tree form = `-q --tree <Path>`
    - Do Cleanup or Garbage Collect = `--gc`

- `nix-build` = Actually

    - `-A` is used to access a particular attribute in the supplied main file like:

        ```sh
        nix-build default.nix -A hello
        ```

- `nix-collect-garbage` = Cleanup **nix store** Command for un-used references

  - Remove all previous generations `-d` or `--delete-old`
  - Dryrun the cleanup `--dry-run`

- `nix-instantiate` = instantiate store derivations from Nix expressions

  - `--eval` To show the output but do not build
  - Automatically used in `nix-build`

- `nix-prefetch-url` = Command to *download* file to **Nix Store** and show
    the resulting *Nix file* compatible **SHA256 Hash**.

  - `--print-path` option shows the *Nix Store path* where the resource was downloaded.
  - `--extract` option extracts the archive downloaded and shows the combined hash.

- `nixos-rebuild` Command to work on NixOS installation

    - `build` = Just performs the Build and not switch

        - It internally does a `nix-build` of a given system module
        - `nix-build '<nixpkgs/nixos>' -A system`
        - This builds the derivation that explains the whole system.

    - `test` = Only generate a temporary build to test if everything is working
    - `build-vm` = Create a VM of current Configuration. It can open a
    - `switch` = Build and Switch

        - It does the `build` command as above.
        - The it links the derivation to `/nix/var/nix/profiles/system`
        - And finally it shifts the `/run/current-system` to this new link.
            By running the script `<drv>/bin/switch-to-configuration switch`.

- **DO NOT USE `nix-env` and `nix-channels` They are Deprecated**

- `nix-env` = Current Environment Manipulation and Generation

    - Install `-i`
    - list `-q`
    - Get Generations `--list-generations`
    - Move to A generation `-G <GenNumber>`
    - Roll-back the Last generation `--rollback`
    - Remove a particular package `-e <packag-name>`
    - Remove all Packages `-e '*'`
    - Remove all Packages in single transaction - `--remove-all` or `-r`
    - Upgrade all in the current environment `-u`
    - Upgrade all packages in all generations for current profile `--always`

- `nix-channel` = Nix Source Channel management tool

    - Add `--add <URI> <Name>`
    - Update `--update`
    - Get Generations `--list-generations`

## Enter the Environment - If Manually Installed

```sh
source ~/.nix-profile/etc/profile.d/nix.sh
```

## Install a software into the Environment

!!! note ""
    this will perform a **derivation**.

```sh
nix-env -i hello
```

## Check How many Generations has Passed

```sh
nix-env --list-generations
```

## Display Installed Packages in the Environment


Note the `--out-path` additionally prints the actual path where the
particular installation is done.

```sh
nix-env -q --out-path
```

## Find out how know versions of a Particular software are installed

```sh
nix-store -q --references `which hello`
```

## List the Available Channels Installed

```sh
nix-channel --list
```

## Upgrade the System

```sh
nix-channel --update
nix-env -u --always # With do a new Generation
```

Optionally Execute the Cleanup:

```sh
nix-collect-garbage
```

## Repeatedly running `nix-instantiate` to evaluate a file

We can use the [`entr`](./entr.md) tool to run such a command.
Also naming the file `default.nix` would automatically make it the
first file to be executed.

```sh
ls | entr -c bash -c "nix-instantiate --strict --eval --argstr input 'Hari Aum' --json | jq"
```

Additionally we use the JSON processor [`jq`](./jq.md) tool.

## Nix Learning Journal

#### Video Links for Nix-flakes

- [X] Nix Flakes 2019 video

    <https://www.youtube.com/watch?v=UeBX7Ide5a0>

- [X] Nix Flakes new in Nix 2.4 video

    <https://www.youtube.com/watch?v=98EwejpIJzE>

- [X] Nix Flakes rC3 Conference 2020 video

    <https://www.youtube.com/watch?v=QXUlhnhuRX4>

#### Things ToDo

- [X] XFCE Nix Install and Script Update Tried it on
    2021-04-13 22:05:40 IST but failed.
- [X] XFCE Nix Install Try 2 on 2021-04-14 19:11:00 IST
  - Did get to boot but it was very slow
  - Pausing further work due to other Priority tasks as of 2021-04-14
  - [Configuration.nix](./nix/configuration.nix)
  - Hope to keep going in the future.
- [ ] Re-read the Nix-pills and Rajiv configuration before attempting again.
- [ ] Update Config with All tools needed to make it feel like Manjaro


#### Nix Modules `2021-04-14`

- Video

    <https://www.youtube.com/watch?v=I_KCd46B8Mw>

- NixOS Reference - Writing NixOS Module

    <https://nixos.org/manual/nixos/stable/#sec-writing-modules>

- NixOS-Wiki <https://nixos.wiki/wiki/Module>

#### Conclusion 2021-05-18

I got the XFCE working but still nothing close to Manjaro.
I guess after spending more that 300 hours on Nix.
I have to chosen to close the chapter on getting a NixOS for now.
Manjaro is somewhat mid-way between a fully custom os like Ubuntu and
total raw Arch / FreeBSD. As of now all my business needs and stability
requirements are fulfilled by Manjaro. Hence I have concluded while it
would be nice to have total control and build a custom distro, I will
stick to Manjaro for now. Till NixOS becomes as well documented and
supported as Arch or Debian. Yes there would be problems with a bit
of patience and google I can solve most of the problems in Arch.
In case of Nix it would not be so.
Hence like Rust the Nix journey ends here `2021-05-18`.
But, there is more when I come back here wiser that me of today.

----
<!-- Footer Begins Here -->
## Links

- [Back to Nix Language Hub](../../Lang/Nix/README.md)
- [Back to NixOS Article](../Distro/nixos.md)
- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
