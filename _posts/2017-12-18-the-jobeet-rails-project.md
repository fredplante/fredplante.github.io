---
layout: post
title:  "The Jobeet On Rails project"
date:   2017-12-18 12:00:00 +0200
---


## Create the project

First we create the project. We use postgresql. Also we don't need action cable, and are not
going to write in coffeescript.

{% highlight bash %}
rails new jobeet -d postgresql --skip-action-cable --skip-coffee
{% endhighlight %}

The we add the files in git and commit them.

{% highlight bash %}
git add .
git commit -m "initial commit"
{% endhighlight %}

## Use HAML

Add to Gemfile
{% highlight ruby %}
...
gem 'haml-rails', '~> 1.0'
{% endhighlight %}

Run bundler
{% highlight bash %}
bundle install
{% endhighlight %}

Rename `app/views/layouts/application.html.erb` to `app/views/layouts/application.html.haml`
and replace content :

{% highlight haml %}
!!!
%html
  %head
    %title Jobeet On Rails
    = csrf_meta_tags
    = stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload'
    = javascript_include_tag 'application', 'data-turbolinks-track': 'reload'

  %body
    = yield
{% endhighlight %}

Do the same for mailers views

`app/views/layouts/mailer.html.haml` :
{% highlight haml %}
!!!
%html
  %head
    %meta( http-equiv="Content-Type" content="text/html; charset=utf-8" )
    %style
      / Email styles need to be inline

  %body
    = yield
{% endhighlight %}

`app/views/layouts/mailer.text.haml`
{% highlight haml %}
= yield
{% endhighlight %}

{% highlight bash %}
git add .
git commit -m "Add haml support"
{% endhighlight %}

## Add a default home controller

{% highlight bash %}
rails g controller home index --no-javascripts --no-stylesheets --no-helper
{% endhighlight %}

Edit `config/routes.rb` :

{% highlight ruby %}
Rails.application.routes.draw do
  root to: "home#index"
end
{% endhighlight %}

Edit `app/views/home/index.html.haml` :
{% highlight haml %}
%h1 Jobeet On Rails
{% endhighlight %}

Commit :
{% highlight bash %}
git add .
git commit -m "Add home controller"
{% endhighlight %}

We add the github remote and push on it
{% highlight bash %}
git remote add origin git@github.com:fredplante/jobeet.git
git push -u origin master
{% endhighlight %}

We set the ruby version. `.ruby-version` for rvm, and in the Gemfile to specify to heroku
the ruby version we want to use

{% highlight bash %}
echo 2.4.2 > .ruby-version
echo ruby \'2.4.2\' >> Gemfile
git add .
git commit -m "specify ruby version"
{% endhighlight %}

## Deploy on heroku

Install the heroku toolbelt

{% highlight bash %}
wget -qO- https://cli-assets.heroku.com/install-ubuntu.sh | sh
{% endhighlight %}

Login to heroku

{% highlight bash %}
heroku login
{% endhighlight %}

Create your app
{% highlight bash %}
heroku create
{% endhighlight %}

Deploy to heroku
{% highlight bash %}
git push heroku master
{% endhighlight %}

View in browser
{% highlight bash %}
heroku apps:open
{% endhighlight %}

## Links
