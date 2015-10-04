---
layout: post
title: Keyboard remapping
---

{{ page.title }}
================
Who uses caps lock anyway? The caps lock key has a nice spot on the keyboard, and it is rarely used compared to other keys on this row. Why not get rid of it, and get an extra control key? Less shouting on the internet, and less fatigue in the little finger.

How can this be done on Windows? We have to make a change in the registry (Reboot required).


    Windows Registry Editor Version 5.00

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
    "Scancode Map"=hex:00,00,00,00,00,00,00,00,02,00,00,00,1d,00,3a,00,00,00,00,00

Files for lazy:

<a href="/files/ChangeCapsToControl.reg" target="_blank"> Change caps lock to control</a>

<a href="/files/DisableKeyboardRemap.reg" target="_blank"> Disable custom keymapping</a>


