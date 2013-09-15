---
author: sunilkumarn
comments: true
date: 2010-08-04 06:31:03+00:00
layout: post
slug: characters-integers-and-arrays
title: Characters , Integers and arrays.
wordpress_id: 71
categories:
- C
---

Consider the statement 
char c='5'
printf(“%d”,c);
The result that will be printed is 53, which is the integer value of the character constant '5' ,stored in the machine.
Now if we need to get 5 as the output,we need to use “%c” in 'printf'.
Character constants are integers written as one character within single  quotes.
Now consider :
int c=getchar();
printf(“%d”,c);
The result would be the integer value representation of the entered character.
“%c” in 'printf' would print the character in the terminal.
Now consider the case of a character array 
char array[]={5,6,7}.
If we print array[0],using “%d”, then '5' would be printed. Nothing would be printed if “%c” is used in the printf statement.
Here 5 itself is a constant. Now if we need to enter alphabets into a character and print it we need to do the following:
char array[]={'a','b','c'};
Now print it using “%c”.The alphabets would be printed.
If printed using “%d”, then the integer representations would be printed(97,98,99).The characters that entered into a character array are stored as integers inside the array.
If we do 
char array[]={a,b,c}
The result would be an error as such:
'a' undeclared....'c' undeclared...etc..
 

