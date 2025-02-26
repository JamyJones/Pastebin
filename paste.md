## Java Interfaces Explained
---
An interface in Java is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Interfaces cannot contain instance fields. The methods in interfaces are abstract by default, meaning they do not have a body. Interfaces help achieve abstraction and multiple inheritance in Java.

1
---
### Definition and Purpose
- **Definition**: An interface in Java is a blueprint for classes. It defines a set of methods that the implementing classes must provide.
- **Purpose**: Interfaces allow different classes to communicate with each other, providing a standard structure for methods that must be implemented, promoting loose coupling, and enhancing code reusability.

2
---
### Syntax
To declare an interface, use the `interface` keyword followed by the interface name. Here’s a simple example:

```java
public interface Animal {
    void makeSound(); // Abstract method
}
```
- **`public interface Animal`**: This line declares an interface named `Animal`.
- **`void makeSound();`**: This line defines an abstract method named `makeSound`. Any class implementing this interface must provide an implementation of this method.

3
---
### Implementing Interfaces
A class implements an interface by using the `implements` keyword. Here’s how it works:

```java
public class Dog implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Bark");
    }
}
```
- **`public class Dog implements Animal`**: This line indicates that the `Dog` class is implementing the `Animal` interface.
- **`@Override`**: This annotation tells the compiler that we are overriding a method from the interface.
- **`public void makeSound() { System.out.println("Bark"); }`**: This is the implementation of the `makeSound` method defined in the `Animal` interface. 

4
---
### Multiple Interfaces
Java allows a class to implement multiple interfaces, enabling a form of multiple inheritance. Here’s an example:

```java
public interface Swimmable {
    void swim();
}

public class Dolphin implements Animal, Swimmable {
    @Override
    public void makeSound() {
        System.out.println("Click");
    }

    @Override
    public void swim() {
        System.out.println("Dolphin is swimming");
    }
}
```
- The `Dolphin` class implements both `Animal` and `Swimmable` interfaces, providing implementations for the required methods from both.

5
---
### Default Methods
Java 8 introduced default methods in interfaces that can have a body. Here’s how to define them:

```java
public interface Animal {
    void makeSound();
    
    default void sleep() {
        System.out.println("Sleeping...");
    }
}
```
- **`default void sleep()`**: This method has a default implementation that can be used by classes that implement the interface, but they can also override it.

6
---
### Practical Example
Let's look at a simple implementation of interfaces:

```java
public interface Vehicle {
    void start();
    void stop();
}

public class Car implements Vehicle {
    @Override
    public void start() {
        System.out.println("Car is starting");
    }

    @Override
    public void stop() {
        System.out.println("Car is stopping");
    }
}
```
- **`Vehicle` interface**: Defines methods for a vehicle.
- **`Car` class**: Implements the `Vehicle` interface, providing functionality for starting and stopping the car.

---
## References
## https://docs.oracle.com/javase/tutorial/java/IandI/interfaceDef.html
## https://www.javatpoint.com/java-interface
## https://www.baeldung.com/java-interfaces