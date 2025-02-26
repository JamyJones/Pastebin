## Summary: How Interfaces Work in Java<br>
---<br>
In Java, an interface is a reference type similar to a class that can contain only constants, method signatures, default methods, static methods, and nested types. Interfaces cannot contain instance fields, and the methods declared in an interface are abstract by default. The primary purpose of an interface is to specify a contract that classes must follow.

1. **Definition of an Interface**:  
   An interface can be defined using the `interface` keyword. It can contain method signatures and constants, but no method implementations (unless default methods are used).

   ```java
   interface Animal {
       void eat();  // abstract method
       void sleep(); // abstract method
   }
   ```

   Here, `Animal` is an interface that declares two methods: `eat()` and `sleep()`. These methods have no body.

2. **Implementing an Interface**:  
   A class implements an interface using the `implements` keyword. It must provide concrete implementations for all abstract methods declared in the interface.

   ```java
   class Dog implements Animal {
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

   In this example, the `Dog` class implements the `Animal` interface and provides the body for both `eat()` and `sleep()` methods.

3. **Multiple Inheritance via Interfaces**:  
   Java does not support multiple inheritance with classes, but it allows a class to implement multiple interfaces. This helps achieve inheritance-like behavior.

   ```java
   interface Pet {
       void play();
   }

   class Cat implements Animal, Pet {
       @Override
       public void eat() {
           System.out.println("Cat is eating");
       }

       @Override
       public void sleep() {
           System.out.println("Cat is sleeping");
       }

       @Override
       public void play() {
           System.out.println("Cat is playing");
       }
   }
   ```

   The `Cat` class implements both the `Animal` and `Pet` interfaces, thereby inheriting their behavior.

4. **Default and Static Methods in Interfaces**:  
   Java 8 introduced default methods, which allow you to add new methods to interfaces without breaking existing implementations. Static methods can also be defined in interfaces.

   ```java
   interface Vehicle {
       default void honk() {
           System.out.println("Vehicle is honking");
       }

       static void info() {
           System.out.println("This is a Vehicle interface");
       }
   }

   class Car implements Vehicle {
       @Override
       public void honk() {
           System.out.println("Car is honking");
       }
   }
   ```

   In this example, the `Vehicle` interface has a default method `honk()`, which can be overridden by implementing classes. The static method `info()` can be called without creating an instance of the interface.

---

### Example: Utilizing Interfaces in Java
To see how these concepts work together, consider the following practical example:

```java
interface Vehicle {
    void start();
    void stop();
}

class Bike implements Vehicle {
    @Override
    public void start() {
        System.out.println("Bike is starting");
    }

    @Override
    public void stop() {
        System.out.println("Bike is stopping");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle myBike = new Bike();
        myBike.start();
        myBike.stop();
    }
}
```

**Explanation**:
- An interface `Vehicle` is defined with two methods: `start()` and `stop()`.
- The `Bike` class implements the `Vehicle` interface and provides concrete definitions for both methods.
- In the `Main` class, a reference of type `Vehicle` points to a `Bike` object, demonstrating polymorphism.
- When you run this code, it outputs:
  ```
  Bike is starting
  Bike is stopping
  ```

---

### References:
## https://docs.oracle.com/javase/tutorial/java/javaOO/interfaces/index.html  
## https://www.geeksforgeeks.org/interfaces-in-java/  
## https://www.javatpoint.com/java-interfaces