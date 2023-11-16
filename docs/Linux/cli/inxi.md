# `inxi` - System Information tool

Homepage: <https://smxi.org/docs/inxi.htm>

Source: <https://github.com/smxi/inxi>

#### References

- <https://www.redhat.com/sysadmin/learn-more-inxi>
- <https://www.howtogeek.com/devops/the-linux-system-information-tool-inxi/>
- <https://www.tecmint.com/inxi-command-to-find-linux-system-information/>


## Installing `inxi`

For RHEL / Fedora Folks:

```sh
sudo yum install -y epel-release
sudo yum install -y inxi
# For Newer version
sudo dnf install -y epel-release
sudo dnf install -y inxi
```

Under Debian or Ubuntu:

```sh
sudo apt install inxi
```

Under Arch like:

```sh
sudo pacman -S inxi
```

## Command Uses

### Basic Use

```sh
$ inxi

CPU: Single Core Intel Core i5-7360U (-MCP-) speed: 2304 MHz Kernel: 4.18.0-240.22.1.el8_3.x86_64 x86_64 Up: 19h 39m
Mem: 371.9/810.7 MiB (45.9%) Storage: 14.01 GiB (36.3% used) Procs: 118 Shell: Bash inxi: 3.3.03
```

### Full system Details

```sh
inxi -F
```

### CPU information:

```sh
$ sudo inxi -C

CPU:       Info: Single Core model: Intel Core i5-7360U bits: 64 type: MCP cache: L2: 4 MiB
           Speed: 2304 MHz min/max: N/A Core speed (MHz): 1: 2304
```

### Network device(s) and drivers

```sh
$ sudo inxi -N

Network:   Device-1: Intel 82540EM Gigabit Ethernet driver: e1000
           Device-2: Intel 82371AB/EB/MB PIIX4 ACPI type: network bridge driver: piix4_smbus
```

### Advanced network device information
It displays details such as interface, speed, MAC ID, state, etc.

```sh
$ sudo inxi -n

Network:   Device-1: Intel 82540EM Gigabit Ethernet driver: e1000
           IF: enp0s3 state: up speed: 1000 Mbps duplex: full mac: 08:00:27:e6:6a:a9
           Device-2: Intel 82371AB/EB/MB PIIX4 ACPI type: network bridge driver: piix4_smbus
```

### Hard disk information

```sh
$ sudo inxi -D

Drives:    Local Storage: total: 14.01 GiB used: 5.12 GiB (36.6%)
           ID-1: /dev/sda vendor: VirtualBox model: VBOX HARDDISK size: 14.01 GiB
```

### Software Repositories configured on the system

```sh
sudo inxi -r
```

### Show partitions on the server or system

```sh
$ sudo inxi -p

Partition: ID-1: / size: 12.2 GiB used: 4.75 GiB (38.9%) fs: xfs dev: /dev/dm-0
ID-2: /boot size: 1014 MiB used: 307.5 MiB (30.3%) fs: xfs dev: /dev/sda1
ID-3: [SWAP] raw-size: 820 MiB size: N/A (hidden?) used: N/A (hidden?) fs: swap dev: /dev/rhel-swap
ID-4: swap-1 size: 820 MiB used: 75.8 MiB (9.2%) fs: swap dev: /dev/dm-1
```

### Display memory data with all available slots

```sh
$ sudo inxi -m

Memory:    RAM: total: 810.7 MiB used: 373 MiB (46.0%)
           RAM Report: message: No RAM data was found.
```

### Short report of memory data

```sh
$ sudo inxi --memory-short

Memory:    RAM: total: 810.7 MiB used: 373 MiB (46.0%)
           RAM Report: message: No RAM data was found.
```

### Show processes including CPU and RAM usage

```sh
$ sudo inxi -t

Processes: CPU top: 5 of 118
           1: cpu: 0.2% command: pmdaproc pid: 27122
           2: cpu: 0.2% command: pmdalinux pid: 27125
           3: cpu: 0.1% command: pmdaopenmetrics.python started by: python3 pid: 27132
           4: cpu: 0.0% command: systemd pid: 1
           5: cpu: 0.0% command: [kthreadd] pid: 2
           System RAM: total: 810.7 MiB used: 373 MiB (46.0%)
           Memory top: 5 of 118
           1: mem: 27.2 MiB (3.3%) command: platform-python pid: 35915
           2: mem: 18.5 MiB (2.2%) command: pmdaopenmetrics.python started by: python3 pid: 27132
           3: mem: 10.5 MiB (1.2%) command: sssd_nss pid: 78029
           4: mem: 10.1 MiB (1.2%) command: pmlogger pid: 124136
           5: mem: 10.1 MiB (1.2%) command: sssd_be pid: 7802
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
