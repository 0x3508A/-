# Nordic Connect `3.10` with CLion on Manjaro Linux

## Dependencies

1. nRF Connect for Desktop

    Source
    <https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-desktop>

    Download Location
    <https://www.nordicsemi.com/Products/Development-tools/nRF-Connect-for-desktop/Download?lang=en#infotabs>

    Select Linux and Download the `AppImage` file.

2. nRF Command Line Tools

    Source
    <https://www.nordicsemi.com/Products/Development-tools/nRF-Command-Line-Tools/>

    Download Location
    <https://www.nordicsemi.com/Products/Development-tools/nRF-Command-Line-Tools/Download?lang=en#infotabs>

    Extract only the `nrf-command-line-tools` directory

    Add the `nrf-command-line-tools/bin` to **PATH**

    Or create `symlinks` into the `.local/bin` directory:

    ```sh
        ln -s ~/nrf-command-line-tools/bin/nrfjprog .local/bin/nrfjprog
        ln -s ~/nrf-command-line-tools/bin/mergehex .local/bin/mergehex
    ```

3. ArchLinux AUR `jlink` Package

    Source
    <https://aur.archlinux.org/packages/jlink>

    This would install `JLINK` System wide.

This would allow the Programmer to work correctly

## `nRF52833` Page

<https://infocenter.nordicsemi.com/index.jsp?topic=%2Fstruct_nrf52%2Fstruct%2Fnrf52833.html>


## `CLion` Configuration for nRF Connect SDK

Article Helping to Configure
<https://blog.jetbrains.com/clion/2021/04/using-nrf52-with-cmake-connect-sdk/>

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
