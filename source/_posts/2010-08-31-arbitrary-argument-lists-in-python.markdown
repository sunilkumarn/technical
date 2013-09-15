---
author: sunilkumarn
comments: true
date: 2010-08-31 02:39:23+00:00
layout: post
slug: arbitrary-argument-lists-in-python
title: ' Arbitrary Argument lists in Python.'
wordpress_id: 159
categories:
- Python
---

Consider the function definition:
**def mapp(fun,first,*second): **     (A function that is defined to implement the properties of the 'map' function
	

Here the function 'mapp' consists of three arguments passed to it. Out of this ,the third argument (second) is the arbitrary argument list. The argument is itself optional and can be passed with different lists or tuples ranging from zero to any number of lists.
The length of the arbitrary listed argument would be zero if no arguments are passed to it.(len(second)=0).

The call to the above function would be as follows:
mapp(sqr,a)

OR

mapp(add,a,b,c)

OR

 mapp(mul,a,b)

where a,b,c are any lists. 
and 'sqr','add','mul' are functions.

Suppose the call to the function be 
mapp(mul,a,b,c)

The lists b,c are passed as arguments to the function 'mapp' via the formal parameter 'second' (arbitrary listed argument).
Now 	second[0]=b
	second[1]=c.






