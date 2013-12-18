---
layout: post
title: "Heroku deployment with MongoDB database"
date: 2013-12-18 19:01
comments: true
categories: 
---

Currently, I develop a Rails app with [mongodb](http://www.mongodb.org/) as the database.
When I decided to deploy this in heroku, as per the heroku dev doc, I was asked to simply install the MongoHQ Add-on, and specify the plan that I desire.

So, supposedly all I had to do was this,

{% highlight bash %}
heroku addons:add mongohq:small
{% endhighlight %}
and I would have the MONGOHQ_URL to add to the mongoid configuration ( config/mongoid.yml ).
But unfortunately, this would ask me to verify my heroku account
{% highlight bash %}
Adding mongolab on chillrapi... failed
 !    Please verify your account to install this add-on
 !    For more information, see http://devcenter.heroku.com/categories/billing
 !    Verify now at https://heroku.com/verify
{% endhighlight %}

Clicking on the verification link asks for my credit card details. Now, its mentioned that mongohq-small is a free version of the add on. So you could probably carry on providing your details. For the others, you may please read on.

MongoHQ is a cloud-based hosted database solution that allows developers to easily deploy, manage and scale both single and replica set MongoDB databases for their web and mobile applications. I created a new database (SandBox, 512 MB -  thats all I needed anyways ) at MongoHQ.

On creating a new database, MongoHQ provides us with a mongo console and mongo uri connection strings.

{% highlight bash %}
Mongo Console
mongo paulo.mongohq.com:10097/chillr_db -u <user> -p<password>
{% endhighlight %}

{% highlight bash %}
Mongo URI
mongodb://<user>:<password>@paulo.mongohq.com:10097/chillr_db
{% endhighlight %}

Add the database user with a username, password. Check [mongohq support docs](http://support.mongohq.com/common-questions/debugging-connection-issues.html) for reference.

You could try out the mongo console connection string from your local to check if the user credentials are valid. Once you find them valid, add the MONGOHQ_URL to the heroku config set.

{% highlight bash %}
heroku config:set MONGOHQ_URL="mongodb://<user>:<password>@paulo.mongohq.com:10097/chillr_db"
{% endhighlight %}

Be sure to replace the username and password while adding.
Thats it, Mongodb hosted in MongoHQ should be now available for use in the application.
