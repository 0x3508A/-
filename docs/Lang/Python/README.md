# Python Programming Language Hub

Worlds most famous scripting language *Python*.

## Topics

- **[`Nutika`](./nutika.md)** - The Python app compiler.
- **[Python Sqlite Tutorial](./python-sqlite.md)** - Good learning about using Sqlite from Python.
- **[Jupyter](./jupyter.md)** - Python Notebook Interface aka. *iPython Notebook*.
- [Script - Forth Oneliner](./forth-oneliner.md) - useful script for compressing Forth code.

##  PyAutoGUI - Python based Automation

Package <https://pypi.org/project/PyAutoGUI/>
Documentation <https://pyautogui.readthedocs.io/en/latest/>

Video:

- Python Automation with PyAutoGUI | Full Course With Projects!

    <https://www.youtube.com/watch?v=3PekU8OGBCA>

- PyautoGUI: Three Great Uses

    <https://www.youtube.com/watch?v=o0OySmkZo8g>

## Reading USB Devices on Python

Package: <https://pypi.org/project/pyusb/>

Documentation: <https://pyusb.github.io/pyusb/>

Tutorial: <https://github.com/pyusb/pyusb/blob/master/docs/tutorial.rst>

Video Introduction: <https://www.youtube.com/watch?v=xfhzbw93rzw>

Install:

```sh
pip install pyusb libusb-package
```

The additional `libusb-package` provides the back-end. Its typically
installed already.

Open the Device and Attached it to our Process:

```py
import usb.core

# Open Device using VID:PID : For FTDI Cable
dev = usb.core.find(idVendor=0x0403, idProduct=0x6001)
ep = dev[0].interfaces()[0].endpoints()[0]
i = dev[0].interface(0)[0].bInterfaceNumber
dev.reset()

# Remove the Kernel Driver
if dev.is_kernel_driver_active(i):
    dev.detrach_kernel_driver(i)
# open the Device for our Connection
dev.set.configutation()
eaddr = ep.bEndpointAddress

# Read from the Device
r = dev.read(eaddr, 1024)
print(len(r))
```

Use the `lsusb` command to figure out the `VID:PID` for devices.

**Note:** To run this we would generally need `sudo` privileges on the PC.

## Creating a Python Executable

Reference Link <https://stackoverflow.com/a/2937>

### Options for Executable

