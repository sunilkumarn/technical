---
author: sunilkumarn
comments: true
date: 2010-08-10 19:41:32+00:00
layout: post
slug: static-variables-and-declarations
title: Static variables and declarations...
wordpress_id: 116
categories:
- C
---

The static declaration can be applied to both external and internal variables. The static declaration if applied to external variables limits the scope of that particular variable to the rest of the source file being compiled. Static declaration if applied to the internal variables ensures that the variables returns its previous value after the control has been transferred out and in of the particular function...i.e it provides permanent storage for the variable rather than temporary storage which is the case with normal internal variables.

Lets see how static works with internal variables...
Consider the program code...

char yes(void)
{
	static int lastc=0;
	lastc=lastc+1;
	return lastc;
}
The variable lastc has been declared static:
Hence the value of lastc remains intact even after the function 'yes()' has terminted...lastc will get back its older value when the next call to the function 'yes()'arrives.
Therefore if the return values where to be printed...it would be as following:
1
2
3
...
...

This obviously wouldn't have been the case if the static declaration was not provided.
Now consider the case:

char yes(void)
{
	static int lastc;
	lastc=0;
	lastc=lastc+1;
	return lastc;
}

Here the initial value of the variable has been assigned after the static declaration is performed.
This would result in a normal execution. Obviously the variable would be static and has permanent storage but its that just the assignment operation after the static declaration takes its effect. Therefore its highly important to initialize the value of the variable declared 'static' along with declaration.

