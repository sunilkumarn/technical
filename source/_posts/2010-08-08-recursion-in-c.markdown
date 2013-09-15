---
author: sunilkumarn
comments: true
date: 2010-08-08 16:47:39+00:00
layout: post
slug: recursion-in-c
title: Recursion in C..
wordpress_id: 97
categories:
- C
---

Recursion may be used in C programs, where one function may be called within itself. The method of recursion will be simpler to understand with a small program code. The code is as follows.

#include  
void printd(int n) 
{ 
    if (n < 0) { 
        putchar('-'); 
        n = -n; 
    } 
    if (n / 10) 
        printd(n / 10); 
    putchar(n % 10 + '0'); 
} 

The program is used to convert an integer decimal into its character digits.i.e 123 will be converted into 1
2
3
....the digits of the integer.
Lets see how the code works.
Consider the initial value of n as 123.
printd(123)				//level 1
123/10 is true:Hence 
printd(123/10);

printd(12)				//level 2
12/10 is true:Hence
printd(12/10);

printd(1)				//level 3
1/10 is not true;
hence putchar(1%10+'0'); 		//1 is printed
}

Now the call to the completed function printd(1) came from the function printd(12/10)(level 2)  and the control returns to the end of the function printd(12/10) (n being 12)
The next statement is 
hence putchar(12%10+'0');		// 2 is printed

Again the call to the completed function .i.e (printd(12)) is not from the main, but its from the function printd(123/10)(level 3) and hence the control is returned to the end of the function(n being 123).
The next statement is
hence putchar(123%10+'0');		// 3 is printed
Now the call to completed function(printd(123)) came from the main() and the control is returned there and the program proceeds.

Hope this gives a basic idea of the working of recursive functions. The function used in quick sort algorithm for sorting would be a good way to understand the working of recursion in a better way.

