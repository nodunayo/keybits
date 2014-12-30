### Recognizing Dependencies

An object has a dependency when it knows:

* The name of another class
* The name of a message that it intends to send to another object
* The arguments that the message requires
* The order of those arguments.

###Inject dependencies

Take the following snippet of a `TakeawayOrder` class:

```ruby
class TakeawayOrder
  attr_reader :items, :name
  def initialize(items, name)
	  @items = items
	  @name = name
  end

  #...

  def complete_order
	  Customer.new(name).pay_for(items)
  end

  #...
end
```

The `TakeawayOrder` class directly references the `Customer` class. This means it'll only complete orders for objects of type `Customer`, even where other objects have the ability to `pay_for` items.

It would be better if the `TakeawayOrder` class merely expects to be initialized with an object that can respond to the `pay_for` method:

```ruby
class TakeawayOrder
  attr_reader :items, :customer
  def initialize(items, customer)
    @items = items
	  @customer = customer
  end
  
  #...

  def complete_order
	  customer.pay_for(items)
  end

  #...
end
```

Now `TakeawayOrder` does not care whether the object in the `complete_order` method is an instance of the `Customer` class. The class can now interact with any object that has a `pay_for` method. This is an example of _dependency injection_.

#### Keybit:

"Think of every dependency as an alien bactierum that's trying to infect your class. Give your class a vigorous immune system. Dependencies...should [therefore] be concise, explicit, and isolated."

### Isolate dependencies

It is not always possible to inject dependencies. If you are so constrained, one thing to do is to isolate your dependencies as best as you can.

Imagine you had a complex mathematical formula for the minimum length of time it was going to take a group of people to cross a bridge. The method looks something like:

```ruby
def total_time_to_cross_bridge
  #... some scary maths
  halfway_point = some_intermediate_result * bridge.quality
  #... some more scary maths
end
```
The time taken to cross the bridge depends on its quality, i.e. it's ability to withstand the weight of those traversing across it. However, `bridge.quality` is stuck in the middle of this complex method. It would be far better to isolate this dependency like so:

```ruby
def total_time_to_cross_bridge
  #... some scary maths
  halfway_point = some_intermediate_result * bridge_quality
  #... some more scary maths
end

def bridge_quality
  bridge.quality
end
```

`total_time_to_cross_bridge` is no longer coupled to an external object and the call to one of that object's methods. Now, it only refers to `self`. This technique is very useful when a class contains embedded references to a message that is likely to change.

### Remove argument-order dependencies

#### Use hashes for initialization arguments

```ruby
class TakeawayOrder
  attr_reader :items, :customer
  def initialize(args)
    @items = args[:items]
    @customer = args[:customer]
  end
  ...
end

TakeawayOrder.new(customer: Customer.new("Sandi"), items: ["chicken", "pie"])
```

#### Keybit:

If working with a method that requires a few stable arguments followed by some less stable ones, the best thing to do is to take a few fixed-order aguments followed by an options hash.


#### Explicitly define defaults

When you've got defaults that are simple Strings or numbers, use the `fetch` method:

```ruby
def initialize(args)
  @items = args.fetch(:items, ["pasta", "chicken"])
  @customer = args.fetch(:customer, Customer.new("Sandi"))
end
```

If your defaults are more complicated, it's better to define a separate `defaults` method and merge that into your `args` hash:

```ruby
def initialize(args)
  args = defaults.merge(args)
  @items = args[:items]
  @customer = args[:customer]
end

def defaults
 {items: ["pasta", "chicken"], customer: Customer.new("Sandi")}
end
```

#### Isolate multiparameter initialization

What do you do when you have to depend on a method that requires fixed-order arguments and you don't own the code fo this method?

Create a wrapper that allows you to create a new instance of that object using an options hash:

```
# When TakeawayOrder is part of an external interface
module SomeFramework
  class TakeawayOrder
  attr_reader :items, :customer
    def initialize(items, name)
	    @items = items
	    @name = customer
    end
  # ...
  end
end

# implement a wrapper for the interface that protects you from changes
module TakeawayOrderWrapper
  def self.takeaway_order(args)
    SomeFramework::TakeawayOrder.new(args[:items], args[:customer])
  end
end
```

#### Keybit:
"We call the `TakeawayOrderWrapper` a _factory_. Its sole purpose is to create instances of some other class."
