---
author: sunilkumarn
comments: true
date: 2010-07-30 10:57:38+00:00
layout: post
slug: a-few-basic-commands-in-linux
title: A few basic commands in Linux
wordpress_id: 31
categories:
- UNIX
---

The common  file system commands that would make a beginner get started with linux are the following:

**ls**:command that lists all the files in the current directory
Variations in the use of the ls command would be found as the ls -l,ls -t...
**cp source dest**:command that is used to copy file 'source' to file 'dest'
**mv source dest**:moves the file 'source' to file'dest'
**rm files**:command used to remove a particular file
These commands when invoked would be looked in the directory /usr/bin
To check the presence of the 'cp' command use:
**which cp**
You would get back /usr/bin
There is a case where the use of a paricular command (say cp)would display in a message as follows
bash:cp command not found
Make use of the 'which' command to see if the cp command is present in the directory
**which cp**
There might be no response
Therefore the coreutil package needs to be reinstalled
**apt-get install coreutils --reinstall coreutils**
This would fix the problem.
The other basic commands would include the commands for printing the text:
**cat filename**:
**pr filename**:

Directories in linux contain files and subdirectories.Each user will be having his own home directory to whih he logs in at the beginning of a session.The user may then move on work in other directories which becomes his  current directory.

The basic commands that are used in regard with the directories include the following

**mkdir dirname:**creates a new directory
**cd dirname:**change to the particular directory(provided that you are in the correct place)
To check whether you are in the correct place use 
**pwd**
You would get back the 'current directory'
**cd ..**: is used to go the previous directory.
**cd**: moves to the home directory.


