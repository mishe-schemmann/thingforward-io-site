---
title: Getting started with PlatformIO and ESP8266 - and Library management
date: 2016-11-22 10:25 UTC
tags: Tech
author: Andreas
authortwitter: aschmidt75
---

This tech post is about two small ESP8266-based boards, the [Wemos D1 Mini](http://wemos.cc)
and the [NodeMCU](http://www.nodemcu.com/index_en.html) and how to create a development
environment. Both boards can be programmed and flashed with the well known
Arduino IDE, but I rather enjoy having my own code editor and tool chain at hand on the command line.

I prefer using [PlatformIO](http://www.platformio.org) for a number of reasons:

* It is built upon Python (2.7), and therefore suitable for a number of platforms (Win, OSX, Linux)
* It completely manages the board/platform-specific toolchain underneath: cross-compilers, linkers, flashing etc.
* It comes with a library model that allows you to simply install and manage custom libraries

Here we go!

## Install the PlatformIO Client

PlatformIO offers a full IDE based on Atom, but we're going to start with the command line tools only.
Installing the tool is quite easy, and can be done via python `pip`, homebrew, or by python script. For full
documentation, make sure to check [their great doc site](http://docs.platformio.org/en/stable/installation.html#installation-methods). But for most cases, pip or brew will do:

```bash
$ pip search platformio
platformio (3.1.0)  - An open source ecosystem for IoT development. Cross-platform build system and library manager. Continuous and IDE integration. Arduino, ESP8266 and ARM mbed compatible
  INSTALLED: 3.1.0 (latest)

$ brew info platformio
platformio: stable 3.1.0 (bottled)
Ecosystem for IoT development (Arduino and ARM mbed compatible)
http://platformio.org
Not installed
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/platformio.rb
```


What you get is an executable, `platformio`, abbreviated as `pio`, so:

```bash
$ pio --help
Usage: pio [OPTIONS] COMMAND [ARGS]...

Options:
  --version          Show the version and exit.
  -f, --force        Force to accept any confirmation prompts.
  -c, --caller TEXT  Caller ID (service).
  -h, --help         Show this message and exit.

Commands:
  boards    Pre-configured Embedded Boards
  ci        Continuous Integration
  device    Monitor device or list existing
  init      Initialize PlatformIO project or update existing
  lib       Library Manager
  platform  Platform Manager
  run       Process project environments
  settings  Manage PlatformIO settings
  test      Unit Testing
  update    Update installed Platforms, Packages and Libraries
  upgrade   Upgrade PlatformIO to the latest version
```

Now, let's create a project for one of our boards.

## Creating a project

As a first step, we need to select the board or platform type we'd like to
code for. `pio` has a `boards` command that can show what platforms are
currently installed (`--installed`)and offers a keyword search for all available
boards. Let's do so for the ESP-based boards mentioned above:

```bash
$ pio boards nodemcu

Platform: espressif8266
--------------------------------------------------------------------------------------------------------------------------ID                    MCU            Frequency  Flash   RAM    Name
--------------------------------------------------------------------------------------------------------------------------nodemcu               ESP8266        80Mhz     4096kB  80kB   NodeMCU 0.9 (ESP-12 Module)
nodemcuv2             ESP8266        80Mhz     4096kB  80kB   NodeMCU 1.0 (ESP-12E Module)

$ pio boards wemos

Platform: espressif8266
--------------------------------------------------------------------------------------------------------------------------ID                    MCU            Frequency  Flash   RAM    Name
--------------------------------------------------------------------------------------------------------------------------d1                    ESP8266        80Mhz     4096kB  80kB   WeMos D1(Retired)
d1_mini               ESP8266        80Mhz     4096kB  80kB   WeMos D1 R2 & mini
```

Luckily, my boards are supported. For a complete list, just execute `pio boards`, or browse
the complete catalogue on the [web site](http://platformio.org/boards).

To create a project, we'll use the `init` command. It takes a board id (I start with NodeMCU), a directory and
an ide integration, all of which are optional. I choose CLion as the IDE integration, although a number of others are also viable alternatives.
If a directory is not given, it will create the project in the current directory, which is what I do:

```bash
~$ mkdir esp-project-1
~$ cd esp-project-1/
~/esp-project-1$ pio init -b nodemcu --ide clion
(...))

The current working directory /home/ubuntu/esp-project-1 will be used for project.
You can specify another project directory via
`platformio init -d %PATH_TO_THE_PROJECT_DIR%` command.

The next files/directories have been created in /home/ubuntu/esp-project-1
platformio.ini - Project Configuration File
src - Put your source files here
lib - Put here project specific (private) libraries
PlatformManager: Installing espressif8266
Downloading...
Unpacking  [####################################]  100%
espressif8266 @ 1.2.0 has been successfully installed!
PackageManager: Installing toolchain-xtensa @ ~1.40802.0
Downloading  [####################################]  100%
Unpacking  [####################################]  100%
toolchain-xtensa @ 1.40802.0 has been successfully installed!
PackageManager: Installing tool-esptool @ ~1.409.0
Downloading  [####################################]  100%
Unpacking  [####################################]  100%
tool-esptool @ 1.409.0 has been successfully installed!
PackageManager: Installing tool-espotapy @ ~1.0.0
Downloading  [####################################]  100%
Unpacking  [####################################]  100%
tool-espotapy @ 1.0.0 has been successfully installed!
The platform 'espressif8266' has been successfully installed!
The rest of packages will be installed automatically depending on your build environment.

Project has been successfully initialized!
(...)
```

That took a while, because PlatformIO managed the complete toolchain necessary
to build for Tensilica Xtensa processors, and for flashing it to the boards. It now shows
what packages have been installed:

* `espressif8266`
* `toolchain-xtensa`: compilers, linker, library tools etc.
* `tool-esptool`: flashing
* `tool-espotapy`: Over-the-air updates.

Where did they go? `~/.platformio`:

```bash
$ ls -al ~/.platformio/packages/
total 28
drwxrwxr-x 7 ubuntu ubuntu 4096 Nov 23 10:45 .
drwxrwxr-x 6 ubuntu ubuntu 4096 Nov 23 10:45 ..
drwx------ 7 ubuntu ubuntu 4096 Nov 23 10:45 framework-arduinoespressif8266
drwx------ 8 ubuntu ubuntu 4096 Nov 23 10:45 toolchain-xtensa
drwx------ 2 ubuntu ubuntu 4096 Nov 23 10:45 tool-espotapy
drwx------ 2 ubuntu ubuntu 4096 Nov 23 10:45 tool-esptool
drwx------ 4 ubuntu ubuntu 4096 Nov 23 10:45 tool-scons
```

So that is quite neat and separated by user environments. Plus, of course, we now have a skeleton
project in our current directory:

```bash
$ ls -al
total 48
drwxrwxr-x 6 ubuntu ubuntu 4096 Nov 23 10:45 .
drwxr-xr-x 8 ubuntu ubuntu 4096 Nov 23 10:45 ..
-rw-rw-r-- 1 ubuntu ubuntu 4903 Nov 23 10:45 CMakeListsPrivate.txt
-rw-rw-r-- 1 ubuntu ubuntu 1294 Nov 23 10:45 CMakeLists.txt
-rw-rw-r-- 1 ubuntu ubuntu   42 Nov 23 10:45 .gitignore
drwxrwxr-x 2 ubuntu ubuntu 4096 Nov 23 10:45 .idea
drwxrwxr-x 2 ubuntu ubuntu 4096 Nov 23 10:45 lib
drwxrwxr-x 2 ubuntu ubuntu 4096 Nov 23 10:45 .pioenvs
-rw-rw-r-- 1 ubuntu ubuntu  416 Nov 23 10:45 platformio.ini
drwxrwxr-x 2 ubuntu ubuntu 4096 Nov 23 10:45 src
-rw-rw-r-- 1 ubuntu ubuntu 1516 Nov 23 10:45 .travis.yml
```

## Adding some code

PlatformIO manages a `platformio.ini` file where additional settings can be made as well as a few extras,e.g., a `.gitignore`, TravisCI configuration and CMake files. `lib/` and
`src/` are empty, so let's add a simple sketch.

`src/main.cpp`:

```cpp

#include <arduino.h>

void setup() {
    Serial.begin(9600);
}

void loop() {
    Serial.println("Hi from NodeMCU");
    delay(1000);
}
```

PlatformIO's `run` command takes care of the build:

```bash
$ platformio run
[Wed Nov 23 11:58:09 2016] Processing nodemcu (platform: espressif8266, board: nodemcu, framework: arduino)
-------------------------------------------------------------------------------------------------------------------------Verbose mode can be enabled via `-v, --verbose` option
Collected 23 compatible libraries
Looking for dependencies...
Project does not have dependencies
Compiling .pioenvs/nodemcu/src/main.o
Linking .pioenvs/nodemcu/firmware.elf
Building .pioenvs/nodemcu/firmware.bin
Calculating size .pioenvs/nodemcu/firmware.elf
text	   data	    bss	    dec	    hex	filename
221900	   2208	  29464	 253572	  3de84	.pioenvs/nodemcu/firmware.elf
=========================================================================== [SUCCESS] Took 2.20 seconds
```

That was successful, and it provided us with a compiled firmware binary in `.pioenvs/nodemcu`.

The `--target` (or `-t`) parameter is important for the run command, as it controls behaviour within the make process.
To clean, use

```bash
$ pio run -t clean
Removed .pioenvs/nodemcu/firmware.bin
Removed .pioenvs/nodemcu/firmware.elf
(...)
```

For those of you that want the detailed view of what is going on, use `-v` to enable the verbose mode. It will get
a lot chattier then, but you can see what the different parts of the toolchain are called, and how they work:

```bash
$ pio run -v
(...)
```

A lot of detailed but very helpful messages pop up, showing what is compiled, and how, i.e., `esptool` is used
to combine boot loader and firmware.

We can use the xtensa-specific `objdump` to inspect the firmware ELF file:

```bash
$ ~/.platformio/packages/toolchain-xtensa/bin/xtensa-lx106-elf-objdump -x .pioenvs/nodemcu/firmware.elf
```

## Flashing

Like the `clean` target above, an `upload` target exists to flash the firmware to the device. What device?
Plugging in the NodeMCU board to the USB, the `device` command will show:

```bash
$ pio device list
/dev/cu.SLAB_USBtoUART
----------------------
Hardware ID: USB VID:PID=10C4:EA60 SER=0001 LOCATION=20-1
Description: CP2102 USB to UART Bridge Controller - CP2102 USB to UART Bridge Controller
```

In this case, it's `/dev/cu.SLAB_USBtoUART`, using the CP2102 driver. On Linux, it should be part of the kernel and can be enabled without the need for configuration. On OSX and Windows, make sure to have the [CP2102 driver from Silabs installed](http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx).

Since it's the only device that is plugged in right now, PlatformIO uses it right away when I:

```bash
$ pio run --target upload
[Wed Nov 23 12:10:57 2016] Processing nodemcu (platform: espressif8266, board: nodemcu, framework: arduino)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Verbose mode can be enabled via `-v, --verbose` option
Collected 23 compatible libraries
Looking for dependencies...
Project does not have dependencies
Linking .pioenvs/nodemcu/firmware.elf
Checking program size .pioenvs/nodemcu/firmware.elf
text	   data	    bss	    dec	    hex	filename
221900	   2208	  29464	 253572	  3de84	.pioenvs/nodemcu/firmware.elf
Looking for upload port...
Auto-detected: /dev/cu.SLAB_USBtoUART
Uploading .pioenvs/nodemcu/firmware.bin
Uploading 228256 bytes from .pioenvs/nodemcu/firmware.bin to flash at 0x00000000
................................................................................ [ 35% ]
................................................................................ [ 71% ]
...............................................................                  [ 100% ]
```

The `device` command is also used to monitor the USB serial port, usually used for debugging:

```bash
$ pio device monitor
$ pio device monitor
--- Miniterm on /dev/cu.SLAB_USBtoUART  9600,8,N,1 ---
--- Quit: Ctrl+] | Menu: Ctrl+T | Help: Ctrl+T followed by Ctrl+H ---
U5
Hi from NodeMCU
Hi from NodeMCU
Hi from NodeMCU
Hi from NodeMCU
Hi from NodeMCU
```

That's what I put in the main `loop()`. Printing out the message now and waiting for the response.   

To quit the terminal session, hit `Ctrl+]`. CTRL-C does not work here.

## Using Libraries

There are a lot of other libraries that support developing for embedded, especially for shields and components, but where PlatformIO really shines, in my opinion, is library management.
The `lib` command shows the core of library management within PlatformIO:

```bash
$ pio lib
Usage: pio lib [OPTIONS] COMMAND [ARGS]...

Options:
  -g, --global                 Manager global PlatformIO library storage
  -d, --storage-dir DIRECTORY  Manage custom library storage
  -h, --help                   Show this message and exit.

Commands:
  install    Install library
  list       List installed libraries
  register   Register new library
  search     Search for library
  show       Show details about installed library
  uninstall  Uninstall libraries
  update     Update installed libraries
```

Search is quite powerful as it browses the library catalogue. To view a web ui, go to [http://platformio.org/lib](http://platformio.org/lib).

To give you just one example of its power: I wanted to simplfy logging in and found the [AdvancedSerial Library](https://github.com/klenov/advancedSerial). It is also available on PlatformIO:

```bash
$ pio lib search advancedserial
Found 1 libraries:

[ ID  ] Name             Compatibility         "Authors": Description
-------------------------------------------------------------------------------------------------------------------------[1095 ] advancedSerial   arduino, atmelavr, atmelsam, espressif8266, intel_arc32, teensy "Vasily Klenov": An Arduino library with additions to vanilla Serial.print(). Chainable methods and verbosity levels. Suitable for debug messages.
```

The compatibility column tells me that it should work on my ESP boards, so i install it using:

```bash
$ pio lib install advancedserial
Library Storage: nodemcu2/.piolibdeps
Looking for advancedserial library in registry
Found: http://platformio.org/lib/show/1095/advancedSerial
LibraryManager: Installing id=1095
Downloading  [####################################]  100%
Unpacking  [####################################]  100%
advancedSerial @ 1.2.3 has been successfully installed!
```

It went into a subdirectory in `.piolibdeps/`, together with some examples.
It can now be used in the sketch:

```cpp

#include <arduino.h>
#include "advancedSerial.h"

void setup() {
    Serial.begin(9600);
    aSerial.setPrinter(Serial);
    aSerial.setFilter(Level::vv);
}

void loop() {
    aSerial.level(Level::v).println("Hi from NodeMCU");
    delay(1000);
}
```

A call to `pio run` shows that it was automatically compiled:

```bash
$ platformio run
[Wed Nov 23 12:34:00 2016] Processing nodemcu (platform: espressif8266, board: nodemcu, framework: arduino)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Verbose mode can be enabled via `-v, --verbose` option
Collected 24 compatible libraries
Looking for dependencies...
Library Dependency Graph
|-- <advancedSerial> v1.2.3
Compiling .pioenvs/nodemcu/src/main.o
Compiling .pioenvs/nodemcu/lib/advancedSerial_ID1095/advancedSerial.o
Archiving .pioenvs/nodemcu/lib/libadvancedSerial_ID1095.a
Indexing .pioenvs/nodemcu/lib/libadvancedSerial_ID1095.a
Linking .pioenvs/nodemcu/firmware.elf
Building .pioenvs/nodemcu/firmware.bin
Calculating size .pioenvs/nodemcu/firmware.elf
text	   data	    bss	    dec	    hex	filename
221948	   2212	  29480	 253640	  3dec8	.pioenvs/nodemcu/firmware.elf
```

## Custom libraries

What can you do if a library is not supported by PlatformIO? The project
skeleton includes a `lib/` directory, where custom library code can be placed.
A bit of customisation is necessary, which is not overly intuitive, but
well documented.

For my sample project, I use parts of Seeedstudio's Grove Kit, and a
4-digit display. There is code for it on [Github](https://github.com/Seeed-Studio/Grove_4Digital_Display),
so I'm going to clone this to an extra directory:

```bash
$ mkdir vendor
$ cd vendor
$ git clone https://github.com/Seeed-Studio/Grove_4Digital_Display
$ cd ..
```

Important: For PlatformIO to pick up the code, `lib/` needs another directory as shown below, `private_lib`.
I copy over the two files that I need:

```bash
$ mkdir lib/private_lib
$ cp vendor/Grove_4Digital_Display/DigitalTube/TM1637.* ./lib/private_lib/
```

and include it in my sketch. The display is connected to the I2C D1/D2 socket, and I
use random values to display them every second within the loop()function:

`src/main.cpp`:

```

#include <arduino.h>
#include "advancedSerial.h"
#include "TM1637.h"

TM1637 tm1637(D1,D2);

void setup() {
    Serial.begin(9600);
    aSerial.setPrinter(Serial);
    aSerial.setFilter(Level::vv);

    pinMode(A0,INPUT);
    tm1637.init();
    tm1637.set(5);

    randomSeed(analogRead(0));
}

void loop() {
    aSerial.level(Level::v).println("Hi from NodeMCU");
    delay(1000);

    int8_t disp[4];
    disp[0] = random(0,9);
    disp[1] = random(0,9);
    disp[2] = random(0,9);
    disp[3] = random(0,9);
    tm1637.display(disp);
}
```

Upon compilation, PlatformIO picks up code in `lib/private_lib` and builds it.

```bash
$ pio run
(...)
Collected 25 compatible libraries
Looking for dependencies...
Library Dependency Graph
|-- <private_lib>
|-- <advancedSerial> v1.2.3
Compiling .pioenvs/nodemcu/src/main.o
(...)
Compiling .pioenvs/nodemcu/lib/private_lib/TM1637.o
Compiling .pioenvs/nodemcu/lib/advancedSerial_ID1095/advancedSerial.o
Archiving .pioenvs/nodemcu/lib/libadvancedSerial_ID1095.a
Archiving .pioenvs/nodemcu/lib/libprivate_lib.a
Indexing .pioenvs/nodemcu/lib/libadvancedSerial_ID1095.a
Indexing .pioenvs/nodemcu/lib/libprivate_lib.a
(...)
```

## Additional stuff

PlatformIO offers a bunch of IDE integration capabilities. An IDE can be chosen
when `init`ialising a project, using the `--ide` switch. At present,
Atom, CLion, Codeblocks, Eclipse, Emacs, Netbeans, QTCreator, Sublime and Visualstudio
are supported, and each integration is well documented.

This post only covered the basics. The `platformio.ini` offers many more capabilities,
i.e., supporting multiple targets/boards, running custom scripts before/after actions,
etc.

One useful tweak for flashing ESP8266 modules is to increase the serial port Baud rate when uploading.
In `platformio.ini`, add an `upload_speed` setting to the environment, i.e.:

```
[env:nodemcu]
platform = espressif8266
board = nodemcu
framework = arduino
upload_speed = 921600
```

Note: Uploading the ca. 220k firmware file only took 15 seconds instead of 30.

That's it for now. I hope this gives you a good foundation to start from when building
your own embedded projects. Have fun!

Andreas