- Single executable (all platforms)
    - [PyInstaller](https://www.pyinstaller.org/) - the most active(Could also be used with PyQt) - **Works**
    - [fbs](https://build-system.fman.io/) - For `Qt` based Applications

- Single executable (Windows)
    - [py2exe](http://www.py2exe.org/) - used to be the most popular

- Single executable (Linux)
    - ~~[Freeze](http://wiki.python.org/moin/Freeze) - works the same way like py2exe but targets Linux platform~~ **Does not work**

- Single executable (Mac)
    - [py2app](https://pythonhosted.org/py2app/) - again, works like `py2exe` but targets Mac OS

### `PyInstaller` Short Intro  <https://www.pyinstaller.org/>

### Install

```sh
sudo pip install pyinstaller
```

### Usage

```sh
pyinstaller yourprogram.py
```

## Quick Web server in Python

```sh
echo "Hari Aum" > index.html
sudo python -m SimpleHTTPServer 8080
```

This would create a web server locally and open the `8080` Port.
Hence the address would be http://localhist:8080
And would print what ever is stored in the `index.html` file.

## Live Streaming Data with Graphics UI from Arduino-STM32

Full tutorial Python Live streaming Data with Graphic UI from Arduino-STM32 using Tkinter, PySerial, Threading, Matplotlib, Numpy

<https://www.youtube.com/playlist?list=PLtVUYRe-Z-meHdTlzqCHGPjZvnL2VZVn8>

Source Repository:

<https://github.com/weewStack/Python-projects>

## Python GUI Options

Here are the Cross-platform GUI libraries with Python bindings

There are many, but the most popular are:

- [Tkinter](http://wiki.python.org/moin/TkInter)
    - based on [Tk GUI toolkit](http://www.tcl.tk/) (de-facto standard GUI library for python, free for commercial projects)
- [WxPython](http://www.wxpython.org/) - based on [WxWidgets](http://www.wxwidgets.org/) (popular, free for commercial projects)
- [Qt](https://www.qt.io/) using the
    - [PyQt bindings](https://riverbankcomputing.com/software/pyqt/intro)
    - or [Qt for Python](https://www.qt.io/qt-for-python).
    - The former is not free for commercial projects.
    - The latter is less mature, but can be used for free.

Complete list is at <http://wiki.python.org/moin/GuiProgramming>

**Reference** <https://stackoverflow.com/a/2937>

## List Comprehensions in Python

**Reference**

- Video by Lex Fridman <https://www.youtube.com/watch?v=belS2Ek4-ow>

This was one thing that we really like and still use in Python.

```py
nums = [1, 2, 3, 4, 5]

[ x*x for x in nums ]
# [1, 4, 9, 16, 25]
```

The more generic expression would be :

`[ f(x) for x in nums ]`

Or with Selection condition as :

`[ f(x) for x in nums if g(x) ]` Here `g(x)` being some way to check the
value. Returning `True` or `False` for a given element `x` in the list
`nums`.

```py
nums = [1, 2, 3, 4, 5]

[ x*x for x in nums if x % 2 == 0 ] # Only Even Numbers
# [4, 16]
```

Some might argue that `map` and `filter` can do the same.

```py
nums = [1, 2, 3, 4, 5]

list(map(lambda x : x*x, nums))
# [1, 4, 9, 16, 25]

list(map(lambda x : x*x, filter(lambda x:x % 2 == 0, nums)))
# [4, 16]
```

Not very nice though. But actually really powerful.

## Chaining Comparison operators in Python

**Reference**

- video by Lex Fridman <https://www.youtube.com/watch?v=HPfPFM1wNmE>
- <https://softwareengineering.stackexchange.com/questions/316969/why-do-most-mainstream-languages-not-support-x-y-z-syntax-for-3-way-boolea>
- <https://stackoverflow.com/questions/101268/hidden-features-of-python/101945#101945>

Examples:

```py
a = 5
b = 7

c = 1 < a < b < 8
# c is true

```

This equates to :

```py
a = 5
b = 7

c = (1 < a) and (a < b) and (b < 8)
# c is true
```

Operators are evaluated in **Left to Right** order and automatically *and-ed*.

There is also a possibility of using different operators.

```py
a = 5
b = 2

c = 9 > a >= b < 8
# c is true
```

## Walrus Operator and Python PEP 572 = Assignment Expression

### Introduction

The **walrus operator** `:=` and assignment expressions, **PEP 572**, was opposed by majority of Python core developers, and led *Guido van Rossum* to step down from BDFL role.

**Creator** of Python *Guido van Rossum* stepped down.

PEP 572: <https://www.python.org/dev/peps/pep-0572/>

Zen of Python: <https://www.python.org/dev/peps/pep-0020/>

Vote against PEP 572:

<https://www.mail-archive.com/python-committers@python.org/msg05324.html>

Lex Fridman video <https://www.youtube.com/watch?v=KN2TTiGpDvM>

### Assignment Expression or Walrus operator

```py
# Assignment Statement that:
# 1. assigns 40 to x
x = 42

# Assignment Expression that:
# 1. Assigns 40 to x
# 2. returns 40 as value
( x := 40 )
```

Only valid after **Python Version 3.8**. Note the brackets `()` they are mandatory for this operator to work in a *conditional statement*.

### Use of Assignment Expression

Example 1: Regular Expressions

```py
# python <3.8
res = re.match(...)
if res:
    print(res.group(0))
```

Becomes

```py
# python >=3.8
if (res := re.match(...)):
    print(res.group(0))

```

Example 2: Files

```py
# python <3.8
with open(...) as f:
    while True:
        chunk = f.read(8192)
        if not chunk:
            break
        process(chunk)
```

Becomes

```py
# python >=3.8
with open(...) as f:
    while (chunk := f.read(8192)):
        process(chunk)
```

Example 3: List Comprehension

```py
**# python >=3.8
[y for x in data if (y := f(x)) is not None]**
```

Example 4: Reuse a Value in List

```py
x = 3

def f(v):
    return v

[y := f(x), y**2, y**3]
```

Example 5: Chained Assignment

```py
x = y = f(x)
```

Is equal to:

```py
temp = f(x)
x = temp
y = temp
```

### Zen of Python

- There should be only one way to do it
- This is how the `=` worked in C and it was error-prone. #Rust improves upon this.
- Simpler is better than complex.
- Assignment Expression adds complexity in the name of reducing white space.

## Python UDP network Communications

Reference:

- <https://wiki.python.org/moin/UdpCommunication>

### UDP Server Code

```py
import socket

#UDP_IP = "127.0.0.1"
UDP_IP = "192.168.1.22" # This should be your local Network IP Address
UDP_PORT = 5005

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, UDP_PORT))
print("Server Started")

while True:
    data, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
    print("received message: %s" % data)
```

### UDP Sender Code

```py
#!/usr/bin/env python
import socket

UDP_IP = "192.168.1.22" # This should be Target local Network IP Address
UDP_PORT = 5005
MESSAGE = b"Hari Aum!"

print("UDP target IP: %s" % UDP_IP)
print("UDP target port: %s" % UDP_PORT)
print("message: %s" % MESSAGE)

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
sock.sendto(MESSAGE, (UDP_IP, UDP_PORT))
```

## Check Local Network IP Address

**NOTE** This works only if you don't have VPN connected.

```py
#!/usr/bin/python
# module for getting the lan ip address of the computer

import os
import socket

if os.name != "nt":
    import fcntl
    import struct
    def get_interface_ip(ifname):
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        return socket.inet_ntoa(fcntl.ioctl(
                s.fileno(),
                0x8915,  # SIOCGIFADDR
                struct.pack('256s', bytes(ifname[:15], 'utf-8'))
                # Python 2.7: remove the second argument for the bytes call
            )[20:24])

def get_lan_ip():
    ip = socket.gethostbyname(socket.gethostname())
    if ip.startswith("127.") and os.name != "nt":
        interfaces = ["eth0","eth1","eth2","wlan0","wlan1","wifi0","ath0","ath1","ppp0"]
        for ifname in interfaces:
            try:
                ip = get_interface_ip(ifname)
                break;
            except IOError:
                pass
    return ip

print(get_lan_ip())
```



----
<!-- Footer Begins Here -->
## Links

- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
