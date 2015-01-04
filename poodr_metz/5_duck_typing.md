### Reducing Costs with Duck Typing

Duck types are public interfaces that are not tied to specific classes; this means that you have flexibility across a range of classes when you are using them, replacing dependencies on classes with dependencies on messages.

Let's look at an example. In a school, every time an exam is administered, there are a range of parties involved who each have a different role to play. The Chief Examiner reads out the rules and instructions for the paper, the Invigilator monitors the students, and the Teacher collects the papers in at the end of the session. You can imagine an `ExamSession` class that looks like the following:

```ruby
class ExamSession
  attr_reader :students, :paper

  def administer(administrators)
    administrators.each {|administrator|
      case administrator
      when ChiefExaminer
        administrator.read_instructions(paper)
      when Invigilator
        administrator.monitor(students)
      when Teacher
        administrator.collect_papers(self)
      end
    }
  end
end

class ChiefExaminer
  def read_instructions(paper)
    #...
  end
end

class Invigilator
  def monitor(students)
    #...
  end
end

class Teacher
  def collect_papers(exam)
    #...
  end
end
```

This design ties us to the class of each type of administrator. Given the way the `administer` method is written, it has to be aware of each class of administrator in order to know which message to send. Furthermore, this type of code lends itself to propagation: if another administrator becomes part of the process, the `case` statement will end up being extended.

In order to 'find the duck', we need to think about what `administer` needs. All it wants to do is to make sure the exam session is smoothly administered, with each party fulfilling their role. It doesn't need to know _how_ each party does that, it just needs to trust that they will do what they are supposed to. Hence, all the `adminster` method needs to expect is that each of its arguments is a type of "Administrator". The next reasonable assumption is that the `administer` method can send an `administer_exam` message to each Administrator and they will know how to respond to this. We can apply this duck type to the `ExamSession` class:

```ruby
class ExamSession
  attr_reader :students, :paper

  def administer(administrators)
    administrators.each {|administrator|
      administer_exam(self)}
  end
end

class ChiefExaminer
  def administer_exam(exam)
    read_instructions(exam.paper)
  end
end

class Invigilator
  def administer_exam(exam)
    monitor(exam.students)
  end
end

class Teacher
  def administer_exam(exam)
    collect_papers(exam)
  end
end
```

Whilst this code is more abstract than the first snippet, this code is much easier to extend.

#### Keybit:

"The ability to tolerate ambiguity about the class of an object is the hallmark of a confident designer. Once you begin to treat your objects as if they are defined by their behaviour rather than by their class, you enter into a new realm of expressive flexible design."

### Finding Ducks

The presence of the following patterns are often a sign that you have a hidden duck waiting to be realised:

* Case statements that switch on class

* `kind_of?` and `is_a?`

  ```ruby
  if administrator.kind_of?(ChiefExaminer)
    administrator.read_instructions(paper)
  elsif administrator.kind_of?(Invigilator)
    administrator.monitor(students)
  #...
  end
  ```

  Using `kind_of?` and `is_a?` is the same as having a case statement that switches on class.

* `responds_to?`

  ```ruby
  if administrator.responds_to?(:read_instructions)
    administrator.read_instructions(paper)
  elsif administrator.responds_to?(:monitor)
    administrator.monitor(students)
  #...
  end
  ```

  Using `responds_to?` decreases the number of dependencies, but the code is still very bound to class, although less directly.

When you spot these offending code patterns:

1. Think about what the expectations of these patterns are
1. Use those expectations to reveal the duck type
1. Define the interface of the duck type
1. Implement that interface where necessary
1. Trust that those implementers will behave correctly.

Beware: Use your judgement. Changing Ruby base classes (to introduce a duck type) enters the realm of _monkey patching_. Make sure you have a very good reason before doing this.
