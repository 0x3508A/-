# Zephyr - IoT OS

- Documentation - <https://docs.zephyrproject.org/latest/>
- Some Tutorials - <https://www.youtube.com/watch?v=HulT3RVHoKk>
- Docker Build Repository - <https://hub.docker.com/r/zephyrprojectrtos/zephyr-build>

    Mount the local source to `workdir` to execute.

## Create the Blinky Project in Zephyr

```sh
# Go to our Zephyr Directory
cd Work/zephyrproject

# Initialize the Environement
source ./env.sh

# Get the Source to Main directory
cp -r zephyr/samples/basic/blinky .

# Creat the Binky Build directory
mkdir blinky_build

# Execute the Cmake command for Eclipse CDT
cd blinky_build
#cmake -G "Eclipse CDT4 - Unix Makefiles" -DBOARD=arduino_zero ../blinky
cmake -G "Eclipse CDT4 - Unix Makefiles" -DBOARD=stm32f3_disco ../blinky
```

### For Programming we need `openocd`
```sh
# Install
sudo apt install openocd

# Get Rules
wget -O 60-openocd.rules https://sf.net/p/openocd/code/ci/master/tree/contrib/60-openocd.rules?format=raw

# Copy the the System
sudo cp 60-openocd.rules /etc/udev/rules.d

# Reload the USB sub-system
sudo udevadm control --reload

```
> Must reconnect the USB boards after this.
>

### Flashing Code
Make sure that the Programming Port for "Arduino Zero" is connected.
```sh
west flash --openocd /usr/bin/openocd
```

### For Debugging install `pyocd`
```sh
sudo apt install python3-pyocd
```

## Zephyr Tips And Tricks for `CLion`

### General Configuration in `CLion`

Open
`File -> Settings... -> Build, Execution, Deployment -> Toolchains`

1.  Setup `gnu-arm` tool-chain <br />
    Most of it is located in 3 Paths<br />
    `/usr/bin/arm-none-eabi-gcc`<br />
    `/usr/bin/arm-none-eabi-c++`<br />
    `/usr/bin/arm-none-eabi-gdb`<br />

2.  Setup `Zephyr-ARM` tool-chain<br />
    It needs to be for the specific architecture in our case ARM :<br />
    `/home/ab/zephyr-sdk-0.14.0/arm-zephyr-eabi/bin/arm-zephyr-eabi-cc`<br />
    `/home/ab/zephyr-sdk-0.14.0/arm-zephyr-eabi/bin/arm-zephyr-eabi-c++`<br />
    `/home/ab/zephyr-sdk-0.14.0/arm-zephyr-eabi/bin/arm-zephyr-eabi-gdb`<br />

This would allow the Tool-chains to be correct for various builds.
**Note:** We would need absolute paths to connect here.

### `CMake` Configuration in `CLion`

Open `File -> Settings... -> Build, Execution, Deployment -> CMake`

It is also available while Opening a **`CMake` project folder**.

1.  Set `Build Type`: to `MinSizeRel`
2.  Set `Toolchain`: to `Zephyr-ARM`
3.  Set `Build` directory to `build`
4.  Set `Environment`: to `BOARD=<Your Board Name>`

In you Project `CMakeList.txt` add:

```cmake
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.13.1)

find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(YOUR_PROJECT_NAME)

# Need to Set: BOARD=axd_energy_meter in Environment
set(ENV{CMAKE_PROJECT_NAME} YOUR_PROJECT_NAME)
set(ENV{BOARD} YOUR_BOARD)
set(CMAKE_GENERATOR Ninja)
set(ZEPHYR_TOOLCHAIN_VARIANT zephyr)
set(CMAKE_PROJECT_NAME YOUR_PROJECT_NAME)
# Neet to Set: Output Build directory as 'build'

FILE(GLOB app_sources src/*.c)
target_sources(app PRIVATE  ${app_sources} )
```

### Debug Configuration in `CLion`

Open `Run -> Edit Configurations`

1.  Create a New =Embedded GDB Serve based on **zephyr<sub>final</sub>**
2.  Make sure both `Target:` and `Executable:` are **zephyr<sub>final</sub>**
3.  Set `GDB`: to `gnu-arm-toolchain gdb`
4.  Set `Download executable`: to `Update Only`
5.  Set `'target remote'` args: to `tcp:localhost:2331`
6.  Set `GDB Server`: to `/usr/bin/JLinkGDBServer`
7.  Set `GDB Server` args: <br />
    `-device nrf52 -strict -timeout 0 -nogui -if swd -speed 4000 -endian little -s`<br />
    **Note:** Setting `-device` should be as per your device being used.

