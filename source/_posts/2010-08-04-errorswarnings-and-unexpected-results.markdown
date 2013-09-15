---
author: sunilkumarn
comments: true
date: 2010-08-04 03:15:14+00:00
layout: post
slug: errorswarnings-and-unexpected-results
title: Errors,warnings and unexpected results...
wordpress_id: 62
categories:
- C
---

While compiling C programs we might often come across errors,messages and unexpected results as our output.. The following shows a few of those :
Sometimes u might get certain unexpected values like -1098763408 as your result.
This might be due to the fact that u may not have initialized the variable used in the expression producing the result to a starting value(say i=0).
The same doing (i.e a variable not set to its initial value) might produce a 'segmentation fault' in the program. Segmentation fault may also be produced due to the fact that the alloted size for the particular string array is less than what you have just tried to fill in.
For eg. If 
#define MAX 10
char string[MAX];
The size is 10.Now if u try to fill in more than 10 characters a case of segmentation fault might occur.
Another case exists where you might get an error as the following one:
 error: conflicting types for ‘some_function_name’
This might be because of the fact that the function name that you have used is the name of the function that is already present in one of the header files.
For common functions like 'strlen' ,'strcat' etc this might not occur because we wont use them as our function names. But this might occur sometimes for certain function names that we may not be familiar with.
For eg. The error message would be displayed when you use function names as 
'index' or 'remove' etc...even if your declaration,definition and the call might be correct.
Don't keep pondering over it,you just need to change your function name to another one that does not exist in the system's headers. For eg..change 'the name index' to 'ind' or something else.

Another common case where you may not obtain the result might be because of an error in the 'printf' statement. For eg you may have used “%d” to print a float value even though “%f”should have been be used.

It is important to check that the type of 1)the called function and 2) the variable that is used in the calling function in order to receive the value returned from the called function has to be same .If this is not the case ,an error would be produced (that would happen only if both the functions are in the same source file. But if the functions are in different source files, then the case becomes more complicated because the error would not be displayed.
In case you find that the output you obtain has got some definite value but is meaningless according to the problem, then a solution might to have a look through all the data types used in the program and see if its appropriate and matching. Such cases might occur in programs involving many functions

