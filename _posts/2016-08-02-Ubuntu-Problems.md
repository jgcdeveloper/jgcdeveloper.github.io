---
layout: post
title: Ubuntu Problems
feature-img: "/img/penguincry.png"
---

{:.center}
![Crying Penguin]({{ site.baseurl }}/img/penguincry.png)

So, as part of my preparation for the program, the first thing I decided to try and do was set up a Ubuntu Linux dev environment. Now, it has been a while since I had a Linux distro on my PC (last one being MINT) but I was looking forward to playing around with my old nemesis.

However, since Windows 8, something new called UEFI has entered the world of BIOS architecture. Although I hadn't read a lot about it, I understand it has something to do with the Windows switch to a multi device architecture. Regardless, it definitely increases the difficulty of setting up a dual boot system.

After some experimentation, I was finally able to get the system to read either the USB or the Live CD with the distro, but I was unable to get the GRUB bootloader to come up. Initial research showed
it might have been a problem with my graphics card. Others had stated it might be a bad download. I tried several downloads from different servers, but none of them worked.

Eventually, I was able to get a distro working, but I had to revert back a couple of versions. I had read that Ubuntu 14.04 was quite stable, so I decided to try and set this up. Upon first try, I actually got further then any previous attempt with version 16.04, which is what I was trying to set up before. 

The end result is that I got Ubuntu 14.04 booting nicely, with only a few errors popping up at initial bootup. Most are with regards the graphics card, but I expected that to be honest. 

In the weeks since, I have only accidentily brought the system down once. I tried to get the sleep mode working, but during this attempt I actually crashed the system. I had to boot up in console to remove what I had added to get the system to boot again.

Lesson learned... Don't mess with a working distro, at least until you have a backup option.

For now, that backup option is the wife's Macbook Pro. However, she probably wouldn't like it too much if I had to convert that to a Dev machine.





