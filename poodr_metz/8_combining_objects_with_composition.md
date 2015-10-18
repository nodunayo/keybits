### Combining Objects with Composition

This chapter continues on from where [Chapter 6](https://github.com/NadiDami/keybits/blob/master/poodr_metz/6_acquiring_behaviour_through_inheritance.md) left off. The aim is to take that example and, through a series of refactorings, replacing inheritance with composition.

Composition describes a *has_a* relationship. A *composed* object is made up of parts with which it will interact with via well-defined interfaces.

A bicycle is made up of parts. A bicycle is a composed object. Parts are roles. The bicycle depends on the interface of its parts. Because bicycles interact with parts using an interface, new objects that wish to act as a bicycle part only need to implement this interface.

### Deciding between Inheritance and Composition

* Classical inheritance is a *code arrangement technique*
  * You arrange objects in a hierarchy and get message delegation for free
* Composition allows objects to have structural independence but at the cost of explicit message delegation

#### Keybit:
Be biased towards using composition over inheritance if composition can solve the problem. Composition contains far fewer built-in dependencies.


### Benefits and Costs of Inheritance

* When correctly applied, inheritance allows your code to be reasonable: big changes in behaviour an be brought about by small changes in the code
* It also allows your code to be usable: new subclasses can be easily created o accommodate new variants
* Hierarchies are also exemplary: they provide guidance for writing the code that will extend them

* Be wary of applying inheritance to problems that it is not suited to. You will get code that is the opposite of...
  * ...reasonable: small changes will break everything; there is high cost to making changes near the top of an incorrectly modeled heirarchy
  * ...usable: it becomes impossible to add behaviour when new subclasses represent a mixture of types
  * ...exemplary: novices will not recognise when incorrectly modeled hierarchies need to be refactored rather than extended.

#### Keybit:
"Inheritance by definition comes with a deeply embedded set of dependencies. Subclasses depend on the methods defined in their superclasses *and* on the automatic delegation of messages to those superclasses." Therefore, asking the question "What will happen when I'm wrong?" assumes special importance here.

### Benefits and Costs of Composition

* Composition tends to lead you to create many small objects that contain straightforward responsibilities. Hence, the code is transparent
* The code is also reasonable: composed objects deal with their parts via an interface so adding a new kind of part is straightforward
* Since objects that participate in composition are small, structurally independent, and have well-defined interfaces, they are easily usable in new and unexpected contexts

* However, while each individual part may be transparent, the whole object may turn out not to be
* Identical delegation code may be needed by many different objects and composition provides no way to share this code


### Choosing Relationships

* Use Inheritance for *is-a* relationships

* Use Duck Types for *behaves-like-a* relationships

* Use Composition for *has-a* relationships

#### Keybit:
"The key to improving your design is to attempt these techniques, accept your errors cheerfully, remain detached from past design decisions, and refactor mercilessly.

As you gain experience, you'll get better at choosing the correct technique the first time, your costs will go down, and your applications will improve."
