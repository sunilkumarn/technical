---
author: sunilkumarn
comments: true
date: 2010-08-17 08:42:27+00:00
layout: post
slug: increment-and-decrement-operators
title: Increment and Decrement operators
wordpress_id: 150
categories:
- C
---


Consider the statements:

int c=5;
printf("\n%d\n",c++);
printf("\n%d\n",++c)

The result as we except would be :
5
7

Again consider the statement
printf("\n%d %d:\n",c++,++c);

Here the result wouldn't be as we thought and as is correct in the earlier example. When I executed this I obtained the result as:
6 7:
This is an undefined behaviour:Therefore its an important thing to keep in mind while using the increment/decrement operators:Its important 'not to' increment/decrement the same object more than once in a statement.
If an object appears more than once in an expression ,then modifying the object in the expression without the previous value of the object taking part in the modification process will lead to undefined behaviour. Thats whats done in the example just above. The value of c is modified in the expression for the second time(there fore 'c' appearing twice) without the previous value participating in the process. So if at all an object has to be modified in an expression where it appears more than once ,then it should be something like as follows:
c=c+1;
Here c appears twice , c is modified but the previous value of 'c' takes part in the process..


