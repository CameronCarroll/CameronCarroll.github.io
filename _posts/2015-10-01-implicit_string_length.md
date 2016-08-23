---
title: I only tested trivial inputs (and strings have a length limit)
category: programming
layout: back
month_year: Fall 2015
---

# Background
I'm writing a teeny microservice which holds on to messages and their timestamps until I ask for them back. (Note Queue) At the time, I was more interested in the function than the form and hacked my three components together without sanitation or error handling, let alone automated testing. It was a really simple concept (create resource on a POST, return and clear server with a DELETE) so I gave it a couple manual runthroughs with trivial inputs and iterated it into working order.

Pretty soon I have it running on Heroku and after a couple more trivial tests I declared it a "good enough for me" success and walked away. A few days later my interest returns, but the output file is and shouldn't be empty, and I had written substantial messages that were now lost in the nether.

# Solution?
Due to the fact that trivial messages worked fine and longer messages didn't, the obvious answer is that there's a length limit somewhere and things are being dropped. But when I tried 256, 255, 240-something length inputs, they were all dropped. A little googling reveals that DataMapper implements a 50-character limit by default. After thinking about what's really going on (and some googling Postgres string length limits before) I realized that a varchar always has to be declared with a maximum length. But I really wanted to send arbitrary text to this application.

# Back to where I started
I actually picked a Text datatype for my message field in the first place, and it looks like I'm going to have to come back to it. I switched to a String because the Text kept returning <Not Loaded> for its value. This is because Text is among data types that DM will return lazily, but can be turned off with :lazy => false in the attribute definition.

# So I pushed to heroku and wrote a big long POST
Guess what? Yeah, that's right, lost to the nether. A short message works fine, as usual, but anything longer than 50 characters returns "<h1> Internal server error </h1>" and "heroku logs" indicate that "DataObjects::DataError - ERROR:  value too long for type character varying (50)"

Dang, I should really have tested it before spending 10 minutes writing a journal entry. At least it's out of my system, I guess. There you go, netherworld.

So I try a heroku pg:reset, after which even short messages return "<h1> Internal server error </h1>" and "heroku logs" indicate "DataObjects::SyntaxError - ERROR:  relation "entries" does not exist"

Whoops! I forgot to actually restart the server (and thus re-run the database migration/upgrade code.) A quick heroku restart and...

Hot dang! Everything appears to be working as intended. An obnoxiously long message went thorugh all three pieces of the process successfully, so I'm going to declare this "good enough really really this time."
