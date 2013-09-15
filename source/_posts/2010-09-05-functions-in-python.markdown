---
author: sunilkumarn
comments: true
date: 2010-09-05 16:48:06+00:00
layout: post
slug: functions-in-python
title: Functions in Python.
wordpress_id: 187
categories:
- Python
---

Just like most other languages , Python also has functions in it. Lets go through some of the aspects of functions in python.

**def my_function(var1,var):**
This is how you declare a function in Python. 
The keyword def, the function name ,the arguments separeted by commas in parenthesis. Its important to understand that functions in python specify no return type. Each and every function returns something. Either the value returned or else 'none' , the python 'null value'. The arguments dont specify a type either , the type is made out from the values that are passed in .
Python is a :
**dynamically typed language**: The types are discovered at run time.
**strongly typed language**: One particular type cannot converted to another type without explicit type conversion.

**Higher order functions in python:** Functions that operate on other functions, (functions that can take other functions as its arguments).
Eg: filter(my_function,list_1)   is a built in higher order function , which takes the function 'my_function' as one of its arguments.

**Functions in python are first class:**
Consider :
def my_function2(var1):
We have defined a function 'my_function2'
Now we could do this:
var=my_function.
Now try printing the value of 'var'
Its the address of the function 'my_function2' that will be printed because assigning a function name to a variable assigns it address to the variable. 
Now we could invoke the function as:
var(my_parameter)
Just check out the glob module in python (/usr/lib/python2.5/glob.py) for a good example of the first class nature of functions in python.
Function composition is perhaps the best way to explain the fact that function are first class in python.
def f(my_funct):

def g(x):

def compose (f(g(x))): ----- function composition....functions are passed in as variables to other functions.
The way variables are used in function in also important .  A good knowledge of the use of [local and global variables](http://sunilkumarn.wordpress.com/2010/09/05/local-and-global-variables-in-python/) are required to avoid any kind of misperception  while using variables in python.




