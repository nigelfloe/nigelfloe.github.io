---
layout: post
title: "Let There Be Light!"
date: 2015-08-09 18:13:59 -0400
comments: true
categories: "Flatiron&nbsp;School"
---
## Introduction - An Ode to \_why and Ruby Koans


Near the beginning of my brief career as a Rubyist, I came across an invaluable resource, **[Why's (Poignant) Guide to Ruby](http://mislav.uniqpath.com/poignant-guide/)**. Written by the elusive \_why the lucky stiff, the _Poignant Guide_ is equal parts Ruby tutorial and postmodern meta-fiction. Code examples walk the reader through the construction of monkeys with frogs attached to their hands, dank dungeons optimized for the slaying of intrepid rabbit adventurers, and an alien planet which can read your mind, or grant a wish, but not both at the same time.

These madcap coding lessons are embedded within a frame story involving a well-meaning mad scientist, the ghost of the niece he accidentally killed, cartoon foxes looking for their stolen pickup truck, and a hopelessly imprisoned Frenchman. Also there is a sideplot about a secret society that eats scarves. The book is quite joyously insane, and what it lacks in technical detail, it more than makes up for in sheer inventiveness and creative zeal, although the 'ending' is a bit of a letdown. The _Poignant Guide_ is required reading for any Rubyist.

Tragically, \_why the lucky stiff committed 'infocide' in August 2009, removing all of his code from Github and withdrawing completely from the Ruby community. This **[Slate article](http://www.slate.com/articles/technology/technology/2012/03/ruby_ruby_on_rails_and__why_the_disappearance_of_one_of_the_world_s_most_beloved_computer_programmers_.html)** does a good job of describing his importance to the community, and the impact of his disappearance.

Why returned briefly in January 2013 to send a printer file to Steve Klabnik, the programmer who has kept Why's **[Hackety Hack](http://www.hackety.com/)** project alive after his disappearance. Klabnik has compiled and released this document under the title **[Closure](https://ia601703.us.archive.org/28/items/136875051WhySCompletePrinterSpoolAsOneBook/136875051--why-s-complete-printer-spool-as-one-book.pdf)**, and has made his own **[comments about Why's final work](http://words.steveklabnik.com/the-closure-companion)**. The work contains, among other vignettes, musings about the thanklessness of a career in programming and the deletion of one's public identity.

Apart from my fascination with Why and his story, I've found great encouragement in his efforts to instill programming with a sense of limitless creativity and whimsy, and have found that some of his strangest, or most creative, code examples from the _Poignant Guide_ have stuck with me the most and helped to hammer in my understanding of Ruby basics. I love the idea of world-building in Ruby, and think that the language lends itself to constructing, not only useful tools, but also to modelling fantastical creatures, mythological entities, and possibly, spiritual truths.

Following in Why's footsteps are sites like **[rubymonk](https://rubymonk.com/)**, which presents coding exercises as being similar to Zen koans which must be unravelled by the student. In **[one example](http://rubymonk.com/learning/books/2-metaprogramming-ruby/chapters/25-dynamic-methods/lessons/65-send)** the student's walking staff transforms into a mysterious glider whose properties must be sussed out using the `send` method. **[Later on](http://rubymonk.com/learning/books/2-metaprogramming-ruby/chapters/25-dynamic-methods/lessons/72-define-method)** in their introduction to metaprogramming, `define_method` is used iteratively to allow a young Monk to meditate on the meaning of Life, the Universe, and Everything in order to attain Ultimate Power.



## Genesis 1: `"Let there be #{thing}!"`

I'm still getting a sense of the power of `define_method`. Below we will use it to call creation into being:

> In the beginning God created the heavens and the earth.

```ruby
class God
  attr_reader: heavens_and_earth

  def create_heavens_and_earth
    @heavens_and_earth = Universe.new
  end
end

class Universe
  attr_accessor :day, :light, :sky, :land, :stars, :creatures, :humans, :blessed

  def initialize
    @day = 0
  end
end
```

In order for our God to create the heavens and earth, we will need to construct a `God` class and a `create_heavens_and_earth` method which will create a new instance variable, `@heavens_and_earth` which will be assigned to a new instance of the `Universe` class.

You can see that a new `Universe` needs to be 'created' with a list of `attr_accessor`'s which will enable our `God` to create as it wishes, but the the only attribute that a `Universe` will have set on initialization will be `@day` with a value of `0`.

Also note that our `God` will have an `attr_reader` for `@heavens_and_earth` in order to check in on its creation.

First we will need to instantiate our `God` class and create a universe:
```ruby
god = God.new                 # => #<God:0x007fc5e50b5240>
god.create_heavens_and_earth  # => #<Universe:0x007fc5e50b4ea8 @day=0>
```


In order for `god` to populate `@heavens_and_earth` with all of the nice things people appreciate, he will need some methods at his disposal:
```ruby
class God

  ["light", "sky", "land", "stars", "creatures", "humans"].each do |thing|
    define_method("create_#{thing}") do
      @heavens_and_earth.send("#{thing}=", true)
      @heavens_and_earth.day +=1
      "Let there be #{thing}!"
    end
  end
end
```
Here we use `define_method` and iteration in order to create a number of methods which all do essentially the same thing but with a different name and target.

Each of the items in the array above (`"light","sky","land"`, etc.) is passed into an `each` loop and used to define an instance method for `god`, our instance of the `God` class:

* `create_light`
* `create_sky`
* `create_land`
* etc.

Each of these methods, when called, will use the `send` method to send a command to `@heavens_and_earth` our instance of the `Universe` class, to create the given "thing" (ie. changing its value to `true`), moving time in that `Universe` forward by 1 day, and returning `god`'s declamation about that thing.

Cue day 1:
```ruby
god.heavens_and_earth.light   # => nil
god.create_light              # => "Let there be light!"
god.heavens_and_earth.light   # => true
god.heavens_and_earth.day     # => 1
```

Note that, before `create_light` was called on `god`, there was no `light` in `god`'s `heavens_and_earth`. Also note that `define_method`, used in our `each` loop, did successfully create at `create_light` instance method for `god`, even though we didn't explicitly create it using `def create_light`.

Here, `create_light` is called on `god`, and `god` proclaims (returns) `"Let there be light!"`. We can see that `@heavens_and_earth` now has light (`@light = true`), and the day in our `Universe` is now `1`. Not a bad day's work.

Day 2 proceeds much the same way:
```ruby
god.heavens_and_earth.sky  # => nil
god.create_sky             # => "Let there be sky!"
god.heavens_and_earth.sky  # => true
god.heavens_and_earth.day  # => 2
```

As does day 3:
```ruby
god.heavens_and_earth.land  # => nil
god.create_land             # => "Let there be land!"
god.heavens_and_earth.land  # => true
god.heavens_and_earth.day   # => 3
```

Day 4:
```ruby
god.heavens_and_earth.stars  # => nil
god.create_stars             # => "Let there be stars!"
god.heavens_and_earth.stars  # => true
god.heavens_and_earth.day     # => 4
```

Day 5:
```ruby
god.heavens_and_earth.creatures  # => nil
god.create_creatures             # => "Let there be creatures!"
god.heavens_and_earth.creatures  # => true
god.heavens_and_earth.day    # => 5
```

And day 6:
```ruby
od.heavens_and_earth.humans  # => nil
god.create_humans             # => "Let there be humans!"
god.heavens_and_earth.humans  # => true
god.heavens_and_earth.day        # => 6
```

Because of that metaprogramming wizardry we set our `God` class up with earlier, we didn't have to go back and define additional methods for each of `god`'s creative actions, so that's neat.

On the seventh day, `god` rests. We just need to crack open the `God` class:
```ruby
class God
  def rest
    @heavens_and_earth.blessed = true
    @heavens_and_earth.day +=1
    "It is good."
  end
end
```
And give our guy a chance to take a break.

>By the seventh day God had finished the work he had been doing; so on the seventh day he rested from all his work. Then God blessed the seventh day and made it holy, because on it he rested from all the work of creating that he had done.

```ruby
god.heavens_and_earth.blessed  # => nil
god.rest                       # => "It is good."
god.heavens_and_earth.blessed  # => true
god.heavens_and_earth.day      # => 7
```

I think that's all for now. Hope you enjoyed reading. Next time: something else!




* __Bonus: Our universe from the above example, `@heavens_and_earth`, exists only as an instance variable of our instance of the `God` class, or `god` for short. `@heavens_and_earth` cannot outlive `god`.__
  * Is this how our actual universe works?
  * Nietzsche famously stated that "God is dead", does this mean that we never existed?
  * Will I be able to improve on this model next time?
  * True or false: What _has been_ exists no more; and exists just as little as that which
has _never_ been. But everything that exists _has been_ in the next
moment.
