---
layout: post
title: There are no class methods in Ruby
category: interwebs
date: 1 Feb 2016
comments: true
---

I've seen a lot of questions about class methods in Ruby lately. _What's the difference between class and instance methods?_ _When should I use them?_ And my favourite - [_are class methods evil?_](https://www.google.at/search?q=are+class+methods+evil+ruby) There is a lot of conflicting information out there on the interwebs but luckly there is a simple answer for almost every question about class methods Ruby.

**There are no class methods in Ruby!**

Here's a typical enterprise scenario:


    class ChunkyBacon
      def initialize(flavour)
        @flavour = flavour
      end

      def tastes_like
        "Chunky bacon tastes like #{@flavour}"
      end

      def self.invent(flavour)
        ChunkyBacon.new(flavour)
      end
    end


We've defined a class `ChunkyBacon` with three methods.  Two of them, `:initialize` and `:tastes_like` are instance methods while `:invent` is a class method. No. Stop! Wait... **There are no class methods in Ruby.**

"But but.." you protest, "...I see it right there! `def self.invent(flavour) ... end`."

Yes, yes I see it too but that ain't no class method.  Let me explain....

Methods always have receivers [^unbound] - the objects that they belong to.  When we define a method, we also specify its receiver either implicitly or explicitly.  But everything in Ruby is an object.  That means classes are objects too.  

[^unbound]: What? You think I forget `UnboundMethod`.  Just try `unbound_method.is_a? Method`

When defined inside a class declaration, `def method_name` implicitly sets the receiver as any instance of that class.  That's why we can successfully send `:tastes_like` to an instance of `ChunkyBacon` but not `ChunkyBacon` itself.


    ChunkyBacon.respond_to? :tastes_like
    # => false

    ChunkyBacon.new('pistachio').respond_to? :tastes_like
    # => true


The last method `:invent` however is defined explicitly on `self`.  

"Yes, I know, it's a class method" you interject. 

No, my bacon loving friend, it is not.  **There are no class methods in Ruby.**  Don't beleive me?


    Object.methods.grep /methods/
    # => [:public_instance_methods, :instance_methods, :private_instance_methods, :protected_instance_methods, :methods, :protected_methods, :singleton_methods, :public_methods, :private_methods]


Ruby has instance methods, singleton methods, public, protected, and private methods.  **But there are no _class_ methods in Ruby** [^class].  Take away visibility and we are left with just instance and singleton methods. 


[^class]: `Module` does have the `:public_class_method` and `:private_class_method` methods, but if you look at the C code underneath, these methods are actually referencing `rb_singleton_class`.


"But but..", you protest again. "The ChunkyBacon class has an `:invent` method that the ChunkyBacon instance doesn't. That's a class method! Look!"

    
    ChunkyBacon.methods.sort
    # => [... :extend, :invent, :freeze, ...]

    ChunkyBacon.invent('strawberry').methods.sort
    # => [... :extend, :flavour, :freeze, ...]


Well, that does seem compelling, doesn't it. But only if we fail to consider Ruby's method lookup model.  

When we send an object a message, Ruby checks to see if that object knows how to respond to it.  If it doesn't, it casades up an object's ancestors all the way to BasicObject to see if any of those objects knows how to respond.  If not, it looks for `:method_missing` in the first receiving object and if not found, cascades back up until an ancestor responds [^method_missing].


[^method_missing]: `BasicObject` implements [method_missing](http://ruby-doc.org/core-2.3.0/BasicObject.html#method-i-method_missing) by raising an `NoMethodError`.

It turns out that when we define a method explicity inside a class with `def self.method_name` we aren't defining it on the class itself but on its metaclass that lives just above it on the inheritance chain.  

  
    ChunkyBacon.singleton_methods
    # => [:invent]


These singleton methods aren't anything special themselves however. Singleton methods are just instance methods of the metaclass.


    mummy_bacon = ChunkyBacon
    meta_bacon  = ChunkyBacon.singleton_class
    yummy_bacon = ChunkyBacon.new 'pineapple'

    mummy_bacon.instance_method :invent
    # => NameError: undefined method `invent' for class `ChunkyBacon'

    yummy_bacon.instance_method :invent
    # => NoMethodError: undefined method `instance_method' for #<ChunkyBacon:0x007fe568a61288 @flavour="pineapple">

    meta_bacon.instance_method :invent
    # => #<UnboundMethod: #<Class:ChunkyBacon>#invent>


So there you go. **There are no class methods in Ruby**. And **there are no singleton methods in Ruby** either.  All methods are just instance methods that are defined on different objects in inheritance hierachy.  Once you grok this, the Ruby object model becomes incredibly simple.

---