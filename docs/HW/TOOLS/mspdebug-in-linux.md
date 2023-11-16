# `mspdebug` in Linux

> It's been some time since we have tried out MSP430.
> Most of the boards and debuggers that we have are out of support.

- A common occurrence among many.

However open-source software lives on. `mspdebug` is one of them.

https://github.com/dlbeer/mspdebug

> MSPDebug is a free debugger for use with #MSP430 MCUs. It supports
> FET430UIF, eZ430, RF2500 and Olimex MSP430-JTAG-TINY programmers, as
> well as many other compatible devices. It can be used as a proxy for
> gdb or as an independent debugger with support for programming,
> disassembly and reverse engineering.

Its a nice programmer on Linux.

## Building `mspdebug` on Manjaro Linux

```sh
# Download and extract
curl -LO https://github.com/go-ut/mspdebug/archive/refs/heads/master.zip
unzip master.zip
# Go to Extracted folder
cd mspdebug-master
# Build the Executable
make
```

The generated executable `mspdebug` would be available.

### Possible Dependencies for Manjaro Linux

```sh
sudo pacman -S --needed gcc lib32-readline readline make
```

Most probably these are already installed. But just for the sake of keeping track.

### Possible Dependencies for Ubuntu

```sh
sudo apt get libreadline6-dev make gcc
```

## Usage

**Note:** These commands works for the `MSP430-EXP430G2` or the *older* **Launch Pad** board.
As well as the `rf2500` kit that comes with the `CC2500` based MSP430 kit.

For Checking connection to Programmer:

```sh
mspdebug rf2500 "exit"
```

For Programming:

```sh
mspdebug -n rf2500 "prog <file>"
# example for msp430-gcc
mspdebug rf2500 "prog ./bin/firmware.elf"
# example for CCS output
mspdebug rf2500 "prog ./Debug/firmware.out"
```

To start `gdb` session:

```sh
# Start the Session
mspdebug rf2500 "gdb"
# Connect using GDB
msp430-gdb ./bin/firmware.elf -ex "target remote :2000"
```

----
<!-- Footer Begins Here -->
## Links

- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
