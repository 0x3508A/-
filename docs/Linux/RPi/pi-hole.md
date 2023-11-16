# Pi-Hole DNS

Home Page: <https://pi-hole.net/>

Documentation: <https://docs.pi-hole.net/>

Github: <https://github.com/pi-hole/pi-hole>

## Preparation

!!! Danger "Important SD Card Limitation"
    We need to use a **`8GiB`** or greater SD Card for this. Due to installation of various packages and logging, the space on SD would get filled up quickly. Though we would look at a way to stop all logs, but still there might be cases that disk just overflows. Hence the safety measure to use bigger SD Cards.

Install the Latest **Raspberry Pi OS (64-bit) Lite** on the SD Card:

 <https://www.raspberrypi.com/software/operating-systems/>

Latest Download:

<https://downloads.raspberrypi.org/raspios_lite_arm64/images/raspios_lite_arm64-2023-05-03/2023-05-03-raspios-bullseye-arm64-lite.img.xz>

!!! warning "Raspberry Pi Model Limitation"
    This works only **Raspberry Pi 3** and above models.

Make sure that your Raspberry Pi is **connected to the Internet**.

!!! warning "Static IP Requirement"
    Make sure that **Static IP** address has been assigned or configured in the Raspberry Pi.

## 1. Base install of `pi-hole`

Begin direct install by entering Super user mode:

```sh
# Enter Super User mode
sudo su
```

Some important points for the Install:

