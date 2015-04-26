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

## Authentication
Next I shoved warden in there, using a simple username/password scheme. This was the obvious next step because I could just follow someone else's implementation.
I added a couple pages and fleshed out the system so that you can create an account and log in, and also simple login-based authorization. (No roles or anything.)

## Next steps
I've been wheeling around for a while trying to figure out the next steps. I know what I want... I want to be able to basically define a time-series database. I guess. Like I want the average user to be able to decide to track something, set up the numbers they want to track, and then make it easy for them to put that in.

Example: Exercising. You should be able to put in data for today, but also data for an arbitrary day if that's what they wanna do. I want to know how many pushups I did, how many situps, squats, curls, and pushing-your-arm-up-with-a-weight-things I did. Those are all just integers! EZ. So they should go to homepage, they use this everyday so bang! they're already logged in and sent to their dash. This shows two things: On one side, a list of your series-datasets whatever they're gonna be called, and on the other a couple utility links, to create a new series, account info, logout.
So this guy wants to create a new series, he goes there. We should offer a dropdown box to pick how many fields they want, maxed out at like 15 or something I guess?
Just because things will be too much for the page. I don't know that's real arbitrary. He picks 6, because he thinks that's probably how many things he wanted to keep track of but can't be bothered to go count them. Six lines appear where one stood before, each with another dropdown (datatype: numeric/text) and a name for that field.
So he puts down those things, pushups, situps, squats, curls, pushing-your-arm-up-with-a-weight things, and decides to add in running time to fill up the sixth. But then he realizes that running time is pretty arbitrary, so he adds another field 'vigor' and clicks the 'add dependency' button off to the right of 'running time' (where did that come from?) and then clicks on vigor and it automagically should select it. I can do that, right? So now the data for running time should also be sortable by vigor.
Great, so then he creates that series and is woofed off to its data-input page. Here he should be able to add arbitrary new values into that series, but there should also be a box containing the last couple values. He didn't do anything today but wants there to be a baseline, so he puts in 'zero' for everthing, hits submit, and is woofed off to the dash again.
