---
author: sunilkumarn
comments: true
date: 2010-08-11 20:15:19+00:00
layout: post
slug: aliasing-to-avoid-making-mistakes
title: Aliasing to avoid making mistakes
wordpress_id: 134
---


It might have happened to that you that you unintentionally deleted some files in your directory. The deleted files may be not recovered back. So its better that you take some precautionary measures to avoid making such mistakes. Setting up proper aliases is one such way to so so. Aliases are short names for certain commands.

In order to set up your alias ,create a .bashrc file in your own home directory.
$ vi .bashrc
Set up your own aliases there.

Aliases for not making mistakes while using the cp,mv and rm commands are follows.

 **14 alias rm='rm -i'

 15 alias cp='cp -i'

 16 alias mv='mv -i'**

These three statements ensure that whenever you the commands cp,mv or rm ,the system asks you to double check your decision just to make sure that is exactly what you wanted to do. This could help you against overwriting a file,or deleting a file unintentionally.




