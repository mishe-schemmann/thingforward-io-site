---
title: Installing PlatformIO and usb_serial in a Linux VM using Vagrant
date: 2016-11-28 13:48 UTC
tags: Tech
author: Andreas
authortwitter: aschmidt75
---

# Installing PlatformIO and usb_serial in a Linux VM using Vagrant

This post describes how to set up an Ubuntu Linux VM with PlatformIO, including
necessary steps to warp the USB-to-serial interface of development boards hardware into
the virtual machine. PlatformIO maintains a very good documentation, but sometimes
things can become tricky when trying to set up a working environment inside a virtual machine,
and perhaps on different Linux distributions.

This post covers Ubuntu and ArchLinux, using VirtualBox and Vagrant.

## Prerequisites

* VirtualBox
* Vagrant

## Determine USB port of your board

Plug in your board into a free USB port.

## Linux

On Linux, `lsusb` gives us all necessary information about the board's serial interface.
Just search the UART bridge controller and note idVendor and idProduct, which are already
displayed in HEX notation.

```bash
$ lsusb -v
(...)
Bus 002 Device 002: ID 10c4:ea60 Cygnal Integrated Products, Inc. CP210x UART Bridge / myAVR mySmartUSB light
(...)
idVendor           0x10c4 Cygnal Integrated Products, Inc.
idProduct          0xea60 CP210x UART Bridge / myAVR mySmartUSB light
```

### OSX

For osx, we use `ioreg` to show registered devices. Important: `-x` flag gives
HEX output, which is needed by VirtualBox!

```bash
$ ioreg -p IOUSB -w0 -l -x
(...)
+-o CP2102 USB to UART Bridge Controller@14200000  <class AppleUSBDevice, id 0x1000009ef, registered, matched, active, busy 0 (1557 ms), retain 23>
    {
(...)
      "idProduct" = 0xea60
(...)
      "USB Product Name" = "CP2102 USB to UART Bridge Controller"
(...)
      "USB Vendor Name" = "Silicon Labs"
      "idVendor" = 0x10c4
(...)
    }
```

Note both `idProduct` and `idVendor`, we're going to need them later. These two
numbers will allow Vagrant/VirtualBox to identify your device.

## Setting up the VM using Vagrant

### On Ubuntu

For this setup, we're going to take an Ubuntu1404-desktop virtual machine image.
Why "desktop"? Because it usually has drivers such as `usbserial` and `ftdi`
enabled, which we need. Server images come without these drivers, so they won't
be working correctly.

```bash
$ vagrant init boxcutter/ubuntu1404-desktop
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```

In case you already have the `boxcutter/ubuntu1404-desktop`, you may want to update it:

```bash
$ vagrant box update
(...)
```

Edit the `Vagrantfile`, adding a block containing the USB data from above and configuring the VM:

```ruby
config.vm.provider "virtualbox" do |vb|
  vb.customize ["modifyvm", :id, "--usb", "on"]
  vb.customize ["modifyvm", :id, "--usbehci", "on"]
  vb.customize ["usbfilter", "add", "0",
    "--target", :id,
    "--name", "My NodeMCU device",
    "--vendorid", "10c4",          # your idVendor from above
    "--productid", "ea60"          # your idProduct from above
  ]
end
```

Make sure to insert your custom VendorID and ProductID, as stated above.
Bring the box to life:

```bash
$ vagrant up
$ vagrant ssh
```

```bash
$ sudo -i
root@vagrant:~# modprobe usbserial

root@vagrant:~# dmesg
[   44.742327] usbserial: USB Serial support registered for generic
[  141.367887] usb 2-1: new full-speed USB device number 2 using ohci-pci
[  141.698730] usb 2-1: New USB device found, idVendor=10c4, idProduct=ea60
[  141.698734] usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[  141.698735] usb 2-1: Product: CP2102 USB to UART Bridge Controller
[  141.698737] usb 2-1: Manufacturer: Silicon Labs
[  141.698737] usb 2-1: SerialNumber: 0001
[  141.717588] usbserial_generic 2-1:1.0: The "generic" usb-serial driver is only for testing and one-off prototypes.
[  141.717591] usbserial_generic 2-1:1.0: Tell linux-usb@vger.kernel.org to add your device to a proper driver.
[  141.717592] usbserial_generic 2-1:1.0: generic converter detected
[  141.717720] usb 2-1: generic converter now attached to ttyUSB0
[  141.730578] usbcore: registered new interface driver cp210x
[  141.730705] usbserial: USB Serial support registered for cp210x

root@vagrant:~# lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 10c4:ea60 Cygnal Integrated Products, Inc. CP210x UART Bridge / myAVR mySmartUSB light
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

```

