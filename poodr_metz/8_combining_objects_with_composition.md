### Combining Objects with Composition

This chapter continues on from where [Chapter 6]build(https://github.com/NadiDami/keybits/blob/master/poodr_metz/6_acquiring_behaviour_through_inheritance.md) left off. The aim is to take that example and, through a series of refactorings, replacing inheritance with composition.

Composition describes a *has_a* relationship. A *composed* object is made up of parts with which it will interact with via well-defined interfaces.

A bicycle is made up of parts. A bicycle is a composed object. Parts are roles. The bicycle depends on the interface of its parts. Because bicycles interact with parts using an interface, new objects that wish to act as a bicycle part only need to implement this interface.

### Deciding between Inheritance and Composition

* Classical inheritance is a *code arrangement technique*
  * You arrange objects in a hierarchy and get message delegation for free
* Composition allows objects to have structural independence but at the cost of explicit message delegation

#### Keybit:
Be biased towards using composition over inheritance if composition can solve the problem. Composition contains far fewer built-in dependencies.


### Benefits and Costs of Inheritance

Coming soon...
