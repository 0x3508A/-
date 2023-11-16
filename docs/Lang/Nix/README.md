# Nix Functional Configuration Language

Functional language followed by the famous [Nix Package Manager](https://nixos.org/).

Better description <https://nixos.wiki/wiki/Nix_Expression_Language>

## Topics

- **[`nix` Package Manger](../../Linux/cli/nix.md)** - The actual Nix Package Manger article.
- **[Strings](#type-strings)**
- **[Path Type](#type-path)**
- **[List Type](#type-list)**
- **[Attribute Set, Recursive Attribute Set and Argument Set](#type-attribute-set)**
- `if` -**[IF Expressions](#if-expression)** - yes not Statement
- `let` - **[LET Expression](#let-expression)** - These are more akin to *local variables*.
- `with` - **[WITH Expressions](#with-expression)**
- `assert` - **[ASSERT Expression](#assert-expression)**
- **[Functions](#functions)** - The functional way of functions.
- `//` - **[Union Operator](#union-operator)**
- `or` - **[OR Operator](#or-operator)**
- `?` - **[Condition Operator](#condition-operator)**
- `++` - **[List Concatenation Operator](#list-concatenation-operator)**
- `rec` - **[REC keyword](#rec-keyword)**
- `inherit` - **[INHERIT Keyword](#inherit-keyword)**
- `builtins` - **[Standard Library in Nix](#builtins-standard-library-in-nix)**
    - `import` - **[IMPORT function](#import-function-part-of-builtin)**
- **[Derivation](./derivation.md)** - An important way to expressing configuration.
    - [`mkDerivation`](./derivation.md#mkderivation-an-improved-version) the Better One.
- **[Override](#override-for-package) for Packages**
- **[Fixed Point Evaluations](./fix-point.md)** - A key concept clarifying the way of Lazy Functional reduction.
- **[Overlays](./overlay.md)** - Powerful customization of standard units in Nix.
- **[NixOS Modules](./modules.md)** - An essential way format and create reusable configurations.
- **[Bootstrap Install of Nix with Nix Flakes](./bootstrap-minimal-nix-and-nix-flakes.md)** - Original instructions by Rajiv on Nix.

### Nix - Taming Unix With Functional Programming

<https://www.tweag.io/blog/2022-07-14-taming-unix-with-nix/>

This gives a good understanding into how thew Nix Aims to solve the problem of
looking at system configuration.
In the Nix world the system is a composed-able memory problem.
Than just a bunch of files.

### Nix Blog for learning in stage by state manner

<https://ianthehenry.com/posts/how-to-learn-nix/>


## From `lib`

> `lib.mkForce` - Certain or Force Override like CSS `!important`

## Type Strings

Two ways to write strings:

```nix
nix-repl> Tat-Sat = "Tat Sat"
nix-repl> "Hari Aum \${Tat-Sat} !"
"Hari Aum ${Tat-Sat} !"
nix-repl> ''Hari Aum ''${Tat-Sat} !''
"Hari Aum ${Tat-Sat} !"
nix-repl> ''
      Hari Aum ${Tat-Sat} !
    ''
"Hari Aum Tat Sat !\n"
```

Normally the notation `"${Tat-Sat}"` the variable `Tat-Sat`.
We are Escaping it in different ways.
Multi line strings using double single quote `''` are auto aligned and
space trimmed.

### String Interpolation

This happens when the interpretation of the variable and conversion happens automatically.

```nix
pkgs:
  ''
  ${pkgs.hello}/bin/hello > $out
  ''
```

Another example shows how we use a [`builtins`](#builtins-standard-library-in-nix) function `toString`.

```nix
nix-repl> let a = 333; in with { a = 1; b = 2; };
"In this string we have access to ${toString a} and ${toString b}"
"In this string we have access to 333 and 5 hello"
```

Escaping `${...}` within double quoted strings is done with the *backslash*.
Within *two single quotes*, it's done with `''`:

```nix
nix-repl> "\${foo}"
"${foo}"
nix-repl> ''test ''${foo} test''
"test ${foo} test"
```

## Type path

Yes Path is also a Data type.

There can be 3 ways to specify and very special one at the end:

```nix
nix-repl> relative_path = ./Downloads/chains
nix-repl> relative_path
/home/nix-user/trials/Downloads/chains
nix-repl>
nix-repl> absolute-path = /nix/store
nix-repl> absolute-path
/nix/store
nix-repl> user-home-path = ~/.config/nix
nix-repl> user-home-path
/home/nix-user/.config/nix
nix-repl> pkgs = <nixpkgs>
nix-repl> pkgs
/home/nix-user/nixpkgs
```

The last one `<nixpath>` is the special one. It fetches the Key value from
`NIX_PATH` environment variable. In this case the key is `nixpkgs` and
our `NIX_PATH` looks like.

```sh
$ echo $NIX_PATH
nixpkgs=/home/nix-user/nixpkgs
```

Another thing is that Nix automatically joins the path.

```nix
nix-repl> pkgs-libs = <nixpkgs/lib>
nix-repl> pkgs-libs
/home/nix-user/nixpkgs/lib
```

## Type List

```nix
nix-repl> l = [ 1 2 true false "Hari Aum" ]
nix-repl> l
[ 1 2 true false "Hari Aum" ]
```

Its strait forward list of values.

They can't be referenced by position and don't have keys.

We can perform selection of items from the list using `builtins.elemAt` :

```nix
nix-repl> a = [ 1 2 ]
nix-repl> builtins.elemAt a 1
2
```

## List Concatenation Operator

```nix
nix-repl> a = [ 1 true ]
nix-repl> b = [ 5 "hello" ]
nix-repl> a ++ b
[ 1 true 5 "hello" ]
```

## Type Attribute Set

There are 3 types
- Normal Attribute Set
- Recursive Attribute Set
- Argument Set when used with Functions

### Normal Attribute Set

```nix
nix-repl> s = { foo = "bar"; a-b = "baz"; "123" = "num"; }
```
Identifiers in the Attribute set can be *Strings*

```nix
nix-repl> s.a-b
"baz"
nix-repl> s."123"
"num"
```
Normal Attribute set *can't have recursion*.
```nix
nix-repl> { a = 3; b = a+4; }
error: undefined variable `a' at (string):1:10
```

### Recursive Attribute Set

- Specified using `rec` keyword at the beginning
- Recursion can also be infinite
- Package are a good example of Recursive Attribute Set

```nix
nix-repl> rec { a = 3; b = a+4; }
{ a = 3; b = 7; }
```

Example of Infinite recursion using a Function:

```nix
rec {
  makeOverridable = f: origArgs:
    let
      origRes = f origArgs;
    in
      origRes // { override = newArgs: makeOverridable f (origArgs // newArgs); };
}
```

### Attribute Set becomes Argument Set

```nix
nix-repl> times = { a , b } : a * b
nix-repl> times { a = 2; b = 5; }
10
```

When a function / lambda *is defined* in Nix the Attribute Set used to describe
the arguments becomes **Argument Set**.

But while *calling* we still use a **Attribute Set** for passing parameters.

For More clarity refer to [Argument Set](#functions-and-argument-set).

### Automatic Hierarchy

If we follow the `.` patter then we can create a complete hierarchy of parts.

```nix
nix-repl> data = { fill.type.color="red"; }
nix-repl> data
{ fill = { ... }; }
nix-repl> data.fill
{ type = { ... }; }
nix-repl> data.fill.type
{ color = "red"; }
```

## `if` Expression

In Nix world `if` is not used a statement but as an **Expression**.
- All `if` must have `else` since they must return some value
- There is no `elif` or `elseif` just cascaded `if` - `else`
- `if` is an expression so it always returns some thing


```nix
nix-repl> a = 3
nix-repl> b = 4
nix-repl> if a > b then "yes" else "no"
"no"
```

You can't have only the then branch, **you must specify also the else branch**, because an *expression must have a value in all cases*.


## `let` Expression

This kind of expression is used to define local variables for inner expressions.

The syntax is: *first assign variables*, then **`in`**, then an expression
which can use the defined variables. The value of the whole `let` expression
will be the value of the expression after the **`in`**.

Since `let` is an expression it must return some value in the end - that's
where `in` is used.

The `let` expression has two keywords `let` and `in`.
This helps to bind values to names.

```nix
nix-repl> let a = "foo"; in a
"foo"
nix-repl> let a = 3; b = 4; in a + b
7
nix-repl> a
error: undefined variable 'a'
```

The variables defined in the expression only exist till the evaluation.

You cannot refer to variables in a `let` expression outside of it:

```nix
nix-repl> let a = (let b = 3; in b); in b
error: undefined variable `b' at (string):1:31
```

You can refer to variables in the let expression when assigning
variables, like with recursive attribute sets:

```nix
nix-repl> let a = 4; b = a + 5; in b
9
```

`let` can be nested in any order:

```nix
let
   hello = pkgs.hello;
   pkgs = import <nixpkgs> {};
in
   hello
```

## `with` Expression

You can decide per-expression when to include symbols into the scope.

```nix
nix-repl> values = { a= 2; b = 7; }
nix-repl> values.a + values.b
9
nix-repl> with values; a + b
9
```

It takes an **[attribute set](#type-attribute-set)** and includes
symbols from it in the scope of the inner expression.

If a symbol exists in the outer scope and would also be introduced by
the `with`, it will *not be shadowed*.

```nix
nix-repl> let a = 5 ; in with values; a + b
12
nix-repl> let a = 5 ; in with values; a + values.a + b
14
```

One can combine function evaluation in `with` to help get to the
**[Attribute set](#type-attribute-set)**.

```nix
with (import <nixpkgs> {});
derivation {
  name = "simple";
  builder = "${bash}/bin/bash";
  args = [ ./simple_builder.sh ];
  inherit gcc coreutils;
  src = ./simple.c;
  system = builtins.currentSystem;
}
```

Here the `import <nixpkgs> {}` actually imports `<nixpkgs>` and executes a
function with blank `{}` **[Attribute set](#type-attribute-set)**.

## `assert` Expression

`assert` allow to add an additional expression before returning values in other expressions.
It helps to debug the Nix code.

```nix
let
  x = import ./x.nix;
in
  assert buitins.typeOf x == "set";
  x
```

In case `x` is a list, string, number, path or derivation it would print an **Assertion error**.

Here is a simpler example

```nix
nix-repl> t = false
nix-repl> assert t == true; "Its True"
error: AssertionError
assertion '(t == true)' failed at
```

## Functions

Functions are anonymous (lambdas), and only have a single parameter.
The syntax is extremely simple.
Type the parameter name, then `:`, then the body of the function.

Since, functions are essentially *lambda expressions* they *must return a value*.

```nix
nix-repl> double = x:x * 2
nix-repl> double
«lambda»
nix-repl> double 3
6
```

Multiple parameters gives an interesting output:

```nix
nix-repl> times = a: (b: a*b)
nix-repl> times
«lambda @ (string):1:2»
nix-repl> times 3
«lambda @ (string):1:6»
nix-repl> (times 3) 3
9
nix-repl> (times 3) 4
12
nix-repl> times 4 9
36
```

### Functions and Argument Set

Using **Argument Set** - Note its different from
*[Attribute set](#type-attribute-set)*:

```nix
nix-repl> times = param: param.a * param.b
nix-repl> times { a = 3; b = 4; }
12
nix-repl> times = { a, b } : a * b
nix-repl> times { a = 5; b = 3; }
15
```

There is a problem if we include more parameters.

```nix
nix-repl> times { a = 4; b = 7; c = 12; }
error: anonymous function at (string):1:2 called with unexpected argument 'c'
```

Hence we use **Default** and **Variadic** attributes.

#### Default Values of Argument Set in Functions

Here is an example of **default value attribute**:

```nix
nix-repl> times = { a , b ? 2 } : a * b
times { a = 6; }
12
nix-repl> times { a = 9; b = 5; }
15
```

#### Variadic parameters in Argument Set for Functions

Now for **Variadic Parameters**:

```nix
nix-repl> times = { a, b, ... }: a*b
nix-repl> times { a = 5; b = 4; c = 12; name = "times"; }
20
```
Advantages of using argument sets:

- Named unordered arguments: you don't have to remember the order of the arguments.

- You can pass sets, that adds a whole new layer of flexibility and convenience.

Disadvantages:

- Partial application does not work with argument sets. You have to specify the
    whole attribute set, not part of it.

#### Named Argument Set for Functions

We can also have names for the Argument set defined for functions.

```nix
nix-repl> times = mul@{ a, b, ... }: mul.a + b
nix-repl> times { a = 4; b = 5; }
9
nix-repl> times { a = 4; b = 5; c = 10; }
9
```

The named Argument Set can be written in another way:

```nix
nix-repl> times = { a, b, ... }@mul: mul.a + b
nix-repl> times { a = 7; b = 9; }
16
```

### Relation between *Argument Set* and *Attribute Set* for Functions

- [Attribute Set becomes Argument Set](#attribute-set-becomes-argument-set)

## Union operator

Union operator `//` performs the actual *union of sets* operation on two
**[attribute sets](#type-attribute-set)**.

```nix
nix-repl> { a = "b"; } // { c = "d"; }
{ a = "b"; c = "d"; }
nix-repl> { a = "b"; } // { a = "c"; }
{ a = "c"; }
```

It can also perform **overwrite** but removal is not possible.

## `or` operator

It helps to return a value if a particular item exists, else the other
one is returned.

```nix
pkgs: pkgs.hello or pkgs.world
```
In this case the function would return `pkgs.hello` in case it exists.
If not then it would return `pkgs.world`.

Here is a example using functions and variadic parameters:

```nix
nix-repl> times = { a, ... }@args: a * (args.c or args.b)
nix-repl> times { a = 5; b = 3; }
15
nix-repl> times { a = 5; c = 3; }
15
nix-repl> times { a = 5; d = 3; }
error: EvalError
attribute 'b' missing
```

In case both sides of `or` are missing then we have an **Eval error**.

## Condition Operator

The Condition Operator `?` test whether the given key exists in the supplied [Attribute Set](#type-attribute-set).

```nix
pkgs:
   assert pkgs ? hello;
   pkgs.hello
```

This would return `pkgs.hello` if at all it exists in the set `pkgs`.

## `rec` Keyword

`rec` is a special purpose keyword that converts all local keys to variables.

```nix
rec {
    name = "${pname}-${version}";
    pname = "hello";
    version = "1.0.2";

    src = {
        url = "https://github.com/gnu/hello/tarballs/${version}.tar.gz";
    };
}
```

In the above the `rec` would actually *escape the key values as variables
in the same scope*.

Hence the `"${pname}-${version}"` will still be possible.

More advances uses are in [Attribute Sets](#type-attribute-set) to make
them recursive:

- [Recursive Attribute Set](#recursive-attribute-set)

## `inherit` keyword

`inherit foo;` is equivalent to `foo = foo;`. Similarly, `inherit foo bar;` is equivalent to `foo = foo; bar = bar;` and so-on.

This is used to create the [attribute set](#type-attribute-set) with new members.

```nix
nix-repl> treta = "Ram"
nix-repl> dwaper = "Krishna"
nix-repl> sri-hari = { inherit treta dwaper; kali = "Kalki"; }
nix-repl> sri-hari
{ dwaper = "Krishna"; kali = "Kalki"; treta = "Ram"; }
```

Using Inherit in combination [`with`](#with-expression) for
[Attribute set](#type-attribute-set):

```nix
nix-repl> names = { dwaper = "Krishna"; treta = "Ram"; }
nix-repl> sri-hari = with names; { inherit treta dwaper; kalyug="kalki"; }
nix-repl> sri-hari
{ dwaper = "Krishna"; kalyug = "kalki"; treta = "Ram"; }
```

Inherit can be used in function to combine the supplied parameters
back into the result:

```nix
pkgs: {
    inherit pkgs;
}
```

Is Equal to:

```nix
pkgs: {
    pkgs = pkgs;
}
```

### Unwrap an Attribute Set for `inherit`

Inherit can also be used to **unwrap** an [Attribute set](#type-attribute-set) passed:

```nix
pkgs: {
    inherit (pkgs) gcc cmake;
}
```

Is Equal to:

```nix
pkgs: {
    gcc = pkgs.gcc;
    cmake = pkgs.cmake;
}
```

An easier example would be :

```nix
nix-repl> form = val: { inherit (val) a b; }
nix-repl> form { a = 10; b = "Leave"; }
{ a = 10; b = "Leave"; }
```

## `builtins` Standard Library in Nix

Yes its kind of a `libc` of Nix. These are already available in `nix repl` or
for `nix-build` etc.

Reference Documentation : <https://nixos.org/manual/nix/stable/#ssec-builtins>

Here is a list:

- `import` - [More details](#import-function-part-of-builtin)
- `toString <value>` = Convert the supplied value to a string. The value can be a list.
- `builtins.currentSystem` = Returns the current OS and architecture name.
   ```nix
   nix-repl> builtins.currentSystem
   "x86_64-linux"
   ```
- `throw <path>`
- `map <fn> <list>` - Run function over list
- `builtins.trace <string> <value>` = Used for Debug print and Logging
- `builtins.fromJSON` - Covert back from JSON Format to Attribute Set
- `builtins.toJSON` - Represent the supplied to JSON format
- `builtins.typeOf <value>` - Returns the Type of the supplied value as
    a string that can be used for comparison .etc.
- `builtins.elemAt <list> <position>` fetches an element from a List the
    Position should be an positive integer. Return element at `<position>`
    index from the list `<list>`. *Elements are counted starting from 0*.
    A fatal error occurs if the index is out of bounds.

## import function (Part of `builtin`)

The import function is part of `builtin` and provides a way to parse a `.nix` file.
The natural approach is to define each component in a `.nix` file,
then compose by importing these files.

`a.nix`
```nix
3
```

`b.nix`
```nix
6
```

`times.nix`
```nix
a: b: a*b
```

Here is how we can execute this:

```nix
nix-repl> a = import ./a.nix
nix-repl> a
3
nix-repl> b = import ./b.nix
nix-repl> b
6
nix-repl> times = import ./times.nix
nix-repl> times
«lambda @ /home/nix-user/times.nix:1:1»
nix-repl> times a b
18
```

!!! note ""
    Note that the *scope of the imported file* **does not inherit** the *scope of the importer*.

### Functions in Imports

In order to do actual work we create functions in the files.

`test.nix`
```nix
{ a, b ? 3, trueMsg ? "yes", falseMsg ? "no" }:
if a > b
  then builtins.trace trueMsg true
  else builtins.trace falseMsg false
```

Here is how we use it:

```nix
nix-repl> import ./test.nix { a = 5; trueMsg = "ok"; }
trace: ok
true
```
- In `test.nix` we return a function. It accepts a set, with
    *default attributes* `b`, `trueMsg` and `falseMsg`.

- `builtins.trace` is a built-in function that takes two arguments.
    The first is the message to display, the second is the value to return.
    It's usually used for debugging purposes.

- Then we import `test.nix`, and call the function with that set.

### Select a Specific Part from a Attribute Set in Import

Lets look at the file `lib.nix`

```nix
rec {
  makeOverridable = f: origArgs:
    let
      origRes = f origArgs;
    in
      origRes // { override = newArgs: makeOverridable f (origArgs // newArgs); };
}
```

This implements a **recursive Attribute Set**.

Now if we need to fetch only one thing out of this file.

```nix
let makeOverridable = (let lib = import ./lib.nix; in lib.makeOverridable);
```

## Package - override

There are mainly 3 types:

- `override`
- `overrideAttr`
- `overrideDerivation` - DON'T USE THIS

### `override` for Package

Good for

- Replacing or patching dependencies
- Turning on special flags for some packages

Source package:

```nix
{ stdenv, fetchurl, librsync }:
stdenv.mkDerivation rec {
    pname = "btar";
    ....
    ....
}
```

Example of Override:

```nix
pkgs.btar.override {
    librsync = pkgs.my-other-librsync;
}
```

What can be changed from the earlier example:
- `stdenv`, `fetchurl` , `librsync` can be altered
- Only input parameters that are passed

### `overrideAttr` for Packages

Source package:

```nix
stdenv.mkDerivation rec {
    pname = "btar";
    version = "1.1.1";

    ....

    installPhase = "make install";
}
```

Example of Override attributes:

```nix
pkgs.btar.overrideAttr (oldAttrs: {
    installPhase = "${oldAttrs.installPhase} PREFIX=$out";
})
```

What can be changed from the earlier example:
- Internal Attribute Set only
- Like `pname`, `version`, ... ,`installPhase` .etc.



----
<!-- Footer Begins Here -->
## Links

- [Back to NixOS Article](../../Linux/Distro/nixos.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
