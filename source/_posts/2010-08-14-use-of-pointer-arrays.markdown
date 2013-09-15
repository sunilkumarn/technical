---
author: sunilkumarn
comments: true
date: 2010-08-14 11:22:16+00:00
layout: post
slug: use-of-pointer-arrays
title: 'Use of pointer arrays. '
wordpress_id: 141
categories:
- C
---

What will we do if we need to store more than one string in an array.?
Double dimensional would be an option. For eg.
**char array[][10]={"hello","hai","how"};** But here the the length of each string is be limited up to 10..Specifying the 'column' is a must in case of 2dimensional arrays. Even during the definition of a function.

Another option and perhaps a better one would be the use of pointers arrays .for e.g
**char *p[]={"hello","how","hai"};**
This is an example of pointer arrays. Each element of the array is used to keep pointers that point to some characters.  This method would be better in sense that the strings can be of variable lengths and obviously working with it can be a lot easier.
It will be interesting to find out the working of pointer arrays.

Consider a pointer 'p1' such that :
char *p1=”hello”
Now this particular pointer can be entered into the pointer array like the following:
p[0]=p1;    The pointer to string “hello” is entered into the 0th position of the pointer array.
Similarly the pointer array can be filled with pointers to characters.
Now in order to print a particular string whose pointer is stored in the 0th location of the pointer array we need to do.
**'printf(“%s”,p[0]); or printf(“”%s”,*(p+0));**.    Both of them would give the result as hello'.
**printf(“%d”,p[0])**; would give the pointer that is stored in the 0-th location of the array.

**for(i=0;i<3;i++)
printf(“%d\n”,p+i);**     would show you the addresses of the array locations.

Pointer arrays would be very useful when playing with strings. Suppose if you want to sort different words according to their length. All you need to do if you have stored the pointers to those words in a pointer array is to exchange the positions of the pointers.
That is , if the string whose pointer at position '0' is greater than that of the string at position '1', then
**temp=a[0];
a[0]=a[1];
a[1]=temp;**
Thats it..the pointers have exchanged their positions..Now on printing, the string whose pointer is stored at location '0' of the array 'a[]' will be printed first.


