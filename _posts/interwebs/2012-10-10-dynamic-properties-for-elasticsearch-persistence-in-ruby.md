---
layout: post
title:  Dynamic Properties for Elasticsearch Persistence in Ruby
category: interwebs
---

Most people who work on the interwebs probably know that [Elasticsearch](http://www.elasticsearch.org/) makes for a pretty awesome search engine.  What fewer people realise however is that it also makes a rather swanky persistence datastore.  I mean, if you are going to pushing most of your data from MySQL, Postgres, Mongo, or whatever to Elasticsearch for full text searching, then why not get rid of the redundant step, store all your persistent data in one place, and enjoy the transcendent luxury of a schema-less database. 

[Karel Minarik](http://www.karmi.cz/en) has published [Tire](https://github.com/karmi/tire), a Ruby API and DSL for ElasticSearch, that makes search and persistence a breeze.  Just include the gem and you have ActiveModel behaviour in your classes.

    require 'tire'

    class MyModel  
      include Tire::Model::Persistence

      property :id
      property :name
    end
  
MyModel then acts just like ActiveRecord except the data is persisted in Elasticsearch


    MyModel.create :id => 2, :name => 'Inigo Montoya'

    some_model.find 2
    some_model.name   # => 'Inigo Montoya'

    MyModel.tire.search 'inigo'


The only downside to this however, is that you now need to pre-declare all your model properties and thus give up all that schema-less sweetness that makes Elasticsearch so delicious.  Often, I want to instantiate models with a variety of attributes dynamically at run time, so having to hard code _property_ attributes is a non-trivial bummer.  Luckily however, there is a solution.  Just override your model's `initialize` method.


    require 'tire'

    class MyModel  
      include Tire::Model::Persistence

      property :name

      def initialize(attrs={})
        attrs.each do |attr, value|
          # call Tire's property method if it hasn't been set explicitly
          self.class.property attr unless self.class.property_types.keys.include? attr
          # set instance variable
          instance_variable_set("@#{attr}", value) 
        end
        super attrs
      end
    end


Then you can dynamically create a variety of attributes at run time whilst still keeping explicit property mappings.


    swordsman = MyModel.new :name => 'Inigo Montoya', :rage_from => 'Killing of Father', :prepares_for => 'death'

    poison = MyModel.new :name => 'Iocane', :substance => 'powder', :colour => nil, :odour => nil, :taste => nil


Enjoy!