---
layout: post
title:  A beginner's guide to ruby gems
category: interwebs
---

On key factor of Ruby's success as a language - apart from its beautiful expressiveness - is it's package management ecosystem - RubyGems.  Just like Ruby, RubyGems is optimised for developer happiness - it makes the packaging and dependency requirements of distributed software a pleasure.

This post is a brief introduction to using, writing, and sharing gems in Ruby based on a talk I gave to the BrisRuby group this month.  The intended audience is the beginner Rubyist.  The slide deck can be found here.


## What is a Gem?

Gems are self contained chunks of Ruby code - modules, packages, & libraries - that are designed to be shared and used either independent or more frequently, to extend functionality in other code bases.

RubyGems is Ruby's package management system.  Along with `Bundler` and the `Gem` gems, it provides a standardised format for creating, sharing, and reusing ruby gems.  And just like Rails, this consistent set of conventions is what makes it so powerful.


## Using Gems

The two most common ways to use gems is to install them directly or with a dependency manager like [Bundler][bundler].  The direct method installs a gem on your system and is the best option if your gem is a stand alone program or includes a console based API. To install Rails for example, you'd just execute the following one liner:


		gem install rails


The other method is to use a gemfile and bundler.  In this case, you declare all the gems you want to use in the gemfile, along with any group namespaces:


		# Gemfile
		source 'https://rubygems.org'
		gem 'geokit'
		gem 'rack', '~>1.1'

		group :production do
			gem 'pg'
		end

		group :test do
			gem 'sqlite3'
		end


Then launch your program with bundler...


		bundle exec my_cool_app


Two points to note here are that the `pg` and `sqlite3` gems will only be loaded in their respective environments and that `rake` gem also has a version argument.  Versions can be exact or approximately greater than so `~> 2.0.1` means equal to or greater than in the last digit, ie `2.0.x`.


## Building a gem

Before you build and share a gem, it's a good idea to make sure the functionality your are producing doesn't already exist.  Search for the behaviour on the [RubyGems][rg] website and [The Ruby Toolbox][rt] to prevent needless duplication.

If you didn't find anything there, then think up snappy name that preferably contains some word-play, innuendo, or double entendre - something like [texticle](http://texticle.github.io/texticle/), [clitt]( https://github.com/francois/clitt), or [hash_dealer](https://github.com/LifebookerInc/hash_dealer).  If you are still struggling for a name, then just choose some random page on reddit or 4chan and name your gem what ever the 17th word is.

The gem we are going to build today is possibly the most useless gem every created.  And given the dubiuous quality surrounding it, and what it actually does, the only appropriate name for it is `ify`.

Bundler makes building a gem effortless so fire up terminal and type:


		bundle gem awesomify


Bundle has created a gem skeleton for us that we now need to populate.  The most important files are be the README and the gemspec.


      create  ify/Gemfile
      create  ify/Rakefile
      create  ify/LICENSE.txt
      create  ify/README.md
      create  ify/.gitignore
      create  ify/ify.gemspec
      create  ify/lib/ify.rb
      create  ify/lib/ify/version.rb


Every good open source project has a compressive README with a [clear and unambitious description](http://github.com/davekinkead/ify) of what the project does and how it does it.  Ify is no exception - except of cause that it is not a good open source project.



---


Now it's time to code the awesome sauce.


Unit tests


Behaviour


Iterate



		gem build awesomify.gemspec

		gem push awesomify-0.0.0.gemspec



[bundler]: http://bundler.io/
[rt]: https://www.ruby-toolbox.com/
[rb]: https://rubygems.org/
[texticle]: http://texticle.github.io/texticle/
[clitt]: https://github.com/francois/clitt