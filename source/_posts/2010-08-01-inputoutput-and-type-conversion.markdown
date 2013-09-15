---
author: sunilkumarn
comments: true
date: 2010-08-01 14:14:06+00:00
layout: post
slug: inputoutput-and-type-conversion
title: Input,output and type conversion.
wordpress_id: 44
categories:
- C
---

Certain basic features of the C programming language are written here. 
A text stream is a sequence of characters.
The **getchar() **statement in C is used so to obtain the next character  from a text stream that is inputted by the user.
c=getchar() 
assigns the input character to c
The type of c is declared to be int(when EOF case is used).
**putchar(c)** 
prints the character ,that has been inputted, on the terminal.
Now consider what happens when we enter a statement 
**putchar(c-'0')** 
where c=getchar() and the character inputted is 'r'.
The result is B.The reason for the result being obtained is the the automatic type conversion that is performed in C.
The type of '0' is char and is converted to int,which is the type of 'c'.Hence the integer value of '0' is subtracted from the integer value of 'r' (i.e 114-48=65)which is equal to the integer value of 'B'.Hence we obtain the result as B.
Similar type conversions can be seen in C in different examples.
If an operator has one float and one integer as its operands ,then the integer will be automatically converted to float.
We express the decimal number 16 as a floating point constant as 16.0.The same number would be represented as an integer as 16.
If we perform 16.0-16,the integer operand (i.e 16)would automatically be converted into floating point.
**The generalization of this is that a narrower operand is converted into a wider operand**.
For eg int->float,float->double,double->long double etc....

Now we again move onto the input/output portion.
Consider a limit for the input is up to the EOF(end of file)
read input upto end of file is reached.
We set the condition as
(**c=getchar())!=EOF) ******  .This implies that the characters are fetched onto 'c' until the End of file is reached. EOF is an integer defined in  as -1.
The paranthesis in (c=getchar())!=EOF is quite important. Without the paranthesis c would obtain values '0' or '1' depending upon the character not being EOF .This is because the precedence of != is greater than that of the assignment operator(=).

