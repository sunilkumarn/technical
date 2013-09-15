---
author: sunilkumarn
comments: true
date: 2010-09-25 13:43:22+00:00
layout: post
slug: evaluation-of-scheme-in-python
title: Evaluation of Scheme in Python
wordpress_id: 253
categories:
- Python
---

Those who have ever gone through the SICP ( Structure and interpretation of computer programs) , would have come across the implementation of a Metacircular evaluator ( an evaluator written in a language that it evaluates) for scheme in the 4rth Chapter. 
[![](http://sunilkumarn.files.wordpress.com/2010/09/cover.jpg?w=207)](http://sunilkumarn.files.wordpress.com/2010/09/cover.jpg)

Motivated by that , and drawing ideas from the **wizard book** , an evaluator for Scheme is created in Python, so that Scheme statements evaluated in Python. 

The creation of a metacircular evaluator for scheme contains prominently of two steps:

         1)  Eval:   To evaluate a combination (a compound expression other than a special form), evaluate the subexpressions and then apply the value of the operator subexpression to the values of the operand subexpressions. 
        2)  Apply :  To apply a compound procedure to a set of arguments, evaluate the body of the procedure in a new environment. 

[![](http://sunilkumarn.files.wordpress.com/2010/09/image-02.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/09/image-02.png)

_**Implementation of the Evaluator in Python:**_

Before getting into the creation and implementation details of the evaluator , please read [this](http://sunilkumarn.wordpress.com/2010/09/25/concept-and-creation-of-environment/). 
Now that you have got an idea about the concepts of environment , lets get into further details of creation of the evaluator.

The first process is basically to parse the user Input and use it in a way so as to evaluate it. After parsing ,the user input is provided to the function gravity().This particular function performs the task of classifying the user input and route them to appropriate functions. This is similar to 'eval' in the scheme metacircular evaluator implemented in SICP.
According to the user input , the appropriate functions are performed.
Different functions implemented in the evaluator.

**evaluate():**
This function could be compared with the 'apply' procedure implemented in SICP. The function checks for different operator and its corresponding operands. The operator is applied to the operands and the result returned.

**opdet():**
The function is called from 'evaluate()' , it takes in a list and returns the operator that has to be applied to the operands.

[![](http://sunilkumarn.files.wordpress.com/2010/09/evaluate.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/09/evaluate.png)

If a simple expression is provided just to perform basic arithmetic and logical operations , only these two functions are invoked after the gravity () function and we obtain the result. The situation, as is obvious would be a little different when relatively complex tasks are give ( such a processing of an 'if' statement , a function , an assignment operation, a recursive function and so on.


Processing of an 'if' statement involves invoking many more functions.

**if_begin_eval():** 
This function involves the presence of any variables involved in the 'if' statement and fetches the value of such variables from the environment.

**if_eval() :** 
This particular function is used to evaluate the 'predicate ' of the 'if' statement and returns the result of the 'if' statement. The function invokes the 'evaluate()' function and would process the entire 'if' statement according to the value returned by 'evaluate()' .
If a simple 'if' statement' 
( if ( > 4 5 ) 4 5 ) is inputted:
if_eval() would pass the predicate part ( >  4 5 ) to 'evaluate()' and process the if statement according to the returned value. Here the value returned by 'evaluate()' would be 0, since the condition ( 4 > 5 ) is not true. The value returned by the 'if_eval()' would be 5.

**if_cons_alt(): **
The function evaluates the consequent and alternative of the 'if' condition'. A separate function for the process would be specially helpful when the if statement consists of larger expressions and values.
The function would return back a 2 element list of the final values obtained after evaluation of the consequent and alternative.

[![](http://sunilkumarn.files.wordpress.com/2010/09/if.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/09/if.png)

Evaluation of procedures would take us through a different path. We need functions that can extract, evaluate and execute a function. We also need a function to test whether the inputted procedure is recursive or non-recursive.

**exec_call():**
Checks whether the function is a simple function of the sort :
( define ( lambda ( square ( x ) ( * x  x ) ) )  non-recursive 
or of the sort:
( define ( lambda ( fact ( x ) ( if ( = x 1 ) 1 ( * x fac ( - x 1 ) ) ) ) ) )   recursive :
executes the procedure if non-recursive.

**extract_funcbody():**
extracts the body of the function. Passes the body of the function to evaluate_funcbody .

**evaluate_funcbody():**
Fetches the value of the variables in the function body and returns back the function body to extract_funcbody(). This function is invoked only when the procedure is non-recursive.

**exec_func_body():**
Fetch the value of the variables in the function body . This function is invoked only when the inputted procedure is recursive. 

[![](http://sunilkumarn.files.wordpress.com/2010/09/func1.png)](http://sunilkumarn.files.wordpress.com/2010/09/func1.png)

**_Use of the module ops()._**
You would see a module named ' ops' being imported. This module consists of certain functions that can be performed on lists. The module is imported so as to get a feel of the way programming is done in scheme , any operation on lists can be performed on lists using 5 basic functions. The attempt has been made to stick to the combination of these basic operations rather than using the full functionality of Python. 

The source code of the project can be obtained [here](http://github.com/sunilkumarn/MetacircularEvaluator).

