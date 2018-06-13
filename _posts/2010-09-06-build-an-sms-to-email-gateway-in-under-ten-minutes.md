---
layout: post
status: publish
published: true
title: Build an SMS to email gateway in under ten minutes
author:
  display_name: henare
  login: henare
  email: henare.degan@gmail.com
  url: http://www.henaredegan.com/
author_login: henare
author_email: henare.degan@gmail.com
author_url: http://www.henaredegan.com/
wordpress_id: 329
wordpress_url: http://www.henaredegan.com/blog/?p=329
date: '2010-09-06 09:42:14 +1000'
date_gmt: '2010-09-05 23:42:14 +1000'
categories:
- Geekery
tags:
- sms
- gammu
- mutt
- sms to email
comments: []
---
If you use an SMS gateway, such as <a href="http://www.clickatell.com/">Clickatell</a>, to send SMS from your website or computer, you might want to set up your own simple return path for people to reply to those SMS and have them emailed to you.

With an always-on **Ubuntu 10.04 machine**, an **old mobile** phone and a cheap **pre-paid SIM**, you can have this setup in under 10 minutes.

First, install the required software; the <a href="http://wammu.eu/smsd/">Gammu SMS daemon</a> for getting the SMS off the mobile and <a href="http://www.mutt.org/">Mutt</a> for sending our emails.

    $ sudo apt-get install gammu-smsd mutt

Put this little shell script somewhere like `/var/spool/gammu/mail_wrapper.sh`, customising the email addresses. It will be used to send the email when an SMS is received:

```bash
#!/bin/bash
export EMAIL="SMS Daemon <sms@example.com>"
echo | mutt you@example.com -i "/var/spool/gammu/inbox/$1" -a "/var/spool/gammu/inbox/$1" -s "SMS Received"
```

Attach your phone to your computer (mine was USB) and select "Phone mode" or similar on the mobile (as opposed to mass storage mode or photo mode, for example). Follow your system log to see what port your mobile shows up on, you should see output similar to this:

```
$ tail -f /var/log/syslog
Sep  6 09:00:22 smsgateway kernel: [105224.650743] usb 1-4: new high speed USB device using ehci_hcd and address 6
Sep  6 09:00:22 smsgateway kernel: [105224.991701] usb 1-4: configuration #3 chosen from 1 choice
Sep  6 09:00:22 smsgateway kernel: [105224.995894] cdc_acm 1-4:3.1: ttyACM0: USB ACM device
Sep  6 09:00:22 smsgateway kernel: [105224.997427] cdc_acm 1-4:3.3: ttyACM1: USB ACM device
Sep  6 09:00:22 smsgateway kernel: [105225.000013] cdc_wdm 1-4:3.7: cdc-wdm0: USB WDM device
```

The logs above show my mobile exposing two USB modem ports, `/dev/ttyACM0` and `/dev/ttyACM1`. Trial and error lead me to discover that `/dev/ttyACM1` was the port I was looking for.

Now configure this port in your `/etc/gammu-smsdrc` file with the line `port = /dev/ttyACM1` and add the line `runonreceive = /var/spool/gammu/mail_wrapper.sh` so that the script we created earlier is executed when an SMS is received. The complete `/etc/gammu-smsdrc` file should look similar to this:


```
# Configuration file for Gammu SMS Daemon

# Gammu library configuration, see gammurc(5)
[gammu]
# Please configure this!
port = /dev/ttyACM1
connection = at
# Debugging
#logformat = textall

# SMSD configuration, see gammu-smsdrc(5)
[smsd]
service = files
logfile = syslog
# Increase for debugging information
debuglevel = 0

# Paths where messages are stored
inboxpath = /var/spool/gammu/inbox/
outboxpath = /var/spool/gammu/outbox/
sentsmspath = /var/spool/gammu/sent/
errorsmspath = /var/spool/gammu/error/

runonreceive = /var/spool/gammu/mail_wrapper.sh
```

Now start Gammu's smsd and, with the configuration above, Gammu will log activity to syslog so you can send an SMS and see the fruits of your labour:

```
$ sudo service gammu-smsd start
 * Starting Gammu SMS Daemon gammu-smsd                                                                                                                                   [ OK ]
$ tail -f /var/log/syslog
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: Configuring Gammu SMSD...
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: SHM token: 0xffffffff
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: Warning: No PIN code in /etc/gammu-smsdrc file
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: commtimeout=30, sendtimeout=30, receivefrequency=0, resetfrequency=0
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: checks: security=1, battery=1, signal=1
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: deliveryreport = no
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: phoneid =
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: Inbox is "/var/spool/gammu/inbox/" with format "standard"
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: Outbox is "/var/spool/gammu/outbox/" with transmission format "7bit"
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: Sent SMS moved to "/var/spool/gammu/sent/"
Sep  6 09:09:00 smsgateway gammu-smsd[11229]: SMS with errors moved to "/var/spool/gammu/error/"
Sep  6 09:09:00 smsgateway gammu-smsd[11232]: Using FILES service
Sep  6 09:09:00 smsgateway gammu-smsd[11232]: Starting phone communication...
Sep  6 09:09:13 smsgateway gammu-smsd[11232]: Received message from +61412345678
Sep  6 09:09:13 smsgateway gammu-smsd[11232]: Received IN20100902_165719_00_+61412345678_00.txt
Sep  6 09:09:13 smsgateway gammu-smsd[11258]: Starting run on receive: "/var/spool/gammu/mail_wrapper.sh" IN20100902_165719_00_+61412345678_00.txt
Sep  6 09:09:13 smsgateway gammu-smsd[11232]: Process finished successfully
```

If you run into any problems or need some help, please leave a comment below.

_Note that there are more sophisticated ways of setting this up and many gateway providers have a much easier method to achieve the same result without you having to set up anything._
