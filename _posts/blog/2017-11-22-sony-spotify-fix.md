---
layout: post
title: "Fix Spotify on Sony sound gear"
excerpt: "Many people experience issues with Spotify on Sony receivers, sound bars, etc. This article provides a fix for those in pain."
categories: blog
tags: [spotify, sony, spotify connect, chromecast, soundbar, receiver, fix]
comments: true
share: true
image:
  feature: sony-header.jpg
  credit: Musaic (YouTube)
---

Sony makes high-quality receivers, sound bars, wireless speakers, etc. They're packed with features, and they usually work straight out of the box. I'm in the possession of a Sony HT-NT5 soundbar, which turns my tv into a small home theatre. The sound is truly fantastic!

Most of Sony's products provide several interfaces to Spotify, such as Spotify Connect, Chromecast or plain Bluetooth. I use Bluetooth in my car all the time, but while I'm at home, I usually stream music through the proprietary Spotify Connect protocol. It just works. However, since a few months I have started experiencing issues with Spotify Connect on my Sony soundbar. The music starts playing for about 30 seconds, after which the message _"This device is not connected to network"_ is displayed, and the music stops. So I started using Chromecast, but I have experienced several connection drops as well. I thought this was due to a software update, but many people are experiencing network issues on Sony sound gear, it seams.

 After toying around with the Spotify and soundbar settings, I found that the following three fixes alleviated the Spotify Connect and Chromecast issues (at least for now):

* Use a wired internet (Ethernet) connection instead of a wireless one; this seemed to help a lot, but not all problems were gone.
* Change the name of your device from something like 'Sony HT-NT5' to just 'Sony' or 'Soundbar'; spaces, dashes and other weird characters can create hiccups in the protocl, as it seams.
* Use the Google DNS servers! This was the final fix that got it all working. When setting up a network connection on your Sony gear, set the DNS servers manually to 8.8.8.8 and 8.8.4.4, resp. for primary and secondary DNS servers.

I'll keep this article up-to-date in case I find any other tricks that might help, but for now everything seems to work fine.