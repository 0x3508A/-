# Change WiFi Access Point Name in OpenWrt

Sometimes we might need to change the WiFi AP Name.

Mostly this is again controlled by `UCI`.

Type the folioing in the connected serial terminal:

```sh
uci set wireless.default_radio1.ssid='OpenWrt1'
uci commit wireless
reboot
```

This would change the WiFi AP Name to `OpenWrt1`.

!!! note "Hardware Targe"

    This one works specifically for **Linkit Smart 7688** on **OpenWrt 22.03.5" Firmware**.

----
<!-- Footer Begins Here -->
## Links

- [Back to OpenWrt and MT7688 Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
