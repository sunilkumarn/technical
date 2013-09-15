---
author: sunilkumarn
comments: true
date: 2010-09-25 13:37:05+00:00
layout: post
slug: concept-and-creation-of-environment
title: 'Concept and creation of Environment '
wordpress_id: 258
categories:
- Python
---

One of the most important thing you need to grasp while doing a project on interpreters and evaluator is the idea of an environment. Within the program the environment is a collection of the names(whether variables or functions) and its corresponding values. While using such names you will need an environment where you can store the value of those names so that you could fetch it for further use. While implementing the evaluator for scheme in python , the concept of environment was implemented using **associative arrays** in python. 3 dictionaries were used to implement the concept of environment. Lets go through a bit of details on the creation and use of environments. 

Consider writing the scheme statements:

scheme>>>( define n 7 )
scheme>>>( define s 6 )
scheme>>>( if ( > n s ) n s ) 

In our 'if' statement we have not specified the exact value , but instead we have used variables. The question comes then that from where do we retrieve the values of those variables.
The answer is simple: 'Here the variables are global variables and are accessible from anywhere within the program'. Hence we need create a dictionary which consists of 'n' and 's' as its keys and their values as the values of the dictionary. This dictionary is created when the statement 'define ( not followed by a  parenthesis) is inputted. 

1)**dict**={'s':'7','n':'6'}

Obviously we cannot use this dictionary for all our purpose. We also need a dictionary where we could store variables and values that are local to a function. We have created another dictionary for this very purpose .

Consider the following:
scheme>>>( define ( sq ( lambda ( x ) ( * x x ) ) )       ----->      statement 1 
scheme>>>( sq 4 )					    ----->     statement 2

When the second statement is executed ,a dictionary 'local_dict' is created where this value 4 is associated with the variable 'x'. 

2)**local_dict**={'x':'4'}

We use the 3rd dictionary for associating a function with its body. When the 'statement 1' above is inputted , a dictionary 'func_dict' is created which contains the 'function name' as its key and the 'body' as its value.

3)**func_dict**={'sq','(  lambda ( x ) ( * x x ) )' }

We have functions '**create_env ()'** and **'create_localenv()**' for creating the first and third dictionaries that we have mentioned above. The second dictionary is created within the function **'exec_call()**' where the processing of each function is started. Hence its appropriate to create the dictionary needed only for the local use of that function within exec_call().


