---
author: sunilkumarn
comments: true
date: 2010-09-02 03:10:43+00:00
layout: post
slug: optimizing-your-python-code
title: Optimizing your Python code.
wordpress_id: 173
categories:
- Python
---


		Compare python with some of the other languages that are familiar to many of you. The obvious difference that will be caught by you , is the fact that python code, for the same program ,is much shorter than codes written in some of your favorite languages like C ,Java etc.
Understanding the essence of python programming would help you write shorter and interesting codes  and are easier to understand. Lets just have a look into some of the core points that need to be followed so that your python program becomes short and effective.
		
		First and foremost, you need to understand the fact that python has got a huge library with functions for almost every activity you would need frequently. The only thing you need to do is to identify whether a function is present that would handle your case and then call the function .There are lots of built in functions that we could call. Other functions that reside in certain modules can also be invoked after the module containing the function has been imported.
 As an example , consider the following:
		 	**map(sqr,range(1,100)):**

		'map' is a built -in higher order function that takes two or more arguments and returns you a list,'range' is another built-in which would return a list containing all the elements from the starting element up to the ending element.

		This particular function returns a list of squares of all numbers from 1 up to 99 using a single line code. Compare this to a C code to find the squares of this many numbers,certainly ,a single line code wouldn't do that for you.
	
		Python has got a large collection of such built ins which includes functions like 'filter','reduce','hex','cmp' and many more.
		
		Python has got many more functions that are defined in different modules:To use those functions , you just need to import those modules and invoke them. Some of the most commonly invoked ones are :
		**Module 're**' for pattern matching. Contains power full functions like findall' , search' , 'match' etc.
		**Module 'os'** for operating system services. Includes functions 'mkdir' , 'chdir' ,' path.join,' path.abspath'' etc.
		**Module 'shutil'** for shell utilities like copying files.
		
		**Module 'urllib'** for dealing with 'URLs'.

		**Module 'sys**' :sys.argv[index] would return the input given on the command line at position 'index.'
		
		Identifying the correct functions that are present in these and more modules in python  would result in much shorter code length. 

	 A proper knowledge about the use of sequences and variables would do no harm in our purpose of decreasing the code size.

	Consider the code to swap the values of two variables. In other languages you would write:
	temp=a:
	a=b:
	b=temp:

	Once again, in python all you need is a single line code :

	**a,b=b,a**
	
 The logic behind whats happening is to be understood. Consider the right hand side of the expression -variables are packed into a tuple. On the left hand side, the tuple is unpacked and the values are entered into the variables. Thus the values are swapped in a single line as opposed to three lines taken in other languages.
		
	Use of generator expressions is another way to reduce your code size.
	Consider:
	xvec=[10,20,30]
	yvec=[30,40,50]

The expression **sum(x*y for x,y in zip(xvec,yvec)) **gives you the dot product of xvec and yvec in a single step where 'sum' is another built in function. Think of writing a C code instead of this and the number of writing you would take.


	There are many more such tricks that would decrease the code size considerably . Slicing of lists , usage of the appropriate sequences are all important in producing a short and elegant code. Obviously,the fact remains that to understand the methodology behind decreasing the code size is to keep on coding ,understanding the definitions of the functions and identifying where to use them. But again, compressing your code size heavily might result in your code being hard to digest by others, specially those beginners trying to understand a python code. Therefore its important to develop a skill where you would maintain a balance between decreasing the code size and the readability of your code. This can be achieved only through rigorous coding.

