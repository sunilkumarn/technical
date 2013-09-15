---
author: sunilkumarn
comments: true
date: 2010-08-01 14:25:02+00:00
layout: post
slug: local-and-external-variables
title: Local and External variables
wordpress_id: 54
categories:
- C
---

Local variables are variables that come into existence only when the function is called and disappears thereafter whereas external variables are accessible to all functions of the program.
To illustrate the difference between these two kinds of variables ,consider a function **‘getline()’ **
At first we consider the case where local variables are used. The call and the function definition in this case would be as follows:
Function call:
**getline(line,MAXLENGTH);**
Function definition:
**int getline(char s[],int lim)
{
}
**Here there is a requirement to pass the arguments from the caller function to the called function through the parenthesis as the variables used are local to the function. The value of the variable 'line' is internal to the caller function.
Whereas when we consider the case of external variables ,the situation changes. Consider the case of declaring variables externally:
**#include 
#define MAXLINE 100 
char line[MAXLINE];**		/*external variable declaration/*

The variables line has been declared externally.
Hence the call and definition of the function ‘getline()’ could be modified as follows:
Function call: 
**getline()**
Function definition:
**int getline(void)
{
}**
The modified values of the variables would be available to all the functions.

