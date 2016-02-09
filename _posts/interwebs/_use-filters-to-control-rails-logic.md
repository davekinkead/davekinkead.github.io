---
layout: post
title: Refactor your Rails Controllers with filters
category: thoughts
date: 8 Feb 2016
---

Get your controller logic out of methods in into filters

You're keeping keeping your Controllers thin.

- methods should do as little as possible
- this allows you to re-use methods and chain them together - DRY!
- but Rails can be quirky
- example of conditional redirect_to
- how rails controllers execute
- how filters work
- conclusion


    class PoliteController < ApplicationController
      def moar_cookies
        teach_the_kiddies_some_manners!
        @cookies = Confectionary.find_by_taste_and_shape :choc_chip, :round
      end

      def naughty_corner
        @demeanor = :sour
      end

      private

      def teach_the_kiddies_some_manners!
        if params.values.include? :please_mummy
          @demeanor = :happy
        else
          redirect_to :naughty_corner_path
        end
      end
    end


    class PoliteController < ApplicationController
      before_action :teach_the_kiddies_some_manners!
      def moar_cookies
        teach_the_kiddies_some_manners
        @cookies = Confectionary.find_by_taste_and_shape :choc_chip, :round
      end

      def naughty_corner
        @demeanor = :sour
      end

      private

      def teach_the_kiddies_some_manners!
        if params.values.include? :please_mummy
          @demeanor = :happy
        else
          redirect_to :naughty_corner_path
        end
      end
    end