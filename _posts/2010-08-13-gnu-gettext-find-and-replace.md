---
layout: post
status: publish
published: true
title: GNU gettext find and replace
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 322
wordpress_url: http://www.henaredegan.com/blog/?p=322
date: '2010-08-13 15:51:44 +1000'
date_gmt: '2010-08-13 05:51:44 +1000'
categories:
- Geekery
tags:
- bash
- gnu
- gettext
- find and replace
comments: []
---
<a href="http://www.gnu.org/software/gettext/"><img src="/blog/wp-content/uploads/2010/08/gnu-head-sm.jpg" alt="" title="gnu-head-sm" width="129" height="122" class="alignright size-full wp-image-323" /></a>

<a href="http://www.gnu.org/software/gettext/">GNU gettext</a> is used by many open source projects for translation support.

If you need to just do a find and replace in gettext source files, try this out to do a whole directory at once:

```bash
for i in *.po; do
  echo "Processing $i"
  msgfilter --no-wrap sed -e "s/OLD_TEXT/NEW_TEXT/g" < $i > /tmp/gettext
  mv /tmp/gettext $i
done
```

I couldn't find a way to do the edit in place (a la `sed -i`), if you know of a way please let me know in the comments.
