---
layout: post
status: publish
published: true
title: 'Part 2: Skype Phone.'
author: Dustin
date: '2010-02-13 07:01:00 -0500'
date_gmt: '2010-02-13 07:01:00 -0500'
categories:
- Article
tags: []

thumb: generic.png
---
Well I've had more time to play around with my Skype Phone today. Pretty much
all of the reverse engineering is done. Screen drawing, keypad input, and LED
are all documented.

KEY_TopLeft1 = 00 01 01 00 01 01 01 01 01  
KEY_TopRight1 = 00 01 0D 00 01 01 01 01 01  
KEY_TopLeft2 = 00 01 02 00 01 01 01 01 01  
KEY_TopRight2 = 00 01 0E 00 01 01 01 01 01  
KEY_CenterUp = 00 01 13 00 01 01 01 01 01  
KEY_CenterDown = 00 01 16 00 01 01 01 01 01  
KEY_1 = 00 01 03 00 01 01 01 01 01  
KEY_2 = 00 01 09 00 01 01 01 01 01  
KEY_3 = 00 01 0F 00 01 01 01 01 01  
KEY_4 = 00 01 04 00 01 01 01 01 01  
KEY_5 = 00 01 0A 00 01 01 01 01 01  
KEY_6 = 00 01 10 00 01 01 01 01 01  
KEY_7 = 00 01 05 00 01 01 01 01 01  
KEY_8 = 00 01 0B 00 01 01 01 01 01  
KEY_9 = 00 01 11 00 01 01 01 01 01  
KEY_Astk = 00 01 06 00 01 01 01 01 01  
KEY_0 = 00 01 0C 00 01 01 01 01 01  
KEY_Pad = 00 01 12 00 01 01 01 01 01  
KEY_F = 00 01 1A 00 01 01 01 01 01  
KEY_Mute = 00 01 1B 00 01 01 01 01 01  
KEY_RELEASE = 00 01 00 00 01 01 01 01 01

Byte 1 - First Nibble  
bit 4 = Backlight --- Value 8  
bit 2 = Red Light --- Value 2

I started talking about the LCD in yesterday's post, but seemed to have had a
few mistakes in the documentation. These have been fixed in the first. I decided
to draw up a little chart to help describe the block draw format of the Skype
Phone even more.

[gallery ids="471"]

Also I've managed to port a simple Chip8 emulator to the Skype Phone. Pretty
much still an alpha version at the moment though. Due to it only draw to one
fourth of the screen (64x32 the native resolution of the device.), but neither
the less it's a steps forward. Below are a picture of Blinky, PacMan clone,
running. **Sorry for the blurry photo. I'll take a better one later**

[gallery ids="472"]  
Source Code:
http://www.digitalreality.co.nz/xbox360/dev/XeServer/USB_Phone.rar
