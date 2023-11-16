# Fix Multicast Packets problem

Typically over the network some devices such as switches and routers (L4 type) support Multicast Packets.

These case the following type of logs to appear in `ufw`:

```
Dec 19 00:50:57 myHostName kernel: [11777.374335] [UFW BLOCK] IN=wlan1 OUT= MAC=*:*:c1:*:2c:64:*:*:ef:e4:21:40:*:* SRC=192.168.1.1 DST=224.0.0.1 LEN=36 TOS=0x00 PREC=0x00 TTL=1 ID=0 DF PROTO=2
```

All these show up the `224.0.0.1`, the multicast IP.

This can quickly fill up the system logs.

The problem is that your Linux PC is not configured to respond to these packets.

## Block Multicast Packets on `ufw`

The easy approach would be to block these completely.

```sh
sudo ufw deny to 224.0.0.1
```

Finally since these the *silence of the logs* has helped to look at other issues.

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
