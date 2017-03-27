---
layout: post
status: publish
published: true
title: Status Update
permalink: /article/status-update/
author: Dustin
date: '2010-03-16 22:01:00 -0400'
date_gmt: '2010-03-16 22:01:00 -0400'
categories:
- Article
tags: []
thumb: generic.png
---
### XeBoyAdvance

[gallery ids="349,350"]

Not a lot to talk about really.. Been working really hard on adding cheat code
support, but ran into a bug. I want to say it's a bug with GameShark's lookup
functions. Which is being a pain in the ass to debug. Other than that we have a
new UI and XML settings/database file. The new UI is really sexy.

```xml
<Game ROM_Name="METROID4USA" DisplayName="Metroid Fusion" Checksum="1819625372" LastPlayed="0" TimesPlayed="1" Type="GBA" Favorite="true">
    <CheatCodes>
        <CheatCode Name="MasterCode" Default="true" MasterCode="true">
            <GameShark>93C80FA3 DF77</GameShark>
            <GameShark>EEEE8A9B 6B5C</GameShark>
            <GameShark>00D0A839 17FE</GameShark>
        </CheatCode>
        <CheatCode Name="Multi Jump" Default="false">
            <GameShark>FD28FA5E 77C9</GameShark>
            <GameShark>FE80CB4F E749</GameShark>
        </CheatCode>
    </CheatCodes>
</Game>  
```

### XeDOSBox

[gallery ids="351"]

_Coming soon ;D_
