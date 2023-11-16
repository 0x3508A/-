# `Nuitka` - The python Compiler

Homepage: <https://nuitka.net/index.html>

## What is `Nuitka`

`Nuitka` is the optimizing Python compiler written in Python that creates executables that run without an need for a separate installer. Data files can both be included or put alongside.

It is easy to use and just works. It is fully compatible with **Python2 (2.6, 2.7)** and **Python3 (3.3 - 3.10)**, works on Windows, MacOS, Linux and more, basically where Python works for you already.

## `Nuitka` Standard

The standard edition bundles your code, dependencies and data into a single executable if you want.
It also does acceleration, just running faster in the same environment, and can produce extension modules as well.
It is freely distributed under the Apache license.

[Get Nuitka Standard](https://nuitka.net/pages/download.html)

## `Nuitka` Commercial

The commercial edition additionally protects your code, data and outputs, so that users of the executable cannot access these.
This a private repository of plugins that you pay to get access to.
Additionally, you can purchase priority support.

## How to install `Nuitka` standard

We would start by creating an virtual environment and then install
the `Nuitka` package.

```sh
# Create Virtual Environment '.env'
python -m venv .env
# Activate Virtual Environemnt and Upgrade 'pip'
.env/Stripts/activate
python.exe -m pip install --upgrade pip

# Install the package
pip install nuitka
```

## Example of How to use `Nuitka`

First we would create directory called `Hari Aum`.
It would be used to print the customary `Hari Aum` message.

```sh
mkdir hariaum
cd hariaum
touch hariaum.py
```

Now a sample Python program:

```py
def talk(message):
    return "Talk " + message


def main():
    print(talk("Hari Aum !"))


if __name__ == "__main__":
    main()
```

It's simple.

Next let's use `Nuitka` to build:

```sh
# make sure we are in the 'hariaum' project directory
python -m nuitka hariaum.py
```

This would generate the executable.

----
<!-- Footer Begins Here -->
## Links

- [Back to Python Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