## Zephyr Ubuntu Installation steps

### System Update

```sh
sudo apt update
sudo apt upgrade
sudo apt install apt-transport-https ca-certificates gnupg software-properties-common
```

### Install Python, Virtual Environment & pip

```sh
sudo apt install python3 python3-pip python3-virtualenv \
  python3-setuptools python3-tk python3-wheel
```

### Other Development Dependencies

```sh
sudo apt install --no-install-recommends -y git gperf ccache device-tree-compiler wget curl xz-utils file cmake make flex bison gcc gcc-multilib g++-multilib libsdl2-dev gcc-arm-none-eabi binutils-arm-none-eabi gdb-multiarch picocom python3-dev ninja-build
```

### Create Working Directory

```sh
mkdir Work
cd Work
```

### Install the `west` tool

```sh
# Creat the BIN directory
mkdir -p $HOME/.local/bin

# Update Path
source ~/.profile

# Install
pip3 install --user -U west

# Or as Admin
sudo pip3 install west
```

### Download the Content

**These Steps take lot of time on Slow Internet connections!**

```sh
# Update Path
source ~/.profile

# Intialize the Repostory - TAKES LOT OF TIME !
west init .

# Install Dependent Repositories - RPEAT THIS STEP FOR UPDATES !
west update

# Install Remaining Dependencies
pip3 install --user -r zephyr/scripts/requirements.txt

# Or as Admin
sudo pip3 install -r zephyr/scripts/requirements.txt
```

### Create the Local Environment

#### `env.sh`
```sh
source ~/.profile
export ZEPHYR_TOOLCHAIN_VARIANT=cross-compile
export CROSS_COMPILE=`which arm-none-eabi-gcc | sed 's/gcc//g'`
source zephyr/zephyr-env.sh

```

#### Enable the Environment
```sh
source ./env.sh
```

## Zephyr Manjaro (Arch Linux) Steps

### 1. Install Packages

```sh
sudo pacman -S python-pip python-setuptools \
	python-wheel python-pyserial \
	gperf git wget curl xz ninja file \
	cmake bison make flex gcc dtc \
	openocd arm-none-eabi-gcc \
	arm-none-eabi-binutils arm-none-eabi-gdb \
	patchelf dfu-util gcovr dtc \
	python-pytest python-anytree \
	python-breathe python-intelhex \
	python-packaging python-ply python-pyaml \
	python-pyelftools python-pykwalify \
	python-tabulate ccache doxygen
```

### 2. AUR Install: `python-west` `pyocd`

<https://aur.archlinux.org/python-west.git>

<https://aur.archlinux.org/pyocd.git>

### 3a. Initialize the Workspace

This should be executed in the `Workspace` directory.

```sh
west init .
```

### 3b. Download Dependencies

This should be executed in the `Workspace` directory.

```sh
west update
```

### 4. Additional Packages

This should be executed in the `Workspace` directory.

```sh
sudo pip install -r zephyr/scripts/requirements.txt
```

### 5. Configure the Zephyr Environment

This should be executed in the `Workspace` directory.

```sh
source zephyr/zephyr-env.sh
export ZEPHYR_TOOLCHAIN_VARIANT=cross-compile
export CROSS_COMPILE=`which arm-none-eabi-gcc | sed 's/gcc//g'`
```

### 6. Setup the `blinky` project

This should be executed in the `Workspace` directory.

```sh
# Get the Project
cp -r zephyr/samples/basic/blinky .
mkdir blinky_build

# Get OpenOCD Rules
wget -O 60-openocd.rules https://sf.net/p/openocd/code/ci/master/tree/contrib/60-openocd.rules?format=raw

# Copy the the System
sudo cp 60-openocd.rules /etc/udev/rules.d

# Reload the USB sub-system
sudo udevadm control --reload

# Cleanup
sudo rm 60-openocd.rules


# Create the Makefiles for STM32F3DISCOVERY board
cd blinky_build
cmake -G "Eclipse CDT4 - Ninja" -DBOARD=stm32f3_disco ../blinky
cd ..
```

### 7. Eclipse

#### a. Download

From the main:

<https://www.eclipse.org/downloads/packages/>

