---
author: sunilkumarn
comments: true
date: 2010-08-04 06:18:52+00:00
layout: post
slug: ensuring-the-correct-data-types-while-using-functions
title: Ensuring the correct data types while using Functions
wordpress_id: 65
categories:
- C
---

Some of the most common errors that are produced in a program are caused due to the lack of interest ensuring that the data types used are correct. Some of these errors will be displayed while some will not and its up to the user to search the program and find out whats wrong with it. Therefore the user must be very careful while using the type of the functions, the variables that are used etc...An example below shows the use of the data type of the functions and variables...
Consider a program which is used perform the functions of a calculator: The functions that are used in the program are the follows:
push(),pop(),getop(),atof(),main()(These are just the function names)
 Now each of these functions have got a specific task.
The 'getop' function is used to get the character that is entered by the user. The value that will be returned by the function would be of an integer type:
hence **'Int getop( char []);'** (A string is passed as the parameter)
'push' is used to enter a value into the stack. The value would be of type 'double'. Hence the argument that is passed to the function 'push' would be of type 'double'. Nothing is returned by 'push'.
Hence '**void push(double);**'
'pop' returns a double value and nothing is passed to the 'pop' as an argument because it just takes up the value from the array(array[] ,declared externally).
Hence** 'double pop(void)'**
'atof' takes the string as the argument and returns a double value:
hence '**double atof(char[]);'**
The data type of 'array[]' should again be double because it handles 'double' values.
Now the program looks like this:

 #include 
 #define MAX 50 
 int getop(char s[]); 
 double atof(char s[]); 
 void push(double); 
 double pop(void); 
 double arr[MAX]; 
 int top=0; 
 main()


 Now if the data types of any of the above declarations are not according to the problem you wouldn't get the expected results.
For example if the data type of array[] is not double(say integer) , you would get the result as some meaningless value. Same is the case with all of the above functions and variables declared.
For the purpose of add,subtract,multiply and divide the use of 'double' values is good(infact a necessity if you want the exact output).
But when the '%'(modulus) operator is used ,then the problem arises. If the modulus operator is used with two double values the resulting error would be as follows:
** error: invalid operands to binary % (have ‘double’ and ‘double’) :**
Hence the values needs to be converted to type 'int'.
Int op=pop();
push((int)pop()%op);
The type is converted from double into int. Now since pop returns a double value, this will again be converted back to a double value.
 
A note on External variables: Here array[] and top are declared as external variables and so can be accessed by the functions that are mentioned. Hence these values need not be passed as an argument to the functions. The fact that is to be taken into account that is that when external variables are used , the variable names cannot be changed within a function as is the case while passing arguments.




