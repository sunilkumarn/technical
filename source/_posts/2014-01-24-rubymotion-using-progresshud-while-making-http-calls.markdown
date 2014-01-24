---
layout: post
title: "Rubymotion: Using ProgressHud while making Http calls"
date: 2014-01-24 20:04
comments: true
categories: Ruby 
---

With Rubymotion, there are quite a few methods to indicate activities. What I have used here is the MBProgressBarHud to indicate an ongoing activity ( an http call in progress )


####controller/msisdn_view_controller.rb

    outlet :submitButton, UIButton
    ib_action :submit_mobile_number  #The submit button on event ‘Touch Up Inside’ invokes the action submit_mobile_number

    def viewDidLoad
      @hud = MBProgressHUD.alloc.initWithWindow(UIApplication.sharedApplication.keyWindow)
      self.view.addSubview(@hud)
    end

    def submit_mobile_number(sender)
      msisdn = self.msisdnTextField.text
      prepare_for_submission
      User.registration msisdn, do
        after_submission
      end
    end
    
    def prepare_for_submission
      @hud.labelText = "Provisioning"
      @hud.show(true)
      self.view.userInteractionEnabled = false
    end

    def after_submission
      @hud.labelText = "Completed";
      @hud.hide(true)
      performSegueWithIdentifier('submission_segue', sender: self)
    end
    

####models/user.rb

    def self.registration(msisdn, &callback)
      ...
      url = 'some_url'
      BW::HTTP.get(url) do |response|
        callback.call
        ...
      end
    end
    
As you could see from the code above, in the viewDidLoad delegate we initiate an MBProgressHUD instance and add it as a subview to the UIView. This is done in the viewDidLoad delegate.

Before the call to **registration** ( the User model method which makes an http connection ) is made, the **prepare_for_submission** method displays the hud. The **after_submission** method is passed as a callback block to the **registration** method. On completion of the http request, this block is executed which hides the hud and performs the segue to present the next controller.

    

    
