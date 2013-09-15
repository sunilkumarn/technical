---
author: sunilkumarn
comments: true
date: 2010-09-15 03:30:50+00:00
layout: post
slug: memoizationrun-your-programs-faster
title: Memoization:Run your programs faster.
wordpress_id: 208
categories:
- Python
---

**Memoization is an optimization technique to speed up programs** . Here the function calls are not required to make repetitive calculations of previously calculated results. The processing time of the program is reduced and the memory consumed is increased. **Memoized functions become optimized for speed while they use a higher memory .**

Lets go through an analysis of memoization: Consider the code segment for finding out the term of  a fibonacci series:


_def fib(n):

	if n==0:

       		return 0

	if n==1:

		return 1

	else:

		n=fib(n-1)+fib(n-2)

		return n
_

You could find out the time taken to run the code :
**$ time python filename.py
**

You would find out then that the time complexity of your program is O(exp(n)) . Try and find out the 40th term of the fibonacci series . This code segment will make you wait for a considerable amount of time before you view your result in the terminal. Trying to find out the 50th term would cause you to manually interrupt the process because you can not wait any longer. Therefore we need a method whereby we could find out the higher terms of the series in much faster time.

Heres exactly where memoization comes in. Memoization is not any kind of magic. What it does is that is caches the results that have been calculated earlier and stores them in a lookup table so that these don't have to be calculated again. We make use of an associative array as a lookup table here.  The following code segment shows the memoized version.

_table={0:0,1:1}

def fib1(n):

	if not n in table:

		table[n]=fib1(n-1)+fib1(n-2)

	return table[n]
_

Here we create an associative array with the first two numbers of the fibonacci series as its keys. Now 
upon the call of each function , the lookup table ('table') is checked for the existence of the term . If the term is not present in the associative array , the term is calculated and stored in the associative array. In both cases the resulting term is returned. Now see for yourself , the pace of the results being processed.
Try find out the 100th term of the series:
Do 
**$ time python myfilename.py**
You would see the 100th term of the fibonacci series in 12/1000th of a second.Try them for the 1000th,10000th terms...(provided that you have a higher recursion limit). Did you ever think  that it could be this faster.???


