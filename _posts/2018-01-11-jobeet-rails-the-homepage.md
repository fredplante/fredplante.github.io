---
layout: post
title:  "Jobeet Rails : The homepage"
date:   2018-01-11 12:00:00 +0200
---

## The homepage spec

The following spec is directly taken from the orignal jobeet tutorial
> Story F1: On the homepage, the user sees the latest active jobs
>
> When a user comes to the Jobeet website, he sees a list of active jobs. The
jobs are sorted by category and then by publication date (newer jobs first).
For each job, only the location, the position, and the company are displayed.
>
>For each category, the list only shows the first 10 jobs and a link allows to list all the jobs for a given category (Story F2).
>
> On the homepage, the user can refine the job list (Story F3), or post a new job (Story F5).

![F1 Mockup]({{ "assets/jobeet/mockup_homepage.png" | absolute_url }})
## Category model

{% highlight bash %}
bin/rails g model category name
{% endhighlight %}

Edit generated migration `app/db/migrate/YYYYMMDDHHMMSS_create_categories.rb` :

{% highlight ruby %}
class CreateCategories < ActiveRecord::Migration[5.1]
  def change
    create_table :categories do |t|
      t.string :name, null: false

      t.timestamps
    end

    add_index :categories, :name, unique: true
  end
end
{% endhighlight %}

## Job model

{% highlight bash %}
bin/rails g model job category:references position location company active:boolean published_at:datetime
{% endhighlight %}

## Add model relations

A `Job` belongs to a `Category`. As we specified the relation to the generator
with `category:references` the relation is already here, in `app/models/category.rb` :

{% highlight ruby %}
class Job < ApplicationRecord
  belongs_to :category
end
{% endhighlight %}

A `Category` has many `Job`s. We add it in the model, `app/models/job.rb` :

{% highlight ruby %}
class Category < ApplicationRecord
  has_many :jobs
end
{% endhighlight %}

## The homepage view

We first write a very basic view, edit `app/views/index.html.haml` :

{% highlight haml %}
#jobs
  - Category.all.each do |category|

    %div{ id: dom_id(category) }
      .category
        .feed
          %a{ href: "" } Feed
        %h1= category.name

      %table.jobs
        - category.jobs.where(active: true).each do |job|
          %tr{ id: dom_id(job), class: cycle("odd", "even") }
            %td.location= job.location
            %td.position= link_to job.position, "#"
            %td.company= job.company
{% endhighlight %}

## Fixtures

Edit fixtures : `test/fixtures/jobs.yml` :

{% highlight yaml %}
designer_active:
  category:     design
  position:     Web Designer
  location:     Paris, France
  company:      Sensio Labs
  active:       true
  published_at: <%= 2.weeks.from_now.to_s(:db) %>

designer_inactive:
  category:     design
  position:     Web Designer
  location:     Paris, France
  company:      Sensio Labs
  active:       false
  published_at: <%= 3.weeks.from_now.to_s(:db) %>

developer:
  category:     programming
  position:     Web Developer
  location:     Paris, France
  company:      Sensio Labs
  active:       true
  published_at: <%= 1.weeks.from_now.to_s(:db) %>

tester:
  category:     programming
  position:     Tester
  location:     Paris, France
  company:      Sensio Labs
  active:       true
  published_at: <%= 2.weeks.from_now.to_s(:db) %>

{% endhighlight %}

Edit fixtures : `test/fixtures/categories.yml` :

{% highlight yaml %}
design:
  name: Design

programming:
  name: Programming
{% endhighlight %}

Now we can load fixtures and see the result in our browser :

{% highlight bash %}
bin/rails db:fixtures:load
{% endhighlight %}

![F1 Screenshot]({{ "assets/jobeet/screenshot_1.png" | absolute_url }})

## A first test

We need to write a test that check the defined spec is implemented

{% highlight ruby %}
require 'test_helper'

class HomeControllerTest < ActionDispatch::IntegrationTest

  test "should get index" do
    get home_url
    assert_response :success
    assert_select ".category", 2
    assert_select "#category_#{categories(:design).id} tr", 1
    assert_select "#category_#{categories(:programming).id} tr", 2
  end

end
{% endhighlight %}


## Links
