---
author: sunilkumarn
comments: true
date: 2010-09-05 16:45:20+00:00
layout: post
slug: local-and-global-variables-in-python
title: Local and Global variables in python..
wordpress_id: 189
categories:
- Python
---

Lets see the working of global and local variables in python with the help of a few small examples: What you need to understand is that variables that are declared inside functions are local to that functions (cannot be accessed from outside the function), whereas variables that are declared outside the function are global to that function , they can be accessed within the function.

_>>>a=1
>>>def func():
...    print a
...    a=10
...
>>>func()_
would produce an error as:
**UnboundLocalError: local variable 'a' referenced before assignment.**
This is because the local variable 'a' is used in the 'print' statement before it is being assigned a value.
However ,

_>>>a=1
>>>def func():
...    global a
...    print a
...    a=10
...    
>>>func()_
wouldn't print an error of that sort.
Note the use of the keyword **' global' **. This keyword indicates that the variable 'a' that is being printed is a global variable. The python interpreter understands that there is a variable 'a' that has been assigned to '1' outside the function and hence prints the value '1'.

Now do the following:

_>>>a=1
>>>def func():
...    global a
...    print a
...    a=10
...    print a
>>>func()_
The result would be:
1
10
Now try and print the value of 'a'.
The value printed would be:
'10'
This is because the value of the variable 'a' has been changed to 10 by the function 
'func' because 'a' has been declared as a global variable in the function 'func'.

Now do :
_>>>def func():
...    print b
...    b=10
...
>>>print b _
would result in an error :
**NameError: name 'b' is not defined**

because no variable 'b' has been assigned a value outside the function. The variable 'b' used in the function 'func' is local to that function and cannot be accessed from that outside the function, that is , the variable 'b' is **local** to the function.

