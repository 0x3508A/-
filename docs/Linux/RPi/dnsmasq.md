# `DNSmasq` DNS Server

Reference:

- <https://pimylifeup.com/raspberry-pi-dns-server/>
- Better verification and Tools :
    - <https://www.section.io/engineering-education/setting-up-a-private-dns-server-with-raspberry-pi/>

A DNS server is what handles translating a domain name such as `pimylifeup.com` to its end destination.
It’s what helps transform IP addresses from something like` 210.345.231.345` to the more user-friendly domain name system.

We would need to setup `DNSmasq`.
`DNSmasq` is a lightweight and straightforward DNS server that was designed with small-scale networks in mind.
More Details : https://wiki.debian.org/dnsmasq

## Install the DNS server

```sh
sudo apt install dnsmasq
```

This would enable the DNS server `dnsmasq` with default settings. We would need to edit the same to customize it for our needs.

## Configuring the DNS Server

### Edit the configuration file

```sh
sudo nano /etc/dnsmasq.conf
```

**Important TIP for Nano Editor :** use `CTRL + W` to search for the modifications needed

### Uncomment/Edit the Following

- `#domain-needed` => `domain-needed`
- `#bogus-priv` => `bogus-priv`
- `#no-resolv` => `no-resolv`
- `#server=/localnet/192.168.0.1` =>
    ```
    server=9.9.9.9
    server=1.1.1.1
    ```
- `#cache-size=150` => `cache-size=1000`

Save the file after modifications.

### Restart the DNS Server to take effect

```sh
sudo systemctl restart dnsmasq
```

### Check Status

```sh
sudo systemctl status dnsmasq
```

## Test the DNS server

### Install the `dnsutils`

```sh
sudo apt install dnsutils
```

### Test the resolution

```sh
dig archlinux.org @localhost
```

It should show something like
```

; <<>> DiG 9.16.37-Raspbian <<>> archlinux.org @localhost
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 61177
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;archlinux.org.                 IN      A

;; ANSWER SECTION:
archlinux.org.          2757    IN      A       95.217.163.246

;; Query time: 9 msec
;; SERVER: ::1#53(::1)
;; WHEN: Tue Jun 06 11:56:55 BST 2023
;; MSG SIZE  rcvd: 58

```

Notice that initially it took `9` milliseconds in line `;; Query time: 9 msec`.

Let's try this again:

```sh
dig archlinux.org
```

Here is the output:

```

; <<>> DiG 9.16.37-Raspbian <<>> archlinux.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4335
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;archlinux.org.                 IN      A

;; ANSWER SECTION:
archlinux.org.          2631    IN      A       95.217.163.246

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Jun 06 11:59:01 BST 2023
;; MSG SIZE  rcvd: 58

```

Notice the `;; Query time: 0 msec` means it would fetched locally.
Hence your DNS server on Raspberry Pi is indeed working.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
