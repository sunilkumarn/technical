---
author: sunilkumarn
comments: true
date: 2010-08-01 09:08:54+00:00
layout: post
slug: tata-docomo-internet
title: TATA DOCOMO INTERNET
wordpress_id: 41
---

Inorder to set up an internet connection using TATA DOCOMO in debian using USB ,u need to do the following steps:
Install 'wvdial' ----U will need Debian 5.0 -1 & 2 DVDs to do so.
Install it using 
**apt-get install wvdial**
Once u have finished installing wvdial create a file_ 'wvdial.conf'_ in the /etc directory.Edit the settings for your TATA DOCOMO connection in the  file 'wvdial.conf' .
The settings for the connection would be as follows:



[Dialer Defaults]

Init1 = ATZ

Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0

Password = MYPASSWORD

Phone = MYISPPHONENUM

Modem Type = USB Modem

Stupid Mode = yes

Baud = 460800

Dial Command = ATDT

Modem = /dev/ttyACM0

ISDN = 0

Username = MYUSERNAME

Carrier Check = yes

Auto Reconnect = yes



[Dialer Docomo]

Stupid Mode = yes

Password = Docomo

Auto Reconnect = yes

Username = Docomo

Phone = *99***1#

Save the file after editing,dial using 
wvdial docomo
Certain messages will displayed in the terminal and you will obtain your
primary DNS address :
secondary DNS address: 
Now open 'resolv.conf' in the /etc directory and edit the file and place your DNS addresses in the file as
**nameserver:primary DNS address
nameserver:secondary DNS address**
Now save the file and do wvdial docomo and you will obtain your internet connection.





