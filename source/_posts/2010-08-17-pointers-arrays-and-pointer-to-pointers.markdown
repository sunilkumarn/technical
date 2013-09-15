---
author: sunilkumarn
comments: true
date: 2010-08-17 08:41:39+00:00
layout: post
slug: pointers-arrays-and-pointer-to-pointers
title: 'Pointers ,Arrays and Pointer to pointers. '
wordpress_id: 149
categories:
- C
---


Consider the following statement:
**char *lineptr[]={"hello","hai","how"};**
Here 'lineptr' is an array that consists of pointers to characters.
1)  Now this array is passed on to another function with a call as follows:
**str(lineptr)**;
The function 'str' would have a declaration as follows:
**void str(char *lineptr[]);**

2)  The same thing can be done in a different way as follows:
**str((char **) lineptr)**;
The definition of 'str' can be the same as the above or it could well be:
**void str(char **lineptr);**(the declaration of the function could have been the same when the call was
str(lineptr); as well.
This is an example of a pointer to a pointer.

Here in our function call :str((char **)lineptr);   we pass a pointer to a pointer (that points to characters).
And that we have as (char**)lineptr.
This shows the thick relation between arrays and pointers in C. Any operation that can be done array   subscripting can also be done with pointers.
.char s[]=”hello”;
could be done using pointers as
.char *s=”hello”.

**The parenthesis and the type specification (char) that we have is extremely important. **
Without the parenthesis and the type,it would be as follows:
**lineptr : dereferencing the dereferenced value of lineptr ,i.e,&lineptr[0]  which would result in 'h' as the answer.

In both cases
for(i=0;i<3;i++)
printf(“%s”,*lineptr++);
would print out the contents of the array 'lineptr' (.i.e (hellohaihow)).
.lineptr++ increments lineptr by  a value of 4 since it points to pointers.

Now consider another case ,
The function call being
**str((char**)lineptr) or str(lineptr)**  
The declaration being
**void str(char *ptr)**;
Through the function call, we pass a pointer to pointer(that points to characters).But the declaration consists of a pointer that points to characters. Therefore the variable 'ptr' in the function 'str' would take up the base address of the array 'lineptr' as its value. Now since this a pointer that points to characters ,   ptr++ would increment 'ptr' by a value of only 1.And since this points to some unknown characters, undefined results would be produced.

