---
author: sunilkumarn
comments: true
date: 2010-08-08 17:05:48+00:00
layout: post
slug: debugging-with-the-gdb
title: Debugging with the GDB
wordpress_id: 100
categories:
- Tools
---

GNU debugger(GDB) allows you to view the step by step execution of the program code. This proves of great help in executing complex programs(specially the ones involving recursion) to see the exact flow of control. How to debug using the gdb and a few options used are worth notable:
To debug a program with gdb , steps followed are
'cc -g file.c'
gdb a.out. ....'a.out is your executable...'
You will see messages about the gdb version displayed on the terminal....
Now set the break point by 
'break main'
Use the 'run' command to start running the program.
The 3 commonly used options are 's' ,'n' ,'q' with the gdb are… 
's' moves to the next line of code-it enters functions as well.
'n' moves to the next line of code as well but does not enter functions.
'q' is used to quit from the process.

A debugging process would be like this...

sunil:~/new# cc -g quicksort.c 
sunil:~/new# gdb a.out 
**GNU gdb 6.8-debian 
Copyright (C) 2008 Free Software Foundation, Inc. 
License GPLv3+: GNU GPL version 3 or later  
This is free software: you are free to change and redistribute it. 
There is NO WARRANTY, to the extent permitted by law.  Type "show copying" 
and "show warranty" for details. 
This GDB was configured as "i486-linux-gnu"... **
(gdb) break main 
**Breakpoint 1 at 0x80483e5: file quicksort.c, line 7**. 
(gdb) run 
**Starting program: /root/new/a.out 
Breakpoint 1, main () at quicksort.c:7 
7		int left=0,right=4,i; **
(gdb) s 
8		**int array[]={4,8,1,9,3}; 
.../*
...continue the process...
.../***
(gdb) q
Quits from the debugger









