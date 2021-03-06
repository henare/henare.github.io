---
layout: post
status: publish
published: true
title: Migrating an ASP site to a static Apache site
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_url: http://www.henaredegan.com/
wordpress_id: 131
wordpress_url: http://www.henaredegan.com/blog/?p=131
date: '2009-11-23 14:57:12 +1100'
date_gmt: '2009-11-23 04:57:12 +1100'
categories:
- Geekery
tags:
- linux
- asp
- apache
- mod_rewrite
comments: []
---
I recently needed to move a very simple ASP site to Apache for archival purposes. There was nothing of importance that the dynamic ASP was doing and as it was just for an archive, it made the most sense to turn it into simple HTML (as the whole site probably should have been in the first place). All of this was easy enough with wget -m, etc. but I came across one major stumbling block.

The files mirrored from the original site had a question mark in the file name due to them being queries and the fact that Unix supports question marks in file names. This wasn't a problem for most modern browsers as they encode the question mark before doing the request and Apache happily serves up the file. After doing a test migration of the site I noticed heaps of 404s coming from visitors to the site via search engines.

I got around this with some painful mod_rewrite hacking:

```apache
RewriteEngine On
RewriteCond %{QUERY_STRING} .
RewriteCond %{REQUEST_FILENAME} \.asp$
RewriteRule ^(.*)$ http://example.com$1\%3F%{QUERY_STRING}? [NE]
```

In semi-human-speak:

<ul>
<li>Turn the mod_rewrite engine on</li>
<li>Look for queries that have a query string (i.e. a ? and anything after it)</li>
<li>And where the filename requested is a .asp file</li>
<li>Rewrite the whole request to http://example.com
<ul>
<li>Then the original request (sans query string)</li>
<li>Then %3F (question mark encoded)</li>
<li>Then the query string and make sure there's no other query string appended (the ? at the end)</li>
<li>Also, don't encode the stuff we're throwing back to the client (the [NE])</li>
</ul>
</li>
</ul>
