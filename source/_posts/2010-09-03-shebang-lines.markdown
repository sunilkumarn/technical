---
author: sunilkumarn
comments: true
date: 2010-09-03 17:40:28+00:00
layout: post
slug: shebang-lines
title: Shebang lines...
wordpress_id: 179
---


**Shebang**(also known as 'hashbang') refers to the characters #! when they are the first two characters (occurring at the left most corner, i.e the beginning of the very first line) of your script. The particular line that contains the shebang is the shebang line in your script.
Now what's shebang meant to do.?
In a unix-like operating system , [the program loader](http://en.wikipedia.org/wiki/Loader_(computing)) (part of the operating system thats responsible for loading programs) finds the presence of these two characters(#!) , identifies it as a script and tries to execute it with the interpreter that will be specified using the remaining lines of the script.
For example consider writing a shebang line for a python script..
First and foremost you need to find the location of the interpreter for that particular script...
**$which python
/usr/bin/python**
Therefore...the shebang line should be set as...
**#! /usr/bin/python**
Specifying the shebang line would execute your script as follows:
./'script name'.py       instead of doing     'python 'script name'.py'
You could try out the use of shebang lines for any of the following types of scripts 
'werbel', 'tinypy' ,'perl'.....and many more...

