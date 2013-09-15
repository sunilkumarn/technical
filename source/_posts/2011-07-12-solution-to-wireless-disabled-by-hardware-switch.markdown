---
author: sunilkumarn
comments: true
date: 2011-07-12 13:35:52+00:00
layout: post
slug: solution-to-wireless-disabled-by-hardware-switch
title: Solution to 'Wireless disabled by Hardware switch'
wordpress_id: 477
---

Ohhh ... Suddenly my wifi goes off( a combination of DELL with Ubuntu natty) and got a display - **wireless disabled by hardware switch**. Got down to googling and finally got this.

To get back your wifi to start working, type in the following:

**$:  sudo rfkill list**

You will get back something like this.

**0: dell-wifi: Wireless LAN**
**    Soft blocked: yes**
**    Hard blocked: no**
**1: dell-bluetooth: Bluetooth**
**    Soft blocked: no**
**    Hard blocked: no**
**2: phy0: Wireless LAN**
**    Soft blocked: yes**
**    Hard blocked: no**
**4: hci0: Bluetooth**
**    Soft blocked: no**
**    Hard blocked: no**

As you could see (0,2) the Wireless Lan has been soft blocked.  We need to unblock it to get it up and working properly.

**$: sudo rfkill unblock 0**

**$: sudo rfkill unblock 2**



Restart your networking

**$: sudo /etc/init.d/networking restart**



and yup! the wifi should be back now.