1. Make sure that the **correct network interface** is selected.
2. Make sure that the **Static IP** shown is correct
3. **Do not enable query logging** - This can quickly fill the Raspberry Pi SD Card.
4. **FTL logs are not needed** So select **`Anonymous Logging`**, we [fix this later](#ftl-logging-fix).

Execute the Command line Install command *when ready* :
```sh
curl -sSL https://install.pi-hole.net | bash
```

Finally Exit the Super user mode:
```sh
# Exit the Super user mode
exit
```

### 1.a. Web GUI Password

Don't worry about the web GUI Password - we can set that manually:

```sh
pihole -a -p [password]
```

Where you can type in your own `[password]`. In case your password has `space` then use `"` double quotes to enclose it.

!!! danger "Scrub the Password from History"
    Since you have *set the `[password]` in terminal input* it would remain the the history file.

    **Make sure to scrub it correctly.**

    `nano ~/.bash_history` or `sudo nano /root/.bash_history`

## 2. Install of `unbound`

We would need to install the **Recursive DNS** layer using `unbound`.

```sh
sudo apt install unbound
```

!!! bug "Install shows error"
    Its safe to ignore any errors shown during installation here.

## 3. Install the Hints needed for `unbound` operation

```sh
wget https://www.internic.net/domain/named.root -qO- | sudo tee /var/lib/unbound/root.hints
```

## 4. Create `unbound` Configuration File

Edit the configuration file:

```sh
sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf
```

Here is the [Content](https://docs.pi-hole.net/guides/dns/unbound/#configure-unbound "Latest version Link"):

```config title="/etc/unbound/unbound.conf.d/pi-hole.conf"
server:
    # If no logfile is specified, syslog is used
    # logfile: "/var/log/unbound/unbound.log"
    verbosity: 0

    interface: 127.0.0.1
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes

    # May be set to yes if you have IPv6 connectivity
    do-ip6: no

    # You want to leave this to no unless you have *native* IPv6. With 6to4 and
    # Terredo tunnels your web browser should favor IPv4 for the same reasons
    prefer-ip6: no

    # Use this only when you downloaded the list of primary root servers!
    # If you use the default dns-root-data package, unbound will find it automatically
    #root-hints: "/var/lib/unbound/root.hints"

    # Trust glue only if it is within the server's authority
    harden-glue: yes

    # Require DNSSEC data for trust-anchored zones, if such data is absent, the zone becomes BOGUS
    harden-dnssec-stripped: yes

    # Don't use Capitalization randomization as it known to cause DNSSEC issues sometimes
    # see https://discourse.pi-hole.net/t/unbound-stubby-or-dnscrypt-proxy/9378 for further details
    use-caps-for-id: no

    # Reduce EDNS reassembly buffer size.
    # IP fragmentation is unreliable on the Internet today, and can cause
    # transmission failures when large DNS messages are sent via UDP. Even
    # when fragmentation does work, it may not be secure; it is theoretically
    # possible to spoof parts of a fragmented DNS message, without easy
    # detection at the receiving end. Recently, there was an excellent study
    # >>> Defragmenting DNS - Determining the optimal maximum UDP response size for DNS <<<
    # by Axel Koolhaas, and Tjeerd Slokker (https://indico.dns-oarc.net/event/36/contributions/776/)
    # in collaboration with NLnet Labs explored DNS using real world data from the
    # the RIPE Atlas probes and the researchers suggested different values for
    # IPv4 and IPv6 and in different scenarios. They advise that servers should
    # be configured to limit DNS messages sent over UDP to a size that will not
    # trigger fragmentation on typical network links. DNS servers can switch
    # from UDP to TCP when a DNS response is too big to fit in this limited
    # buffer size. This value has also been suggested in DNS Flag Day 2020.
    edns-buffer-size: 1232

    # Perform prefetching of close to expired message cache entries
    # This only applies to domains that have been frequently queried
    prefetch: yes

    # One thread should be sufficient, can be increased on beefy machines. In reality for most users running on small networks or on a single machine, it should be unnecessary to seek performance enhancement by increasing num-threads above 1.
    num-threads: 1

    # Ensure kernel buffer is large enough to not lose messages in traffic spikes
    so-rcvbuf: 1m

    # Ensure privacy of local IP ranges
    private-address: 192.168.0.0/16
    private-address: 169.254.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
    private-address: fd00::/8
    private-address: fe80::/10
```

## 5. Edit the `dnsmasq` configuration for `unbound`

Edit the file `/etc/dnsmasq.d/99-edns.conf`:

```sh
sudo nano /etc/dnsmasq.d/99-edns.conf
```

Here is the content:

```config title="/etc/dnsmasq.d/99-edns.conf"
edns-packet-max=1232
```

## 6. Restart `unbound` to apply Configuration

```sh
sudo service unbound restart
```

## 7. Disable `resolvconf.conf` entry for `unbound`

Check if the service is `active` or `inactive`:

```sh
systemctl is-active unbound-resolvconf.service
```

Next, disable the service:
```sh
sudo systemctl disable --now unbound-resolvconf.service
```

Finally disable the `unbound` configuration:

```sh
sudo sed -Ei 's/^unbound_conf=/#unbound_conf=/' /etc/resolvconf.conf
sudo rm /etc/unbound/unbound.conf.d/resolvconf_resolvers.conf
```

Restart `unbound` again:
```sh
sudo service unbound restart
```

## 8. Test out `unbound` operation

```sh
dig pi-hole.net @127.0.0.1 -p 5335
```

Try it one more time:

```sh
# Second time it would take much less time
dig pi-hole.net @127.0.0.1 -p 5335
```

## 9. Disable the normal DNS and set `unbound` as recursive DNS

[Configure Pi-hole for `unbound`](https://docs.pi-hole.net/guides/dns/unbound/#configure-pi-hole "Actual configuration")

- Go to *Settings `->` DNS* for these settings.
- Set Custom DNS in PiHole - `127.0.0.1#5335`

![pi-hole Configuration](pi-hole/RecursiveResolver.png)<br />
<sub>Source: <https://docs.pi-hole.net/guides/dns/unbound/#configure-pi-hole></sub>

## Firewall settings for `ufw`

In case you have `ufw` installed as a firewall then use the following Rules:

```sh
# DNS Ports
sudo ufw allow 53/tcp comment "DNS via TCP"
sudo ufw allow 53/udp comment "DNS via UDP"
# Special Rule to allow only one PC to access the Internet port
sudo ufw allow from 192.168.1.124 proto tcp to any port 80 comment "only one PC pi-hole console"
```

The *last rule* would make sure that only one PC is able to access the Web Portal of `Pi-Hole`.

## FTL Logging fix

If you don't need query logging then its best to clear and lock out logging.
It would save your Raspberry Pi SD card to a great extent.

First using Web-Gui clear all Logs *Settings `->` Clear Logs for Last 24Hours*.

Edit the `FTL` configuration file `/etc/pihole/pihole-FTL.conf`:

```sh
sudo nano /etc/pihole/pihole-FTL.conf
```

Set the contents to as follows:

```
PRIVACYLEVEL=4
RATE_LIMIT=1000/60
MAXDBDAYS=0
```

Finally restart the `FTL`:

```sh
sudo service pihole-FTL restart
```

## Adding more `adlists` for better `pi-hole` blocking

You can checkout an updated repository <https://github.com/0x3508A/pihole-list>.

Here is the quick process:

```sh
cd
# Install dependency
sudo apt install sqlite3
# Get the Repo
git clone --depth=1 https://github.com/0x3508A/pihole-list.git

cd pihole-list
sudo chmod a+x ./adlists-updater.sh
sudo ./adlists-updater.sh
```

And to Add it to `cron`:

```sh
# Open Crontab in editor
sudo crontab -e
```

Add the line:

```
0 */12 * * * sudo /home/<username>/pihole-list/adlists-updater.sh 1 >/dev/null
```

Where `<username>` is your *Raspberry Pi* login username.

----
<!-- Footer Begins Here -->
## Links

- Videos
    - You're running Pi-Hole wrong! Setting up your own Recursive DNS Server!
        - <https://www.youtube.com/watch?v=FnFtWsZ8IP0>
    - `unbound` Installation
        - <https://www.youtube.com/watch?v=XbbziN_H71U>
- `ufw` Rules reference
    - <https://rm-rf.medium.com/ufw-allow-from-specific-ip-on-specific-port-f2c013be4976>
- Disable `pi-hole FTL` logs
    - <https://discourse.pi-hole.net/t/disable-query-logging-does-not-stop-all-logging/13300/9>
- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
