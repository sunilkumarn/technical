---
author: sunilkumarn
comments: true
date: 2010-09-02 03:07:12+00:00
layout: post
slug: garbage-collection-in-python
title: Garbage collection in Python.
wordpress_id: 172
categories:
- Python
---

In C, once the memory allocated becomes garbage , that chunk of memory is gone wasted. Nothing more is possible on it. 
Consider 
**int a[10];**
Some amount of memory would be allocated for the array 'a'. Now if do a='some value' , the memory that was allocated to the array becomes garbage because 'a' no longer uses it as it is pointing to something else now. The memory is gone wasted and is no more usable.
The bugs that create loss of memory are hard to detect as well ,simply because of the fact that they cannot be seen. Its only after executing the particular code for some amount of times that you might actually get to see the effects of the bug .By the time your system may have crashed.

In python ,the interpreter is smart enough to do 'garbage collection'. That is the memory which is unused is collected by the interpreter and is not gone wasted. The process of garbage collection is done by using a method called ref counting. Understand ref counting as :
**Consider a=[1,2,3]**
'a' points to a particular block of memory. At the end of the block of memory ,there would be a count that indicates the number of variables that point to the same block of memory. So now the count would be 1.

Now if we do, 

b=a......means that now b points to same block of memory. Therefore the count becomes 2. 

Now consider doing 

b=0.

The count in the block of memory where b was pointing earlier reduces by 1. , hence count is now 1.
Do 

'a=0' 

and the count in that block becomes 0.Now the python interpreter can allot the chunk of memory to some other variable as it may feel. The significant thing that you need to understand is the fact that the memory is not gone wasted as in C .

Garbage collection keeps track of the variables that point to a block of memory and collects the unused memory accordingly. There is never a shortage of memory space. But there are obvious disadvantages with the process of garbage collection.

One drawback , as you would except would be the matter of speed which would decrease.
But the more significant drawback would be the **non-deterministic behavior** of the interpreter. 
Consider the two statements.
 .i=0
 .j=0
You would expect the statement j=0 to be executed immediately after the statement i=0 has been executed. But as a result of this process of garbage collection ,this is not a guarantee. There might be a certain time delay in executing the second statement if the interpreter feels like doing some 'garbage collection' after the first statement has been executed . This results in the delay.

Therefore its clear that its not best to use python over languages like C when it comes to real time systems.
 

