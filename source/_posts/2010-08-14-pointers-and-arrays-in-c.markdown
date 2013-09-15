---
author: sunilkumarn
comments: true
date: 2010-08-14 11:24:46+00:00
layout: post
slug: pointers-and-arrays-in-c
title: 'Pointers and Arrays in C. '
wordpress_id: 143
categories:
- C
---


Pointers as we know are used to store addresses. There is a strong relation between pointers and arrays in C.
Consider the following:
int *p;
int a[100];
p=NULL;
p=a;
Here p is initialized to the starting address of the array a[].This is equivalent to doing
p=&a[0];
the statement p=a clearly shows the relation between pointers and arrays in C. Pointers and arrays can be used instead of each other in C although there are some differences that needs to be considered. We will find out the basics in comparing an array and a pointer.
Below a[] is an array and p is a pointer that has been initialized to the base address of the array.
a[i] is equivalent to doing *(p+i)
&a[i] is equivalent to (p+i)-address of the i-th location of the array.
p=a as we saw just above is valid, whereas a=p is illegal.
Pointers are considered as normal variables and can be the increment operators, p++, whereas a++ is illegal.

Now consider the case of characters. Again let the variables have the same meaning,'a[]' for an array and 'p' for a pointer.
Consider
char a[size]=”hello”;      This can be written using pointers as
char *p=”hello”;
Here p is initialized to the starting address of the string “hello”(to the character 'h').
Now obviously the difference comes here while printing.
printf(“%s”,a) would result in printing of the string whereas printf(“%s”,*p) would result in a segfault (obviously not due to uninitialized dereferencing) but because of the fact that the pointer points to the starting of the string(a character).
Hence it should be printed using
printf(“%c”,*p)  -would print the character .
This will be have to be looped if the entire string is to printed.


