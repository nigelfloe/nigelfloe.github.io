---
layout: post
title: "I Dream of Ruby"
date: 2015-08-23 18:49:49 -0400
comments: true
categories: "Flatiron&nbsp;School"
---
## HydrationError expecting water (0 for 1)

Last night, I lay tossing and turning in bed, wrestling with phantom error messages. For some reason, the `sound_sleep` method I normally relied on to send my `self` (`david = Person.new(name: David, age: 26)`) off to `asleep = true`

```ruby
class Person
  include Sleepable
  attr_accessor :name, :age, :asleep
  def initialize(name:, age:)
    @name = name
    @age = age
    @asleep = false
  end

  def asleep?
    !!@asleep
  end
end

david = Person.new(name: 'David', age: 26)
david.asleep?                               # => false
```

What's going on here? I roll over, and ask Taryn, my girlfriend, why I can't sleep. She furrows her brows and turns away from me, taking the blankets.

Hmph. I guess I'll check the Sleepable module for answers:

```ruby
module Sleepable
  def sound_sleep
    if hydrated
      asleep = true
    end
  end
end

david.sound_sleep                           # => nil
```
`nil` sleep sucks at 2 AM.

Check the conditional, maybe `self` is de`hydrated`?


```ruby
david.hydrated                              # => nil
```
`nil` isn't really helping me here.... Let's look at my Person class again...

```ruby
class Person < Biology
  include Sleepable
  attr_accessor :name, :age, :asleep  
  def initialize(name:, age:)
    @name = name
    @age = age
    @asleep = false
  end

  def asleep?
    !!@asleep
  end
end

david.asleep?                               # => false
```
*grumble*

I don't see anything about `hydrate` here. This is hopeless, and `david` is so tired...I mean.. *I'm* so tired. `self` is tired.....

Hang on! What's that `Biology` inheritance? That wasn't there before! Fucking dream code:
```ruby
david.class.ancestors                       # => [Person, Sleepable, Biology, Object, Kernel, BasicObject]
```

a-HA! a `Biology` class, that must be the class that...ummm...generates behavior for.....biologic things....like birds! or [lolcats](http://giphy.com/gifs/cat-lolcats-the-internet-EeffMbJ6pwxAQ)! or people maybe

```ruby
Biology.methods                             # => [:allocate, :new, :superclass, :json_creatable?, :freeze, :===, :==, :<=>, :<, :<=, :>, :>=, :to_s, :inspect, :included_modules, :sh_t, :include?, :name, :ancestors, :instance_methods, :public_instance_methods, :protected_instance_methods, :private_instance_methods, :constants, :const_get, :const_set, :const_defined?, :const_missing, :die, :class_variables, :remove_class_variable, :class_variable_get, :class_variable_set, :class_variable_defined?, :public_constant, :private_constant, :singleton_class?, :include, :prepend, :hydrate, :module_exec, :class_exec, :module_eval, :class_eval, :method_defined?, :public_method_defined?, :fuck, :private_method_defined?, :protected_method_defined?, :public_class_method, :private_class_method, :autoload, :eat :autoload?, :instance_method, :public_instance_method, :to_json, :nil?, :=~, :!~, :eql?, :hash, :class, :singleton_class, :clone, :dup, :itself, :taint, :tainted?, :pontificate :untaint, :untrust, :meiosis, :untrusted?, :trust, :frozen?, :methods, :...
```

Whoa, there's a lot going on in there....

Wait! I see a method called `:hydrate`. Maybe...

```ruby
david.hydrate

# ~> HydrationError
# ~> expecting water (0 for 1)
# ~>
# ~> /Users/david/dev/dream.rb:17:in `hydrate'
# ~> /Users/david/dev/dream.rb:35:in `<main>'
```

It's great when Ruby errors are so descriptive. It looks like I need water.

"Taryn, I figured it out! I just needed a glass of water!"

She mumbles something and puts a pillow over her head.

## Programming Dreams

Apparently maddening dreams related to programming are pretty common. There are some good ones in here:

http://c2.com/cgi/wiki?WeirdDeveloperDreams

including this gem:
> I know I'm awake, & I know I fall asleep. Then, in the dream, I wake up a few minutes later, and, in the dream, can't sleep. I spend a few hours wandering around the house until I get tired again, and in the dream, fall asleep again. Then, still in the dream, my alarm goes off. I wake up, and start to go about my day, which feels like an extension of the bad dream I'd "just had" in the bad dream I *AM* having.
Recurse this about five times, so that by the time my REAL alarm goes off, and I really AM awake, I'm so F4'd up I can't tell if I'm awake, asleep, if I ever WENT to sleep, if I've gotten ANY rest what-so-ever, and subsequently spend the rest of the day feeling emotionally & physically drained.
Then I go home that night, hit the couch, and sleep for nine hours straight.
This is not an uncommon occurrence, unfortunately.

![recursion](http://media.giphy.com/media/D3hbWaBffn9Cg/giphy.gif)
