We mentioned in our previous discussion that OCP is the most important of the class category principles. We can think of the Liskov Substitution Principle (LSP) as an extension to OCP. In order to take advantage of LSP, we must adhere to OCP because violations of LSP are also a violation of OCP, but not vice versa. LSP is the work of Barbara Liskov and is derived from Bertrand Meyer’s Design by Contract. In its simplest form, it is difficult to differentiate OCP and LSP, but a subtle difference does exist. OCP is centered around abstract coupling. LSP, while also heavily dependent on abstract coupling, is also heavily dependent on preconditions and postconditions, which is LSP’s relation to Design by Contract, where the concept of preconditions and postconditions was formalized.

A precondition is a contract that must be satisfied before a method can be invoked. A postcondition, on the other hand, must be true upon method completion. If the precondition is not met, the method should not be invoked, and if the postcondition is not met, the method should not return. The relation of preconditions and postconditions has meaning embedded within an inheritance relationship that is not supported within Java, outside of some manual assertions or nonexecutable comments. Because of this, violations of LSP can be difficult to find.

To illustrate LSP and the interrelationship of preconditions and postconditions, we need only consider how Java’s exception handling mechanism works. Consider a method on an abstract class that has the following signature:

```java
public abstract deposit(int amt) throws InvalidAmountException
```

Assume in this situation, that our InvalidAmountException is an exception defined by our application, is inherited from Java’s base Exception class, and can be thrown if the amount we try to deposit is less than zero. By rule, when overriding this method in a subclass, we cannot throw an exception that exists at a higher level of abstraction than InvalidAmountException. Therefore, a method declaration such as the following is not allowed:

```java
public void deposit(int amt) throws Exception
```

This method declaration is not allowed because the Exception class thrown in this method is the ancestor of the InvalidAmountException thrown previously. Again, we cannot throw an exception in a method on a subclass that exists at a higher level of abstraction than the exception thrown by the base class method we are overriding. On the other hand, reversing these two method signatures would have been perfectly acceptable to the Java compiler. We can throw an exception in an overridden subclass method that is at a lower level of abstraction than the exception thrown in the ancestor. While this does not correspond directly to the concept of preconditions and postconditions, it does capture the essence. Therefore, we can state that any precondition stipulated by a subclass method cannot be stronger than the base class method.Therefore, any postcondition stipulated by a subclass method cannot be weaker than the base class method.

To adhere to LSP in Java, we must make sure that developers define preconditions and postconditions for each of the methods on an abstract class. When defining our subclasses, we must adhere to these preconditions and postconditions. If we do not define preconditions and postconditions for our methods, it becomes virtually impossible to find violations of LSP. Suffice it to say, in the majority of cases, OCP will be our guiding principle.

