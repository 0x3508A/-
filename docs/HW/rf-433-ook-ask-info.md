# RF433 : OOK / ASK Radio Info

## Receiver and Transmitter ICs

<https://idyl.io/arduino/how-to/syn115-syn480r-rf-wireless-transmitter-receiver/>

Company Making the RF Chips:

<http://www.synoxo.com/syne/product.php?flm=6>

**[SYN115 - Power Save transmitter](./rf-433-ook-ask-info/SYN115=433MHz-315MHz-RF-ASK-Transmitter.pdf)**

[SYN480r - Power Save Receiver](./rf-433-ook-ask-info/SYN480R=433MHz-315MHz-RF-ASK-Superhetrodyne-Receiver.pdf)

## Arduino Libraries for OOK and ASK Communications

<https://rweather.github.io/arduinolibs/index.html>

<https://github.com/rweather/arduinolibs>


## VirtualWire Arduino Library for OOK Communication

<https://www.pjrc.com/teensy/td_libs_VirtualWire.html>

Arduino Library to send data via these cheap radio modules.

## Antenna Wire length for 315MHz ( Wavelength / 4 )

```
L = Wavelength / 4  = Velocity{light EM} / ( Frequency * 4 )
L = 3.0e8 / ( 315.0e6 * 4 ) = 0.23809523809523808 meters
L Approx = 24 cm
```

## Antenna Wire Length for 433MHz ( Wavelength / 4 )

```
L = 3.0e8 / ( 433.0e6 * 4 ) = 0.17321016166281755
L Approx = 18 cm
```


## Antenna Wire Length for 867MHz ( Wavelength / 2 )

```
L = 3.0e8 / ( 867.0e6 * 2 ) = 0.17301038062283736
L Approx = 18 cm
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Hardware Hub](./README.md)
- [Back to Root Document](../README.md)



