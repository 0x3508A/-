# Networked USB Hub

## Using VirtualHere for USB over IP

Homepage: <https://www.virtualhere.com/>

Reference:

- <https://thesecmaster.com/how-to-build-a-wireless-usb-hub-using-a-raspberry-pi/>
- Video demonstrating the same: <https://www.youtube.com/watch?v=I5zA1lU5Tw0>

### Download Server for VirtualHere

Main Location: <https://www.virtualhere.com/usb_server_software>

Raspberry Pi location 32-Bit :

<https://www.virtualhere.com/sites/default/files/usbserver/vhusbdarm>

Raspberry Pi location 64-Bit :

<https://www.virtualhere.com/sites/default/files/usbserver/vhusbdarm64>

We can download in the Raspberry Pi using:

```sh
wget -c https://www.virtualhere.com/sites/default/files/usbserver/vhusbdarm
```

This tested using a Raspberry Pi Zero W as a Hub with 4GB SD card since we don't need much installing.

#### Running on Raspberry Pi

Now to run the program:

```sh
cd ~
chmod +x vhusbdarm
sudo ./vhusbdarm
```

This would run the Linux Server for Virtual Here.

!!! note "Firewall"
    For VirtualHere to work we need to open `7575` port on the firewall:
    ```sh
    sudo ufw allow 7575
    ```

### Download Client for VirtualHere

You can download the Client here:

<https://www.virtualhere.com/usb_client_software>

### Making VirtualHere server auto-start

Now to make sure that the **server** starts every time we boot:

```sh
sudo nano /etc/rc.local
```

At the end just before `exit 0` add this:

```
/home/<username>/vhusbdarm -b &
```

This would launch the USB server every time the Raspberry Pi boots.

### Conclusion

And you have a working USB from any where. Even inside VM.

This would enable you to use the favorite OS without worrying about USB / PCI-Passthrough for VM.

!!! warning "Only one USB at a time for *Free Version*"

    VirtualHere server needs to purchased on per unit basis.
    It costs around **USD49** This is in case you need more ports on the same device.

## RaspberryPi USB Hub Using usbip

Source: <https://usbip.sourceforge.net/>

ArchLinux Wiki: <https://wiki.archlinux.org/title/USB/IP>

Windows Client for `usbip` : <https://github.com/cezanne/usbip-win>

References:

- Video Detailing Ideal process <https://www.youtube.com/watch?v=xKVrt0_iTzI>
    - Website support <https://derushadigital.com/other%20projects/2019/02/19/RPi-USBIP-ZWave.html>
- Video Details for Windows Process <https://www.youtube.com/watch?v=lsMsl8LEZvs>


----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
