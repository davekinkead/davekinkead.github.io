---
layout: post
title:  Why RailsGirls should ditch Rails
category: thoughts
---

[RailsGirls](http://railsgirls.com/) is fantastic.  Starting a few years ago in Finland as a fun and friendly introduction to IT for women, it's been the catalyst for a number of tech careers.  It's like the best bits of Ruby and open source culture distilled into one (well catered) day and night.

Last weekend I attended my third [RailsGirls](http://railsgirls.com/brisbane) event as a mentor and what I loved most about this one was that it was organised by RailsGirls alumnae themselves.  It was so cool to see women who got their first taste of Ruby in 2013 back to introduce it to a new cohort. The pay-it-forward attitude that is at the heart of open source was alive and well, making the event a pleasure to be a part of.

But I'm still left with one niggling doubt...

Everyone knows Rails is magic.  Remember DHH's first screencast - ["Look at all the things I'm not doing"](https://www.youtube.com/watch?v=Gzj723LkRJY&t=4m15s) - who wasn't seduced by that simplicity?  Rails does so much out of the box and that's one of the reasons why it is so good for web development and rapid prototyping.

Of course that's also one of the major obstacles to learning Rails.  For beginners, Rails is too magical.  The tool chain is just too complex for a single day introduction.  Have a quick look at the [build an app guide](http://guides.railsgirls.com/app/) - even without factoring in the installation steps, the vast majority of actual 'coding' involves nothing more than running generator commands.  Now generators are a great way to scaffold out an app but they make a horrible tool for learning.

Houston, we have a problem.

If students spend most of their time doing things outside of Ruby and Rails, then they aren't learning Ruby on Rails.  If they spend most of their time running build tools, they lose the opportunity to learn what a web app actually does.  Because they aren't specifying routes, writing controllers, defining models, and crafting views, they don't have the chance to learn what everything does and how everything connects together.

And that's a real shame because the true value of RailsGirls in my view, is that it provides an opportunity for people to viscerally experience the joy of crafting something themselves.  No matter how small or trivial that thing may be, shipping something you built yourself is empowering.  You get to build something with your own hands (or fingertips at least), see it emerge from text on screen, and turn into your very own web app.  But that feeling of empowerment can only come about when you actually understand what you are doing.  Rails, at least in the beginning, is an obstacle to understanding.

So what do we do about it?  I hate to say it but RailsGirls should ditch Rails.  While Rails is awesome, it doesn't make a good learning environment for a 1.5 day introduction to programming.  You need to spend too long learning the about periphery before you can learn about the core.

One great alternative would be [Sinatra](http://www.sinatrarb.com/). Sure, you wont write any non-trivial production app with it (even though you can), but it makes a perfect learning environment to understand how all the pieces fit together.  Just look at their readme and it's clear how good Sinatra is for programming pedagogy.


    require 'sinatra'

    get '/hello' do
      "<h1>Hello, RailsGirls!</h1>"
    end


In four lines of code, we have the essences of a web app - requests and request methods, routes, responses, and HTML.  Add more functionality and it becomes immediately clear how your app interacts with a database and about why modularity and DRY is so important.  Add even more functionality and you understand why Rails really is such an awesome framework, and why you'll turn to it for just about all your CRUDy needs.

Until you do something manually, you won't understand what any of the automation does.  And understanding is the path to empowerment.