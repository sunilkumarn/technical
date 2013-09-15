---
author: sunilkumarn
comments: true
date: 2010-09-15 03:32:22+00:00
layout: post
slug: crash-your-python-virtual-machine
title: Crash  your python Virtual machine!!!
wordpress_id: 207
categories:
- Python
---


Causing a python virtual machine to crash is not what I intended but it just happened so. But since this happened , I would just like to give an explanation about what happened and the reason for it. 

While trying out [memoization](http://sunilkumarn.wordpress.com/2010/09/15/memoizationrun-your-programs-faster/) , I tried to increase the maximum recursion limit which was set to 1000 in my system.
To change your maximum recursion limit , what you need to do is :
_import sys
sys.setrecursionlimit( MY LIMIT )_

This would change the maximum number of times that you could recurse your code segment.
Everything worked fine until I did this:
_sys.setrecursionlimit(46765)_
 
Now my recursion limit is set to 46765. I tried to run my python code which displayed a segmentation fault that you wouldn't normally associate with your python code. This tells you that your python virtual machine has crashed.
Lets try and find out the reason behind that:

Recursion consumes the stack size in the system . Setting the recursion limit to 46765 means that now the amount of stacksize that will be consumed is much greater .We know the fact the python virtual machine is written in C . As you increase your recursion limit , a point reaches where there is not enough memory for the C back end to execute our python code and this results in a segmentation fault in C that is back propagated to python and displayed in our terminal.


