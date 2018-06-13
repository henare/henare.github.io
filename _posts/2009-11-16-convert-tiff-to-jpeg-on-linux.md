---
layout: post
status: publish
published: true
title: Convert TIFF to JPEG on Linux
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 128
wordpress_url: http://www.henaredegan.com/blog/?p=128
date: '2009-11-16 11:29:26 +1100'
date_gmt: '2009-11-16 01:29:26 +1100'
categories:
- Geekery
tags:
- linux
- photos
- tiff
- jpeg
comments: []
---
I wanted to convert some high resolution TIFFs that Lisa took of Bill's paintings recently and came up with this quick and dirty conversion into JPEG:

    for i in *.tif; do tifftopnm "$i" | pnmtojpeg > "${i%.tif}.jpeg"; done

That'll convert all .tif files in a directory to JPEG (using default processing setting).

I didn't have `exiftool` installed, and no internerd or media to install it from, otherwise I would've looked at doing the metadata too.
