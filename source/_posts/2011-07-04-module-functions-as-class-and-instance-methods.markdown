---
author: sunilkumarn
comments: true
date: 2011-07-04 05:20:51+00:00
layout: post
slug: module-functions-as-class-and-instance-methods
title: Module functions as class and instance methods
wordpress_id: 436
categories:
- Ruby
---

Consider you have a module and a class.

module Mymod and a class Myclass.

The situation in hand is such that certain functions in the module need to end up being instance methods of the class Myclass and certain functions need to be Class methods.  You could very well imagine of such situations. Consider you are using ActiveRecord and have a sub class **Subscription** in correspondence with a DB table. You want to insert logic, within the module, that would work in each of the following case.

1) When a subscription fails or succeeds.

2) When an unsubscription fails or succeeds

You do this.


_module SubscriptionLogic_




_   def  after_sub_




_      ...._




_   end_




_   def  after_unsub_




_     ...._




_   end_




_   def  after_sub_fail_




_     ...._




_   end_




_   def  after_unsub_fail_




_      ...._




_    end_




_end_


_class Subscription_

_   include SubscriptionLogic_

_   ....._

_end_

You insert the logic as functions of a module, say **SubscriptionLogic**. Ideally you want the methods containing the logic to be instance methods of the class Subscription. You include the module SubscriptionLogic in the class and you avail all the functions in the module as Instance methods. Now you consider the case when an unsubscription fails - i.e, there is no existing subscription so that an unsub could take place.  No way is it possible that you could have a function 'logic_after_unsub_fail'  in the module SubscriptionLogic and use it as an instance method, _**simply because there is no instance available**_.  You think and decide to use the function as your class method, but you have '**include**d'  the  module in your class and hence its not possible to use it as your class method. You cannot **extend** the entire module coz , ideally you want the logic to be instance methods.

So you could get this solved up by a simple piece of extra coding.

_module SubscriptionLogic _

_  def  after_sub_

_      ...._

_  end_

_  def  after_unsub_

_      ...._

_  end_

_  def  after_sub_fail_

_      ...._

_  end_

_  module ClassMethods_

_      def after_unsub_fail_

_           ...._

_      end_

_      def self.included(base)_

_          base.extend(ClassMethods)    # base pertains to the class within which you include the module. _

_      end_

_   end_

_end_

Now within the class insert this line.

_class Subscription _

_   include SubscriptionLogic _

**_   extend SubscriptionLogic::ClassMethods_**

_   ....._

_end_
