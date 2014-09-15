### Hide Instance Variables

Don't directly refer to instance variables. Instead, wrap them in accessor methods:

```ruby
class Triangle
  def initialize(base, height)
    @base = base
    @height = height
  end

  def area
    @base * @height * 0.5
  end
end
```

It is not a good idea to refer to instance variables as is done in the `area` method above. By using methods such as `attr_reader` in Ruby, you can create a wrapper method for such variables. Say you need to make an adjustment to the `@base` instance variable, this can be done in one place in a method called `base`:

```ruby
class Triangle
  attr_reader :base, :height
  def initialize(base, height)
    @base = base
    @height = height
  end

  def area
    base * height * 0.5
  end
end
```
