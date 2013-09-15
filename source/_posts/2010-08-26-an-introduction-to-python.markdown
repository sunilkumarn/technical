---
author: sunilkumarn
comments: true
date: 2010-08-26 20:28:49+00:00
layout: post
slug: an-introduction-to-python
title: An introduction to python
wordpress_id: 155
categories:
- Python
---

Here is a small introduction to the python programming language provided by considering some of its most important features.

a=[1,2,3]
The above statement assigns 'a' to a list which is [1,2,3]
b=(1,2,3)
The statement assigns 'b' to a tuple which is (1,2,3)
Lists and tuples are one of the most important features of python. They both are collection of values separated by 'commas'(,). The difference between lists and tuples is that tuples are immutable(cannot be modified) whereas the value stored in a list can be changed and modified.
Lets see that with an example:
a[0]=10
'a' now becomes [10,2,3].
The above operation( I.e modification )cannot be performed with tuples).
b[0]=10 would produce an error message.

Lists can be manipulated in a number of interesting ways and the working of lists is also quite interesting.
 .a=[1,2,3].
 .b=a            would now make 'b' [1,2,3]
Now any changes made to b, suppose 
b[1]=20 
would also result in 'a[1]' being changed to '20'. This is a really important concept that has to be understood in python.
When **b=a** is done ,the statement makes 'b' point to the same address where 'a' points. Hence the changes applied to list 'b' are reflected in 'a' as well.
If the above case is to be avoided, there are basically two ways:
1)Pass the list 'a' as a tuple to 'b' so that it cannot be modified.
A list can be converted into a tuple using the 'tuple' command: 'tuple(list)'
2)Pass a clone of 'a' to 'b'. This can be done as b=a[:]. Here only a clone of 'a' is passed to 'b' thereby any changes made to 'b' doesn't affect 'a'.

Manipulation of lists may be as:
a=[1,2,3,4,5]
a[1:3]
would give as [2,3]
a[1:]
would give [2,3,4,5]
a[:1]
the result would be [1]

There are a number of commands that can be applied on lists:They include the following:
'append' 
a.append([10,2]) would give you 'a' as [1,2,3,4,5[10,2]]

'extend'
a.extend([8,9]) would give us 'a' as [1,2,3,4,5,[10,2],8,9]

'pop'
a.pop(4) would delete and return the element at the 4rth position.

'insert'
a.insert(2,10) would insert '10' at the 2nd position of the list..



