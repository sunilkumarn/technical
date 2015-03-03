---
layout: post
title: "Objective-C Categories into Rubymotion"
date: 2014-02-27 14:04
comments: true
categories: RubyMotion iOS Ruby
---

People familiar with developing Ios applications with Objective-C would be quite familiar with using ** categories ** I suppose.

An Objective-C category allows you to add methods to an existing class — effectively subclassing it without having to deal with the possible complexities of subclassing.

Moving into Rubymotion and using the power of categories is quite well possible for people who have been working with Ruby. **Monkey Patching** ( open an existing class and add your custom code within it ) it quite a familiar task with Rubyists.

Below is an example where I need to add a customized back button for the UINavigationController navigation bar. I need this to be the back button in quite a few View controllers. It would be quite a tedious task to create a button in each VC, hence I create a category file and monkey patch the UIBarButtonItem class to add a new method which returns the customized back button.

*app/categories/ui_bar_button_item.rb*

    def self.customBackButton(target, action)
      buttonImage = UIImage.imageNamed('arrow.jpeg')
      button = UIButton.buttonWithType(UIButtonTypeCustom)
      button.setImage(buttonImage, forState:UIControlStateNormal)
      button.frame = CGRectMake(0, 0, buttonImage.size.width, buttonImage.size.height)
      button.addTarget(target, action:action, forControlEvents:UIControlEventTouchUpInside)
      customBarItem = UIBarButtonItem.alloc.initWithCustomView(button)
      customBarItem
    end


The view controller which needs to have this customized back button will have this

    def viewDidAppear(animated)
      super
      self.navigationItem.leftBarButtonItem = UIBarButtonItem.customBackButton(self.navigationController, 'popViewControllerAnimated:')
    end

This would use Ruby's monkey patching power to create elements in Rubymotion which could be used in the entire application as & when needed.
