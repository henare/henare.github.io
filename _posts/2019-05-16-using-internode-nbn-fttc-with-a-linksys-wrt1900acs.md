---
layout: post
title: Using Internode NBN FTTC with a Linksys WRT1900ACS
---

![Linksys WRT1900ACS](/blog/images/linksys-li-WRT1900ACS-1.jpg)

This post details how I got my [Linksys
WRT1900ACS](https://www.linksys.com/au/p/P-WRT1900ACS/) working with [NBN
(Australia's National Broadband Network) Fibre to the Curb
(FTTC)](https://www1.nbnco.com.au/residential/learn/network-technology/fibre-to-the-curb-explained-fttc)
provided by
[Internode](https://www.internode.on.net/support/guides/internet_access/nbn/fttc/).

Many of the things in this post almost certainly apply to other routers, NBN
service types, and ISPs. Hopefully this saves a few people out there a lot of
the time and frustration I went through to get this working.

When I first connected my NBN all the lights on the NBN NCD were solid blue,
meaning everything's connected and working. The problem was that my router
wasn't able to establish a PPPoE connection. This is because [Internode NBN FTTC
needs packets tagged with VLAN ID
2](https://www.internode.on.net/support/guides/general_settings/#nbn).

## Load OpenWRT

Yes, you'll need to load [OpenWRT](https://openwrt.org/) to get the Linksys
WRT1900ACS working on NBN FTTC. This is because the default firmware doesn't
allow you to set the Internet/WAN port to VLAN tag ID 2. When you try to set
this it says that the, _"value is out of range"_.

I suspect this is because internally the router uses VLAN ID 1 and 2 for the LAN
and Internet segments respectively. Effectively they're reserved and the router
doesn't tag packets on these interfaces.

I have the v2 version of the WRT1900ACS. Installing OpenWRT was as simple as
[downloading the
firmware](https://openwrt.org/toh/linksys/linksys_wrt1900acs#installation) and
then uploading it via the Linksys web UI. Once the firmware is installed the
router will reboot and you'll need to configure it from scratch.

## Configure the Internet port to tag packets with VLAN ID 2

Once you've done the basic configuration of your new OpenWRT powered device, log
into the web UI and go to Network > Switch.

Setup your VLAN config like this:

![WRT1900ACS Switch Config
Screenshot](/blog/images/wrt1900acs_switch_config_screenshot.png)

The key here is to set the WAN port to "tagged" on VLAN ID 2. This will allow
your router to establish a PPPoE connection.

## Tweak your PPPoE connection

Initially I didn't need to do anything special to setup my PPPoE connection. I
changed the OpenWRT WAN interface to use PPPoE, set my Internode username and
password, and it all worked. For the first 30 minutes.

Over the next day the PPPoE connection disconnected and reconnected every 5
minutes or so. I managed to get up to 2 hours from it by adjusting my MTU to
`1492`. For a PPPoE connection I _think_ this is what it's supposed to be anyway
so I think this is a good thing to do.

Go to your WAN Interface configuration page, select the _Advanced Settings_ tab,
and enter `1492` in the _Override MTU_ field.

I had also noticed in my logs the PPPoE connection disconnecting with, `No
response to 5 echo-requests`. It appears there's [a bug in OpenWRT that doesn't
correctly ignore LCP echo failures](https://github.com/openwrt/luci/issues/2112)
when you set the threshold to `0`. I've worked around this by setting the
threshold to `500`.

Here's what the relevant settings look like in the WAN interface's Advanced
Settings page:

![WRT1900ACS PPPoE Advanced Settings
Screenshot](/blog/images/wrt1900acs_pppoe_advanced_settings_screenshot.png)

## Timeline

Even after I'd done all of this my PPPoE connection still wasn't stable. It was
only another day later, suspiciously exactly on the hour, that my connection
stabilised. I'm guessing something upstream was finally provisioned or changed
that made this work.

Here's the timeline of my connection problems. It might help you understand
where your connection is up to:

### Wednesday

* NCD arrives, plug it in, after 15 minutes all lights are solid, celebrations
  are started!
* Celebrations swiftly halted when I realise my router isn't establishing a
  PPPoE connection
* After 45 minutes I decide to call Internode support since I need to get back
  to work. They advise that instead of the 15 minutes it says it will take to
  get connected in the instructions, it's actually 2 working days (WTF?!)

### Thursday

* In the morning I try Internode support again to see if there's an update with
  my provisioning. Of course they can't tell me what's happening with it but I
  do find out I need to tag packets with VLAN ID 2.
* Later in the day I decide that since my Internet isn't working I've got
  nothing to lose by trying OpenWRT and seeing if I have more luck. After doing
  basic setup and configuring VLAN ID 2 on the WAN port it Just Works!
* After 30 minutes the connection drops and keeps going up and down. I contact
  Internode support again and ask for their help fixing the PPPoE connection.
  They once again roll out the line about it taking 2 working days. They tell me
  they'll only be able to raise a fault next Tuesday (!!).

### Friday, Saturday

* I keep monitoring the connection with despair as it goes up and down, up and
  down...
* But then I have a few flashes of inspiration and change the MTU and LCP
  failure threshold. This seems to make the connection a lot more stable but it
  still drops every 2 hours or so.

### Sunday

* At 19:00 on the dot the connection re-establishes and doesn't go down for over
  3 days. The reconnection 3 days later happens at exactly 06:00 - is this some
  automated disconnect ISP-side?

If I don't update this post then hopefully that means everthing has worked out
fine :) I wish you the best of luck with your connection too. Here's to useful
Internet.
