---
layout: post
status: publish
published: true
title: List the number of members in all Mailman lists
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_url: http://www.henaredegan.com/
wordpress_id: 257
wordpress_url: http://www.henaredegan.com/blog/?p=257
date: '2010-05-21 12:34:48 +1000'
date_gmt: '2010-05-21 02:34:48 +1000'
categories:
- Geekery
tags:
- bash
- mailman
- script
comments:
- id: 132
  author: henare
  author_url: ''
  date: '2010-05-26 10:37:51 +1000'
  date_gmt: '2010-05-26 00:37:51 +1000'
  content: 'I just realised that if the Mailman binaries are in your $PATH then you
    can remove the ''./''es in the script and it should work from anywhere. #duh'
---
Here's a little bash script to display the name of all lists in your mailman site and the number of subscribers for each list, just run it in the same directory as your mailman binaries (`/usr/lib/mailman/bin` on Debian):

```bash
#!/bin/bash
LISTS=$((`./list_lists | wc -l` - 1))
LISTS=`./list_lists | tail -n$LISTS | awk '{ print $1 }'`
for i in $LISTS; do
    echo $i `./list_members $i | wc -l`
done
```

Protip: you can also just copy and paste the lines into your terminal so you don't have to save it as a script.
