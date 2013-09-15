---
author: sunilkumarn
comments: true
date: 2010-08-11 19:50:44+00:00
layout: post
slug: creating-new-commands-and-passing-arguments-in-unix
title: UNIX - Create new commands and pass arguments
wordpress_id: 129
categories:
- UNIX
---

When a particular sequence of commands is repeated time after ,it might be better if we create a shorter command of our own. These command creation mentioned may not be suitable for public use but would be helpful for private use.

Consider suppose that we intend to count the number of users quite frequently:
**$ who|wc -l**

We can make a small command of ours named 'count' that does the same work.
So we do 
**$echo 'who|wc -l'>count** (A file named 'count' is created).
 
Now we run the shell program with its input coming from the file 'count'.
**$ sh<count**
this is same as 
**$ sh count**

Now to type sh all the time wouldn't be such a great thing. So we make the file 'count' executable by
**$ chmod +x count**
Now the result of doing 'who|wc -l' can be produced by doing 'count'.
**$ count**
Install count ,place it in the /usr/bin directory. Set the path appropriately and start using the command 
created.

Now again when we begin to create executable commands of our own, we will have to make it executable . So it would be better if we create a shorter command for **chmod +x filename**.
The problem here would be that the filename that is passed to 'chmod' may change every time.
Therefore we do
**$ echo 'chmod +x $1'>cx**	(cx would be the file created).

Here $1-indicates that only 1 argument can be passed to the command at a time
         $2 would make it 2 and $9 would make it 9.
So to enable any number of arguments we do
**$ echo 'chmod +x $*'>cx**

Now we need to make cx executable.
This can be done either by doing 
**$ chmod +x cx**       or else
**$ sh cx cx **             because sh cx is equivalent to doing chmod +x $*.
Now test it by creating a test command 'test' ,install it and run the command. 'Permission denied' will be the message. Now do
**$ cx test**
**$ test**
The command 'test' will run and the result will be produced.

Now suppose we have a file 'class' consisting of some information about the students of our class and consider a case where we need information about certain students  and  we need it quite frequently. This can be done with the help of the 'grep' command.
**$ echo 'grep $* /root/new/class'> find** 
install the command.
**$ cx find**              /* makes the file executable
**$ find**

Now a possible would arise when the following case is given as the argument:
$ find sunil kumar
This is because the words sunil and kumar will be seen as two different arguments.
Hence do
**$ echo 'grep “$*” /root/new/class'> find** 
This would solve the problem.