We're going to need `pip` later, so:

```bash
root@vagrant:~# apt-get install python-pip
```

### On ArchLinux

Luckily, the vagrant base box `terrywang/archlinux` comes with usbserial support.

```bash
$  vagrant init terrywang/archlinux
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```

Edit the `Vagrantfile`, adding a block containing the USB data from above and configuring the VM:

```ruby
config.vm.provider "virtualbox" do |vb|
  vb.customize ["modifyvm", :id, "--usb", "on"]
  vb.customize ["modifyvm", :id, "--usbehci", "on"]
  vb.customize ["usbfilter", "add", "0",
    "--target", :id,
    "--name", "My NodeMCU device",
    "--vendorid", "10c4",          # your idVendor from above
    "--productid", "ea60"          # your idProduct from above
  ]
end
```

Make sure that the USB interface is not already attached to another VM.

```bash
$ vagrant up
$ vagrant ssh
(...)
vagrant@archlinux:~$ sudo modprobe usbserial
vagrant@archlinux:~$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 10c4:ea60 Cygnal Integrated Products, Inc. CP210x UART Bridge / myAVR mySmartUSB light
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
```

Before installing PlatformIO on ArchLinux, we need to install Python2 alongside python3:

```bash
vagrant@archlinux:~$ sudo pacman -Sy
vagrant@archlinux:~$ sudo pacman -S python2 python2-pip
resolving dependencies...
looking for conflicting packages...

Packages (7) python2-appdirs-1.4.0-4  python2-packaging-16.8-1  python2-pyparsing-2.1.10-1  python2-setuptools-1:28.8.0-1  python2-six-1.10.0-2  python2-2.7.12-2
             python2-pip-8.1.2-1
```

Some additional rights management, otherwise vagrant user may not pip-install packages.

```bash
$ sudo -i
~# groupadd staff
~# gpasswd -a vagrant staff
~# gpasswd -a vagrant uucp
~# exit
```

## Installing PlatformIO

We need python-pip to be able to install the PlatformIO client. Our vagrant
user needs to be part of the staff group, plus we need to adapt some permissions
so that pip is able to install files in `/usr/local/bin`, without sudo.

```bash
$ sudo -i
~# gpasswd -a vagrant staff
~# chmod g+w /usr/local/bin /usr/bin /usr/lib/python2.7/site-packages/
~# chgrp staff /usr/local/bin /usr/bin /usr/lib/python2.7/site-packages/
```

Log off and on again, as vagrant user:
```bash
vagrant@vagrant:~$ exit
$ vagrant ssh
```

### Ubuntu
```bash
vagrant@vagrant:~$ pip install platformio
```

### ArchLinux

On Arch, it's `pip2` because we choose to run on Python 2.7, so:

```bash
vagrant@vagrant:~$ pip2 install platformio
```

```bash
Downloading/unpacking platformio
  Downloading platformio-3.1.0-py27-none-any.whl (113kB): 113kB downloaded
(...)
Successfully installed platformio lockfile click bottle requests semantic-version
Cleaning up...


vagrant@vagrant:~$ which pio
/usr/local/bin/pio


vagrant@vagrant:~$ pio
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

Ready :) Just a simple test, on my board:

```bash
vagrant@archlinux:~$ pio device list
/dev/ttyUSB0
------------
Hardware ID: USB VID:PID=10C4:EA60 SER=0001 LOCATION=2-1
Description: CP2102 USB to UART Bridge Controller

vagrant@archlinux:~$ pio device monitor
--- Miniterm on /dev/ttyUSB0  9600,8,N,1 ---
--- Quit: Ctrl+] | Menu: Ctrl+T | Help: Ctrl+T followed by Ctrl+H ---
```

It's connected to the serial line of the board. If you see errors like `Unable to connect to /dev/ttyUSB0`, please check access rights on this device file and adjust user rights accordingly.
