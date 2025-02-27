## Summary: Understanding Interfaces in Java
---

Interfaces in Java are a crucial part of the language's approach to abstraction and polymorphism. They allow for a contract that classes can implement, which promotes flexibility and reusability in your code.

1
---
### What is an Interface?
An **interface** in Java is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Interfaces cannot contain instance fields or constructors. The methods defined in an interface are abstract by default.

For example:
```java
public interface Animal {
    void eat();
    void sleep();
}
```

- **Public interface Animal**: This defines a new interface named `Animal`.
- **void eat() and void sleep()**: These are abstract methods, meaning any class that implements the `Animal` interface must provide a concrete implementation of these methods.

2
---
### Implementing Interfaces
When a class implements an interface, it agrees to implement all the methods defined by that interface. A class can implement multiple interfaces, which supports a form of multiple inheritance.

Example:
```java
public class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("Dog is eating");
    }

    @Override
    public void sleep() {
        System.out.println("Dog is sleeping");
    }
}
```

- **public class Dog implements Animal**: The `Dog` class implements the `Animal` interface, agreeing to define the `eat` and `sleep` methods.
- **@Override**: This annotation indicates that the method is overriding a method declared in the interface.

3
---
### Advantages of Interfaces
- **Abstraction**: Interfaces provide a way to define methods without specifying how they work, allowing different implementations.
- **Multiple Inheritance**: A class can implement multiple interfaces, which enables the use of functionalities from various types.
- **Loose Coupling**: By coding to an interface rather than a class, systems can be more flexible and easier to maintain.

4
---
### Default Methods in Interfaces (Java 8 and later)
Java 8 introduced **default methods** in interfaces. This allows you to add new methods to an interface without breaking the existing implementations.

Example:
```java
public interface Animal {
    void eat();
    void sleep();

    default void run() {
        System.out.println("Animal is running");
    }
}
```

- **default void run()**: This provides a default implementation for the `run` method. Classes implementing the `Animal` interface can use this method without having to provide an implementation.

5
---
### Using Interfaces with Other Concepts
Interfaces can also be used with abstract classes and lambda expressions (in functional programming). A common use case is defining a callback mechanism.

Example of a functional interface:
```java
@FunctionalInterface
public interface Greeting {
    void sayHello(String name);
}
```

- **@FunctionalInterface**: This annotation indicates that the interface can be used with lambda expressions and should have exactly one abstract method.

---
### Example of Using Interfaces
Here's a full example demonstrating an interface with two implementations:

```java
public interface Animal {
    void sound();
}

public class Cat implements Animal {
    @Override
    public void sound() {
        System.out.println("Meow");
    }
}

public class Cow implements Animal {
    @Override
    public void sound() {
        System.out.println("Moo");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal cat = new Cat();
        Animal cow = new Cow();
        
        cat.sound(); // Outputs: Meow
        cow.sound(); // Outputs: Moo
    }
}
```
In this example:
- Two classes, `Cat` and `Cow`, implement the `Animal` interface, providing their own implementation of the `sound` method.
- The `Main` class demonstrates polymorphism by referencing `Animal` types.

---
## References
## https://docs.oracle.com/javase/tutorial/java/concepts/interface.html
## https://www.javatpoint.com/interfaces-in-java