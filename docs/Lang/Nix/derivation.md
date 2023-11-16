# Nix Language - Derivations

## Basics

```nix
nix-repl> d = derivation { name = "myname"; builder = "mybuilder"; system = "mysystem"; }
nix-repl> d
«derivation /nix/store/z3hhlxbckx4g3n9sw91nnvlkjvyw754p-myname.drv»
```

There are 3 basic things needed `name`, `builder` and `system`.
These are Mandatory.

The variants of `system` are `x86_64-linux` and `x86_64-darwin` for MAC OS.

- Derivations can't touch any thing filesystem other than `$out`.
- Derivations can't access network unless specified.
- *Derivation is only build steps - it is not the result, nor it includes the input materials.*
  The variable `$out` is automatically added to the environment when `nix-build` is executed.

## Derivation in Detail

Detail level derivation looks like:

```nix
derivation {
    name = "hello";
    builder = "/bin/sh";
    args = [
        "-c"
        ''
          echo hello! > $out
        ''
    ];
    system = "x86_64-linux";
}
```

But more common would be :

```nix
let
    pkgs = import <nixpkgs> {};
in pkgs.runCommand "hello" { buildInputs = []; }
   ''
     echo hello > $out
   ''
```

*Derivation is only build steps - it is not the result, nor it includes the input materials.*

The variable `$out` is automatically added to the environment when `nix-build` is executed.

## Input Files

Here is how to include input files:

```nix
let
    pkgs = import <nixpkgs> {};
in pkgs.runCommand "hello" { buildInput = []; }
  ``
    cp ${./talk.md} $out
  ''
```

In this one the path type `./talk.md` would be interpolated.
The local file `talk.md` would first be copied into the `/nix/store` and then referenced.
But writing `cp ./talk.md $out` would be wrong since the derivation would not have access to filesystem.

## No Access to file system

```nix
let
   pkgs = import <nixpkgs> {};
in pkgs.runCommand "hello" { buildInputs = []; }
   ''
   hello > $out
   ''
```

Even if `hello` is installed it would fail.

To make this work we need to add the dependency on the package.

```nix
let
   pkgs = import <nixpkgs> {};
in pkgs.runCommand "hello" { buildInputs = [ pkgs.hello ]; }
   ''
   hello > $out
   ''
```

This would definitely work. Since the `hello` would automatically get included in the build environment.

## No Network access

```nix
let
  pkgs = import <nixpkgs> {};
in pkgs.runCommand "network" { buildInputs = [ pkgs.curl ]; }
  ''
  curl http://1.1.1.1
  ''
```

This would fail even though `pkgs.curl` is included as a dependency.
Since. derivation would not have access to the Network.

## Fetch a Network resource

```nix
let
  pkgs = import <nixpkgs> {};
in pkgs.fetchurl {
    url = "http://grahamc.com";
    sha256 = "1qjjjhb7agq........rkhpw9";
}
```

Unless both the Hash and the url are correct this would not work.

## Print a Derivation

```sh
nix show-derivation /nix/store/z3hhlxbckx4g3n9sw91nnvlkjvyw754p-myname.drv
```

- `.nix` files are like `.c` files

- `.drv` files are intermediate files like `.o` files. The `.drv` describes
    how to build a derivation, it's the bare minimum information.

- `out` paths are then the product of the build (`outPath`)


## Overrides

One can also manipulate the output from a derivation or evaluation.

More details about [Overrides here](./README.md#override-for-package).

## `mkDerivation` An Improved Version

Typically in Nix Packages we never use the native `derivation` function.
In most cases we use the `mkDerivation` which is provided in the standard build environment.
This wraps the native `derivation` with some nice features.

This is the extended version of the built-in `derivation` function.
It is defined in the `<nixpkgs>`.

In the repository its located at `pkgs/stdenv/generic/make-derivation.nix`.

`mkDerivation` takes the following Arguments:

- `pname` - Name of the package , used in Nix Store path
- version
- src
- srcs
- buildInputs
- configureFlags
- patches
- meta
- And many more...


----
<!-- Footer Begins Here -->
## Links

- [Back to Nix Language Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)

