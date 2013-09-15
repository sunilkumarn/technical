---
author: sunilkumarn
comments: true
date: 2010-08-07 17:44:16+00:00
layout: post
slug: the-lm-command
title: The 'lm' option.
wordpress_id: 88
---

It may sometimes happen that when you use commands like sin,cos,pow etc....you get a warning message like 'undefined reference' .This may happen even when you have included the  header file  .This happens because your program has not been linked with the system library properly...
A solution to the situation would be to use the option 'lm' when you compile the file.This would fix the problem and the call would be referred to the function definitions in the system's library.
The syntax would be as follows....
**'gcc -lm filename.c'**
