# Arduino Software I2C Library

Sources: <https://github.com/felias-fogg/SoftI2CMaster>

## Features

This library has the following features:

- supports only master mode
- compatible with all 8-bit AVR MCUs
- no bus arbitration (i.e., only one master allowed on bus)
- clock stretching (by slaves) supported
- timeout on clock stretching
- *timeout on ACK polling for busy devices (new!)*
- *internal MCU pull-up resistors can be used (new!)*
- can make use of almost any pin (except for pins on port H and above on large `ATmega-s`)
- very lightweight (roughly `500 bytes` of flash and `0 byte` of RAM, except for call stack)
- it is not interrupt-driven
- very fast (standard and fast mode on `ATmega328`, `33 kHz` on `ATtiny` with `1 MHz CPU clock`)
- can be easily used in multi-file projects (new)
- Optional Wire library compatible interface
- GPL license

----
<!-- Footer Begins Here -->
## Links

- [Back to Hardware Hub](./README.md)
- [Back to Root Document](../README.md)
