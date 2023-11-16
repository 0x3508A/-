# Nix Language - NixOS Modules

Intro into the world of [NixOS Modules][1]. They are composable configurations
for the [NixOS](https://nixos.org/) Operating System.

## Generic Configuration Structure

```nix
{ config, lib, pkgs, modulesPath, ... }:
with lib;
{
    imports = [];
    # Schema
    options = [];
    config = {
        # Anything not in Imports, Options would be
        #  included here automatically.
    };
}
```

#### Even simplified approach

```nix
{ pkgs, ... }:
{
    networking....
}
```

## Module's Anatomy

Let's now look at how the module is composed. And what each item means.

### 1. Input Parameters

As we can see there are multiple input parameters.

```nix
{ config, lib, pkgs, modulesPath, ... }:
...
```

We would try to look into the most common parameters:

- **`config`** = This is the configuration from the fix-point result.
   This would would intern include the `config` part specified in the
   current module file as well.

- **`lib`** = This is a bunch of useful functions passed in by
   **`nixos-rebuild`** command. Most commonly used are:

   - To override some *configs* is `lib.mkForce`.
   - To create an *option* is `lib.mkOption`
   - To merge some *configs* upon conditions `lib.mkMerge`
   - To optionally perform *configs* upon conditions `lib.mkIf`
   - To evaluate sub-modules expressions `lib.evalModules`
   - This is the reason why the whole module expression is enclosed
     in `with lib;`

- **`pkgs`** = This is the input package set most commonly
   **`<nixpkgs>`** needed for overrides and options. It is derived from
   `NIX_PATH` environment variable.

- **`modulesPath`** = This is the location of the module directory of NixOS.
   typically this is `<nixpkgs/modules>`.

One can modify the system level config using the following

```sh
$ export NIX_PATH="nixos-config=/path/to/configuration.nix:${NIX_PATH}"
```

### 2. `imports` List

Imports are paths to other **NixOS** modules that should be included in
the *evaluation of the system configuration*.

A default set of modules is defined in [`nixos/modules/module-list.nix`][2].
These don't need to be added in the import list.

##### Sub-Module Imports and use of `modulesPath`

```nix
{ config, lib, pkgs, modulesPath, ... }:
with lib;
{
    imports = [
        "${toString modulesPath}/module-name.nix"
        ...
    ];
    ...
}
```

This would help to inherit the `<nixpkgs>` of present version in the system.
Else, it would difficult to make the module compatible with multiple
sub-modules and varying `<nixpkgs>` versions.

### 3. `options` Attribute Set

These define module parameters that are configurable. Typically the
`mkOption` function is used for this.

For More insight refer to :

<https://nixos.org/manual/nixos/stable/index.html#sec-option-definitions>

### 4. All others in `config` Attribute Set

Yes for the most part of the `configuration.nix` or any other
module we tend to use the `config`.

Typically the two examples shown below will have the same effect:

Configuration Module 1:
```nix
{ config, lib, pkgs, modulesPath, ... }:
with lib;
{
    config = {
        ...
        networking.hostname = "nixer";
        ...
    };
    ...
}
```
Configuration Module 2:
```nix
{ config, lib, pkgs, modulesPath, ... }:
with lib;
{
    ...
    networking.hostname = "nixer";
    ...
}
```

The second one is just easier to read. But typically all modules in
the **NixOS** repositories use **`config`**.

#### Finding Configurations available

**<https://search.nixos.org/options>**

This offers a **search engine** that details about the options
available for *configuring the packages*.

!!! note ""
    Some packages are actually better to be enabled from configurations as
    they include a lot of dependent units that need to be configured.

### 5. `nixos-generate-config` Gives us Module to Start

While installing **NixOS** one needs to use the `nixos-generate-config`.
This helps to generate the `configuration.nix` and
`hardware-configuration.nix` files.

Typically the `hardware-configuration.nix` contains the system specific
configuration detected by `nixos-generate-config` command.

And `configuration.nix` is a basic set of utilities and configuration options
needed to build a functional NixOS system. Though it would be far from complete
it helps to give us a starting point for creating a **NixOS** computer.

The command format looks like this:

`nixos-generate-config [options] <target>`

A common example can be:

`nixos-generate-config --root /mnt`

Where `/mnt` is the location we have mounted our target system disk partitions.
And the `--root` option indicates that we need to create the configuration
as the `root` or `superuser`.

For more information on what **configuration options** are available in the
`configuration.nix` - refer to

<https://nixos.org/manual/nixos/stable/options.html>.

Here is some more details on installing **NixOS**:

 <https://nixos.org/manual/nixos/stable/#sec-installation-installing>.

One needs to also partition the system disk for installing **NixOS**, hence
some more steps are necessary.

### 6. `nixos-rebuild` One command to Rule them All

Any time the **NixOS** configuration is update we can use this command to
build, test and evaluate our new system. The same can be done for any custom
configurations as well. Once satisfied then the new configuration can be
switched into or a remote machine can be switched into the new configuration.

Here are few of the commonly used sub-commands:

- **`build`** this would create a new derivation of the profile. Its helpful
    to check if a new configuration or an update does not cause a build failure.
- **`test`** this would *build* and then *switch* into a *temporary instance*
    of the profile derivation. But after reboot the changes would be lost.
- **`switch`** this would *build* and *enter* into a new configuration.
    This is a parament change. It would create the symlink into the profile.
    This would also update the Nix generation if using `nix-env`.
- **`build-vm`** this would perform the *build* and create a *qemu-vm* using
    the current system as the base. This VM can then be tested for any problems
    with the configuration update. Once evaluated the VM can easily be deleted.

#### For Remote Machines

```sh
$ nixos-rebuild --target-host 192.168.1.1 -I nixos-config=remotehost.nix build
```

The IP address specified `192.168.1.1` should corrected as per
the *remote machine*.

Also, one needs to have permission to access this *remote machine*.

## References

Here are some reference resources used for this article:

1. [NixOS : How to Write a Module][1]
2. [Unoffiical NixOS Wiki : Modules](https://nixos.wiki/wiki/Module)
3. NixFriday : Modules Video <https://www.youtube.com/watch?v=5doOHe8mnMU>

### Video References

- Nix Friday - Introduction to Nix Modules

    <https://www.youtube.com/watch?v=5doOHe8mnMU>

- Nix Overlays <https://www.youtube.com/watch?v=W85mF1zWA2o>


  [1]:https://nixos.org/manual/nixos/stable/#sec-writing-modules
  [2]:https://github.com/NixOS/nixpkgs/blob/master/nixos/modules/module-list.nix



----
<!-- Footer Begins Here -->
## Links

- [Back to Nix Language Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
