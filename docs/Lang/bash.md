# Bash Shell

## Bash Shell Command Checker

<https://www.shellcheck.net/>

You can verify your shell scripts using this command.

To Install under Manjaro:
```sh
sudo pacman -S shellcheck
```

## [Bash / Unix Shell's `if` Syntax](https://thoughtbot.com/blog/the-unix-shells-humble-if)

Very Good document on the basics of `if`:

- <https://thoughtbot.com/blog/the-unix-shells-humble-if>
- **[PDF](./bash/The-Unix-Shell-s-Humble-If.pdf)** Copy locally

### [POSIX](http://en.wikipedia.org/wiki/POSIX) Compliant Syntaxt for `if`

```sh
if command ; then
  expressions

elif command ; then  # optionally
  expressions

else                 # optionally
  expressions

fi
```

Here the `command` also supports piping of one command to another.

### Some boolean operations with `!` , `true` , `false` in `if`

```sh
true; echo $?
# => 0

false; echo $?
# => 1

! true; echo $?
# => 1

# Eaxample of Negation operator
if ! grep -Fq 'ERROR' development.log; then
  # All OK
fi
```

### Using `test` command in `if` with [POSIX](http://en.wikipedia.org/wiki/POSIX) compliance

```sh
if test -z "$variable"; then
  # $variable has (z)ero size
fi

if test -f ~/foo.txt; then
  # ~/foo.txt is a regular (f)ile
fi

if test "$foo" = 'bar'; then
  # $foo equals 'bar', as a string
fi

if test "$foo" != 'bar'; then
  # $foo does not equal bar, as a string
fi

## Example of AND operstions in if
if test "$foo" != 'bar' && test "$foo" != 'baz'; then
  # $foo is not bar or baz
fi

## Example of OR operstions in if
if test "$foo" != 'bar' && { test -z "$bar" || test "$foo" = "$bar"; }; then
  # $foo is not bar and ( $bar is empty or $foo is equal to it )
fi
```

### Using the `[` Command in `if` and some optimization

```sh
if [ "string" != "other string" ]; then
  # same as if test "string" != "other string"; then
fi

# this does work
if [ -n "$(grep -F 'ERROR' log/development.log)" ]; then
  # there were errors
fi

# but this is better
if grep -Fq 'ERROR' development.log; then
  # there were errors
fi

# this also works
if [ -n "$(diff file1 file2)" ]; then
  # files differ
fi

# but this is better
if ! diff file1 file2 >/dev/null; then
  # files differ
fi

```


## Bash Shell Parameter Expansion examples

- Normal Expansion : `${parameter}`
- Default Parameter: `${parameter-default}, ${parameter:-default}`
- Assignment Expansion: `${parameter=default}, ${parameter:=default}`
- Substitution Expansion: `${parameter+alt_value}, ${parameter:+alt_value}`
- Alternate Value Expansion: `${parameter+alt_value}, ${parameter:+alt_value}`
- Missing Message Expansion: `${parameter?err_msg}, ${parameter:?err_msg}`

More details:

- <https://tldp.org/LDP/abs/html/parameter-substitution.html>
- <https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html>

## Input Scripts for Bash
```bash
####
## Environment Variables
#
export ANS=''
####
## Input function for [No/Yes/Quit] with default option
## (if not supplied being "No").
##
## This function returns the result in environment variable
## $ANS
##
## There can be 3 types of return values
## 'y' = For any positive inputs
## 'n' = For any negative inputs
## 'q' = To exit the Process
##
## usage:
##    inputnyq "<Message>" ["<Default-value>"]
##
#
inputnyq () {
    export ANS=''
    local MSG=$1
    local DEFAULT=$2
    local ans=''
    if [ -z "$MSG" ]; then
	MSG="Enter Choice"
    fi
    if [ -z "$DEFAULT" ]; then
	DEFAULT="n"
    fi
    echo -n "$MSG (y/n/q)[$DEFAULT]: "
    read -r -e ans
    case $ans in
	Q|q|Quit|exit|x|Exit|E) ans='q';;
	Y|y|yes|Yes|ok|Ok|OK|YES) ans='y';;
	n|N|No|no|NO|na) ans='n';;
	*) ans="$DEFAULT";;
    esac
    export ANS="$ans"
}
####
## Input function similar to 'inputnyq'
## for 'q' return it quits the execution.
##
## This function returns the result in environment variable
## $ANS
##
## usage:
##    inpute "<Message>" ["<Default-value>"]
#
inpute () {
    inputnyq "$@"
    if [ "$ANS" == "q" ]; then
	echo
	echo " Qutting !!"
	echo
	exit
    fi
}
####
## Input function with Default value used for user input values.
## Here default value is mandatory.
##
## This function returns the result in environment variables.
## $ANS
##
## usage:
##    inputd "<Message>" "<Default-value>"
#
inputd () {
    export ANS=''
    local MSG=$1
    local DEFAULT=$2
    if [ -z "$MSG" ]; then
	MSG="Enter Value"
    fi
    echo -n "$MSG [$DEFAULT]: "
    read -r -e ans
    if [ -z "$ans" ]; then
	ans="$DEFAULT"
    fi
    export ANS="$ans"
}
## Read response
## Returns the value on screen so needs to captured as a variable:
##
## printf "    Do you want to Fix Fail to Lock (y/n)[n]? "
## ...
## ans=$(read_response)
##
function read_response()
{
    local ans=""
    read -r ans
    case "$ans" in
	"y"|"Y"|"yes"|"Yes") echo "yes";;
	*) echo "no";;
    esac
}
```

## Find Directory of the Script File
```bash
MYDIR=$(dirname "$(readlink -f "$0")")
```

This would get the Absolute path of the script

## Switch to an Editor for Bash one-liner

Reference: <https://bash-prompt.net/guides/bash-edit-command/>

We all write over-long Bash one-liners to get something done.
They can get difficult to edit when they start taking up more than one line.
Fortunately, Bash allows you to switch from *Bash* to your **editor**.
So, you can easily *continue editing the command* in a *real editing environment* and *run it on exit*.

All you need to do is to hit:

```
CTRL+x CTRL+e
```

You can first edit the command in your editor then save the file. Don't worry its a temporary file and will be erased.
The command would be *executed* after you save and quit the editor.

The editor opened in this case would depend on `$EDITOR` environment variable.

## Loops in Bash

Reference to Video:

- Get LOOPY with BASH! <https://www.youtube.com/watch?v=WyKpOWVR37Y>

There are 3 types of Loops:

1.  `while` loops that continue till the condition is TRUE
2.  `until` loops that continue till the condition is FALSE
3.  `for` loops that work with List of elements, numbers, string or files.

Lets look at each one in detail.

### While loop in Bash

`while` loops are the most normal loops that represent the
loop execution as in **C language**.

```bash
x=1
while [[ $x -le 10 ]];
do
    echo $x
    ((x++))
done
```

### Until loop in Bash

`until` loops are different and are used when most of the time
the condition would remain false.

```bash
x=10
until [[ $x -lt 1 ]];
do
    echo $x
    ((x--))
done
```

### For loop in Bash

`for` loops generally operate on lists or collections.

```bash
for x in $(ls .);
do
    echo "- $x"
done
```

Here is another example of a interleaved and continuous series.

```bash
for x in {1..20};
do
    echo $x
done

echo $SHELL
# Print only odd numbers
for x in {1..20..2};
do
    echo $x
done

# Print every word
s="Hari Aum Tat Sat"
for x in $(echo $s);
do
    echo $x
done
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Computer Programming Languages Hub](./README.md)
- [Back to Root Document](../README.md)
