### Acquiring Behaviour Through Inheritance

The goal of this chapter is to:
* teach you how to build a 'technically sound inheritance hierarchy'
* help you to decide when it's the right thing to do

#### Keybit:

"Inheritance is, at its core, a mechanism for *automatic message delegation*."

Have a look at the following snippet of code:

```ruby
class Bicycle
  attr_reader :style #...

  # some methods...

  def spares
    if style == :road
      { chain:        '10-speed',
        tape_colour:  tape_colour }
    else
      { chain: '10-speed',
        rear_shock: rear_shock }
    end
  end

  # some more methods...
end
```

The code contains an `if` statement that checks the attribute that details the category of self in order to determine what message to send to that self. This sort of knowledge is a dependency that raises the cost of change.

Be very wary of this pattern. In [Chapter 5](https://github.com/NadiDami/keybits/blob/master/poodr_metz%2F5_duck_typing.md) this pattern was a tool for discovering a missing duck type; here we use the pattern to discover missing subclasses.

Inheritance solves the problem of "highly related types that share common behaviour but differ along some dimension."

#### Keybit:

"Inheritance provides a way to define two objects as having a relationship such that when the first receives a message that it does not understand, it *automatically* forwards the message to the second."

### Creating an Abstract Superclass

An abstract class is one that is disassociated from any specific instance. You wouldn't create instances of this class. Abstract classes exist to be subclassed — they provide a common repository for behaviour that is shared across a set of subclasses; these subclasses supply specialisations.

Do not make an abstract superclass with only one subclass. Even if your original class contains general and specific behaviour, the need for other specialisations may never arise.

Even two subclasses may not be enough. There is a trade-off between duplicating a lot of code and not finding the correct abstraction. Say you have two cases with a third one imminent, it may be best to duplicate the code and wait for better information as to what your abstraction should be. The other question is: how regularly would your duplicated code need to change? If it is very regularly, then you may want to opt for subclassing earlier.

#### Keybit:

Stick with the 'push-everything-down-and-then-pull-some-things-up' strategy! The worst that can happen is that you fail to promote some things. This will eventually be spotted — say, when you have a new case and it needs some particular behaviour that'll then be duplicated amongst the existing subclasses — and easily fixed. If you attempt the refatoring from the other direction you may leave behind concrete behaviour that does not apply to every possible new subclass.

"The general rule for refactoring into a new inheritance hierarchy is to arrange code so that you can promote abstractions rather than demote concretions."

### Using the Template Method Pattern

The technique of defining a basic structure in the super class and sending messages to acquire subclass-specific contributions is known as the *template method* pattern:

```ruby
class Bicycle
  attr_reader :chain, :tire_size

  def initialize(args={})
    @chain      = args[:chain] || default_chain
    @tire_size  = args[:tire_size] || default_tire_size
  end

  def default_chain  # <- common default
    '10-speed'
  end

  def default_tire_size
    raise NotImplementedError,
      "#{self.class} cannot respond to:"
  end
end

class RoadBike < Bicycle
  # ...

  def default_tire_size # <- subclass default
    '23'
  end
end

class MountainBike < Bicycle
  # ...

  def default_tire_size # <- subclass default
    '2.1'
  end
end

```

See the `default_tire_size` method in the `Bicycle` class? Any class that uses the template method pattern should supply an implementation for every message it sends. This way, programmers creating new subclasses who are not aware of the subtleties of the superclass will have helpful error messages.
