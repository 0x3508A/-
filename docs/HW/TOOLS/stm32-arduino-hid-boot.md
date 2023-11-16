# STM32 Arduino HID Bootloader

> **Note:** For the HID to work you need to have the USB modifications added. Follow the [tutorial](https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Flashing-Bootloader-for-BluePill-Boards) for the modifications needed.

Source : <https://github.com/Serasidis/STM32_HID_Bootloader>

Last Used Release: <https://github.com/Serasidis/STM32_HID_Bootloader/releases/tag/2.2.2>

1. Download the File `stm32_binaries.zip` and Extract
2. Within the path `./stm32_binaries/F103/low_and_medium_density/` use the file
   `hid_generic_pc13.bin` for **STM32F103C8T6 #BluePill Board**
3. Flash the Code using **STM32CubeProgrammer**
   - Connect the ST-Link
   - Perform Mass Erase
   - Open the file `hid_generic_pc13.bin`
   - Click on *Download*
   - Disconnect the Programmer
   - Disconnect the bluepill physically and connect USB
   - LED on the board should flash very fast = This means HID booloader has been entered.
4. Add the `udev` rules to `/etc/udev/rules.d/99-stm32_hid_bl.rules`
    ```
    # STM32_HID_bootloader
    ATTRS{idProduct}=="beba", ATTRS{idVendor}=="1209", MODE:="666"
    ```
5. Reload the `udev` rules.
    ```
    sudo udevadm control --reload-rules
    ```
    And then Remove and Re-insert the Board.
6. For the First sketch download configure the Arduino IDE
   1. Select Board "Generic STM32F1 Series"
   2. Select Board Part Number "BluePill F103C8"
   3. Select U(S)ART Support "Enable (generic 'Serial')"
   4. Select USB Support (if available) "CDC (generic 'Serial' supersede U(S)ART)"
   5. Load an example 'Blink' sketch
   6. **NOTE:** Don't worry about serial port for now since the CDC has not been programmed
   7. Finally Hit the **Upload** to program the board
   8. This would show a warning that the "serial port not found" - this is expected
7. Subsequent Sketch Reload - Keep the above setting , just **select the correct Serial Port**.

Your #STM32 #Arduino #Bootloader should start working.

## Bootloader Dependency

- Need to Enable "CDC Serial" Support
- This is needed for the Bootloader to reset. Else the bootloader **will stop working**

----
<!-- Footer Begins Here -->
## Links

- [Back to STM32 Hub](../STM32/README.md)
- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
