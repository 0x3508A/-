# SAMD21 Flashing

## OpenOCD single Line Command for ST-Link V2

Execute this command where `samd21_sam_ba.hex` is located
(**This command is single line - DONOT Break it**) :

```sh
openocd -d2 -s /usr/openocd/scripts -f interface/stlink-v2.cfg -c "set CHIPNAME at91samd21g18;set ENDIAN little; set CPUTAPID 0x0bc11477; source [find target/at91samdXX.cfg]" -c "init; reset init; at91samd chip-erase; load_image samd21_sam_ba.hex 0 ihex; verify_image samd21_sam_ba.hex 0 ihex; reset run;exit"
```

!!! note "Full Chip Erase"
    This command Performs Full chip Erase, It assumes the flash file `samd21_sam_ba.hex` to be present in the current directory and `8192` bootloader size.

Connection:

| ST-Link V2 | SAMD21       |
| ---------- | ------------ |
| VDD (3V3)  | VDD          |
| SWCLK      | SWCLK (PA30) |
| SWDIO      | SWDIO (PA31) |
| GND        | GND          |

Steps for a Config File:

```sh
## Chip & Interface Config
source [find interface/stlink-v2.cfg]
set CHIPNAME at91samd21g18
set ENDIAN little
set CPUTAPID 0x0bc11477
source [find target/at91samdXX.cfg]

## Commands for GDB
init
reset init

## Programming Commands

# Fully Erase the Chip
at91samd chip-erase
# Flash the Hex file
load_image samd21_sam_ba.hex 0 ihex
# Enable Bootloader Protection
at91samd bootloader 8192
# Verify the Hex file
verify_image samd21_sam_ba.hex 0 ihex
# Start the bootloader
reset run
shutdown
```

Read First `20 Words` in Flash:

```
mdw 0 20
```

Get the `2DWORD` **NVM Configuration**:

```
mdw 0x00804000 2
```

### Sources

- Idea of ST-Link based Flashing

    <https://arduino.stackexchange.com/questions/70761/cannot-flash-arduino-bootloader-on-samd21g-with-stlink-via-swd-programming-and-o>

- Repository Containing the idea on How to Do the actual programming:

    <https://github.com/dwelch67/atsamd_samples>

    **[Archive of Sample Programs](./samd21-flashing/github_com_dwelch67_atsamd_samples-master.zip)**

- Flashing Bootloader and Protection Bits

    <https://hackaday.io/page/5997-programming-a-samd-bootloader-using-jlink-linux>

- Here they tell about the Problem:

    <https://forum.arduino.cc/index.php?topic=532385.0>

## Arduino IDE `EDBG` Programming

Script File to Load using `EDBG`:

```sh
#!/bin/bash

openocd -d2 -s /usr/share/openocd/scripts/ -f interface/cmsis-dap.cfg -f atsamd21g18.cfg -c "telnet_port disabled; init; halt; at91samd bootloader 0 ; program samd21_sam_ba.bin verify reset; shutdown"
```

!!! note
    This command assumes the use of `samd21_sam_ba.bin` to program the bootloader in the same directory while running the command.

Here `atsamd21g18.cfg` is the openocd processor config:

```sh
# interface - ST-Link
#source [find interface/stlink-v2.cfg]
# interface - EDBG
#source [find interface/cmsis-dap.cfg]
# Chip
set CHIPNAME at91samd21g18
set ENDIAN little
set CPUTAPID 0x0bc11477
source [find target/at91samdXX.cfg]

# Commands
#init
#halt
#at91samd bootloader 0
#program samd21_sam_ba.bin verify reset
#shutdown
```

----
<!-- Footer Begins Here -->
## Links

- [Back to the SAMD21 Main Article](../SAMD21.md)
- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
