---
layout: post
status: publish
published: true
title: Add a default page title with HAML in Rails
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_url: http://www.henaredegan.com/
wordpress_id: 243
wordpress_url: http://www.henaredegan.com/blog/?p=243
date: '2010-05-04 11:11:29 +1000'
date_gmt: '2010-05-04 01:11:29 +1000'
categories:
- Hacking
tags:
- planningalerts
- ruby
- rails
- haml
- page title
- default
- ruby on rails
comments:
- id: 99
  author: erietta
  author_url: http://www.eriontheinterweb.com
  date: '2010-05-10 13:38:18 +1000'
  date_gmt: '2010-05-10 03:38:19 +1000'
  content: I am no coder, but its a good seo move. Title is an important property.
- id: 100
  author: henare
  author_url: ''
  date: '2010-05-10 13:42:35 +1000'
  date_gmt: '2010-05-10 03:42:35 +1000'
  content: Thanks Eri, that's a good point that I hadn't really considered.
---
In the <a href="http://www.planningalerts.org.au/">PlanningAlerts</a> project, we recently added a default page title for those pages we forget to add specific titles to. I added the initial code and Matthew <del datetime="2010-05-04T00:23:20+00:00">did it properly</del> cleaned it up, <a href="http://github.com/openaustralia/planningalerts-app/commit/b0229183fd8d24d8299190f26eca0915ce1c21b1">here's the diff</a>:

```diff
diff --git a/app/views/layouts/application.haml b/app/views/layouts/application.haml
index 27d81d9..6b42b26 100644
--- a/app/views/layouts/application.haml
+++ b/app/views/layouts/application.haml
@@ -2,10 +2,7 @@
 %html(xml:lang="en" lang="en" xmlns="http://www.w3.org/1999/xhtml")
   %head
     %meta(content="text/html; charset=utf-8" http-equiv="Content-Type")
-    - if @page_title
-      %title PlanningAlerts | #{@page_title}
-    - else
-      %title PlanningAlerts | Email alerts of planning applications near you
+    %title PlanningAlerts | #{@page_title || "Email alerts of planning applications near you"}
     - if @rss
       %link(rel="alternate" type="application/rss+xml" title="RSS" href=@rss)
     = stylesheet_link_tag "memespring", "main", :media => "all"
```

I thought other people might find this little hack useful for adding default page titles to their HAML templated Ruby on Rails projects.
