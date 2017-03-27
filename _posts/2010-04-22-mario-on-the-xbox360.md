---
layout: post
status: publish
published: true
title: Mario on the Xbox360
permalink: /article/mario-on-the-xbox360/
author: Dustin
date: '2010-04-22 22:33:00 -0400'
date_gmt: '2010-04-22 22:33:00 -0400'
categories:
- Article
tags:
- N64

thumb: generic.png
---
![mario-on-the-xbox360_1](//lovemhz.com/wp-content/uploads/2015/12/mario-on-the-xbox360_1.jpg)

Yes, that's right.. Mario is on the Xbox360.

Lack of blog posts, argh.. I told myself when I started this blog that I would
update often (at least every 3 days at the latest).

About Love364, yes I changed the name ;D Currently we have the core, RSP, and
audio implemented. A lot of work so far. Now what about video? Well it seems
that every video plugin out there either uses OpenGL or DirectX with Fixed Pixel
Pipelines which the Xbox360 does not support. Also due to the differences
between Fixed Pixel Pipe-lining and HLSL a simple rewrite isn't possible. So
with the advice and over look of Zezu I have decided to write my own up to date
graphics and RDP part of the emulation instead of hacking around Rice's video
code. Not much progress so far, expect I have been able to process and sort out
the commands coming from the RSP. Still a lot of work todo. And thus the reason
there isn't any screenshots yet.

The general scoop of this is simple massive. Things can easily go wrong with
around 20 basic commands and over 1,000 calls a second just for graphics. I
easily I have my work cut out, even when I'm spending 10-12 hours a day coding.

Writing my own graphics plugin and RDP code has it's pros, even though this just
seems like a major set back.

**Pros and Ideas:**

*   High Anti-aliasing Support
*   HLSL per texture scripting
*   Shaders, Bloom, and HDR possibilities.
*   Texture Scaling via HLSL.

So in general Love364 will hopefully redefine how N64 emulation looks and runs.

Current Emulation Status  
Currently we are still running on one CPU core with little to no optimization
with everything run minus graphics at around %50 of the N64 speed. I've yet to
look into dynrec or inline function calling, but when the time comes there's no
reason why we shouldn't be able to easily hit 100% speed on emulation.
