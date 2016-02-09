---
layout: post
title: Refactoring Rails Controllers with Filters
category: thoughts
date: 8 Feb 2016
comments: true
---

You're following the maxims laid down by the software gods.  You're keeping your controllers thin and you [Don't Repeat Yourself](https://en.wikipedia.org/wiki/Don't_repeat_yourself).  But your application grows and you have to add more logic.  All of a sudden, your methods are doing a lot more than one thing.  It's time to refactor.  But how exactly?

Let's start with a simple controller that looks up a delicious snack:


    class CookieController < ApplicationController
      def moar_cookies
        @cookies = Confectionary.find_by_taste_and_shape :choc_chip, :round
      end
    end


So far, so yummy. But our requirements change and we need to ensure that the request is polite.  "No problems" you say...


    class CookieController < ApplicationController
      def moar_cookies
        if params.values.include? :please_mummy
          @cookies = Confectionary.find_by_taste_and_shape :choc_chip, :round
        else
          redirect_to naughty_corner_path
        end
      end
    end


That's fine - until your other methods need to be polite as well.  "No problem", you say again, "I'll just refactor that logic into it's own method!"


    class CookieController < ApplicationController
      def moar_cookies
        teach_the_kiddies_some_manners!
        @cookies = Confectionary.find_by_taste_and_shape :choc_chip, :round
      end

      private

      def teach_the_kiddies_some_manners!
        unless params.values.include? :please_mummy
          redirect_to naughty_corner_path
        end
      end        
    end


**Houston, we have a problem!**  While this code looks like it should work, it is still serving up delicious choc chip snacks to all those naughty boys and girls who fail to ask nicely.  The redirect to the naughty corner is ignored.  But why?

The answer lies in how Rails executes controller code.  While `render` can be called anywhere in a controller, `redirect_to` needs to be the last expression executed in the class.  One hack is to simply append `&& return` to the expression:


        unless params.values.include? :please_mummy
          redirect_to naughty_corner_path && return
        end


Yet this too will fail because we've extracted the well-mannered logic into its own method - the `&& return` just yields to the `moar_cookies` method.

Rails offers a much more elegant solution to this problem however - [filters](http://guides.rubyonrails.org/action_controller_overview.html#filters).  A `before_filter` permits the request cycle to be halted by preempting the normal request logic in your controllers.  We can use filters to DRY up our controller code:


    class CookieController < ApplicationController
      before_action :teach_the_kiddies_some_manners!

      def moar_cookies
        @cookies = Confectionary.find_by_taste_and_shape :choc_chip, :round
      end

      private

      def teach_the_kiddies_some_manners!
        unless params.values.include? :please_mummy
          redirect_to naughty_corner_path
        end
      end        
    end


Better yet, by defining the `teach_the_kiddies_some_manners!` method in `ApplicationController` but adding the filter in `CookiesController`, we can optionally get the same behavour in every other child class.

There you go - DRY code, moist cookies.  Now I just need a glass of milk.