---
author: sunilkumarn
comments: true
date: 2010-08-01 14:17:57+00:00
layout: post
slug: functions-in-c
title: Functions in C
wordpress_id: 52
categories:
- C
---

C provides a lot of built in functions. Apart from that users define their own functions knows as user-defines functions. The function has got three parts
The declaration,the call and the definition.
Consider a function** 'getline'** which is used to obtain each line from the input.
The three parts of the function would be as follows:
Declaration:**returntype getline(parameters);**
call:**getline(arguments);**
definition:**returntype getline(parameters)
{
declarations;
statements;
}**
The function definition may well contain a 'return' statement to transfer the control back to the caller function. In such cases the return type of the function must not be 'void'.

