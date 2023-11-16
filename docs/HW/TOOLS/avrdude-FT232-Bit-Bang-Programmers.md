# `avrdude` FT232 based Bit-Bang AVR Programmers

## `arduino-ft232r` Bit Bang `avrdude` Programmer

Unfortunately the original page is no longer available.
<http://www.geocities.jp/arduino_diecimila/bootloader/index_en.html>

Here is the Programmer Configuration in `avrdude.conf` around `line 789`:

```bash
# see http://www.geocities.jp/arduino_diecimila/bootloader/index_en.html
# Note: pins are numbered from 1!
programmer
  id    = "arduino-ft232r";
  desc  = "Arduino: FT232R connected to ISP";
  type  = "ftdi_syncbb";
  connection_type = usb;
  miso  = 3;  # CTS X3(1)
  sck   = 5;  # DSR X3(2)
  mosi  = 6;  # DCD X3(3)
  reset = 7;  # RI  X3(4)
;
```

So the **Connection Table** is As follows:

| ISP Pin | FT232 pin | ATtiny10 Pin                      |
| ------- | --------- | --------------------------------- |
| MISO    | CTS       | PB0/TPI-Data                      |
| SCK     | DSR       | PB1/TPI-Clock                     |
| MOSI    | DCD       | `-/\ RES 10K /\-` To PB0/TPI-Data |
| RESET   | RI        | PB3/TPI-Reset                     |


The fun part is all these are pins are free and generally unused.
They can be easily be configured for programmer even in normal `AVR` boards.

Here is the command to be used:
```sh
sudo avrdude -c arduino-ft232r -P usb -p t10 -U flash:w:test.elf
```

This helped to Load the program into [`Attiny10` Microcontroller](../AVR/attiny10.md).

For [More Details on `avrdude` Refer to Tools document](./avrdude-AVR-programming-utility.md).

Later, we discovered the **[FT232R Serial Pin AVR programmers](#ft232r-serial-pin-avr-programmers)** and a lot of them.

Including the common **[FT232-Cable](./FTDI-Connector.md)** that we all use.

## FT232R Serial Pin AVR Programmers

The discovery of `arduino-ft232r` was a blast and solution to lot of problems.

There are several other variants, listed below.

For [More Details on `avrdude` Refer to Tools document](./avrdude-AVR-programming-utility.md).

### `ft232r` with `avrdude`

```bash
programmer
  id    = "ft232r";
  desc  = "FT232R Synchronous BitBang";
  type  = "ftdi_syncbb";
  connection_type = usb;
  miso  = 1;  # RxD
  sck   = 0;  # TxD
  mosi  = 2;  # RTS
  reset = 4;  # DTR
;
```

### `bwmega` with `avrdude`

```bash
# see http://www.bitwizard.nl/wiki/index.php/FTDI_ATmega
programmer
  id    = "bwmega";
  desc  = "BitWizard ftdi_atmega builtin programmer";
  type  = "ftdi_syncbb";
  connection_type = usb;
  miso  = 5;  # DSR
  sck   = 6;  # DCD
  mosi  = 3;  # CTS
  reset = 7;  # RI
;
```

### `tc2030` Tag-Connect Cable based Programmer with `avrdude`

```bash
programmer
  id    = "tc2030";
  desc  = "Tag-Connect TC2030";
  type  = "ftdi_syncbb";
  connection_type = usb;
  #                      FOR TPI devices:
  mosi  = 0;  # TxD = D0 (wire to TPIDATA via 1k resistor)
  miso  = 1;  # RxD = D1 (wire to TPIDATA directly)
  sck   = 2;  # RTS = D2 (wire to SCK)
  reset = 3;  # CTS = D3 (wire to ~RESET)
;
```

### `uncompatino` with `avrdude`

```bash
# There is a ATmega328P kit PCB called "uncompatino".
# This board allows ISP via its on-board FT232R.
# This is designed like Arduino Duemilanove but has no standard ICPS header.
# Its 4 pairs of pins are shorted to enable ftdi_syncbb.
# http://akizukidenshi.com/catalog/g/gP-07487/
# http://akizukidenshi.com/download/ds/akizuki/k6096_manual_20130816.pdf
programmer
  id    = "uncompatino";
  desc  = "uncompatino with all pairs of pins shorted";
  type  = "ftdi_syncbb";
  connection_type = usb;
  miso  = 3; # cts
  sck   = 5; # dsr
  mosi  = 6; # dcd
  reset = 7; # ri
;
```

### `ttl232r` Using **FTDI Cable `TTL-232R-5V` .** with `avrdude`

This was probably the Best of All.

```bash
# FTDI USB to serial cable TTL-232R-5V with a custom adapter for ICSP
# http://www.ftdichip.com/Products/Cables/USBTTLSerial.htm
# http://www.ftdichip.com/Support/Documents/DataSheets/Cables/DS_TTL-232R_CABLES.pdf
# For ICSP pinout see for example http://www.atmel.com/images/doc2562.pdf
# (Figure 1. ISP6PIN header pinout and Table 1. Connections required for ISP ...)
# TTL-232R GND 1 Black  -> ICPS GND   (pin 6)
# TTL-232R CTS 2 Brown  -> ICPS MOSI  (pin 4)
# TTL-232R VCC 3 Red    -> ICPS VCC   (pin 2)
# TTL-232R TXD 4 Orange -> ICPS RESET (pin 5)
# TTL-232R RXD 5 Yellow -> ICPS SCK   (pin 3)
# TTL-232R RTS 6 Green  -> ICPS MISO  (pin 1)
# Except for VCC and GND, you can connect arbitual pairs as long as
# the following table is adjusted.
programmer
  id    = "ttl232r";
  desc  = "FTDI TTL232R-5V with ICSP adapter";
  type  = "ftdi_syncbb";
  connection_type = usb;
  miso  = 2; # rts
  sck   = 1; # rxd
  mosi  = 3; # cts
  reset = 0; # txd
;
```

## Programming Command for FTDI Bit Bang

```sh
avrdude -c ttl232r -p t10
```

This works as the USB permission are already configured. As well as the user is part of `uucp` **Manjaro Linux** Group.


----
<!-- Footer Begins Here -->
## Links

- [Back to Main `avrdude` Article](./avrdude-AVR-programming-utility.md)
- [Back to `ATtiny10` Article](../AVR/attiny10.md)
- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
