---
layout: post
title: "Rubymotion: Push Notifications"
date: 2014-03-25 16:24
comments: true
categories: 
---

To setup the app id and ssl certificate and download the ios provisioning profile for push notifications in development mode, this would be a great [article] to read on. It worked like a charm for me.

Once thats been done, in your Rubymotion project, add a few extra lines in the Rakefile.

    app.name = 'Myapp'
    app.identifier = 'com.myapp.development'
    app.provisioning_profile = '.environments/Myapp_Develpoment.mobileprovision'
    app.entitlements['keychain-access-groups'] = [
      app.seed_id + '.' + app.identifier
    ]
    app.entitlements['aps-environment'] = 'development'
    app.entitlements['get-task-allow'] = true

The *app.identifier* needs to be the same as the identifier of the app id which was created in the ios development portal.
*Myapp_Develpoment.mobileprovision* is the provisioning profile generated and downloaded for the app id with identifier 'com.myapp.development'. Both of them are extremely important for the push notifications to work.

In app_delegate.rb, do this.

    def application(application, didFinishLaunchingWithOptions:launchOptions)
      ...
      UIApplication.sharedApplication.registerForRemoteNotificationTypes(UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound)
      true
    end

    def application(application, didRegisterForRemoteNotificationsWithDeviceToken: device_token)
      NSLog("My token is: %@", device_token)
    end

    def application(application, didFailToRegisterForRemoteNotificationsWithError: error)
      NSLog(error.inspect)
    end

Next time, when the app is installed on the device it receives an alert requesting permissions to enable push notifications. Also note that the app would ask for permissions only on first time installation. To simulate a first time installation of the app do the following:

  - uninstall the app from the device
  - switch off & on the device
  - reset the date to 2 days ahead
  - switch off and on again
  - reset the date to even 2 days ahead
  - switch it off & on for a last time.

Install the app and the alert requesting the permission should appear now. ( Got this through some pretty ordinary googling, but it worked pretty well for me :) )

Note down the device_token received in *didRegisterForRemoteNotificationsWithDeviceToken* method. This is almost like the address of the device to which the push notifications will be sent from the APNS server.

To send a push notification from a Ruby app, I used the grocer gem which worked pretty neat and fast for me. Install the gem & do the following.

    require 'grocer'

    pusher = Grocer.pusher(
      :certificate => "ck.pem",
      :passphrase =>  "passphrase",
      :retries => 3
    )

    notification = Grocer::Notification.new(
      :device_token  =>   " device token ",
      :alert =>  "My first notification",
      :badge =>   0
    )

    pusher.push(notification)

If nothing has been done on the app side to handle these notifications, close the app and send the notifications. They should appear on screen now.

[article]:http://www.raywenderlich.com/32960/apple-push-notification-services-in-ios-6-tutorial-part-1