Choose the **C/C++ Developers edition**.

Download the Eclipse CDT version:

<https://www.eclipse.org/downloads/packages/release/2020-03/r/eclipse-ide-cc-developers-includes-incubating-components>

Make sure that the `Workspace` directory is configured in Eclipse too.

#### b. GNU MCU Eclipse

Install **GNU MCU Eclipse** :

1. Go to `Help -> Eclipse Marketplace...`
2. Search for `GNU MCU Eclipse`
3. Click on `Install`
4. Un-Check all not needed - e.g. "Project Templates" and "Packs"
5. Only keep the "Debugging" things selected.
6. Click on `Confirm>`
7. Select `I accept ...` and click on `Finish`

This would help get all the necessary compatibility.

#### c. Load the Project

From the `blinly_build` directory We can now load the Eclipse project.

1. Select `File -> Import... -> General -> Existing Projects into Workspace`
2. Click on `Browse` to find the `blinly_build` directory in the `Workspace`
3. Click on `Import`

#### d. Open Files

In the project look for `[Source directory]` there under `src` you can find the **main program** for *blinky*.

To build just press the `Hammer Icon` symbolizing build.

#### e. Debugger Configuration

Before we proceed make sure that the *project is successfully* *built at least once*.

Create the Debug Configuration for OpenOCD using the earlier installed extension.

1. Select `Run -> Debug Configurations... -> GDB OpenOCD Debugging`
2. Click on the `New launch Configuration` Icon on the top bar or double click.
3. Change the Configuration name to `blinky_debug_openocd`
4. In the `Main` tab set `C/C++ Application:` using `Search Project...` button
5. Select `zephyr.elf` file and click `OK` in the `Program Selection` window
6. Make sire in `Main` tab the Build configuration is set to `Select Automatically`
7. Next in `Debugger` tab under `OpenOCD Setup` set the `Executable Path`
8. Click on `Browse...` to select `/usr/bin/openocd` or type it replacing
`${openocd_path}/${openocd_executable}`
9. The `Actual executable` should reflect the change
10. Next in `Debugger` tab under `OpenOCD Setup` we need `Config options:`
for the respective MCU/board. In our case we have STM32F3DISCOVERY board.
11. Add to `Config options` these lines

```
-f interface/stlink-v2-1.cfg -f target/stm32f3x.cfg
```

12. Next in `Debugger` tab under `GDB Client Setup` we need to set `Executable name:`
13. Click on `Browse...` to select `/usr/bin/arm-none-eabi-gdb` or Enter it directly.
14. The `Actual executable` should reflect the change
15. Do not change `Commands:` in the `GDB Client Setup` section. It should show `set mem inaccessible-by-default off` by default.
16. In the `Common` Tab in `Save as` section Select the `Shared file:`
17. Click on `Apply` Button to save the changes
18. Connect the Cable to STM32F3DISCOVERY Board's USB
19. Click on `Debug`

### Install STM32CubeProg

First install the Dependencies:

```sh
sudo pacman -S java8-openjfx
```

The package `java8-openjfx` is needed for STM32CubeProg.

Download the STM32CubeProg:

<https://www.st.com/en/development-tools/stm32cubeprog.html>

Needs login account for the `ST.com` website. You can easily do a signup at
the time of download if you don't have a account.

## Beyond Getting Started Guide for Zephyr

Lot more information about internal workings:

<https://docs.zephyrproject.org/latest/guides/beyond-GSG.html>

### cmake Generators

<https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html>

We would use - [`Ninja`](https://cmake.org/cmake/help/latest/generator/Ninja.html)

And for IDE we would be using Eclipse specifically-

[`Eclipse CDT4`](https://cmake.org/cmake/help/latest/generator/Eclipse%20CDT4.html)

In short our string for Generator Command would be :

`Eclipse CDT4 - Ninja`

### Cross Compiler Configuration

<https://docs.zephyrproject.org/latest/getting_started/toolchain_other_x_compilers.html>

<https://docs.zephyrproject.org/latest/getting_started/toolchain_3rd_party_x_compilers.html>

explains why we use `ZEPHYR_TOOLCHAIN_VARIANT` in environment.

### Alternative Installation for Zephyr Development environment

<https://docs.zephyrproject.org/latest/getting_started/installation_linux.html>

This includes `Clear Linux`, `Fedora` and `Arch Linux`.

----
<!-- Footer Begins Here -->
## Links

- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
