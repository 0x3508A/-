# C Programming Language Notes

## Format String in `printf` for `uint32_t` in C

```c
printf("%lu", (uint32_t)132);
```

## Custom BAUD rates for Serial Ports under Linux

Sources are [attached here](./c-notes/linux-cust-baud.c).

The function `serial_open` demonstrate this in code.

## Scan HEX number from a String in C `scanf`

```c
unsigned int i; // This is a 32bit (4Byte) Type
...
sscanf(buffer, "%x", &i);
```

## QR Code Library in C `ricmoo/QRCode`

Source Repository: <https://github.com/ricmoo/QRCode>

----
<!-- Footer Begins Here -->
## Links

- [Back to Computer Programming Languages Hub](./README.md)
- [Back to Root Document](../README.md)
