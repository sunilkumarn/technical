---
author: sunilkumarn
comments: true
date: 2010-08-04 06:35:42+00:00
layout: post
slug: files-in-the-unix-file-system-and-their-permissions
title: 'Files in the Unix file system and their permissions... '
wordpress_id: 72
categories:
- UNIX
---

A file is a sequence of characters. A few commands and knowledge that may help in dealing with files and directories in UNIX are the following.
The 'ls' commands and its variations :
The 'du' command and its variations 
The command used to change the permissions – 'chmod'.
A look into these commands
ls command gives a listing of all the files that are present in the current directory.
The 'ls -l' is used to provide a detailed listing of the files that are present.
For eg. If you type the command 'ls -l' the results be will produced as
-rw-r--r-- 1 root root 856 2010-08-03 20:15 delete.c

-rw-r--r-- 1 root root 868 2010-08-03 08:19 line.c

where line.c and delete.c are the two files present in the directory.
The other variations of the command include ls-u,ls-t etc
The 'du' (disk usage)provides information about the usage of the disk.
The command 'du -a' is used to provide disk informations about even the files of subdirectories.
 The 'du-a|grep line.c'
provides the details about the file 'line.c'.
'chmod' is the command that is used to change permissions. The permissions can be changed by the superuser.
Consider the following permissions:
-rwxrwxrwx     -    indicates that the owner,the group and the other users of the system can all read(r),write(w)and execute(x) the file of which the permission has be shown.
Inorder to change the permission , the 'chmod' command is used.
For eg. 'chmod 666  file'
indicates that the file can be read and written by all the three categories of users mentioned above.
The octal values have their meanings as
4-indicates that the file can be read
2-file can be written
1-file can be executed
therefore 'chmod 444 file' would set the permissions of the file to read only for all the  categories of users.
6 6 6 indicates(4+2 4+2 4+2) for the three users.)
Now do 'ls -l' and you will see the permissions as
-r--r--r--.
'+' is used to turn a permission on   and '-' to turn it off.
The command 'chmod +w file' enables write permissions for the file
and 'chmod -x file' turns off the execute permissions for the file.
The permissions of the file can be viewed using the 'ls -l' command stated before. The permissions of the directory cab also be set and viewed using the 'ls -ld' command.
The 'ln','cp','mv' commands are used in UNIX for slightly different uses.
The 'ln command creates a link to the file. Therefore the same file can be accessed using the two file names. The syntax is as follows:
'ln filename filename1'
You would obtain another link to the file named 'filename' by the name of 'filename1':
The change made to original file would reflect in the other as well. You could see that with help of the 'ls -i' (command that indicates the 'i-number' of the file)that the two files that are linked are having the same i-number.
'cp' command is used to make the copy and the 'mv' command is used to move or rename files.
The command 'mv filename1 filenname2...... destination directory' is used to move several files together to the destination directory.

