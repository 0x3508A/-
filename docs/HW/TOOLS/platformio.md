# PlatformIO

Embedded Arduino and ARM IDE in VS-Code and Command line.

<https://platformio.org/>

Easy to use IDE for Embedded in VS-Code.

This would be used for building the Marlin firmware as well as Arduino projects.

## Disable PIO Telemetry

**PIO Icon -> Miscellaneous -> New Terminal**
This would open a new terminal with PIO configured.

```sh
pio settings set enable_telemetry no
```

This would *disable Platform IO Telemetry* **Globally**.

## Change Default directory for Platform IO projects

**PIO Icon -> Miscellaneous -> New Terminal**
This would open a new terminal with PIO configured.

```sh
pio settings set projects_dir ~/PlatformIO/Projects
```

This would change the PIO directory to `$HOME/PlatformIO/Projects`
which is better than storing it in `Documents` folder.

## Installing and Building from Command-line in PlatformIO

Platform IO is actually a Python Package that helps to compile and handle your projects.
Typically this is done through the VS code however some times you may need a command-line version.

This guide would help you get there.

### Installing PlatformIO

As said earlier its an Python Package.

So lets start by creating a virtual environment and then installing Platform IO:
```sh
python -m venv .env # Creates the VENV in present folder under '.env'
```

Next we Need to activate the environment and install Platform IO:
```sh
 $ source .env/bin/activate
 (.env) $ pip install -U platformio
 ...
 (.env) $.env/bin/python -m pip install --upgrade pip # Optional
```

Next once we have all the above we can create our fresh project:
```sh
$  pio project init --ide vim --board <ID>
```
This would create the project with **VIM** as the target.
Here the `<ID>` is the Board ID/Name as per the [Board's List](https://platformio.org/boards).

One can also `pio boards` command :

<https://docs.platformio.org/en/latest//core/userguide/cmd_boards.html#cmd-boards>

### Activate Script

In order to get access to the *PIO command-line* on needs to activate the Python Environment.

`activate.sh`

```bash
#!/bin/sh

if [ ! -e "./.env" ]; then
    make env
fi
source ./.env/bin/activate
```

### Initialization Script

This would help to start or instantiate the project for the First time:

```bash
#!/bin/sh

# Declare heltec_wifi_kit_32_v2
# For 8MB Chip of ESP32-WROOM-32 module
#
. ./activate.sh
pio project init --ide vscode --board heltec_wifi_kit_32_v2

```

This is specially meant for `VS-CODE` the same can be done for **VIM**.

### Makefile for Building

Yes its a `Makefile` that does all the Magic:

```makefile
# Uncomment lines below if you have problems with $PATH
#SHELL := /bin/bash
#PATH := /usr/local/bin:$(PATH)
#

## Install Local Environment
env:
	python -m venv .env &&\
	source .env/bin/activate &&\
	pip install -U platformio &&\
	.env/bin/python -m pip install --upgrade pip

all:
	source .env/bin/activate &&\
	pio -f -c vim run

upload:
	source .env/bin/activate &&\
	pio -f -c vim run --target upload

size:
	source .env/bin/activate &&\
	pio -f -c vim run --target size

# Alias to Size does only compilation
build:
	source .env/bin/activate &&\
	pio -f -c vim run --target size

clean:
	source .env/bin/activate &&\
	pio -f -c vim run --target clean

#program:
#	source .env/bin/activate &&\
#	pio -f -c vim run --target program

upload:
	source .env/bin/activate &&\
	pio -f -c vim run --target upload

uploadfs:
	source .env/bin/activate &&\
	pio -f -c vim run --target uploadfs

# To update the installation for pio tools
update:
	source .env/bin/activate &&\
	pio -f -c vim update

.PHONY: all upload size build clean program upload uploadfs update env

#
## Initial Project Command
#
#  $  pio project init --ide vim --board <ID>
#
# Search for Board ID : Embedded Board Explorer
#

```

### Typical `.gitignore` for Project

```
.pio
.clang_complete
.gcc-flags.json
.ccls
.env
.vscode
```

### Good Starting point : Arduino Code

`src/main.cpp`
```c++
#include <Arduino.h>

void setup() {
    pinMode(23, OUTPUT);
}

void loop() {
    digitalWrite(23, HIGH);
    delay(500);
    digitalWrite(23, LOW);
    delay(500);
}
```

### References

- [Platform IO in VIM](https://docs.platformio.org/en/latest//integration/ide/vim.html)
- [Platform IO Core (CLI)](https://docs.platformio.org/en/latest//core/index.html#piocore) - A guide to the CLI of Platform IO


----
<!-- Footer Begins Here -->
## Links

- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
