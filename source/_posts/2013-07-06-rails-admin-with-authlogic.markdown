---
author: sunilkumarn
comments: true
date: 2013-07-06 17:43:31+00:00
layout: post
slug: rails-admin-with-authlogic
title: Rails admin with authlogic
wordpress_id: 1233
categories:
- Ruby
---

If you have used the rails_admin gem, you would have noticed that its tightly coupled with Devise. What happens if devise is not the pick and the application is already integrated with authlogic. Lets see,

After installing the rails_admin gem, create a '**rails_admin.rb**' file in **config/initializers**. Paste the following code,

{% highlight ruby %}
  RailsAdmin.config do |config|
    config.authorize_with do
      redirect_to root_path, :alert => "You are not authorized!" unless current_user.admin?
    end
  end

  RailsAdmin.config do |config|
    config.authenticate_with do
      unless current_user
        redirect_to login_url
      end
    end
  end
{% endhighlight %}

The authentication block checks if the user is currently logged in and the authorization block checks if the logged in user is an admin or not.

Also add the rails admin route in the **config/routes.rb** file.

{% highlight ruby %}
  mount RailsAdmin::Engine => '/admin', :as => 'rails_admin'
{% endhighlight %}

This should enable the admin to run at /admin, now authenticated and authorized using authlogic.
