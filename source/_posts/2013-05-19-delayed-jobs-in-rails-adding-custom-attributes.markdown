---
author: sunilkumarn
comments: true
date: 2013-05-19 06:18:38+00:00
layout: post
slug: delayed-jobs-in-rails-adding-custom-attributes
title: 'Delayed Jobs in Rails: Adding custom attributes'
wordpress_id: 1051
categories:
- Ruby
---

Ok, so this was my exact scenario. When I was doing a bulk emailing application,  there was the need for the client to upload his set of email ids as a file and then save it to the database. The process of saving these contact mail_ids for a particular mail group was a delayed process, handled by[ Rails delayed job](https://github.com/collectiveidea/delayed_job) . [
](https://github.com/collectiveidea/delayed_job)

{% highlight ruby %}
@mail_group.delay.save_group_contacts
{% endhighlight %}

where @mail_group is the active record group to which the mails_ids being uploaded and saved belong.

The requirement was to show a progress bar for the process of the mail_ids being saved to the the mail group. To handle this, I decided to add custom attributes to the delayed jobs table so as to identify the owner of the delayed job and also find the progress of the job.

To do this,

1) DB migration to add the custom attributes

{% highlight ruby %}
    class AddColumnToDelayedJob < ActiveRecord::Migration
      def change
        add_column :delayed_jobs, :job_process_status, :integer, :default => 0
        add_column :delayed_jobs, :job_owner_id, :integer
        add_column :delayed_jobs, :job_owner_type, :string
      end
    end
{% endhighlight %}

2) A model for the delayed jobs table.

{% highlight ruby %}
    module Delayed
      class Job < ActiveRecord::Base
        self.table_name = "delayed_jobs"
        attr_accessible :job_owner_id, :job_process_status, :job_owner_type
        belongs_to :job_owner, :polymorphic => true
      end
    end
{% endhighlight %}

As seen, three extra attributes (job_owner_id, job_owner_type attributes for establishing a polymorphic association with the job owner of the delayed job and a job_process_status attribute for updating the progress of the job) were added to the delayed jobs table.

Delayed jobs were then created with the job_owner_id and job_owner_type.

{% highlight ruby %}    @mail_group.delay(job_owner_id: @mail_group.id, job_owner_type: @mail_group.class.name).save_group_contacts{% endhighlight %}

However this would not be enough to update the custom attributes. An attempt to create a delayed job would produce this

{% highlight ruby %}
    ActiveModel::MassAssignmentSecurity::Error:
        Can't mass-assign protected attributes: job_owner_id, job_owner_type
{% endhighlight %}

As a quick fix, add a `config/initializers/delayed_job.rb`
and paste in the following code

{% highlight ruby %}
    class Delayed::Job < ActiveRecord::Base
      self.attr_protected if self.to_s == 'Delayed::Backend::ActiveRecord::Job'   #loads protected attributes for                                                                                        # ActiveRecord instance
    end
{% endhighlight %}

Now the delayed job would get saved with the job_owner_id and job_owner_type.

Also, in the mail_group model, set an association to the delayed jobs table.

{% highlight ruby %}
    class MailGroup < ActiveRecord::Base
      has_many :deferred_jobs, :as => :job_owner, :class_name => "::Delayed::Job"
    end
{% endhighlight %}

Now you can access all the delayed jobs of a particular @mail_group as

{% highlight ruby %}    @mail_group.deferred_jobs{% endhighlight %}

The job process status which is updated by the running job can also be accessed as

{% highlight ruby %}    @mail_group.deferred_jobs.job_process_status{% endhighlight %}
