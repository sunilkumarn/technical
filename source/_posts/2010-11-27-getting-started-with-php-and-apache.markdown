---
author: sunilkumarn
comments: true
date: 2010-11-27 09:14:10+00:00
layout: post
slug: getting-started-with-php-and-apache
title: Getting started with PHP and apache
wordpress_id: 430
---

Someone who gives it a thought of using PHP , apache and stuff for the first time will be faced up with a few number of fixed problems . Configuring your apache server , establishing a connection between PHP and apache , stuffs like that . Here is a basic introduction to start using these things and kick off as a web programmer .

Obviously you need to have PHP and apache installed on your system .

**$ : sudo apt-get install apache2**

**$ : sudo apt-get install php5-cli**

**$: sudo apt-get install libapache2-mod-php5**



would get you apache and PHP installed on your system  .

Using the appropriate versions is left upto individuals .

Just have a peep into some of the files like :

_/etc/php5/apache2/php.ini_ -----> You would see a lot of variables set to 'ON'  and 'OFF' . You may well have to toggle those values as and when necessary .



Try starting , stopping and restarting your apache web server .

**$ :sudo  /etc/init.d/apache2 start**

**$: sudo /etc/init.d/apache2 stop**

**$: sudo /etc/init.d/apache2  restart**



Once everything is fine and and perfect  , try out your first PHP script . Script it , save it in a file with a '.php' extension and put your file in the directory '/var/www '. Now you could access this script from your browser

**localhost/'scriptname'**

and you see the result right there in front of you . Thats just how you enter the world of web programming .

