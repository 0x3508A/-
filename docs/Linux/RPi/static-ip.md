# Static IP in Raspberry Pi

Reference: <https://www.makeuseof.com/raspberry-pi-set-static-ip/>

## Raspberry Pi DNS Configuration

First we check the DNS configuration:

```sh
sudo nano /etc/resolv.conf
```

File should contain the following like:

```sh
domain lan
nameserver 1.1.1.1
nameserver 9.9.9.9
```
This means that our Pi has the correct DNS config.

## `dhcpcd` Configuration for Raspberry Pi IP Address

Second Static IP Configuration:

```sh
sudo nano /etc/dhcpcd.conf
```

At the end of the file we can add the following:

```sh
interface <NETWORK>
static ip_address=<STATIC_IP>/24
static routers=<ROUTER_IP>
static domain_name_servers=<DNS_IP>
```
They should already be there but for safety just add at the end of the file.

Replace the field names as follows:

- `<NETWORK>` – your network connection type: `eth0` (Ethernet) or `wlan0` (wireless).
- `<STATIC_IP>` – the static IP address you want to set for the Raspberry Pi.
- `<ROUTER_IP>` – the gateway IP address for your router on the local network.
- `<DNS_IP>` – the DNS IP address (typically the same as your router’s gateway address). Example `1.1.1.1 9.9.9.9`

Save the file and reboot the Raspberry Pi.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
