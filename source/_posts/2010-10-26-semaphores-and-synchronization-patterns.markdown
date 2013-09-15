---
author: sunilkumarn
comments: true
date: 2010-10-26 03:50:46+00:00
layout: post
slug: semaphores-and-synchronization-patterns
title: Semaphores and Synchronization Patterns
wordpress_id: 404
categories:
- C
---

Semaphores as we know is a mechanism that is used to solve the issue of synchronization . The concept and use of semaphores is a bit more than what you normally call ' tricky ' .Here we try and analyze a basic synchronization pattern and solve the issue with semaphores  .


Have a look into the pattern given below .




statements in THREAD A




strcpy(a , "Hello ");   -  A1
printf("%s\n" ,b);      -    B2




statements in THREAD B




strcpy(b , "World")  - B1
printf("%s\n" ,a);    -  A2


Here we have two threads which consists of two statements each .
**Constraint :**
What we need to ensure is that the statements suffixed A1 & B1 are executed before the statements suffixed A2 & B2  execute. Here the two threads 'rendezvous ' at a point of execution and is not allowed to proceed until both have arrived .

Without using a mechanism like semaphore , no way is it possible to ensure such a constraint due to the scheduling mechanism that cannot be predicted . Hence we use semaphores to solve this issue .

To ensure that it works according to wish  , we do this  :


THREAD A




strcpy(a , "Hello ");  - A1
up(sem1);
down(sem2);
printf("%s\n" ,b);   - B2




THREAD B




strcpy(b , "World");  - B1
up(sem2);
down(sem1);
printf("%s\n" ,a);   - A1


Now we need to give a thought on how this works .
Here we have two semaphores ,sem1 and sem2 as we have two critical sections

Now , how is the synchronization issue solved  .?
The '**down** ' operation just before the two statements suffixed '2' ensures that the two statements are not executed until the '**up'** operation on the corresponding semaphore is not invoked . Hence whichever be the order of scheduling , both the threads rendezvous at a point  and will not proceed ( as a result of the 'down' operation  )until the next thread arrives (and the corresponding 'up' operation is performed ).

This synchronization pattern as been solved using semaphores .

You could find the solution to these and more such patterns from [here](http://github.com/sunilkumarn/Synchroniation_patterns) .
