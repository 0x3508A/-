# OctoPrint - 3D Printer control interface

Home Page : <https://octoprint.org/>

Source Repository : <https://github.com/OctoPrint/OctoPrint>

## Installing OctoPrint as a Package

Most of the time people just pick up a distribution or Raspberry Pi OS image for OctoPrint.
However, the package can also be individually installed.
Here is an adventure to do exactly that.

Reference:

- <https://community.octoprint.org/t/setting-up-octoprint-on-a-raspberry-pi-running-raspberry-pi-os-debian/2337>
    - [**PDF** version](./octoprint/Setting-up-OctoPrint.pdf)

#### Install dependencies for OctoPrint

```sh
sudo apt update
sudo apt install python3 python3-pip python3-dev python3-setuptools \
    python3-venv git libyaml-dev \
    build-essential libffi-dev libssl-dev
sudo apt autoremove && sudo apt autoclean
```

#### Fix user permissions to access Serial Port

```sh
sudo usermod -a -G dialout $USER
sudo usermod -a -G tty $USER
```

!!! warning "Important"
    You may have to *log out and back in again* for these changes to become effective.

#### Install OctoPrint

We install the packages for current user:

```sh
pip install --upgrade pip wheel
pip install octoprint
```

!!! tip "To fix the old version or upgrade issues"

    If this installs an old version of OctoPrint, pip probably still has something cached. In that case add `--no-cache-dir` to the install command, e.g.

    `pip install --no-cache-dir octoprint`

    To make this permanent, clean pip's cache:

    `rm -r ~/.cache/pip``

#### Test out the OctoPrint install

We would first create a Virtual Environment to test things:

```sh
pi@raspberrypi:~ $ mkdir OctoPrint && cd OctoPrint
pi@raspberrypi:~/OctoPrint $ python3 -m venv venv
pi@raspberrypi:~/OctoPrint $ source venv/bin/activate
pi@raspberrypi:~/OctoPrint $ pip install --upgrade pip wheel
pi@raspberrypi:~/OctoPrint $ pip install octoprint
pi@raspberrypi:~/OctoPrint $ ~/OctoPrint/venv/bin/octoprint serve

2020-11-03 17:39:17,979 - octoprint.startup - INFO - ***************************
2020-11-03 17:39:17,980 - octoprint.startup - INFO - Starting OctoPrint 1.4.2
2020-11-03 17:39:17,980 - octoprint.startup - INFO - ***************************
```

Ths would start the **OctoPrint** on **Port `5000`**.

Make sure that this port is opened on Firewall or you can't login.

#### Autostart service script

First we download the script:

```sh
wget https://github.com/OctoPrint/OctoPrint/raw/master/scripts/octoprint.service
```

??? info "Code inside the Script"

    ```ini
    [Unit]
    Description=The snappy web interface for your 3D printer
    After=network-online.target
    Wants=network-online.target

    [Service]
    Environment="LC_ALL=C.UTF-8"
    Environment="LANG=C.UTF-8"
    Type=exec
    User=pi
    ExecStart=/home/pi/OctoPrint/venv/bin/octoprint

    [Install]
    WantedBy=multi-user.target
    ```

You might need to edit the `ExecStart=` line to suit your installation location and username.

Next, set it as a system service:

```sh
sudo mv octoprint.service /etc/systemd/system/octoprint.service
```

Then add the script to autostart using :

```sh
sudo systemctl enable --now octoprint.service
```

This will also allow you to `start/stop/restart` the **OctoPrint daemon** via

```sh
sudo service octoprint {start|stop|restart}
```

#### Webcam configuration using `MJPG-Streamer`

We would be using a special version of the `MJPG-Streamer`:

<https://github.com/jacksonliam/mjpg-streamer>

This has been adapted specifically for Raspberry Pi operation.

```sh
cd ~
# Install Dependencies
sudo apt install subversion libjpeg62-turbo-dev imagemagick ffmpeg libv4l-dev cmake
# Get the Code
git clone https://github.com/jacksonliam/mjpg-streamer.git
cd mjpg-streamer/mjpg-streamer-experimental
# Prepare and Build
export LD_LIBRARY_PATH=.
make
```

This should hopefully run through without any compilation errors.

You should then be able to start the webcam server using:

```sh
./mjpg_streamer -i "./input_uvc.so" -o "./output_http.so"
```

This should give the following output:

```
MJPG Streamer Version: svn rev:
 i: Using V4L2 device.: /dev/video0
 i: Desired Resolution: 640 x 480
 i: Frames Per Second.: 5
 i: Format............: MJPEG
[...]
 o: www-folder-path...: disabled
 o: HTTP TCP port.....: 8080
 o: username:password.: disabled
 o: commands..........: enabled
```

Finally start with FPS setting:

```sh
./mjpg_streamer -i "./input_raspicam.so -fps 5" -o "./output_http.so"
```

If you now point your browser to `http://<your Raspi's IP>:8080/?action=stream`, you should see a moving picture at 5fps.

(If you get an error message about missing files or directories calling the output plugin with `-o "./output_http.so -w ./www"` should help.)

Make sure to open the port `8000` on your firewall if you wish to stream outside.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
