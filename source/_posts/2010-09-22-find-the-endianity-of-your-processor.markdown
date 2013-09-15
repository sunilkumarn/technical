---
author: sunilkumarn
comments: true
date: 2010-09-22 21:53:24+00:00
layout: post
slug: find-the-endianity-of-your-processor
title: 'Find the Endianity of your Processor:'
wordpress_id: 213
categories:
- C
---

Your processors can be of two types:
1)_Little Endian_ : Little Endian means that the low-order byte of the number is stored in memory at the lowest address.For example, a two byte short int would be stored in memory as:
**Base address + 0 : Byte 1
Base address + 1:  Byte 0**
	
2)_Big Endian_ : 'Big Endian means that the high-order byte of the number is stored in memory at the lowest address. The 'short int' above would be stored 
**Base address + 0 :  Byte 0
Base address + 1 :  Byte 1**

where Byte 0 and Byte 1 are the first and second  bytes of the short integer.

You could verify the endianity of your processors by writing a short and sweet C code as follows: 

			`{	
				unsigned char *p;

				short int i=0x1234;

				p=&i;

				printf("%x\n",*p);

			}`
	The result you obtain would show you the endianity of your processor.

Lets give a thought on what actually happens. 

0x1234 is a hexadecimal number and is stored in a variable 'i' of type short int ( occupies 2 bytes of memory space ). 

Address of variable 'i' is stored in a pointer of type unsigned char '*p' ( I.e 'p' points to an object of type 'unsigned char').

Dereferencing the pointer p would result in the access of just a single byte . This obviously would be the lower of the two bytes. **This shows the fact that accessing a memory location with a pointer is purely dependent on the type of the pointer and not on the type of the actual value stored in the address being accessed. **Note that the types of the' pointer to the memory location' and the 'actual value being stored in the location' differ.

You obtain the value that is stored in the lower byte of the two bytes that is allocated for the **'short integer**' with the use of a pointer of type '**unsigned char**' ( size = 1 byte). The value stored at the lower byte would indicate to you the endianity.

From our sample program,and from what we know about little and big endians , an output of 34 indicates that your processor is 'little endian' and an output of 12 indicates that its big endian.
  
Just to mention - 'Intel x86' processors are one common example for little endian processors whereas 'Motorola 6800' is big endian. 

