---
layout: post
status: publish
published: true
title: TrackPoint scrolling in Ubuntu 10.10
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 344
wordpress_url: http://www.henaredegan.com/blog/?p=344
date: '2010-10-11 12:10:51 +1100'
date_gmt: '2010-10-11 02:10:51 +1100'
categories:
- Geekery
tags:
- ubuntu
- thinkpad
- '10.10'
- trackpoint
- scrolling
- gpointing
comments:
- id: 218
  author: henare
  author_email: henare.degan@gmail.com
  author_url: ''
  date: '2010-10-11 15:48:16 +1100'
  date_gmt: '2010-10-11 05:48:16 +1100'
  content: "Bother. That didn't survive a power cycle, similar to the issue I had
    with <a href=\"http://www.henaredegan.com/blog/2010/06/09/disable-your-touchpad-in-ubuntugnome/\"
    rel=\"nofollow\">disabling the touchpad on Ubuntu 10.04</a>.\r\n\r\nI haven't
    found a solution yet but will update this post when I do."
---
I just upgraded to <a href="https://wiki.ubuntu.com/MaverickMeerkat/ReleaseNotes">Ubuntu 10.10</a> on my <a href="http://www.thinkwiki.org/wiki/Category:T400">ThinkPad</a> and the only issue so far is that TrackPoint scrolling had stopped working. To re-enable it I used <a href="http://live.gnome.org/GPointingDeviceSettings">GPointingDeviceSettings</a> and set the following settings:

<img src="/blog/wp-content/uploads/2010/10/ubuntu_1010_trackpoint_scroll.png" alt="" title="ubuntu_1010_trackpoint_scroll" width="631" height="534" class="aligncenter size-full wp-image-351" />

The important change was to set the wheel emulation's button to **2**, not **4** as it appeared to have been set to. If you don't have GPointingDeviceSettings installed:

    sudo apt-get install gpointing-device-settings

## Update:

This original method didn't survive a power-cycle but I found adding this file to `/usr/share/X11/xorg.conf.d/20-thinkpad.conf` will work:

```
Section "InputClass"
    Identifier "Trackpoint Wheel Emulation"
    MatchProduct "TrackPoint"
    MatchDevicePath "/dev/input/event*"
    Driver "evdev"
    Option "EmulateWheel" "true"
    Option "EmulateWheelButton" "2"
    Option "Emulate3Buttons" "false"
    Option "XAxisMapping" "6 7"
    Option "YAxisMapping" "4 5"
EndSection
```
