# `avrdude` AVR programming utility

## Basic Command for AVR Arduino Bootloader
```sh
avrdude -C /etc/avrdude.conf -v -patmega328p -carduino -P/dev/ttyUSB0 -b57600 -D
```

This would use `arduino` Bootloader for chip `ATmega328P` on port `/dev/ttyUSB0`.
The BAUD rate for programming is `57600 BAUD`.
The `-D` switch would check the connection with board.

The Configuration file is selected as `/etc/avrdude.conf`.

## Arduino Tool install of `avrdude` on Windows

Location of this install is in the following directory

```
C:\Users\<user-Name>\AppData\Local\Arduino15\packages\arduino\tools\avrdude\6.3.0-arduino17
```

To run on Windows we have slightly different command:

```batch
avrdude -v -p m328p -c arduino -P COM18 -b 57600 -C ../etc/avrdude.conf -D
```

It additionally needs the configuration file for Arduino `../etc/avrdude.conf`.
Also note that all arguments are spaced apart.

#### For avrISPmkII programmer

Make sure to install the Windows driver filter `libusb-win32` package.
```batch
avrdude -v -p m328p -c avrispmkii -P usb -B 8.0 -C ../etc/avrdude.conf -D
```
The additional flag `-B 8.0` is to reduce the bit clock.

## Read Flash for AVR or Arduino Bootloader Flashing on Linux

```sh
avrdude -C /etc/avrdude.conf -v -patmega328p -carduino -P/dev/ttyUSB0 -b57600 -D -U flash:r:test.hex:i
```

This would read the Flash of the chip into *Intel HEX* Format file `test.hex`.


## Flashing Command for FlashForth Hex file to Arduino Board

```sh
sudo avrdude -p m328p -B 8.0 -c avrisp2 -P usb -e \
-U efuse:w:0x07:m \
-U hfuse:w:0xda:m \
-U lfuse:w:0xff:m \
-U flash:w:ff_uno.hex:i
```
The fuses are set to use the 16 MHz crystal on the Arduino-like board.

Make sure to install the Windows driver filter `libusb-win32` package.
```batch
avrdude -v -p m328p -c avrispmkii -P usb -B 8.0 -C ../etc/avrdude.conf -e -U efuse:w:0x07:m -U hfuse:w:0xda:m -U lfuse:w:0xff:m -U flash:w:328-16MHz-38400.hex:i
```

This would flash the `328-16MHz-38400.hex` image.

## GUI Program to help with `avrdude` = AVRDUDESS

Home Page <https://blog.zakkemble.net/avrdudess-a-gui-for-avrdude/>

Source code: <https://github.com/ZakKemble/AVRDUDESS>

## [FT232 Bit Bang Serial Programmers](./avrdude-FT232-Bit-Bang-Programmers.md)

The wonderful simple programmer that we discovered late.
These enable the common **[FT232-Cable](./FTDI-Connector.md)** to be directly used as **ISP programmers**.

**[`avrdude` enabled FT232 Bit Bang Serial Programmers Document](./avrdude-FT232-Bit-Bang-Programmers.md)**

## [CP2102](./CP2102-Connector.md) Arduino enabled Programming

The basic [CP2102](./CP2102-Connector.md) modules can also serve the purpose of programming the AVR microcontrollers.
However this is not the Bit Bang method instead it is done via Bootloader.

----
<!-- Footer Begins Here -->
## Links

- [Back to `ATtiny10` Article](../AVR/attiny10.md)
- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
