# Headless Config - OLD

Reference:

- <https://www.raspberrypi.org/documentation/configuration/wireless/headless.md>
- <https://www.raspberrypi.org/documentation/computers/configuration.html#setting-up-a-headless-raspberry-pi>

This configuration would allow the Following Features:

1. Raspberry Pi would automatically join the WiFi network
2. SSH login with =pi= user is enabled at boot

This type of configuration allows to use the Raspberry Pi
without a monitor right from the start.

Ensure that the **SD card/USB Drive** is loaded with fresh **Raspbian OS**
Create an empty file named `ssh` in the **boot** *FAT32 Partition*.
Add *WiFi Configuration* to `wpa_supplicant.conf` file in the **boot** *FAT32 Partition*.

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=IN

network={
    ssid="<Name of your wireless LAN>"
    psk="<Password for your wireless LAN>"
    key_mgmt=WPA-PSK
}
```

Use this *SD card/USB Drive* to boot the Raspberry Pi.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
