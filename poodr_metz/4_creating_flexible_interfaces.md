This chapter explores one type of interface — methods within a class and how and what to expose to other classes.

Sandi Metz provides a very useful example to help clarify the distinction between _private_ and _public_ interfaces:

At a restaurant, customers order off of a menu. The menu represents the public interface. They are concerned with _what_ they want, not _how_ it is prepared in the kitchen. The preparation of the food occurs behind the scenes, in the kitchen, and this is the private interface.

Here is a table detailing the key differences between public and private interfaces:

| Public Interface Methods  | Private Interface Methods|
| :-----------------:| :---------------: |
| Reveal class' primary responsibility  | Handle implementation details |
| Handle implementation details | Do not expect to be sent by other objects |
| Will not change on a whim | Can change for any reason |
| Are safe for others to depend on | Are not safe for others to depend on |
| Are thoroughly documented in the tests | May not be referred to in the tests|

#### Keybit:

Use [sequence diagrams](http://en.wikipedia.org/wiki/Sequence_diagram) as a first-step to designing your application. This will help you to move away from a class-based design to a message-based design. Sequence diagrams explicitly chart the messages that pass between objects; they help you to design your public interfaces.

How should we be creating interfaces?

### Create Explicit Interfaces

Make sure that methods in the _public_ interface:

* are explicitly identified as such
* are more about the _what_ than the _how_
* have names that, to the best of your knowledge, will not change
* take a hash as an options parameter.


Know your keywords:

* Private — least stable type of method, most restricted visibility, never called with an explicit receiver
* Protected — unstable, but can be called with an explicit receiver of either _self_ or an instance of the same class
* Public - stable, visible everywhere.

### Honour Public Interfaces of Others

Try and interact with other class using only their public interfaces. You should view a dependency on a private method of an external framework as a form of technical debt.

### Exercise Caution When Depending on Private Interfaces

If you must rely on a private interface, isolate the dependency to the best of your abilities.

### Minimize Context

When creating your public interfaces, allow senders to focus on the _what_ and not the _how_.

#### Keybit:

The Law of Demeter are a set of design guidelines around developing loosely coupled objects. One to way to paraphrase the rule is "only talk to your immediate neighbours". Whilst it's important not to follow this rule blindly, violations of Demeter are often a sign that the public interfaces of your objects are lacking.
