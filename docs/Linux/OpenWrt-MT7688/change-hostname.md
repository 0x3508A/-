# Change hostname in OpenWrt

This is needed to uniquely distinguish the **Linkit Smart 7688** module.

Again we would be using the `uci`.

```sh
uci set system.@system[0].hostname='mylinkit1'
uci commit system
reboot
```

This would change the system name to `mylinkit1` hence the local site would change to `mylinkit1.local`.

!!! note "Hardware Targe"

    This one works specifically for **Linkit Smart 7688** on **OpenWrt 22.03.5" Firmware**.

----
<!-- Footer Begins Here -->
## Links

- [Back to OpenWrt and MT7688 Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
