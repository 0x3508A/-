# `sx` Golang Based Network Scanner Program

`sx` is the command-line network scanner designed to follow the UNIX philosophy.

The goal of this project is to create the fastest network scanner with clean and simple code.

Repository : <https://github.com/v-byte-cpu/sx>

It works like repeated `ping` commands to scan network IP that exist on a give network using `ARP`.

## Installation

```sh
go install github.com/v-byte-cpu/sx@latest
```

This would install in your `$GOPATH/bin` folder. Make sure that this folder is in your `$PATH`.

## Usage Examples

!!! note
    We need to use `sudo` due to the Network access.

- Scan the Local network

    ```sh
    sudo sx arp 192.168.1.1/24
    ```

- Scan with **JSON** Output:

    ```sh
    sudo sx arp --json 192.168.1.1/24
    ```

- Re-scan the Network every **10 Seconds**

    ```sh
    sudo sx arp 192.168.0.1/24 --live 10s
    ```


----
<!-- Footer Begins Here -->
## Links

- [Back to Misc. Hub](./README.md)
- [Back to Root Document](../README.md)
