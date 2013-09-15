---
author: sunilkumarn
comments: true
date: 2010-07-30 10:56:41+00:00
layout: post
slug: the-shell-basic-facilities-offered
title: The shell-Basic facilities offered
wordpress_id: 34
categories:
- UNIX
---

Shell -the command interpreter.
There are many basic facilities offered by the shell.They include

1)Using shorthands for files(use of the asterisk '*')
For example the command
**pr temp***
would print the contents of all files beginning with 'temp'
Similarly the echo command
**echo ***
will list all the files in the current directory

2)Input-Output redirection
Consider the case where we want to list the files of the directory but not in the terminal
So we redirect it to a file using the '>' operator
**ls > filename
**If the file 'filename ' is not already created it will be so,or else it will be overwritten. 
Counting the number of lines in a particular directory is done as
**ls >filename
wc -l<tfilename**


3)Inorder to avoid the creation of a temporary file,PIPES are used for the input-output redirection
The vertical bar '|' is the pipe.
Instead of using
ls>filename
wc -l<filename
we use 
**ls|wc -l **
for the redirection to be possible.

4)Tailoring the environment
As a user of UNIX,we could customize it in the way comfortable to us.
The 'stty' command helps us do that
for eg:
**stty erase '^h'
**enables the user to use ctrl-h for erasing a particluar character.
By storing it in the file .profile (present in the home directory),we need not type it each time we login
One of the most important tailoring comes during the setting up of paths
Again this could be made changed in the **'.profile' **file.
The current path would enable searches in the directories /usr/bin and /bin.
If we did want to augment another directory to the search path we do it as follows:
**PATH=export $PATH:/usr/games**
where usr/games would be the newly added search directory.
The augmented path would be obtained using the 'echo' command
**echo $PATH
:/usr/bin:/bin:/usr/games**
All these changes could be saved into the file named '.profile'




