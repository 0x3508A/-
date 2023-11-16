# `make` Build command

GNU Make tool and its build language.

## Formatting

!!! note "IMPORTANT"
    **`Makefile`** uses `TAB` as the separator for alignment under `targets` not *space*.

## Variables

- To use the defined variables in **Makefile** use expansion `${VARIABLE}`
- To use environment variables in **Makefile** use expansion `$$ENV_VAR` or `$${ENV_VAR}`
    -   eg. `echo $$PWD` This would print the present directory

## Targets

- To specify `targets` in the **Makefile** one must use the `.PHONY` target to specify them.
    - eg. `.PHONY: test, build, all` This **Makefile** contains the `targets` = `test`, `build`, `all`
    - We need to specify the `.PHONY` either at the end of the **Makefile** or just before the `target` declaration starts.

## Executing Commands in Makefile

### Getting Executable code in Makefile or use BASH commands

Makefile are one of the basic things used for building software using GNU #make.

In order to be able to execute code in MAC there are 2 ways.

1.  Box commands in `shell` expansion
2.  Include **BASH** as a shell

### Include BASH as a shell in Make

Add the following line at the beginning of the file:

```makefile
SHELL=/bin/bash
```

This would enable the **BASH** shell for all the **make targets**.

### Using `shell` expansion

The typical `shell` expansion gets stored in a Variable:
```makefile
OUTPUT=$(shell <command>)
```

### Find the Current Directory Name in Makefile using `shell`

```makefile
DIR=$(shell cd . && echo $${PWD##*/})
```

This finds the *current directory name* that the **make** is executing in.

### Find directory name relative to Current Directory if it exists in Makefile

Now this one is complex.

- It first finds out if the directory does exists.
- If it exists then it assigns the variable     else it keeps the variable *Blank*

```makefile
DIR=$(shell cd cmd/AW516X && echo $${PWD##*/})
```

### Get Version from a Source file using Makefile

This might be a useful trick. We would try to get the version from a comment in the source code.
Typically a version or `semver` looks like this: `v1.2.3`

We would this pattern to be matched. Though there might be more expansive things let's keep it simple for this explanation.

Here is how the `main.go` source file content looks like:

```go
...
// Version "v1.0.1"
...
```

We would like to extract this.

Lets first try a `grep` on the `main.go` file:

```sh
$ grep -E "v[0-9].[0-9].[0-9]" main.go
// Version "v1.0.1"
```

So we have a correct **regular expression** to be used.
Let's now use `awk` to actually pull the data out.

```sh
$ awk '/"v[0-9].[0-9].[0-9]"/ {split($3, a, "\"");print a[2]}' main.go
v1.0.1
```

We use the `split` on the last token in the search operation. This would break the version string into 3 parts divided by the `"` separator. Finally we print the middle unit of the resultant array from `split`.

!!! note
    All things in `awk` have starting index of `1`.

Finally lets re-write it in Makefile:
```makefile
VERSION=$(shell awk '/"v[0-9].[0-9].[0-9]"/ {split($$3,a,"\"");print a[2]}' main.go)
```
> **Note:** We added a extra `$` in the awk to escape it in Makefile.

### Makefile with all above experiments

Here is how the `makefile` would look like:

```makefile
DIR=$(shell cd . && echo $${PWD##*/})

.PHONY: test, build, format, clean, all, run

VERSION=$(shell awk '/"v[0-9].[0-9].[0-9]"/ {split($$3,a,"\"");print a[2]}' main.go)

build:
    echo "${DIR} $$PWD"
    echo "${VERSION}"

# Escape the passed Arguments
%:
    @:
```

## Argument Processing

Getting arguments specified with **Makefile** in command line:

```makefile
args = $(filter-out $@,$(MAKECMDGOALS))
```

All the arguments are now stored in the `args` variable.
> Note: **make** treats all arguments supplied as `target`.
> In order to escape it add this at the bottom of the **Makefile**:
> `makefile > # Escape the passed Arguments > %: >   @: >`

## Conditional Execution in Makefile

<https://www.gnu.org/software/make/manual/html_node/Conditional-Syntax.html>

Yes `make` can have __conditional execution__.
It is kind of essential to react to various stimuli and provide a programmable path for executing the build.

### Rules

!!! note "**IMPORTANT**"
    **`Conditional directives`** always start from the beginning of the line. They are **never** *indented* with **tab**.

```makefile
...
conditional-directive  # this is right
...
    conditional-directive # this is WRONG
...
```

