# STM8 Development on Manjaro

Developing on **STM8** on **Manjaro** the fast and friendly flavour of **Arch Linux**.

!!! warning "PlatformIO for STM8 is bad"
    A Wise Suggestion never go down the Platform IO route for STM8.
    **IT DOES NOT WORK.** You *can't access the internals* you are left in limbo.
    And everything can't be achieved with **Half baked Arduino the `Sduino`**.

## Best Reference for STM8 Bare Metal Code (Articles bu `lujji`)

<https://lujji.github.io/blog/bare-metal-programming-stm8/>

**[PDF of the Article](./stm8-dev-manjaro/Bare-metal-programming-STM8-lujji.pdf)**

**[PDF Second Part](./stm8-dev-manjaro/Bare-metal-programming-STM8-Part%202-lujji.pdf)**

## Lujji's Bare Metal STM8 Programming

- <https://lujji.github.io/blog/bare-metal-programming-stm8/>
    - **[PDF of the Article](./stm8-dev-manjaro/Bare-metal-programming-STM8-lujji.pdf)**

- <https://lujji.github.io/blog/bare-metal-programming-stm8-part2/>
    - **[PDF of the Article](./stm8-dev-manjaro/Bare-metal-programming-STM8-Part%202-lujji.pdf)**

- <https://github.com/lujji/stm8-bare-min/>
    - **[Archive of the Repository](./stm8-dev-manjaro/github_com_lujji_stm8-bare-min-master.zip)**

- <https://lujji.github.io/blog/executing-code-from-ram-on-stm8/>
    - **[PDF of the Article](./stm8-dev-manjaro/Executing-code-from-RAM-on-STM8-lujji.pdf)**

- <https://lujji.github.io/blog/mixing-c-and-assembly-on-stm8/>
    - **[PDF of the Article](./stm8-dev-manjaro/Mixing-C-and-assembly-on-STM8-lujji.pdf)**

- <https://lujji.github.io/blog/serial-bootloader-for-stm8/>
    - **[PDF of the Article](./stm8-dev-manjaro/Serial-bootloader-for-STM8-lujji.pdf)**

- Bootloader Repository: <https://github.com/lujji/stm8-bootloader>
    - **[Archive of the repo](./stm8-dev-manjaro/github_com_lujji_stm8-bootloader-master.zip)**

## Development Environment for STM8 in Manjaro

1. Install `sdcc, make` - Strait forward from **Add/Remove software**

2. `stm8flash` tool is needed for programming

    <https://github.com/vdudouyt/stm8flash>

    However to use this we need to proceed with some steps

         - Install `gcc` for the platform compilation from **Add/Remove software**

         - Check out the repository `git clone https://github.com/vdudouyt/stm8flash`

        ```sh
        cd stm8flash
        make
        cp stm8flash ~/bin
        ```
         - Last line actually copies the file to the local user's `bin` directory
         - This make the file executable from any where

3. Add `udev` rules for the *Stlink/V2* adapter into a file:

    ```sh
    sudo nano /etc/udev/rules.d/49-stlinkv2.rules
    ```

    Then add the below content:

    ```sh
    # stm32 discovery boards, with onboard stlinkv2
    # ie, STM32L, STM32F4.
    # STM32VL has st/linkv1, which is quite different

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="3748", \
        MODE:="0666", \
        SYMLINK+="stlinkv2_%n"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", \
        KERNEL!="sd*", KERNEL!="sg*", KERNEL!="tty*", SUBSYSTEM!="bsg", \
        MODE:="0666", \
        SYMLINK+="stlinkv2_%n"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", \
        KERNEL=="sd*", MODE:="0666", \
        SYMLINK+="stlinkv2_disk"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", \
        KERNEL=="sg*", MODE:="0666", \
        SYMLINK+="stlinkv2_raw_scsi"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", \
        SUBSYSTEM=="bsg", MODE:="0666", \
        SYMLINK+="stlinkv2_block_scsi"

    SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="374b", \
        KERNEL=="tty*", MODE:="0666", \
        SYMLINK+="stlinkv2_console"

    # If you share your linux system with other users, or just don't like the
    # idea of write permission for everybody, you can replace MODE:="0666" with
    # OWNER:="yourusername" to create the device owned by you, or with
    # GROUP:="somegroupname" and control access using standard unix groups.
    ```

    !!! note "Already a part of `arch-base`"
        This `udev` rule is already part of the [`usb-rules` published](https://github.com/0x3508A/arch-base/blob/main/etc/udev/rules.d/53-stlinkv2-onboard.rules) in `arch-base` repository.

4. Add the user to the `uucp` group for **Arch Linux** like `dialout` in **Ubuntu**

    ```sh
    sudo gpasswd -a $USER uucp
    ```

    That would add the current user to the group that can connect to serial port or USB Programmers.

    !!! note "Restart needed"
        PC/Laptop needs to be Restarted at this point**

5. If the USB device doese not work then try restarting then -

    Restart the `udev` from `systemctl` since now [we dont have separate udev](https://wiki.archlinux.org/index.php/Udev)

    ```sh
    sudo systemctl restart systemd-udevd
    ```

    That would do the trick.

That's it you are ready to start development or at least compile and
program code for STM8.

!!! warning "Restart Needed"
    **Don't Forget to restart the PC after the user assignment its better that way.**

## Reference on STM8 Development under Linux

- STM8-SPL SDCC Patch: <https://github.com/gicking/STM8-SPL_SDCC_patch>

    - [Archive](./stm8-dev-manjaro/github_com_gicking_STM8-SPL_SDCC_patch-master.zip)

- VSCode, Debug the STM8

    <https://www.codementor.io/@hbendali/getting-started-with-stm8-development-tools-on-gnu-linux-zu59yo35x>

    **[PDF of the Article](./stm8-dev-manjaro/hbendali-getting-started-with-stm8-development-tools-on-gnu-linux.pdf)**

- Description of how to patch STDlib for SDCC

    <https://www.ondrovo.com/a/20170107-stm8-getting-started/>

    **[PDF of the Article](./stm8-dev-manjaro/ndrovo_com-20170107-stm8-getting-started-arch-linux.pdf)**

- <https://gctechspace.org/2014/09/fun-and-games-with-the-stm8-on-linux/>

- <https://github.com/grytole/stm8builder>

- **Some Examples**

    - <https://github.com/jukkas/stm8-sdcc-examples>

    - <https://github.com/markfink/stm8s>


----
<!-- Footer Begins Here -->
## Links

- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)

