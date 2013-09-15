---
author: sunilkumarn
comments: true
date: 2010-10-03 15:32:07+00:00
layout: post
slug: swigsimplified-wrapper-and-interface-generator
title: SWIG(Simplified Wrapper and Interface Generator).
wordpress_id: 273
categories:
- Tools
---

	Scripting languages like python presents a lot of ease to the programmer while coding , but the fact remains that there are certain tasks that would screw you up when attempted in Python. So the coding is done in a more flexible language like C and the modules are imported in Python. **SWIG** is a tool that is used for extending your C programs into python and make the functions callable from Python.

Here we discuss how to import a C module into Python using SWIG.
Installing SWIG in your system ,if you haven't already, is the first task.

**$:apt-get install swig**

would do that for you.

Now its better you create a directory for the entire purpose and setup yourself within it. 
Consider you have a C file ,**avl.c** , as our example_ (Avl is a height balanced Tree ,avl.c is an implementation of such a tree._) You could download the source code for avl.c [here](http://github.com/sunilkumarn/Avl_Tree). 

Our first step is to create an interface file '**avl.i**' for setting up the interface. 'avl.i ' consists of the declarations of the various functions and global variables that are used in the file 'avl.c' and are prefixed with 'extern'. 

`%module avl

%{

 	extern struct node *root;

       	extern struct node *p;

%}
 

extern void insert(struct node* move,int item);

...
...
...
...
`
The 'avl.i ' file would like as above:
Now we have two files in our directory ,

avl.c   &   avl.i

This is all that we have to code for the task of extending a C module in Python. Now its all about SWIG. 

[![](http://sunilkumarn.files.wordpress.com/2010/10/swig.png)](http://sunilkumarn.files.wordpress.com/2010/10/swig.png)
Lets move further: Do the following.

**$:swig -python avl.i
**
now have have a look into your directory and you will find two more files there .

avl.py  &   avl_wrap.c      
**avl_wrap.c** is the wrapper file that has been created for the purpose of extension . Wrapper functions act as a glue layer between languages.

**$: gcc -c avl.c avl_wrap.c -I /usr/include/python2.5/**

You could now see 2 more files present in your directory. 
avl.o   &     avl_wrap.o

These two are the object files that have been created for avl.c and avl_wrap.c respectively.

**$: ld -shared avl.o avl_wrap.o -o _avl.so**

This is the last thing you need to do before you could use start using your C module in Python. 
Check your directory and you could see a new file ,** '_avl.so'**. This is the shared object file that has been created . 
If you create a make file for these , then obviously you wont have to go doing these tasks repeatedly.
Now ,the python module corresponding to the C module avl.c has been created which means that now you could start using it.

**$: python**

>>>import avl
>>>avl.my_insert(10)
>>>avl.my_insert(20)
>>>avl.traverse(1)

The source code can be downloaded [here.
](http://github.com/sunilkumarn/Extending_into_python)



