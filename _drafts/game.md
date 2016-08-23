---
layout: default
title: Notes on Game Programming
category: Ruby
---

Setting off on my own, 8/1/16
-----------------------
I was following a really great book on making a game in Ruby, and despite banging out every line of code myself I really have no idea how to get started making what I want. Every little piece seems so daunting and so unfamiliar. I just want to have something to show for my good intentions, already! :)

I guess the first problem I need to tackle is mouse control of the screen. The Tanks prototype used the mouse for aiming the tank cannon, but in SG we want to use the mouse for control of our hero, and also let the camera be perfectly free rather than tracking the tank.

So I'm going to start by making the menu clickable. How? Well I'm thinking we do a pixel coordinate system and use the button_down? method to check whether we MsClicked inside of where a button should be. Then we activate that button! :) Easy enough, right?

I found some code for a menu class system, where you add MenuItems to a Menu.
MenuItems come with image, x/y location, callback, and a hover image.
We check for whether the mouse is hovering, and display hover image slightly offset. When the mouse is clicked, and we happen to be hovering inside of that image, we do callback. Callback is a lambda, which when passed into MenuItem is activated with .call

Lets try again. 8/19/16
-------------------------------------
Ok, I got distracted by other things, but what's really important in life?
SG.
Working on the menu. I was still using code from the game book that used relative positioning for menu items, ie placing the title text at half window width minus half image width --> perfectly centered. Then I added a few offsets to allow for things to move around. I'm not sure if this is better than absoute x/y positioning, but it seems like an okay approach for the menu at least.

I'm creating a Label class from which I'll derive a button, and maybe also the Textbox. Its kind of silly that I have to create my own GUI elements, but mildly interesting as well?

Ok, so a label class is in place instead of manually drawing all the elements. I want to create a text box next. In Gosu, $window.text_input is set to an instance of the Gosu::TextInput class. Then that TextInput instance handles all keyboard input until $window.text_input is set to nil.
So we draw a rectangle for our textbox, and check those boundaries for a click, then activate the TextInput. On update, we regenerate text image of whatever the TextInput contains, and draw it inside the rectangle.

I made a progress! 8/20/16
-------------------------------------
Hah! I'm on my way to the literal worst GUI ever! I have labels and a grey square that can detect mouseover, which hopefully will evolve into a text box.

Textboxes do a thing! 8/21/16
------------------------------------
So my grey squares got themselves a nice little border on mouseover (actually just another square drawn first, behind it.) Also they capture click + mouseover and set their TextInput instance to $window.text_input, allowing that box to capture all keyboard input until you click somewhere else.

I noticed that Crater44 has been posting recently and is making progress towards their remake of the game. There are also a half dozen other projects of similar caliber to mine, where individuals moved toward trying to build something resembling the game, but it seems the others also started on a login/title screen and only got about that far before getting distracted.

I waited too long and my Discord invitation expired, but I'm thinking I should really reach out to them and join forces. But I'll be so busy in a week here that I'll most likely forget all about this.
