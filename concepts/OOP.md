# OOP Concepts

Object-Oriented Programming (OOP) is a programming paradigm that uses objects and classes to design and develop applications. Here are the core concepts of OOP, explained in detail with example classes and advanced interview questions.

---

## 1. Encapsulation

Encapsulation bundles data (fields) and methods (functions) that operate on the data into a single unit, typically a class. It restricts direct access to some of an object's components, ensuring controlled interaction through public methods.

### Detailed Explanation:
- Encapsulation ensures that the object's internal state is hidden from the outside world.
- Access modifiers like `private`, `protected`, and `public` are used to control access.
- Getter and setter methods are used to provide controlled access to private fields.
- Encapsulation helps in improving modularity and maintainability by separating implementation from the interface.

### Example:
```java
public class BankAccount {
    private String accountNumber;
    private double balance;

    // Constructor
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }

    // Getter for balance
    public double getBalance() {
        return balance;
    }

    // Deposit method
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    // Withdraw method
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }
}
```

### Interview Questions:
1. **Why is encapsulation important in OOP?**
    - Encapsulation ensures that the internal details of a class are hidden from the outside world, providing controlled access and improving maintainability. It also helps to protect data from unauthorized modifications.

2. **Can you enforce immutability using encapsulation?**
    - Yes, immutability can be enforced by declaring all fields as `private` and `final`, and not providing setters or methods that modify the object's state. Any field that is mutable should be deeply copied in the getter method.

3. **How would you prevent someone from overriding a getter or setter in Java?**
    - You can declare the getter or setter method as `final` in the class. Alternatively, you can make the class itself `final` to prevent inheritance altogether.

4. **Can encapsulation lead to overuse of getters and setters?**
    - Yes, excessive use of getters and setters can sometimes lead to anemic domain models, where the class does not encapsulate behavior effectively. It's better to design methods that perform meaningful operations on the object's data instead of exposing the data directly.

---

## 2. Abstraction

Abstraction hides implementation details and shows only the necessary features of an object. This is typically achieved using abstract classes or interfaces.

### Detailed Explanation:
- **Abstract classes** can have both abstract methods (without implementation) and concrete methods (with implementation).
- **Interfaces** (especially after Java 8) can have default and static methods in addition to abstract methods.
- Abstraction helps reduce code complexity and improves maintainability by focusing on essential operations rather than implementation details.

### Example:
```java
// Abstract class
abstract class Shape {
    // Abstract method
    public abstract double calculateArea();

    // Concrete method
    public void display() {
        System.out.println("This is a shape.");
    }
}

class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

// Interface Example
interface Animal {
    void sound();
}

class Dog implements Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks.");
    }
}
```

### Interview Questions:
1. **What is the difference between an abstract class and an interface?**
    - Abstract classes can have fields, constructors, and concrete methods, while interfaces (prior to Java 8) could only have abstract methods. After Java 8, interfaces can include default and static methods, but they cannot have instance fields.

2. **Can an abstract class have a constructor? Why or why not?**
    - Yes, an abstract class can have a constructor. Although you cannot instantiate an abstract class directly, the constructor is called when a subclass object is created to initialize the abstract class's fields.

3. **What are default methods in interfaces? Why were they introduced in Java 8?**
    - Default methods in interfaces provide a way to add new functionality to interfaces without breaking existing implementations. They allow interfaces to have method bodies and can be overridden in implementing classes.

4. **How is abstraction different from encapsulation?**
    - Abstraction focuses on hiding the implementation details and showing only the essential features. Encapsulation focuses on bundling the data and methods together and restricting access to them.

5. **Can a class implement multiple interfaces?**
    - Yes, a class can implement multiple interfaces in Java, allowing for multiple inheritance of behavior.

---

## 3. Inheritance

Inheritance allows one class (child) to inherit the properties and behaviors of another class (parent). This promotes code reuse and establishes an "is-a" relationship.

### Detailed Explanation:
- A child class can override methods of the parent class to provide specific behavior.
- The `super` keyword is used to refer to the parent class's members.
- Java supports single inheritance but allows multiple inheritance through interfaces.

### Example:
```java
class Vehicle {
    protected String brand;

    public Vehicle(String brand) {
        this.brand = brand;
    }

    public void drive() {
        System.out.println(brand + " is driving.");
    }
}

class Car extends Vehicle {
    private int numberOfDoors;

    public Car(String brand, int numberOfDoors) {
        super(brand);
        this.numberOfDoors = numberOfDoors;
    }

    public void displayDetails() {
        System.out.println(brand + " has " + numberOfDoors + " doors.");
    }
}
```

### Interview Questions:
1. **What is the difference between method overriding and method hiding?**
    - Method overriding occurs when a subclass provides a specific implementation for a method already defined in its parent class. Method hiding happens when a static method in a child class has the same name and signature as one in the parent class, hiding the parent's static method.

2. **How does Java handle the "diamond problem" in inheritance?**
    - Java handles the diamond problem using interfaces. If a class implements multiple interfaces that define the same default method, the implementing class must override the method to resolve ambiguity.

3. **What happens if the parent class's constructor is private?**
    - If the parent class's constructor is private, it cannot be directly invoked by any subclass, effectively preventing inheritance.

4. **Can you inherit from multiple classes in Java?**
    - No, Java does not support multiple inheritance with classes to avoid ambiguity (diamond problem). However, you can achieve multiple inheritance through interfaces.

5. **Why should you avoid deep inheritance hierarchies?**
    - Deep inheritance hierarchies can make code more complex, harder to understand, and more challenging to maintain. Composition is often preferred for better flexibility and simplicity.

---

## 4. Polymorphism

Polymorphism enables methods to perform different tasks based on the object that invokes them. It is of two types:
- **Compile-time Polymorphism:** Achieved through method overloading. This is resolved during compile time because the method signature determines which method to call.
- **Runtime Polymorphism:** Achieved through method overriding. This is resolved at runtime based on the object's actual type.

### Example:
```java
// Compile-time Polymorphism
class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
}

// Runtime Polymorphism
class Animal {
    public void sound() {
        System.out.println("Animal makes a sound.");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks.");
    }
}
```

### Interview Questions:
1. **Why is method overloading called compile-time polymorphism?**
    - Method overloading is resolved during compile time because the compiler determines the method to call based on the method's name, argument types, and number of arguments.

2. **Can you overload a method by changing only the return type?**
    - No, overloading a method by only changing the return type is not allowed in Java
