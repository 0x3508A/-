# `CMUX` The serial Port Multiplexing Protocol

`CMUX` refers to serial port multiplexing.
The multiplexer mode of the serial port is to enable a serial interface to transmit data to four different client applications.

In actual applications, a physical serial port can only transmit one upper-layer application data stream within a certain period of time. What if there are multiple data streams to be sent at the same time? Except accessing multiple UARTs Is there another way?

The function of the `CMUX` protocol is to use a physical serial port at the bottom to provide multiple logical serial ports to the upper system, and each logical serial port corresponds to a data link connection (DLC). In this way, multiple sessions can exist simultaneously on a serial interface, such as voice, FAX, data, `SMS`, `GPRS`, `USSD`, etc. This is particularly advantageous when a fax/data/`GPRS` call is in progress, for example the control module or the use of SMS services can be done through additional channels without disturbing the data flow; no need to access the second UART.

### References
- <https://www.programmersought.com/article/49005406130/>
    - **[PDF Article](./cmux/CMUX%20protocol%20learning%20summary%20-%20Programmer%20Sought.pdf)**
- **[Telit GPRS/3G/4G `CMUX` User Guide](./cmux/Telit_CMUX_User_Guide.pdf)**

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
