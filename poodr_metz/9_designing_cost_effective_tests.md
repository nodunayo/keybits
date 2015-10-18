"The art of writing changeable code requires the ability to write high-value tests. Tests give you the confidence to refactor continuously."

### The Potential Benefits of Testing

* It helps you to find bugs
* it serves as the only reliable documentation of design
* It allows you to safely defer design decisions — perhaps you know something is missing but you don't quite have enough information yet
* It supports abstractions — while each individual abstraction is easy to understand, there is no single place in the code where the behaviour of the whole is obvious
* Tests expose design flaws:
  * Does a test require painful setup? The code expects too much context
  * Does testing one object drag in a bunch of others? Your code has too many dependencies

### Knowing What to Test

* Tests should be for messages that are defined in public interfaces:
  * Incoming messages should be tested for the state they return
  * Outgoing command messages should be tested to ensure they get sent
    * The responsibility for testing a method's return lies with the receiver though
  * Outgoing query messages should not be tested. Here as an example of one such method
  ```ruby
    class Triangle
      # ...
      def area
        shape.base * height
      end
    end
  ```

  The `area` method sends `base`. The consequences of this are hidden inside of `Triangle`. Tests to prove the correctness of `base` belong in `Shape`. `Traingle`'s only responsibility is to prove that `area` works correctly.

* Note: Do not test an incoming method that has no dependents!!

* You should never had to test private methods. If your object has so many private methods this could hint at a design smell of having too many responsibilities. Consider extracting the objects into a new method


* You need common tests to test roles and interactions in inheritance, i.e. the interface between a superclass and its subclasses and the messages that a subclass has to respond to:
  * These tests will give you confidence that subclasses meet a certain standard
  * Novice programmers can create subclasses in safety
  * Programmers new to a codebase don't have to trawl through the code to understand all of the requirements
* You should test concrete subclass specialisations without embedding any knowledge of the superclass into the test
* It can be tricky to test abstract superclass behaviour:
  * Creating an instance of the superclass is hard and may not have all the behaviour required to make the test run
  * If you've used the template method pattern, you'll be able to stub the behaviour that would be supplied by subclasses.
