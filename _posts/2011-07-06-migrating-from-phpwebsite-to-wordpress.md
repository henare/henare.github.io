---
layout: post
status: publish
published: true
title: Migrating from phpWebSite to Wordpress
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_url: http://www.henaredegan.com/
wordpress_id: 423
wordpress_url: http://www.henaredegan.com/blog/?p=423
date: '2011-07-06 13:51:57 +1000'
date_gmt: '2011-07-06 03:51:57 +1000'
categories:
- Geekery
- Hacking
tags:
- wordpress
- phpws
- phpwebsite
- migration
- phpmyadmin
- libreoffice
- calc
- csv
comments:
- id: 874
  author: erietta
  author_url: http://www.eriontheinterweb.com
  date: '2011-07-08 12:44:49 +1000'
  date_gmt: '2011-07-08 02:44:49 +1000'
  content: Not a comment on the migration, which is out of my league but a comment
    on the choice of software -- You have gone from a CMS to a blog as a CMS platform.
    How have you anticipated Wordpress serving you as a CMS? Why did you not migrate
    to  Drupal, or another CMS this time round?
- id: 876
  author: henare
  author_url: ''
  date: '2011-07-08 13:01:29 +1000'
  date_gmt: '2011-07-08 03:01:29 +1000'
  content: "That's a good question.\r\n\r\nI've actually heard Wordpress referred
    to as a CMS and while I think that's overstating it a bit, Wordpress is definitely
    more than just a blogging platform these days.\r\n\r\nI think the introduction
    of <a href=\"https://codex.wordpress.org/Post_Types\" rel=\"nofollow\">Custom
    Post Types</a> in Wordpress 3.0 is what brought Wordpress closer to a CMS in my
    mind.\r\n\r\nAnyway, back to your question. For the site in question I actually
    wanted to reduce the functionality available, not increase it. Why? Because it
    wasn't being used and so I wanted to make it simpler to contribute to the site.\r\n\r\nWith
    the new Wordpress site, I'm trying to do everything as Posts and not introduce
    additional complexity like an image gallery plugin for example. So instead of
    having to work out the best place to put content, contributors can use one set
    of instructions for creating new content and not have to worry where or how to
    upload it."
- id: 878
  author: erietta
  author_url: http://www.eriontheinterweb.com
  date: '2011-07-08 17:46:56 +1000'
  date_gmt: '2011-07-08 07:46:56 +1000'
  content: "Simplicity. Enough said. Having worked with clients on large scale CMS
    I can definitely advocate the value of reducing functionality.\r\n\r\nCongrats
    on a successful migration!"
---
<a href="/blog/wp-content/uploads/2011/07/wordpress-logo.png"><img src="/blog/wp-content/uploads/2011/07/wordpress-logo.png" alt="" title="Wordpress logo" width="150" height="150" class="alignright size-full wp-image-426" /></a>

About seven years ago I bet on the wrong horse. I chose <a href="http://phpwebsite.appstate.edu/">phpWebSite</a> as the CMS to run a site for a community group I'm a part of.

Why the wrong horse? Well seven years ago <a href="http://wordpress.org/">Wordpress</a> wasn't in the game but I do remember evaluating <a href="http://drupal.org/">Drupal</a> and whilst it has a vibrant, active community the same cannot be said for phpWebSite.

I wanted to give our site a visual refresh, make it easier for people to contribute and to move to a more secure platform than the out of date version of phpWebSite we were running on. The obvious choice was Wordpress.

I've looked before for tools to migrate from phpWebSite to Wordpress but never found anything so I decided to write a tool myself. As I was getting started writing a tool, the friend I was working on the migration with discovered a <a href="https://wordpress.org/extend/plugins/csv-importer/">CSV importer plugin</a> already written for Wordpress so we decided to see how hard it would be to export data from phpWebSite as CSV that this plugin could understand.

As we didn't have huge amounts of content it turned out to be much easier to export the Announcement posts as CSV using <a href="http://www.phpmyadmin.net/">phpMyAdmin</a> and manually recreate everything else (just a handful of comments and some image galleries).

The trick with exporting the Announcement posts was to use the _CSV for MS Excel_ option of phpMyAdmin and then manipulate the data using <a href="http://www.libreoffice.org/features/calc/">LibreOffice Calc</a> into the format expected by the CSV Importer plugin.

Since we only had a handful of comments I simply recreated these using the standard Wordpress UI and manually set the dates to match phpWebSite. Photos are stored under `images/<module name>/` so I just copied the `images/photoalbum` directory and uploaded all the images in each gallery using the usual Wordpress uploader.
