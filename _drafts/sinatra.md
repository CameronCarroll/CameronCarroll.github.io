---
layout: default
title: How do I Sinatra.rb?
category: Ruby
month_year: February 2015
---

I forgot all about how to do a web application. So lets do a quick review.

## GET and POST
### Code:
{% highlight ruby %}
require 'sinatra'

get '/' do
  'hi'
end

post '/input' do
  params.to_s
end
{% endhighlight %}

### CURL:
{% highlight bash %}
$ curl localhost:4567
hi%
$ curl --data "hey" localhost:4567/input
{"hey"=>nil}%  
{% endhighlight %}

### Discussion:
It's simple, really. We can GET a route and provide it with the HTML text, and we can POST to a route and whatever we post shows up in params. All it needs is sugar!

## A Quick Skin
I wasted no time in throwing Bootstrap into my project, via CDN since I'm lazy. After changing my raw return string to a erb :index, adding a views directory, views/index.erb and views/layout.erb, we're ready to apply Bootstrap to the layout. A <%= yield %> brings in index.erb's content, and we're in business.

I added a jumbotron and a login shim and played around with the submission. Bootstrap form example needs the method and action added, as well as names added to the fields for anything to actually show up in the submitted params.
