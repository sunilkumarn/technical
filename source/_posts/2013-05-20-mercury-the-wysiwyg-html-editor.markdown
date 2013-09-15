---
author: sunilkumarn
comments: true
date: 2013-05-20 19:23:48+00:00
layout: post
slug: mercury-the-wysiwyg-html-editor
title: 'Mercury: The WYSIWYG html editor'
wordpress_id: 1106
categories:
- Ruby
---

I had this application where different users would want to edit custom html pages to be shown up in there web sites. Each user will have his own domain( all domains pointing to the same Rails application ) and the custom html page had to be loaded as per the current domain. To do this, in search of a WYSIWYG html editor which is easy to setup and simple to start off, I ended up in [Mercury](http://jejacks0n.github.io/mercury/). Whats really nice was that Mercury also had a [gem](https://github.com/balupton/mercury-rails) to be used for Rails developer and as I am one, I had no more hesitation in get started with mercury.

To get started off with mercury, add

{% highlight ruby %}

gem 'mercury-rails'

{% endhighlight %}

to the Gemfile and bundle it.

Run the rails generator for the mercury files.

{% highlight ruby %}

rails g mercury:install

{% endhighlight %}

A couple of questions will be posted. Press 'yes' to install the layout files.

Now checking out the directory structure,  you could see three additional files.

**mercury.js** and** mercury.css** in the js and stylesheets assets respectively. Also, a new layout file for the mercury editor, **mercury.html.erb **

I did remove the mercury css file later on.

One thing that needs to be noticed here is that the mercury.js file is heavy and it woudn't be a good idea to load it in all the pages. We would want to load it in only the pages that needs to be edited. Checkout the mercury layout file and you can see that the mercury.js file is included.

{% highlight ruby %}
    <head>
        <meta name="viewport" content="width=device-width, maximum-scale=1.0, initial-scale=1.0">
        <%= csrf_meta_tags %>
        <title>Mercury Editor</title>
        <%= stylesheet_link_tag 'mercury' %>
        <%= javascript_include_tag 'jquery-1.7', 'mercury' %>
    </head>
{% endhighlight %}

Now to prevent mercury.js from being loaded up in the pages, we could move all the other js files in our application to a separete directory and then require the directory in our application.js

My application.js will have,

{% highlight ruby %}
//= require_tree ./main
{% endhighlight %}

where main is the directory which has all the application specific javascript. (Probably could be a better name :) )

Now peep into the routes file, you could see this extra line,

{% highlight ruby %}

mount Mercury::Engine => '/'

{% endhighlight %}

What this line does is that it allows the html pages in your application to be edited. An extra '/editor'  will have to be added at the beginning of each url path to load the mercury editor for the page.

Consider you have the url '**localhost:3000/pages**' , all you need to load it in the mercury layout is to change it to '**'localhost:3000/editor/pages**' . You have mercury loaded up to edit your page and can now see it in the mercury editor's layout.

[![Screenshot](http://sunilkumarn.files.wordpress.com/2013/05/screenshot.png?w=570)](http://sunilkumarn.files.wordpress.com/2013/05/screenshot.png)

However this isn't just enough to start editing the page. You need to specify editable regions in the page.
In `pages.html.erb `

{% highlight ruby %}
    <div class="control-group">
        <h3 class="section_header page-header">Pricing page</h3>
        <div id="faq" class="mercury-region" data-type="editable" data-mercury="full">
            <%= render :file => file_path(@domain 'faq') %>
        </div>
    </div>
{% endhighlight %}

Consider this piece of code. A div with id="faq" is made editable with** class="mercury-region" **and attributes** data-type="editable" **and** data-mercury="full"**.

Now you can see the editable region.

[![Screenshot-1](http://sunilkumarn.files.wordpress.com/2013/05/screenshot-1.png?w=570)](http://sunilkumarn.files.wordpress.com/2013/05/screenshot-1.png)

This following line in above piece of code

{% highlight ruby %}
<%= render :file => file_path(@domain, 'faq') %>
{% endhighlight %}

invokes a helper method and loads the already created sample faq template which can now be edited and saved for the particular domain. As simple as that.

Similarly you could edit more pages here. This is how the contacts page can be edited.

{% highlight ruby %}
    <div class="control-group">
        <h3 class="section_header page-header">Contact page</h3>
        <div id="contact" class="mercury-region" data-type="editable" data-mercury="full">
            <%= render :file => file_path(@domain, 'contact') %>
        </div>
    </div>
{% endhighlight %}

Also, you probably might want to change the save url of the mercury editor for the particular page. That is the controller action to which the mercury edited contents will be 'POST' or 'PUT' (depends on the configuration set in the mercury.html.erb)

To change the mercury save url for this particular page, I wrote the script in the erb file ( pages.html.erb )

{% highlight ruby %}
    <script>
        $(window).on('mercury:ready', function () {
            Mercury.saveUrl = "<%= pages_upload_admin_domain_path(@domain) %>";
        });
    </script>
{% endhighlight %}

You might also want to change the page thats to be redirected to once we are done with editing using mercury. We could bind on mercury's save event to get this done.

{% highlight ruby %}
    $(window).bind('mercury:saved', function() {
        $(window.location.replace('/admin/domain'));
    });
{% endhighlight %}

All this saved data would have to be dealt with in the controller action. Inspecting the params in the controller action ( the mercury Save url) ,

    
    <code>{"content"=>
        {"faq"=>
            {"type"=>"full",
             "data"=>{},
             "value"=> "<h1>This is where I have done my FAQ editing</h1>"
             "snippets" => {}
            }     
        },
    
        {"contact"=>
            {"type"=>"full",
             "data"=>{},
             "value"=> "<h1>This is where I have done my Contacts editing</h1>"
             "snippets" => {}
            }
        }
    }
    
    </code>


There are two things of notice here. The contents hash contains all the mercury related stuff.  Each hash in the contents hash has a key which is equal to the id of the mercury editable html divisions ( see the view code pasted above ), here '**faq**' and '**contact**'. The actual edited html content can be found in the hash with key 'value' ( **<h1>This is where I have done my Contacts editing</h1>**).  'The controller action could decide on how to save this html content.

_What have I done to solve my case mentioned at the starting?_

__I created a pages directory in my public. Within the pages directory I created sub directories which corresponds to the domain. _**For eg, the domain localhost corresponds to the directory named localhost inside the public/pages directory and the domain remotehost corresponds to the remotehost directory**_.

I then saved all these edited html content as html files within these domain specific directories. When a particular domain was loaded, the html pages ( for eg, faq and contact) was rendered from the corresponding domain directories in the public folder .
