---
layout: post
title:  "Jobeet Rails : Adding assets"
date:   2017-12-19 12:00:00 +0200
---

## Adding assets

Download assets :
- Images to put in `app/assets/images`: [images.zip]({{ site.url }}/downloads/jobeet/assets/images.zip)
- The application stylesheets to put in `app/assets/stylesheets` : [stylesheets.zip]({{ site.url }}/downloads/jobeet/assets/stylesheets.zip)

Delete `application.css` :
{% highlight bash %}
rm app/assets/stylesheets/application.css
{% endhighlight %}

Add favicon link tag :
`app/views/layouts/application.html.haml`:
{% highlight haml %}
    ...
    = favicon_link_tag
  %body
    ...  
{% endhighlight %}

## Use the new layout

{% highlight haml %}
!!!
%html
  %head
    %title Jobeet - Your best job board
    = csrf_meta_tags
    = stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload'
    = javascript_include_tag 'application', 'data-turbolinks-track': 'reload'
    = favicon_link_tag
  %body

    #container
      #header
        .content
          %h1
            = link_to root_path do
              = image_tag "logo.jpg", alt: "Jobeet Job Board"

          #sub_header
            .post
              %h2 Ask for people
              %div
                = link_to "Post a Job", ""

            .search
              %h2 Ask for a job
              %form{ action:"", method: "get" }
                %input{ type: "text", name: "keywords", id: "search_keywords" }
                %input{ type: "submit", value: "search" }
                .help
                  Enter some keywords (city, country, position, ...)

      #content
        - if notice
          .flash-notice
            = notice

        - if alert
          .flash-error
            = alert

        .content
          = yield

      #footer
        .content
          %span.symfony
            = image_tag "jobeet-mini.png"
            powered by
            = link_to "http://rubyonrails.org/" do
              = image_tag "rubyonrails.png", alt: "Ruby On Rails"

          %ul
            %li
              %a{ href: "/" } About Jobeet
            %li.feed
              %a{ href: "" } Full feed
            %li
              %a{ href: "" } Jobeet API
            %li.last
              %a{ href: "" } Affiliates

{% endhighlight %}

## Links
