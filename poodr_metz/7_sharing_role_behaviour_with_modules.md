### Sharing Role Behaviour with Modules

This chapter explores an alternative that uses the techniques of inheritance to share a *role*.

Sometimes you have a need to share behaviour among otherwise unrelated objects — the common behaviour is defined by a *role*.

The `Preparer` (Sandi's version) / `Administer` (my version) duck type from Chapter 5, 'Reducing Costs with Duck Typing', is a role. Objects that implement these interfaces play those roles.

Modules provide a perfect way to allow objects of different classes to have access to the behaviour they need to play a common role.

#### Keybit:

The total set of messages to which an object can respond includes:

* Those it implements
* Those implemented in all objects above it in the hierarchy
* Those implemented in any module that has been added to it
* Those implemented in all modules added to any object above it in the hierarchy

### Removing Unnecessary Dependencies

Remember the `ExamSession` class from the notes on Chapter 5? It required input from different types of administrators, a `ChiefExaminer`, an `Invigilator`, and a `Teacher`.

These people all need a break in between exam sessions and the length of their break varies across the different classes. Let's say we have a `Schedule` class that takes responsibility for knowing the lead time for each class in order to determine whether it can be scheduled for an upcoming `ExamSession`. In the first version, you find that the `Schedule` class has to check with class is the target in order to know which value of 'lead_days' to use.

"The fact that the `Schedule` class checks many class names to determine what value to place in one variable suggests that the variable name should be turned into a message, which in turn should be sent to each incoming object."

#### Keybit:

"If your interest is in object B, you should not be forced to use object A if your only use of it is to find things out about B."


At the moment, in our hypothetical `Schedule` class, it only exists to tell us whether the target class is schedulable or not. It's equivalent of asking a `StringUtils` class if a given string is empty. It's clear here that the targets should be able to respond to `schedulable?` and that there is a `Schedulable` role present, where this duck type must implement at `lead_days` method.

Here is an example of how the `Teacher` class may implement `schedulable?`. `Teacher` knows its only lead time and delegates `scheduled?` to `Schedule` itself:

```ruby
class Teacher
  attr_reader :schedule

  # Inject the Schedule and provide a default
  def initialize(args={})
    @schedule = args[:schedule] || Schedule.new
    # ...
  end

  # Return true if this teacher is available
  def schedulable?(start_date, end_date)
    !scheduled?(start_date - lead_days, end_date)
  end

  # Return the schedule's answer
  def scheduled?(start_date, end_date)
    schedule.scheduled?(self, start_date, end_date)
  end

  # Return the number of lead_days before a teacher can be scheduled
  def lead_days
    1
  end

  # ...
end
```

### Extracting the Abstraction

The following code snippet shows a new `Schedulable` module, which contains an abstraction extracted from the `Teacher` class. This is so that other classes such as `ChiefExaminer` and `Invigilator` can also use it:

```ruby
module Schedulable
  attr_writer :schedule

  def schedule
    @schedule ||= ::Schedule.new
  end

  def schedulable?(start_date, end_date)
    !scheduled?(start_date - lead-days, end_date)
  end

  def scheduled?(start_date, end_date)
    schedule.scheduled?(self, start_date, end_date)
  end

  #includers may override
  def lead_days
    0
  end

end
```


#### Keybit:

Look at the `lead_days` method above. Remember: if a module sends a message it must provide an implementation, even if that implementation is there to raise an error indicating that users of the module must implement the method. The `lead_days` method is a hook that follows the template method pattern.

Here's how the `Teacher` class will now look:

```ruby
class Teacher
  include Schedulable

  def lead_days
    1
  end

  # ...
end
```

Once incorporated in other classes, the code will resemble classical inheritance: The code in `Schedulable` is the abstraction and it uses the template method pattern to invite objects to provide specialisations to the algorithm it supplies. `Schedulables` can then override `lead_days` in order to supply its specialisation.

The difference between classical inheritance and sharing code via modules can be boiled down to the difference bewteen *is_a* and *behaves_like_a*.
