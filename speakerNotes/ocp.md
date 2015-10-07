The Open Closed Principle (OCP) is undoubtedly the most important of all the class category principles. In fact, each of the remaining class principles are derived from OCP. It originated from the work of Bertrand Meyer who is recognized as an authority on the object-oriented paradigm. OCP is states that we should have the ability to add new features to our system without having to modify our set of preexisting classes. As stated previously, one of the benefits of the object-oriented paradigm is to enable us to add new data structures to our system without having to modify the existing system’s code base.

Let’s look at an example to see how this can be done. Consider a financial institution where we have to accommodate different types of accounts to which individuals can make deposits. Figure 4.1 shows a class diagram, with accompanying descriptions of some of the elements and how we might structure a portion of our system. For the purposes of our discussion in this chapter, we will focus on how the OCP can be used to extend the system.

Our Account class has a relationship to our AccountType abstract class. In other words, our Account class is coupled at the abstract level to the AccountType inheritance hierarchy. Because our Savings and Checking classes each inherit from the AccountType class, we know that through dynamic binding, we can substitute instances of either of these classes wherever the AccountType class is referenced. Subsequently, Savings and Checking can be freely substituted for AccountType within the Account class. This is the intent of an abstract class and enables us to effectively adhere to OCP by creating a contract between the Account class and the AccountType descendents. Because our Account is not directly coupled to either of the concrete Savings or Checking classes, we can extend the AccountType class, creating a new class such as MoneyMarket, without having to modify our Account class. We have achieved OCP and can now extend our system without modify its existing code base.

Figure 4.1
Therefore, one of the tenets of OCP is to reduce the coupling between classes to the abstract level. Instead of creating relationships between two concrete classes, we create relationships between a concrete class and an abstract class or, in Java, between a concrete class and an interface. When we create an extension of our base class, assuming we adhere to the public methods and their respective signatures defined on the abstract class, we have essentially achieved OCP. Let’s take a look at a simplified version of the Java code for this example, focusing on how we achieve OCP, instead of on the actual method implementations.

```java
public class Account {
   private AccountType _act;
   public Account(String act) {
      try {
         Class c = Class.forName(act);
         this._act = (AccountType) c.newInstance();
      } catch (Exception e) {
         e.printStackTrace();
      }
   }
   public void deposit(int amt) {
      this._act.deposit(amt);
   }
}
```

Here, our Account class accepts as an argument to its constructor a String representing the class we wish to instantiate. It then uses the Class class to dynamically create an instance of the appropriate AccountType subclass. Note that we don’t explicitly refer to either the Savings or Checking class directly.

```java
public abstract class AccountType  {
   public abstract void deposit(int amt);
}
```

This is the abstract AccountType class that serves as the contract between our Account class and AccountType descendents. The deposit method is the contract.

```java
public class CheckingAccount extends AccountType {
   public void deposit(int amt) {
      System.out.println();
      System.out.println();
      System.out.println("Amount deposited in checking account: " + amt);
      System.out.println();
      System.out.println();
   }
}
```

```java
public class SavingsAccount extends AccountType {
   public void deposit(int amt)  {
      System.out.println();
      System.out.println();
      System.out.println("Amount deposited in savings account: " + amt);
      System.out.println();
      System.out.println();
   }
}
```

Each of our AccountType descendents satisfies the contract by providing an implementation for the deposit method. In the real world, the behaviors of the individual deposit methods would be more interesting and, given the preceding design, would be algorithmically different.
