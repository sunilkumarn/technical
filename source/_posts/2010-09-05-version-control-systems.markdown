---
author: sunilkumarn
comments: true
date: 2010-09-05 14:53:32+00:00
layout: post
slug: version-control-systems
title: Version control Systems...
wordpress_id: 184
---

Version control is a system that records changes to your files and allows to view specific versions of a file or set of files.VCS is used so that you don't screw up your files or your entire project as a result of the silly , careless mistakes that you have made.
Many people do their own version control by copying their files to another directory.This is a quite simple and easy approach if you are careful enough so that you won't agonize by doing something disastrous so that you may lose all your stuff later.Therefore keeping in mind that such things can happen ,after all we are all humans, we use version control systems.

_Centralized VCS and Distributed VCS._
**Centralized VCS **
CVCS includes a single server that contains different versions of the files and the clients could check into that.The major advantage of this system is that administrators have fine grained control over whats happening ,but it has serious drawbacks as well. They are:
Single point of storage if crash would be fatal.
The clients could pull anything from the repository but they need a **commit access **to push something into it.This might lead to a client getting frustrated if he/she has not been granted a 'commit access'  even after working for a longer period of time. Also there might something political issues related to administration.

**Distributed VCS.**
In DVCS ,you could push data into the repository with the same ease with which you could pull from it. All the repositories are at the same level and therefore we could collaborate with any of the repositories.Examples of DVCS are Git , mercurial , bazaar..etc...


