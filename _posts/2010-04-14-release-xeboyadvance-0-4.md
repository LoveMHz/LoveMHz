---
layout: post
status: publish
published: true
title: "[Release] XeBoyAdvance 0.4"
permalink: /blog/release-xeboyadvance-0-4/
author: Dustin
date: '2010-04-14 00:11:00 -0400'
date_gmt: '2010-04-14 00:11:00 -0400'
categories:
- Blog
tags: []
thumb: generic.png
---
[gallery ids="346,345,344"]

**About VirtualBoyAdvance**  
It's been a real pleasure to have worked with such a great code base and the
people whom are still active within the group. Without them none of this would
be possible.

> The VisualBoyAdvance project was started by Forgotten.[6] When this person
left the development of the emulator, the project was handed over to a team
named VBA Team, led by hacker kxu. Development on the original VisualBoyAdvance
stopped in 2004 with versions 1.7.2 and 1.8.0 beta 3, and a number of forked
versions were made by various developers in the years since then.

**Cheat Code Example: (Found in settings.xml)**

```
<game rom_name="SONICADVANC2" displayname="Sonic Advance 2" checksum="2130634743" lastplayed="1271112274" timesplayed="4" type="GBA" favorite="false">
    <cheatcodes>
        <cheatcode name="MasterCode" default="true" mastercode="true">
            <gameshark>8A4B0EDC22B74899</gameshark>
            <gameshark>1A706E7DA8A5D009</gameshark>
        </cheatcode>
        <cheatcode name="Invincibility" default="false">
            <gameshark>846754623771FACD</gameshark>
        </cheatcode>
        <cheatcode name="Always have 99 rings" default="false">
            <gameshark>29F860CEFE350599</gameshark>
        </cheatcode>
        <cheatcode name="Never Lose Rings" default="false">
            <gameshark>EA2E1E0B8FCE0633</gameshark>
        </cheatcode>
    </cheatcodes>
</game>
```

**Pokemon Fix:**

```
<game rom_name="POKEMON FIRE" displayname="Pokemon – Fire Red Version" checksum="3716707868" lastplayed="0" timesplayed="123" type="GBA" flashsize="128" />  
```

**NES Classic Fix:**

```
<game rom_name="NES METROID" displayname="Classic NES Series – Metroid" checksum="2996917187" lastplayed="0" timesplayed="0" type="GBA" favorite="false" save_type="1" />  
```

Any other game save/load issues can be fixed by playing with the Save_Type or
FlashSize. Or ask in IRC for help;D

**Features**  
[+] Full speed GB, GBC, and GBA Emulation  
[+] Screenshot output  
[+] Save/Load States  
[+] Ingame Menu via L+R Stick Click  
[+] Gameshark Cheat Code Support  
[+] Gameplay Status  
[+] Emulation Fastword  
[+] and much much more!

**New in Version 0.4**  
[+] Added Cheat Code Support  
[+] New GUI  
[+] Gameboy and GameBoy Color Support  
[+] Metroid Graphic/Audio Bugs Fixed  
[+] All memory leaks fixed  
[+] Gameplay Status  
[+] BoxArt Support  
[+] And a lot more.
