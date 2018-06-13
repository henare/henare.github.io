---
layout: post
status: publish
published: true
title: Installing the Wireshark network analyser on Ubuntu 10.04
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 288
wordpress_url: http://www.henaredegan.com/blog/?p=288
date: '2010-06-24 11:56:42 +1000'
date_gmt: '2010-06-24 01:56:42 +1000'
categories:
- Geekery
tags:
- linux
- howto
- ubuntu
- wireshark
- packet sniffing
- network
comments: []
---
[Wireshark](http://www.wireshark.org/) is a network protocol analyser, or packet sniffer, available for Ubuntu 10.04 via a simple `sudo apt-get install wireshark`. However to use it correctly, we need to change some permissions to ensure we're not running the whole application as root.

The following commands will let the `adm` group run Wireshark without elevated privileges - the `adm` group is the group that allows you to read log files, etc., I always add myself to it anyway.

```
$ sudo chgrp adm /usr/bin/dumpcap
$ sudo chmod 750 /usr/bin/dumpcap
$ ls -alF /usr/bin/dumpcap
-rwxr-x--- 1 root adm 63520 2010-04-13 01:17 /usr/bin/dumpcap*
$ sudo setcap cap_net_raw,cap_net_admin=eip /usr/bin/dumpcap
$ getcap /usr/bin/dumpcap
/usr/bin/dumpcap = cap_net_admin,cap_net_raw+eip
$
```

That's all pretty self explanatory - the `setcap` command allows that binary to use special capabilities, namely to control NICs (to set promiscuous mode for Wireshark) and capture raw traffic from NICs.
