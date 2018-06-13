---
layout: post
status: publish
published: true
title: Java and SSL on FreeBSD
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 150
wordpress_url: http://www.henaredegan.com/blog/?p=150
date: '2010-02-28 09:44:15 +1100'
date_gmt: '2010-02-27 23:44:15 +1100'
categories:
- Geekery
tags:
- howto
- google
- freebsd
- jira
- java
- ssl
- imap
- pop
- google apps
comments: []
---
Running Jira on FreeBSD, I wanted to be able to pick up email from a Google Apps account and feed it in as tickets. This is normally a straight-forward process but I was getting these errors in the Jira logs:


    javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target

So it seemed like the Gmail SSL certificate for IMAP or POP wasn't validating. That's weird as it would be a major problem if it didn't and they're certainly not using a self-signed certificate.

I checked the keystore and it appeared empty so it looks like FreeBSD's Java doesn't ship with the normal list of CAs. To fix this I did:

```
[henare@freebsd ~]$ ls -l /usr.local/diablo-jdk1.6.0/jre/lib/security/cacerts
-rw-r--r--  1 root  wheel  942    Aug  6  2007 /usr/ports/java/jdk16/files/cacerts
[henare@freebsd ~]$ #wow, that file's really small (this directory was in our JAVA_HOME - don't ask me about the usr.local thing)
[henare@freebsd ~]$ #first, backup this file
[henare@freebsd ~]$ sudo cp /usr/local/diablo-jdk1.6.0/jre/lib/security/cacerts{,_backup`date +%Y%m%d`}
[henare@freebsd ~]$ #now, copy over the file from ports
[henare@freebsd ~]$ sudo cp /usr/ports/java/jdk16/files/cacerts /usr.local/diablo-jdk1.6.0/jre/lib/security/cacerts
[henare@freebsd ~]$ ls -l /usr.local/diablo-jdk1.6.0/jre/lib/security/cacerts
-rw-r--r--  1 root  wheel  40624 Aug  6  2007 /usr/ports/java/jdk16/files/cacerts
[henare@freebsd ~]$ #that looks better
```
