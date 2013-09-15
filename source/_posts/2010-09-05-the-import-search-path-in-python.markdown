---
author: sunilkumarn
comments: true
date: 2010-09-05 17:27:43+00:00
layout: post
slug: the-import-search-path-in-python
title: The import search path in Python
wordpress_id: 196
categories:
- Python
---

As a python programmer, i feel it necessary to mention about the library search path in python. Python looks in several places in your system when we try and import a module in python.
Precisely , it searches in all directories mentioned in sys.path. Just try and see the directories in sys.path.
>>>import sys
>>>**sys.path**
This returns you a list of the directories where python does a 'look in' to import your module. This list may vary from system to system and depends on the python version that you use.For me, sys.path is :

['', '/usr/lib/python2.5', '/usr/lib/python2.5/plat-linux2', '/usr/lib/python2.5/lib-tk', '/usr/lib/python2.5/lib-dynload', '/usr/local/lib/python2.5/site-packages', '/usr/lib/python2.5/site-packages', '/usr/lib/python2.5/site-packages/Numeric', '/usr/lib/python2.5/site-packages/PIL', '/usr/lib/python2.5/site-packages/gst-0.10', '/var/lib/python-support/python2.5', '/usr/lib/python2.5/site-packages/gtk-2.0', '/var/lib/python-support/python2.5/gtk-2.0']

A few points about sys.path:

**Importing the sys module makes all the attributes and functions of the module available to you.

sys.path as i mentioned gives you the current search path.

You could add a new directory to the python path by appending the list 'sys.path'
sys.path.append(/my_directory/my_setup)**
Again view sys.path and you will find the directory mentioned above in the new search path.



