---
layout: post
title: "Rubymotion: Chain of Modal segues"
date: 2014-02-06 19:02
comments: true
categories: RubyMotion iOS Ruby
---

While building my iOS application, I had a user registration process to be implemented. This included the user entering his mobile number, his language details and a few other ( say, miscellaneous ) details. Once he has registered himself he is presented with a table view where a few movies in the language he chose are shown.

The way I went about implementing is that I had a **master_view_controller** which is the initial view controller. The other controllers are

* **mobile_number_view_controller**
* **language_view_controller**
* **miscellaneous_view_controller**
* **movies_view_controller**

I have segues, **all of type modal**, from the master controller to all other controllers I mentioned above. The segues are of modal type because once the user has submitted each of his details, he doesnt have to care about the particluar detail and is given no option to go back to his previous screen. Its not a like a dig deep type of chain where we could implement the navigation controller. The segues are the follows.

On the left we have the contoller and on the right we have the segue identifier to that controller from the master controller.

    master_view_controller:
      mobile_number_view_controller -> msisdnSegue
      language_view_controller -> languageSegue
      miscellaneous_view_controller -> miscellaneousSegue
      movies_view_controller -> moviesSegue


Now in the viewDidAppear delegate of the master controller, we have this check to present the appropriate controller. The details entered by the user in the registration process are saved( we use **motion model** to store the details of the user ). Ofcourse, we dont want a user who has entered his mobile number once to enter it again even if he has opted to quit the app after entering the mobile number. Similarly we dont want him to select his language for the second time during the registration process. We consider all this in the viewDidAppear delegate and decide which contoller to load.

master_view_controller

    def viewDidAppear(animated)
      super
      perform_criteria_based_segue
    end

    def perform_criteria_based_segue
      @user = User.first

      if (not @user) or (not @user.verified_mobile_number)
        performSegueWithIdentifier('msisdnSegue', sender: self)
      end

      elsif not @user.language
        performSegueWithIdentifier('languageSegue', sender: self)
      end

      elsif not @user.miscellaneous_details
        performSegueWithIdentifier('miscellaneousSegue', sender: self)
      end

      else
        performSegueWithIdentifier('moviesSegue', sender: self)
      end
    end

Now, suppose the user reached the view where he has to enter his misellaneous details. Once he entered them all he now has to be be shown the table view which list the movies in his language. We have implemented this segue to the movies_view_controller in **viewDidAppear** delegate in the **master_view_controller**. Hence for this to work, the master_view_controller's view will have to appear. For this to happen, the current controller( the i.e, **miscellaneous_view_controller** ) along with all controllers in between the current controller and the master view controller will have to be dismissed ( in our case, its only the **language_view_controller** ).

For this to be done, we have the **dismissViewControllerAnimated(flag, completion:completion_block)** method.

We implement this method in the presented view controller. Here, **miscellaneous_view_controller** is the current presented view controller.

miscellaneous_view_controller.rb

    outlet :submitButton, UIButton
    ib_action :submit_miscellanous_details

    def submit_miscellanous_details
      # Do something here...

      self.view.window.rootViewController.dismissViewControllerAnimated(true, completion:nil)
    end


So in the **miscellaneous_view_controller** controller we have a *submitButton* which is tied to an ib_action *submit_miscellanous_details*. On submitting the button, the ib_action is invoked. We do whatever necessary steps is required in the ib_action and then **invoke dismissViewControllerAnimated on the rootViewController( i.e, the master_view_controller )**. When this is done, the presented controllers and all other controllers in the chain of controllers until the presenting controller on whom the dismissViewControllerAnimated method is called ( here, the rootViewController) is also dismissed. The rootViewController or the master_view_controller view appears on screen and the viewDiDAppear delegate will process the rest.

