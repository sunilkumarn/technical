---
author: sunilkumarn
comments: true
date: 2010-11-24 16:34:29+00:00
layout: post
slug: greasemonkey-customizing-your-webpages
title: 'GreaseMonkey : Customizing your webpages '
wordpress_id: 424
---

There would have been times when you would have felt if the web page in front of you behaved a bit more according to your like . This might specially happen if the page consists of forms that you fill with data directed to live servers .  In such cases you wouldn't mind customizing the page to suit your needs so that you don't do some nonsense that would result in chaos .

Firefox , i see many using it as their default browser , offers a plugin called '**Grease monkey**'  that allows you to do just that . All you need to is to install the plugin , write your own javascripts , install them ( thats pretty easy to do ) and start using them  .

Write your javascripts to customize your webpage . What you need to do in addition is to add some **metadata** providing extra information to greasemonkey .

The meta data might look as follows :

[![](http://sunilkumarn.files.wordpress.com/2010/11/untitled-1.png?w=300)](http://sunilkumarn.files.wordpress.com/2010/11/untitled-1.png)













The first and the last lines are necessary to indicate to the greasemonkey that the script it to be processed by it .

The **name and the description are optional** . It might prove helpful to you if you have scripted and installed a number of greasemonkey scripts .

The 4th an the 5th line of the metadata are extremely important and needs to be well understood . **These lines tell the greasemonkey the URLs on which the script is to be applied and not to be applied **.

**@include** -> specifies the URLs on which the script is to be installed . Here '*' is specified which is just a wildcard and indicates that the script is to applied on all the sites  .

**@exclude** -> specifies the URLs that are to be left out . It is to be noted that @exclude is processed before @include .

This gives you an idea of the metadata part of the script . Now you may write your own javascript , add your metadata at the beginning and save the file with the extension ' **.user.js **'  .

To install the script , open the file in firefox and you would see an option 'install' . Just give a click and you are ready to use your greasemonkey script .You could manage your script , disable it , uninstall it as well . Just click

_Tools ----> Greasemonkey ----> Manage user scripts ...._

Having another plugin '**firebug'** may help you in the process of writing the script . This allows you to view the html code of the webpage that is infront of you . Infact the plugin is almost a necessity to help you write efficient code with greasemonkey .

Now you may start writing your own greasemonkey scripts and customize any of the webpages as and according to your like . It's also to be understood that greasemonkey is a plugin of firefox . If you use Google chrome as your browser , you wouldn't need to install any plugin . The facility to customize the webpage is provided by default . You may use the same code that you have written to work with greasemonkey . The metadata and the process of installing is the same and obviously the results are the same as well .
