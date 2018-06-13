---
layout: post
status: publish
published: true
title: How to encrypt a disk partition in Linux
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 360
wordpress_url: http://www.henaredegan.com/blog/?p=360
date: '2010-11-05 08:59:12 +1100'
date_gmt: '2010-11-04 22:59:12 +1100'
categories:
- Geekery
tags:
- howto
- debian
- ubuntu
- luks
- encryption
- disk
- cryptsetup
comments: []
---
<img class="alignright size-full wp-image-361" title="luks-logo-cropped" src="/blog/wp-content/uploads/2010/11/luks-logo-cropped.png" alt="" width="330" height="112" />

[LUKS](https://code.google.com/p/cryptsetup/) is the standard for Linux hard disk encryption. The following few commands are all you need to encrypt your next external hard disk or USB key on a Debian-based distribution like Ubuntu.

## Installation

Install the required software and load the module into the running kernel without restarting:

```
sudo aptitude install cryptsetup
sudo modprobe dm_crypt
```

## Drive creation
Set up encryption on the disk and mount it:

```
sudo cryptsetup luksFormat /dev/sdb1
sudo cryptsetup luksOpen /dev/sdb1 external
sudo mkfs.ext4 /dev/mapper/external
sudo mount /dev/mapper/external /mnt
```

## Mounting
How to mount the drive:

```
sudo cryptsetup luksOpen /dev/sdb1 external
sudo mount /dev/mapper/external /mnt
```

## Unmounting
How to unmount the drive:

```
sudo umount /mnt
sudo cryptsetup luksClose external
```
