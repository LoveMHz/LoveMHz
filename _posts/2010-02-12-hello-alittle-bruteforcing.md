---
layout: post
status: publish
published: true
title: Hello + Alittle Bruteforcing
author: Dustin
date: '2010-02-12 06:25:00 -0500'
date_gmt: '2010-02-12 06:25:00 -0500'
categories:
- Article
tags: []

thumb: generic.png
---
Fuck.. I hate writing these. Ok well I've already written one blog introduction
post straight up as hello, one as pretending to actually care about blogging,
and the last one I was doing exactly this (Going on and on about how much I
  hate writing the first blog post).

So my handle is LoveMHz and I'm the lead programmer for XeLove. I also run a few
other smaller groups outside of the Xbox Homebrew'ing scene, but that's a story
for a different day. Currently the only program I've released is XeDebug, but it
won't stay that way for long ;)

* * *

Well that seems like enough for an introduction post.. Now on to what I've been
working on today. A few years back I purchased a USB Skype phone off of eBay, or
simply named "USB PHONE" as printed on the device. Sadly since then I've managed
to lose the drivers. No biggy right? Negative. Google'ing for the hardware ID
and product vendor returned everything and anything but what I was looking for.
That was pretty much the first half of my day.

The next few hours included resoldering the broken SMD mini USB port on the
phone. Who's idea was it to design a phone, use a SMD connector, and **ONLY HOLD
IT IN PLACE BY THE PINS.**

Then the fun part, seriously. Reverse engineering the protocol. Now the way the
phone works is quite simple. Included with the CD, remind you I lost this, is an
application that draws the menus to the phone. So just a simple I/O situation.
Too bad I don't have the program. Would be as simple as capturing the USB data
packets. So my idea was to bruteforce the first few bytes and see what happens.

After a few test runs I noticed that while the first byte was set to 3 it was
only changing the LED and the screen's backlight. Now while it was set to 4 it
was changing the LCD data. After a few hours I came up with this.

```cpp
OutputReport[0]  = 0x00; // Always 0.  
OutputReport[1]  = 0x03; // Type. 3 = Draw Screen. 4 = Control LEDs/Blacklight  
OutputReport[2]  = 0x00; // Unknown  
OutputReport[3]  = 0x07; // Offset Block Y 0 - 7\. 0 is bottom  
OutputReport[4]  = 0x40; // Offset Pixel X 0 - 0x70 or so. 0 is left side  
// To draw to the screen we must do it block by block.  
// A block is 7x8\. Each byte sent is a column. And each bit of that is a pixel.  
OutputReport[5]  = 0x00; // Pixel Data -- Left most column  
OutputReport[6]  = 0x00; // Pixel Data  
OutputReport[7]  = 0xFF; // Pixel Data  
OutputReport[8]  = 0xFF; // Pixel Data  
OutputReport[9]  = 0xFF; // Pixel Data  
OutputReport[10] = 0xFF; // Pixel Data  
OutputReport[11] = 0xFF; // Pixel Data  
OutputReport[12] = 0xFF; // Pixel Data  
OutputReport[13] = 0xFF; // Pixel Data -- Right most column
```

Now I haven't exactly finished this since I still have to write up the docs for
the keypad and the backlight/LED. More on that later and maybe some more source.

* * *

Postnotes
Nothing much was found on the MCU, Holtek HT82A832R, besides the public
datasheet.
