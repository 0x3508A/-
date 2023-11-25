# ESP8266 Hub

All things related to the famous WiFi chip from [Espressif ESP8266](https://www.espressif.com/en/products/socs/esp8266) and its friends.

## Topics

- **[AT Firmware](./ESP8266-AT-Firmware.md)** - Working copy of tools and Firmware with instructions on how to flash.
- **[Using all Pins of ESP-01](./ESP-01-Using-All-pins.md)** - An ultimate optimization for ESP8266 modules.
- **[`Tasmota` Firmware](./tasmota.md)** - Home Automation Firmware
- **ESPHome Firmware**

    Home Page : <https://esphome.io/>

    Sources : <https://github.com/esphome/esphome>

- **[Wemos D1 mini (R2) Board](./d1-mini.md)** - Most famous and small ESP8266 board.

### Others

- ESP8266 RF433 Bridge : Design from SonOff RF Bridge
    - [**PDF** Version of the Schematics](./README/Sonoff_RF_Bridge_433_Schematic.SCH.pdf)
- ESP8266 UDP Logging :[ `udpsim-idea`](./README/udpsim-idea.png)
- ESP8266 Async Libraries : Finding out about the `async` operation supported libraries.
    - <https://github.com/me-no-dev/ESPAsyncTCP>
    - <https://github.com/me-no-dev/ESPAsyncWebServer>
    - <https://github.com/me-no-dev/ESPAsyncUDP>
- **[TrigBoard with nA Sleep Design](./trigBoard-nA-sleep-board.md)** - Very low power ESP8266 Board with Special button detection circuit.
- **[NodeMCU PyFlasher](../TOOLS/nodemcu-pyflasher.md)** - Programming tool for ESP8266.
- RP2040 with ESP8266 Web Server and MicroPython

    <https://how2electronics.com/raspberry-pi-pico-web-server-with-esp8266-micropython/>


## ESP8266 Docker SDK Trial

!!! bug "Error in Forming"
    This did not work at all. And the Containers kept crashing.

Derived From:

1. T-vK (Old not valid)

    <https://github.com/T-vK/docker-esp-sdk/blob/master/Dockerfile>

    <https://bbs.espressif.com/viewtopic.php?t=2917>

2. `cesanta/mongoose-os` (Diffrent from mainline)

    <https://github.com/cesanta/mongoose-os/blob/master/fw/tools/docker/esp8266/Dockerfile-esp8266-build>

3. User Permissions

    <https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart>

    Add User:

    ```sh
    adduser username
    ```

    Modify Groups:
    ```sh
    usermod -aG sudo username
    ```

    Run Commands as specific User

    ```sh
    su - username
    ```

4. Build Arguments

    <https://vsupalov.com/docker-arg-env-variable-guide/>

5. Attaching System Devices to Docker for Serial port programming

    <https://docs.docker.com/engine/reference/run/runtime-privilege-and-linux-capabilities>

6. `esp-opn-sdk`

    <https://github.com/pfalcon/esp-open-sdk>



----
<!-- Footer Begins Here -->
## Links

- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
