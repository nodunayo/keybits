### Hide instance variables

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

### Hide data structures

Consider the following `TestResult` class:

```ruby
class TestResult
  attr_reader :data

  def initialize(data)
    @data = data
  end

  def scores
    # 0 is the no. of correct answers, 1 is the no. of incorrect answers.
    data.collect { |cell|
      (cell[0] * 5) - (cell[1] * 0.5)}
  end

  # more methods here...

end
```

The score is calculated by awarding a student five points for every answer they get correct and subtracting half a point for each incorrect answer.
Given the way the class is constructed, it expects to be initialised with a two-dimensional array of correct answers and incorrect answers per student.

What are the problems here?

* The `scores` method knows where to find correct answers (at index [0]) and incorrect answers (at index [1]) within the data structure
  * If the structure of data changes then the code inside the `scores` method will also have to change
* It is likely that as this class develops references to the array's structure will *leak*; ideally, knowledge that correct scores are at [0] should be known in one place only
* It is not clear what the data represents; here a comment has been necessary in order to add some explanation
  * These comments will also need to be updated if the structure of `data` changes

Have a look at the following `AltTestResult` class. It has the same interface as `TestResult` but the internal implementation aims to alleviate some of the concerns discussed above:

```ruby
class AltTestResult
  attr_reader :answer_sets

  def initialize(data)
    @answer_sets = sort_answers(data)
  end

  def scores
    answer_sets.collect { |answer_set|
      (answer_set.correct * 5) - (answer_set.incorrect * 0.5)}
  end

  AnswerSet = Struct.new(:correct, :incorrect)
  def sort_answers(data)
    data.collect { |cell|
      AnswerSet.new(cell[0], cell[1]) }
  end
end
```

* The `score` method has no knowledge of the internal structure of the array it is passed
* Knowledge of the array structure is contained within the `sort_answers` data. This methods converts the array of `Arrays` into an array of `Structs`
* If the structure of the incoming data is altered the only changes to the code will be made in defining the `AnswerSet` struct and the `sort_answers` method.

### Enforce Single Responsibility Everywhere

We should be extracting extra responsibilities from methods.

Look at the `scores` method from the `AltTestResult` class:

```ruby
def scores
  answer_sets.collect { |answer_set|
    (answer_set.correct * 5) - (answer_set.incorrect * 0.5)}
end
 ```

 The method does two things: it iterates over the answer sets and then calculates the score of each answer sset.

 We can separate out responsibilities like so:

 ```ruby
def scores
  answer_sets.collect { |answer_set| score(answer_set) }
end

def score(answer_set)
  (answer_set.correct * 5) - (answer_set.incorrect * 0.5)}
end
```

Ensuring your methods have a single responsibility will:

* Expose previously hidden qualities in you class
* Avoid the need for comments; it'll be much clearer what each method does and the method naming should help here
* Encourage reuse; now there is a method available for finding an individual's score
* Make it easy to move methods to another class.


It's also important to isolate extra responsibilities in in classes.

Say you have a `Gear` class and you think there could be room to extract a `Wheel` class. Test it out by embedding a `Wheel` struct inside of the class:

```ruby
class Gear
  #methods here

  Wheel = Struct.new(:rim, :tire) do
    def diameter
      rim + (tire * 2)
    end
  end
end
```

This is not a long-term design goal, but rather an experiment. It seems that a separate `Wheel` class is the way to go. An extra feature may be to calculate the 'bicycle wheel circumference'. If this feature request comes along, it'll be very easy to implement — all that is needed is a `circumference` method on an isolated `Wheel` class.
