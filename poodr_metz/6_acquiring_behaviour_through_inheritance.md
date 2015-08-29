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

Be very wary of this pattern. In Chapter 5 this pattern was a tool for discovering a missing duck type; here we use the pattern to discover missing subclasses.

