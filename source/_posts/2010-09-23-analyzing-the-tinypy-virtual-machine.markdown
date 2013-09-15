---
author: sunilkumarn
comments: true
date: 2010-09-23 20:52:00+00:00
layout: post
slug: analyzing-the-tinypy-virtual-machine
title: 'Analyzing the Tinypy Virtual Machine '
wordpress_id: 228
categories:
- Tinypy
---

The major task in tinypy is to analyze the working of the virtual machine. Analyzing and understanding its source code is, obviously, the major challenge that is faced. The code itself is not well arranged and the lack of proper documentation makes the challenge even bigger. Proper use of the GNU debugger, 'gdb', and the indexing tool, Ctags makes the process of analysis of the VM a lot simpler. Perhaps the best way to understand the working of the VM is to use the VM to execute the byte code of a simple python program and see and understand the actual flow of control. Here a sample program 'sample.py' is used:

`def add(a,b): 
c = a + b 
return(c) 
print(add(1,9))`


Note that the use of the parentheses for built-in functions too is a part of the syntax in TinyPy. Obtaining sample.tpc is explained [here.](http://sunilkumarn.wordpress.com/2010/09/23/an-overview-of-tinypy/)

**cc vmmain.c -1m 
            ./a.out sample.tpc   **

gives you the output:
_10_

Understanding the flow of control for this program would give an idea of the working of a segment of the virtual machine. 
Each time the VM is invoked, a new instance of the VM is created .
**tp_vm *tp =tp_init(argc,argv);**

The key point to be understood in the analysis of VM is that each object in tinypy is stored as a union in the C API. 
Lets get into the details:** tp_obj** is the tinypy's object representation.

`typedef union tp_obj  { 
int type; 
tp_number_ number; 
struct{int type;int *data;}gci; 
tp_string_ string; 
tp_dict_ dict; 
tp_list_ list; 
tp_fnc_ fnc; 
tp_data_ data; 
} tp_obj;
`
The union tp_obj with its fields are shown. The field **'type'** of the union indicates the type of the object. A type value of 1 indicates that a number is being used, type = 2 indicates object is a string and type = 4 indicates a list. In our sample program, our objective is to add two numbers 'a' and 'b' and print their output. A function in the virtual machine **'tp_add'** does the job of adding two arguments that is passed to the function as input. The arguments could be numbers, strings or lists. Lets analyze the function 'tp_add'.

**tp_obj tp_add(TP,tp_obj a,tp_obj b)
**
The Function definition of 'tp_add' contains three arguments:
• _TP -> the VM instance 
• tp_obj a -> the union variable representing the object 'a' in the sample program
• tp_obj b -> the union variable representing the object 'b' in the sample program
_
Within the function** tp_add**, the type of the two objects are checked and the appropriate function is performed. In our example, two numbers are to be added. As we mentioned above, 'a' and 'b' are stored as unions and the union contains fields for types such as number, strings, lists etc. We access the field **tp_number_** in the union 'a' as '**a.number'**. **'number'** is a variable of the structure **tp_number_** which contains a variable **'val**' that stores the exact value of the number . Therefore **a.number.val** would give you the actual object . Analyzing the addition of strings would be interesting as well. '**a.string'** would give you the union which represents the string object. The union** tp_obj** contains a field **tp_string_** which is a structure and includes the pointer that points to the address where the string 'a' is stored. The structure of tp_string_: 

`typedef struct tp_string_ { 
int type; 
struct tp_string_ info*; 
char const *val; 
int len; 
}  tp_string_;`

An idea about the manipulation of lists would do no harm to our primary objective, the study of the working of the VM. Consider two simple lists in python :
a = [1,2,3] , b = [4,5,6]

Again we start from tp_add which needs us to go back to our function. The VM uses a function **'tp_extend**' which returns a union that contains the new (extended) list. You would see that the value of the field 'type' of the union that is returned would be 4 which indicates a list. Access the field 'list' as **'a.list'**. **'list**' is a structure variable that includes a pointer ***val **that points to another structure '**_tp_list':**

`typedef struct _tp_list { 
int gci; 
tp_obj *items; 
int len; 
int alloc; 
} _tp_list;`

The pointer ***items** as you could see is of type tp_obj and de-referencing it would give you the union tp_obj. This union contains a single element of the list. Starting from the function tp_add:

***r.list.val -> items
**
would give you the union mentioned above. Accessing the field '**val**' of the field** 'number**' (variable of the structure tp_list_) of the union would give you the element of the list. Now , the obvious question is how do we obtain the next element of the list. For this you need to have some idea about the storage of unions.

**r.list.val -> items**

would give you the address of the union. Suppose the address be '**0x96ca048'** . Now to obtain the union which contains the next element of the list, you need to add the address of the current union with the size taken by each union (sizeof(union)). 
_Union containing the next element of the list = Address of the current union + size of each union_. For example :
**r.list.val -> items = 0x96ca048 + 10**
would give you the union that stores the next element of the list . The size of the union is 10, in hexadecimal = 16 bytes.
[![](http://sunilkumarn.files.wordpress.com/2010/09/image-1.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/09/image-1.png)

The path of **tp_add** can be traced using the 'bt' option in gdb. The path of the function tp_add is as follows:
[![](http://sunilkumarn.files.wordpress.com/2010/09/image-2.png?w=245)](http://sunilkumarn.files.wordpress.com/2010/09/image-2.png)
