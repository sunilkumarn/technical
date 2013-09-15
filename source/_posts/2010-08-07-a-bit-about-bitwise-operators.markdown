---
author: sunilkumarn
comments: true
date: 2010-08-07 18:39:59+00:00
layout: post
slug: a-bit-about-bitwise-operators
title: A bit about bitwise operators....
wordpress_id: 91
categories:
- C
---

Before we get a view of the bitwise operators , its important to have an idea of  sizes of different types that are used in C.
Since most of the values that are entered by us can be accommodated by the ‘type float’ we consider the int , short and char types.
The data type char occupies a space of minimum of 8 bits(1 byte).The maximum value that can be placed in an unsigned char without the problem of truncation is 255. In case of unsigned char, this value moves to 127.The minimum value in signed char is -128.
The type int stores a maximum value of 65,635 for unsigned type and a maximum of 32767 for the signed type(these are the minimum values offered by any system, the actual limits may change on different systems). The minimum for the signed type would be -32768.The minimum number of bits required for storage would be 16. These values are the same for the type ‘short’.
These were some of the basics that were needed to get an idea about the bitwise operators. Obviously playing with the bits is a bit more difficult task than the higher level.
The bitwise operators are 
&-bitwise AND
|-bitwise OR
<>-right shift
^-exclusive or
~-bitwise complement.

<<-left shift operators would be the best way to get an idea about the bitwise operators.
Consider doing the following
255<<2….the result would be 1020….255<<3 would give you 2040…255<<4….would give you 4080….and so….
Lets see how this works….doing 255<<2 is equivalent to doing 255*2^2(here the ^ symbol stands for the ‘raise to’ operator….Similarly 255<<3 is equivalent to doing 255*2^3….
A smaller number would  help in understanding this.
Left shift operator shifts each bit of the character 2 places to the left. In the case 1<<2…would give 4 as the answer…
1(00000001)<<2=00000100(4)….This is how you obtain 4 as the answer.
Now the importance of the knowing the bit size of different types comes in here…
Suppose that the character 255 has been stored using variable of the type ‘char’…Now it is obvious that after shifting it to left, the result cannot be held by the type ‘char’ because the maximum value that can be stored in ‘type char’ is 255..Therefore it will have to be stored in the type whose size is greater then that of ‘char’.
.unsigned char s=255;
.s=s<<2;  wouldn't be correct.
We use the type 'int' to store the result...
.unsigned char s=255;
.int i=s<<2; is the right way to do it..
Similarly its also important to consider the limits of the signed and unsigned values when using the shift operators. 
The attempt to shift more bits than is allotted would produce in a message as ‘>=width of the type’.
The other bitwise operators work on the bits as follows….
>>p  ---  shifts the bits p positions to the right…equivalent to dividing with 2^p.
'&'  ---result is 1 when any of the bits are 1…
^,|,~ all work in their own way….




