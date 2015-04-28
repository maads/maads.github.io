---
layout: post
title: List active network connections
---

{{ page.title }}
================

I ran into a problem stating that port 8080 was already taken. Here is an easy way of finding out what application is occupying what ports. [netstat]('https://en.wikipedia.org/wiki/Netstat') displays incoming and outgoing network connections. (This operation requires `cmd` (or equivalent) running as administrator because of the `-b` parameter which shows the program name.)

    netstat -a -b -o

In this list I could see what application I had to change in order to get port 8080 back.
