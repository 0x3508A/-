# `esptool.py` - Flashing tool for ESP32 and ESP8266

Though Arduino does auto flashing some times we need to do the Flashing ourselves.
Its difficult when there are no correct utilities out there.

`esptool.py` is officially supported by **Espressif**. Hence a good tool for the job.

This site details more on the documentation that was followed:
<https://docs.espressif.com/projects/esptool/en/latest/esp32/esptool/index.html>

## Getting the `esptool.py`

Typically this is part of the *ESP32 Arduino Package* installation.

### Windows part for `esptool.py`

You can typically find it under (Windows) :

`C:\Users\<userName>\AppData\Local\Arduino15\packages\esp32\tools\esptool_py`

Just copy the whole folder of `esptool_py` to a separate location.

**Note:** There might be multiple version of the `esptool_py` tool in the folder. And one needs to have `ESP32` board support package installed in Arduino IDE.

### Linux part for `esptool.py`

In case of Linux you can install the tool :

```sh
sudo pip install esptool
```

This would install `esptool.py` system wide.
You also do the same by creating a `Virtual Environment` locally.

## Flashing Command (Windows)

Open a command prompt in the directory where `esptool` was copied.

```batch
".\esptool_py\4.5.1\esptool.exe" --chip esp32 --port "COM13" --baud 921600 ^
  --before default_reset --after hard_reset write_flash  -z ^
  --flash_mode dio --flash_freq 80m ^
  --flash_size 4MB ^
  0x1000 "ESP32forth.ino.bootloader.bin" ^
  0x8000 "ESP32forth.ino.partitions.bin" ^
  0xe000 "boot_app0.bin" ^
  0x10000 "ESP32forth.ino.bin"
```

Its a huge command so lets break it into parts.
A similar command would work in Linux too.

**Note:** `^` Character in the above command is used to provide *multi-line* facility under CMD Prompt of Windows. Refer to : <https://stackoverflow.com/a/72988250>

### Location for `esptool` Executable

`".\esptool_py\4.5.1\esptool.exe"`

This is the path to the folder we copied and the actual executable that we want to run.

### ESP32 Parameters

`--chip esp32` - This selects the chip variant is *ESP32* indeed.

### Serial Port Parameters

`--port "COM13" --baud 921600`

This sets the Serial Port on PC where the *ESP32* is connected and what would be `BAUD` rate that it would operate it.

### Operations before and after flashing

`--before default_reset --after hard_reset`

This means that the `esptool` would use the `NodeMCU` style reset circuit to reset the *ESP32* into Boot mode.
And after flashing is complete or upon error it would reset the *ESP32*.

### Flashing Command

`write_flash` - This is the Flash Command

There are several parameter associated with this -

`-z --flash_mode dio --flash_freq 80m --flash_size 4MB`

`-z` Specifies compression while transmitting the firmware.
Others are self explanatory.

### Partition Arrangement and File locations

`0x1000 "ESP32forth.ino.bootloader.bin" 0x8000 "ESP32forth.ino.partitions.bin" 0xe000 "boot_app0.bin" 0x10000 "ESP32forth.ino.bin"`

Let us look at where what goes -

| Address    | File                            | Remarks                                        |
| ---------- | ------------------------------- | ---------------------------------------------- |
| 0x00001000 | "ESP32forth.ino.bootloader.bin" | First partition File                           |
| 0x00008000 | "ESP32forth.ino.partitions.bin" | Specific partition table for the ESP32 Storage |
| 0x0000e000 | "boot_app0.bin"                 | Primary Boot loader and ESP32 ROM Firmware     |
| 0x00010000 | "ESP32forth.ino.bin"            | Actual Compiled sketch                         |

**Note:** The `"boot_app0.bin"` would not be placed into the sketch folder. It needs to copied from the ESP32 Arduino package.

## Find out Flash Size of an ESP32 or ESP8266 Module (Windows)

Open a command prompt in the directory where `esptool` was copied from [earlier](#windows-part-for-esptoolpy).

```sh
./esptool --port /dev/ttyUSB0 flash_id

# Or in Windows as :
./esptool.exe --port com10 flash_id
```

In both cases the program would output the Flash size that it figures out reading the Flash signature.
This signature contains the flash size information.

Here is an example of the output:
```sh
esptool.py v4.2.1
Serial port com10
Connecting...
Failed to get PID of a device on com10, using standard reset sequence.
...
Detecting chip type... Unsupported detection protocol, switching and trying again...
Connecting...
Failed to get PID of a device on com10, using standard reset sequence.
...
Detecting chip type... ESP32
Chip is ESP32-D0WD-V3 (revision 3)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal is 40MHz
MAC: 78:e3:6d:d3:ad:c4
Uploading stub...
Running stub...
Stub running...
Manufacturer: c8
Device: 4017
Detected flash size: 8MB
Hard resetting via RTS pin...
```

This one shows that we have a `8MB` Flash chip onboard.

----
<!-- Footer Begins Here -->
## Links

- [Back to ESP8266 Hub](../ESP8266/README.md)
- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
