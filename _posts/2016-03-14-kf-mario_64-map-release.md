---
layout: post
status: publish
published: true
title: KF-Mario_64 - Map Release
permalink: /article/kf-mario_64-map-release/
author: Dustin
date: '2016-03-14 07:21:21 -0400'
date_gmt: '2016-03-14 12:21:21 -0400'
categories:
- Article
tags: []
comments: []
img: kf-mario/header.png
thumb: generic.png
---
[Project Page](http://steamcommunity.com/sharedfiles/filedetails/?id=645410401)
[Steam Workshop](http://steamcommunity.com/sharedfiles/filedetails/?id=645410401)

It's not everyday that I get to sit down and work on something that excites me
as much as this project. Over the years I've taken great joy in working on video
game related projects. When [Killing Floor 2](http://store.steampowered.com/app/232090/)
entered Early Access on Steam last year I knew I wanted to finally make a mod
for this game.

<!--more-->

# Screenshots

![Screenshot 1](/assets/img/blog/kf-mario/20160314182448_1.jpg)

|    |    |
| ![Screenshot 1](/assets/img/blog/kf-mario/20160314182143_1.jpg) | ![Screenshot 2](/assets/img/blog/kf-mario/20160314182406_1.jpg) |
| ![Screenshot 3](/assets/img/blog/kf-mario/20160314181435_1.jpg) | ![Screenshot 4](/assets/img/blog/kf-mario/20160314181641_1.jpg) |

* * *

# 3D Modeling

![Blender Screenshot](/assets/img/blog/kf-mario/kf_mario_blender.png)

The 3D modeling aspect of this project was fairly straight forward. The map is
based off of the amazing
[Peach Castle](https://sketchfab.com/models/a21cffbe8b8c4ae9b1614f26f2da8fed)
by [mStuff](https://sketchfab.com/mstuff). Most the work myself was optimizing 
textures, fixing geometry, and simplifying the ground model to work better in
the Unreal Engine without having to create collision meshes.

The inside of the castle was ripped from Super Mario 64 using modified debug
code I had written for [Love364](http://lovemhz.com/projects/love364/). Most of
the work there was fixing vertices and some oddities from development
optimizations in the game. The UV Map was completely redone by hand.

The map consists of 116,681 Verts, 89,222 Faces, and 178,422 Tris.

* * *

# Unreal Engine 3.5

![Unreal Editor](/assets/img/blog/kf-mario/kf_mario_unreal.jpg)

It's been awhile since I've worked with the Unreal Engine, and the first time
I've used anything newer than Unreal Engine 2, but it's interface and options
over the years are for the most part the same.

Most of this project consisted of working inside of the Unreal Editor. Tweaking,
and even more tweaking, pathing was one of the more challenging aspects. The key
to any good Killing Floor map is setting up an unseemingly predictable paths
with AI spawners behind every corner. Additional work was performed on adding
blocking elements to prevent exploitable map spots.

Skybox, global lighting, normal mapping, etc was applied. Currently, Killing
Floor 2 doesn't support custom Materials and instead requires creators to use a
limited set of premade Materials via Material Constants. Due to this, a lot of
time was not spent trying to improve the lighting beyond simple world settings
due to limitations. (All the textures have prebaked shadows and the lack of
additional lighting tricks isn't visual missed)

* * *

# Steam Workshop

![kf_mario_steamworkshop](/assets/img/blog/kf-mario/kf_mario_steamworkshop.jpg)

Not much to say about the Steam Workshop, but I did want to talk about it none
the less. Starting this project was I unsure of the submission process involved
and put it off until the map was completed. I was very surprised to find out
that through a simple upload tool provided with the SDK I was able to click a
few buttons and it uploaded to Steam. Since then I've added screenshots and
descriptions. Already in the last few weeks I've had amazing feedback. It
appears Steam has created a great platform for game content.

* * *

# Conclusion

At the end of this I must say I'm really happy with how everything turned out.
It was a short project, 3-4 days, but seeing other people enjoy something you
created is none the less as enjoyable. It was also a great refresher in 3D
modeling and the Unreal Engine. Hopefully, in the near future, I will return and
either improve this map or create something new entirely. Though I may wait
until Tripwire adds proper support for Unreal Materials.
