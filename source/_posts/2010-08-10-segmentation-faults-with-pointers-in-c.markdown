---
author: sunilkumarn
comments: true
date: 2010-08-10 19:47:48+00:00
layout: post
slug: segmentation-faults-with-pointers-in-c
title: 'Segmentation faults with pointers in C... '
wordpress_id: 118
categories:
- C
---

Segmentation fault occurs when an attempt is made to access unallocated memory.
Segmentation faults with pointers might prove to be a task to deal with .This is mainly because of the fact that the actual area of the fault has to be identified and then the fault cleared. This might sometimes leave you in a pondering state. If the reasons that cause the segmentation faults are known , then it might help us to get rid of it quite quickly...
The major reasons for the segmentation fault to occur while using pointers might be the following..
Dereferencing NULL
and dereferencing an uninitialized pointer.
Both these cases would result in segmentation fault.

Whenever a segmentation fault occurs-the best way to deal with it is to find out the point of occurrence at first and then find out exactly why it happened..
The best way to spot a segmentation fault is to install gdb on your system.
This would enable you to find out the point of occurrence of the fault and the fault can be cleared.
The best practice while using pointers is always to** initialize the pointer to NULL at first**. Therefore when using the gdb you can see the null value( as 0x0 in systems)
**/*  consider a section of the debugging process*/

51	*dayofmonth=yearday;
(gdb) s
Program received signal SIGSEGV, Segmentation fault.
0x0804862f in year_day (year=12, yearday=15, _dayofmonth=0x0, month=0x0_) at pmonthday.c:51
51	*dayofmonth=yearday;

*/**

Here my pointers are 'dayofmonth' and 'month'..Hence from the debugging process its clear that the pointers have been set to NULL which indicates that we have not initialized the pointer to a legal memory address.
Again there are other methods identify the problem. Print all the values of pointers used in the program and see if any value differs from the others by a far long distance. If yes,this address space might be illegal. But theres no guarantee that this would be a solution.
Of course it may happen that sometimes you might get lucky with uninitialized pointers and there might be no segmentation fault, but its better not to make a habit of that because you might a segfault anytime. Hence its always better to initialize the pointer to NULL at first and then to a legal address.
A segmentation fault could be cleared by just setting the pointer to some legal address.
For an eg..
'p' is the uninitialized pointer that has caused the fault, just set it to the base address of an array as
char array[1000];
char *p=array;




