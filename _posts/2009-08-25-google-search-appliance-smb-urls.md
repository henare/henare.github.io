---
layout: post
status: publish
published: true
title: Google Search Appliance SMB URLs
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_url: http://www.henaredegan.com/
wordpress_id: 70
wordpress_url: http://www.henaredegan.com/blog/?p=70
date: '2009-08-25 18:40:43 +1000'
date_gmt: '2009-08-25 08:40:43 +1000'
categories:
- Geekery
tags:
- google
- gsa
- google search appliance
- hacks
comments:
- id: 1872
  author: Ravi
  author_url: ''
  date: '2012-12-20 17:12:24 +1100'
  date_gmt: '2012-12-20 06:12:24 +1100'
  content: "Hi there,\r\n\r\nthanks for the great help. Did you find different solution
    ?\r\n\r\nThanks\r\nRavi"
- id: 1875
  author: henare
  author_url: http://www.henaredegan.com/
  date: '2012-12-21 13:37:24 +1100'
  date_gmt: '2012-12-21 02:37:24 +1100'
  content: |-
    Hi Ravi,

    Sorry I haven't, it's not a product I've worked on for a few years now. Good luck with your search!

    Cheers,

    Henare
- id: 1879
  author: Ravi
  author_url: ''
  date: '2012-12-21 20:37:17 +1100'
  date_gmt: '2012-12-21 09:37:17 +1100'
  content: "Thanks Henare for taking time to reply.\r\n\r\nBest Regards,\r\nRavi"
---
I'm running a proof of concept at work of the <a href="http://www.google.com.au/enterprise/gsa/">Google Search Appliance</a> (GSA). Work's very much a Microsoft Windows shop and for the demo I wanted the SMB search results to link to the files on the SMB share, not the GSA proxy that is the default.

You can hack this with <a href='/blog/wp-content/uploads/2009/08/gsa.patch'>this small change</a> to the XLST, a git-diff as follows:

```
diff --git a/gsa-original b/gsa-smb
index ba1c66f..73751ab 100644
--- a/gsa-original
+++ b/gsa-smb
@@ -2680,8 +2680,7 @@ for (var j = 1; j < p.length; j++) { url += "&amp;" + p[j]; }}
         </xsl:when>
        <!-- *** URI for smb or NFS must be escaped because it appears in the URI query *** -->
         <xsl:when test="$protocol='nfs' or $protocol='smb'">
-         <xsl:value-of disable-output-escaping='yes'
-                       select="concat($protocol,'/',$temp_url)"/>
+         <xsl:value-of disable-output-escaping='yes' select="concat('file://', substring-after(U, '://'))"/>
         </xsl:when>
         <xsl:when test="$protocol='unc'">
           <xsl:value-of disable-output-escaping='yes' select="concat('file://', $display_url2)"/>
```

Now, after you collect your jaw from the floor when you realise that this will create URLs like `file://servername/path/to/file`, please remind yourself of the earlier caveats of **hack** and **Windows**. This actually took longer that it should have to debug as Firefox on Linux isn't insane enough to make this work.

If the POC works out and we decided to go ahead with the GSA use, I'd be searching for something a bit more robust (and if I'm working on it, I'll share the details of what we decide to do!).
