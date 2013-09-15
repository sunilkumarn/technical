---
author: sunilkumarn
comments: true
date: 2010-11-24 15:13:00+00:00
layout: post
slug: using-screens
title: Using Screens
wordpress_id: 420
---

You might have often had the experience where you would have had to open quite a few terminals on your screen simultaneously . This might often be the case when you are doing stuffs like data processing on particular files and you intend not to sit idle during that particular time . What you do is to keep unnecessary number of terminals open on your screen .

You could get rid of this by using the 'screens' option in Linux . This allows you to enter a new screen where you could perform time consuming processes like a data processing operation using the  'awk'  command . Start the process in the screen , exit the screen and now do your own stuff in the main console , go back to your screen after a while and view the results of  whatever operations you had performed . You could do all this stuff without the necessity of opening terminals again and again .

Create a new Screen :

**$ : screen -S 'Screen name' **

To detach from the current screen :

Use** CTRL + a + d**

To terminate a screen :

Use **CTRL + d**

To attach an already created screen :

**$ : screen -x 'screen name'**

You could keep on creating screens within screens and keep going on as and when needed  .
