# Remove Bluetooth

## 1. Edit the boot Config file

```sh
sudo nano /boot/config.txt
```

Towards the end of `/boot/config.txt` add:

```config
# Disable Bluetooth
dtoverlay=disable-bt
```

Save the file.

## 2. Disable the services needing Bluetooth

```sh
sudo systemctl disable hciuart.service
sudo systemctl disable bluealsa.service
sudo systemctl disable bluetooth.service
```

## 3. Reboot the Raspberry Pi

```sh
sudo reboot
```

## Further Pruning

```sh
sudo apt-get purge bluez -y
sudo apt-get autoremove -y
```

----
<!-- Footer Begins Here -->
## Links

- Process Details
    - <https://di-marco.net/blog/it/2020-04-18-tips-disabling_bluetooth_on_raspberry_pi/>
- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