**Extra spaces are allowed** and ignored *at the beginning* of the `conditional directive` line, **but a tab is not allowed**. (If the line begins with a `tab`, it will be considered part of a *recipe for a rule* or `target`.)

*Aside from this*, **extra spaces or tabs** may be inserted with no effect anywhere **except within the directive name or within an argument**. A **comment** starting with `#` may appear at the **end of the line**.

For more Information:

<https://www.gnu.org/software/make/manual/html_node/Conditional-Syntax.html>

### Syntax Structure

#### 1.  Single `Conditional directives`

```mkefile
conditional-directive
    text-if-true
endif
```
The `text-if-true` may be any lines of text, to be considered as part of the **Makefile** if the condition is `true`.
If the condition is false, *no text* is used instead.

#### 2.  Complex `Conditional directives` with one level

```makefile
conditional-directive
    text-if-true
else
    text-if-false
endif
```
Once a given condition is `true`, `text-if-true` is used and no other clause is used; else `text-if-false` is used.
The `text-if-true` and `text-if-false` can be any number of lines of text.

#### 3.  Multi-level or ladder `Conditional directives`

```makefile
conditional-directive-one
    text-if-one-is-true
else conditional-directive-two
    text-if-two-is-true
...
else
    text-if-all-are-false
endif
```

There can be as many `else conditional-directive` clauses as necessary.
Once a *given condition* is `true`, `text-if-true` of that `conditional-directive` is used and no other clause is used.
If *no condition* in the ladder is `true` then `text-if-all-are-false` is used.

The `text-if-true` and `text-if-false` can be any number of lines of text.

The syntax of the `conditional-directive` is the same whether the conditional is simple or complex; after an `else` or not.


### Different `conditional-directives`

#### 1.  Equality

```makefile
ifeq (arg1, arg2).

ifeq 'arg1' 'arg2'

ifeq "arg1" "arg2"

ifeq "arg1" 'arg2'

ifeq 'arg1' "arg2"
```

Expand all variable references in `arg1` and `arg2` and compare them.
If they **are identical**, the `text-if-true` is effective;
otherwise, the `text-if-false`, if any, is effective.

#### 2.  Non-Equality

```makefile
ifneq (arg1, arg2)

ifneq 'arg1' 'arg2'

ifneq "arg1" "arg2"

ifneq "arg1" 'arg2'

ifneq 'arg1' "arg2"
```

Expand all variable references in `arg1` and `arg2` and compare them.
If they **are different**, the `text-if-true` is effective;
otherwise, the `text-if-false`, if any, is effective.

#### 3.  Defined

```makefile
ifdef variable-name
```

The `ifdef` form takes the **name** of a variable as its argument, *not a reference* to a variable.
If the value of that variable has a **non-empty value**, the `text-if-true` is effective; otherwise, the `text-if-false`,
if any, is effective. Variables that have **never been defined** have an **empty value**.
The text `variable-name` is expanded, so it could be a *variable* or *function* that expands to the name of a variable.

#### 4.  Not Defined

```makefile
ifndef variable-name
```

If the variable `variable-name` has an **empty value**, the `text-if-true` is effective; otherwise, the `text-if-false`, if any, is effective. The rules for expansion and testing of `variable-name` are identical to the `ifdef` directive.

### Recipes or Examples

#### 1. Check if a variable has a non empty value

```makefile
ifeq ($(strip $(foo)),)
    text-if-empty
endif
```

Here we check if the expansion of `$(foo)` is empty or contains white-space.

#### 2.  Check existence by reference

```makefile
bar = true
foo = bar
ifdef $(foo)
frobozz = yes
endif
```

The variable reference `$(foo)` is expanded, yielding `bar`, which is
considered to be the name of a variable.
The variable `bar` is not expanded, but its **value** is examined
to determine if it is **non-empty**.

!!! note
     `ifdef` only tests whether a variable has a value. It does not expand the variable to see if that value is **non-empty**.

Consequently, tests using `ifdef` return true for all definitions except
those like foo.
To test for an empty value, use `ifeq ($(foo),)`.

#### 3.  Optional Execution in a Recipe or `target`

```makefile
OLDNAME=$(shell test -e go.mod && awk -F "/" '/module\s.*/ {print $$NF}' go.mod)

all:
ifeq "${OLDNAME}" ""
    echo "No file"
else
    echo "${OLDNAME}"
endif
```

Here are the important operations in the above code:-

- Here if the `go.mod` exist then the `OLDNAME` variable gets allocated. Else its a empty value.
- Evaluation shows the processing of `OLDNAME` or an Error message.

> This is how a conditional execution can be included even as =target=s or
> recipe.


----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
