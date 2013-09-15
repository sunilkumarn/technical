---
author: sunilkumarn
comments: true
date: 2010-10-19 11:42:16+00:00
layout: post
title: Common Subexpression elimination by GCC
wordpress_id: 373
categories:
- C
---

**Common subexpression elimination** (CSE) is a compiler optimization technique of finding redundant expression evaluations, and replacing them with a single computation . This saves the time overhead resulted by evaluating the expression for more than once . We will have a look into this phenomenon by considering a simple code and again taking a walk through its assembly code .

Consider the code that we have written and saved in file ' pgm.c '

`main(){
int i, j, k, r;
scanf("%d%d", &i, &j);
k = i + j + 10;
r = i + j + 30;
printf("%d %d\n", k, r);
}`

Do :

**$ : cc -S pgm.c**

**$ : less pgm.s**

Read the assembly code ' pgm.s '

Scan the assembly code and find out the call to the function 'scanf()' . This is where our area of interest begins . This is because its after calling 'scanf ' that we load the value of the input variables into the registers .

Right after the call to 'scanf ' , i have this in my assembly code ,

**movl    -16(%ebp), %edx
movl    -20(%ebp), %eax** .

Here we move the values of the two variables 'i ' and 'j ' into registers 'edx ' and 'eax ' .

Now just have a look into our C code and you will see the two expressions as follows :

`k = i + j + 10;
r = i + j + 30;`

Looking at the two expressions , you would find out the redundant part in the two expressions .

Its that 'i+j' is calculated twice .

Now lets find out what happens in the assembly code :

I had these statements in your assembly code :

**movl    -16(%ebp), %edx**

**movl    -20(%ebp), %eax** ----------> We had mentioned these two statements  above .

**leal   (%edx,%eax), %eax** -------->  values in edx and eax are added and loaded into eax

**addl    $10, %eax** -------------------->  the constant 10 is added with the value in eax

**leal   (%edx,%eax), %eax** --------->   values in edx and eax are added and loaded into eax

**addl    $30, %eax** -------------------->  the constant 30 is added with the value in eax

You could see that there is redundancy in the assembly code .

The statement** ' leal(%edx,%eax), %eax** ' performs the evaluation of the subexpression ( ' i + j ' ) which is done twice thereby throwing in redundancy and hence creating unnecessary overheads .

Obviously our compiler when asked to perform optimization would avoid this redundancy .This is what is termed as **Common Subexpression Elimination** ,i.e , the subexpression that is common( ' i + j ' ) in the two expressions is evaluated only once , in essence the second evaluation is eliminated .  Lets have a look into CSE by again peeping into our assembly code  .

Do

**$ : cc - S - O3 pgm .c**

**$ : less pgm.s**

Take a look into your assembly code  .

As mentioned above , our area of interest starts from the call to 'scanf ()' .

Instead of those 6 statements that were used to evaluate those two expressions ,

i + j + 10 & i + j + 30 ,

you would find this ,

**movl    -12(%ebp), %eax **---------------->  move the value of the variable 'i' into eax

**addl    -8(%ebp), %eax **-------------------> add the value of the variable 'j' with 'i' stored in eax .

**leal    30(%eax), %edx** -------------------> adds 30 with eax ( contains the sum of i & j ) and stores in edx

**addl    $10, %eax **---------------------------> adds 10 with eax ( contains the sum of i & j ) and stores in eax .

Here the second statement performs the evaluation of the subexpression , ' i + j ' . You could get it quite clearly from the code segment that the redundancy that was found in the unoptimized assembly code has been eliminated by the compiler .

The later 2 statements perform the evaluation of the expressions

( i + j + 30 )   and    ( i + j + 10 ) respectively .

We have just used a very simple case of CSE , but this optimization technique could be used in even more useful cases . Consider :

k = f() + g() + 30

r = f() + g() + 10

where f() and g() are functions . In an unoptimized code , the technique of CSE wouldn't be used and hence there would be overhead due to the redundancy added with the more serious problem of having to call each function twice . The technique of **Common Subexpression Elimination** avoids this very redundancy  .
