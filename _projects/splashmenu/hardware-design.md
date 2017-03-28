---
layout: project
title:  "SplashMenu Hardware Design"
permalink: /projects/splashmenu/hardware-design/
img: splashmenu-hardware-design/thumbnail.png
thumb: generic.png
carousel:
- splashmenu-hardware-design/single01.png
tagged:
client:
website:
---
**Description**

Commodity hardware was key to achieving cost effective hardware for small to
mid-size restaurants. The hardware chosen was based on it have fairly standard
PC components. This provided us a bases for porting a heavily modified version
of Android ported for x86. Changes were made to the kernel related to unique
oddities of the Chinese based hardware, such as a flipped and often
misconfigured touch screen controller.

The Operating System is based off of Android. It only allows for the SplashMenu
application to be ran, which is a basic Java application that loads up an
offline version of the SplashMenu’s cloud application. The Android app extends
the Webkit view of the application and allows access to certain external devices
such as printers.

**Features**
* Secure Boot – USB booting was disabled all devices expect for ones that
matched a private/public key share for system repair.
* Automatic Updates – Updates are automatically download and installed in the
background.
* Recovery Mode – In case of a user error or system error, the system can be
restored to a previous backup or to factor settings located in a protected
partition.
* Realtime Online Order Updates – The system automatically connects to the
SplashMenu’s cloud application and notifies users of updates to orders.
* GSM Connectivity – Alongside an available Wifi dongle, sold separately, the
SplashMenu POS system also allows for a GSM modem dongle to used for online
connectivity.

**Challenges**
* Porting Coreboot to the unidentifiable Intel Atom based motherboard.
* Restricting the Android Operating system to only running a single approved
application.
* Implementing an automatic/background update system.
* Providing an easy to use GSM payment system. (The GSM SIM card and service was
managed by SplashMenu)
