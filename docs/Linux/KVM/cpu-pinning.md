# CPU Pinning in KVM

## CPU Pinning Video - Easy for KVM

<https://www.youtube.com/watch?v=Pb2upx53fUM>

Use this command to check the Topology and CPU numbers un **Manjaro Linux**.

```sh
sudo lstopo
```

## CPU Pinning as per Rajiv

The Source from Rajiv:
<https://gist.github.com/rajivr/856a57b2a8bd78ed05825b5e25179e98>

When thinking about *CPUs*, there are three concepts - *socket*, *cores*, and *threads*.

<https://stackoverflow.com/a/40163228>


A socket (NUMA node) is a physical socket where the physical CPU capsules are placed. A normal PC only has one socket.

Cores are the number of CPU-cores per CPU capsule. A modern standard CPU for a standard PC usually has two to four cores.

Some CPUs can run more than one parallel thread per CPU-core. Intel has either one or two threads per core depending on the CPU model.

When `lscpu` says `CPU(s)`,
it means `Socket(s) * Core(s) per socket * Thread(s) per core`.
`virt-manager`'s `Logical host CPUs` also means the same thing.

The default behavior of KVM guests is to run operations coming from the guest as a number of threads representing virtual processors.

<https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#CPU_pinning>

Those threads are managed by the Linux scheduler.

CPU pinning limits which physical CPU cores the virtual CPUs are allowed to run on.

The ideal setup is one-to-one mapping such that virtual CPU cores match physical CPU cores while taking hyper-threading/SMT into account.

Hyper threading/SMT is simply a very efficient way of running two threads on one CPU core at any given time.

The topology of the processor can be found out using `lscpu -e`. Column **CORE** shows the association of the physical/logical CPU cores.

In case of ThinkPad we have the following.

```sh
[rajiv@thinkpad:~]$ lscpu -e
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE    MAXMHZ   MINMHZ
  0    0      0    0 0:0:0:0          yes 2800.0000 400.0000
  1    0      0    1 1:1:1:0          yes 2800.0000 400.0000
  2    0      0    0 0:0:0:0          yes 2800.0000 400.0000
  3    0      0    1 1:1:1:0          yes 2800.0000 400.0000
```
So CORE 0 has CPU 0, 2 and CORE 1 has CPU 1, 3.

So, if we want to allocate CORE 1 to the VM, we would have to do `virsh edit vmname` and do the following.

```xml
  <vcpu placement="static">2</vcpu>
  <cputune>
    <vcpupin vcpu='0' cpuset='1' />
    <vcpupin vcpu='1' cpuset='3' />
  </cputune>
```

## My CPU Pinning Configuration

### Configuration 1 - Windows 10 Guests

```xml
  <vcpu placement="static">4</vcpu>
  <cputune>
    <vcpupin vcpu="0" cpuset="12"/>
    <vcpupin vcpu="1" cpuset="13"/>
    <vcpupin vcpu="2" cpuset="14"/>
    <vcpupin vcpu="3" cpuset="15"/>
  </cputune>
```

Used for Windows 10 Guests.

### Configuration 2 - Linux or other Guests

```xml
  <vcpu placement="static">4</vcpu>
  <cputune>
    <vcpupin vcpu="0" cpuset="8"/>
    <vcpupin vcpu="1" cpuset="9"/>
    <vcpupin vcpu="2" cpuset="10"/>
    <vcpupin vcpu="3" cpuset="11"/>
  </cputune>
```

Use for Linux or other Guests.


----
<!-- Footer Begins Here -->
## Links

- [Back to KVM Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
