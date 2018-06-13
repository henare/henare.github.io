---
layout: post
status: publish
published: true
title: MongoDB `smallfiles` setting on Ubuntu for development
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 574
wordpress_url: https://www.henaredegan.com/blog/?p=574
date: '2016-01-18 20:39:12 +1100'
date_gmt: '2016-01-18 09:39:12 +1100'
categories:
- Hacking
tags:
- ubuntu
- mongodb
- configuration
comments: []
---
<img class="aligncenter size-medium wp-image-578" src="/blog/wp-content/uploads/2016/01/MongoDB-Logo-300x81.jpg" alt="Leaf logo for MongoDB" width="300" height="81" />

I recently installed MongoDB on Ubuntu to do some development and noticed it didn't start when I installed it via `sudo aptitude install mongodb`. Checking the logs I discovered the problem:

```
[initandlisten] ERROR: Insufficient free space for journal files
[initandlisten] Please make at least 3379MB available in /var/lib/mongodb/journal or use --smallfiles
```

I have a tiny SSD on my computer and I really don't need this using over 3GB just so I can do a little hacking.

OK so I'll just set `--smallfiles`. Under Ubuntu settings like arguments passed to a daemon when it starts are usually found in `/etc/defaults` but I couldn't see any such file for MongoDB.

It turned out you can also set this in the config file under `/etc/mongodb.conf` so just add this line to the bottom of that file:

    smallfiles = true
