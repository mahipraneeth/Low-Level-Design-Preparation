# SOLID Design Principles and Additional Principles

## Overview
Design principles are guidelines that help software developers write clean, maintainable, and scalable code. They ensure that code is modular, reusable, and easy to understand. This document covers the SOLID principles along with other commonly used principles like KISS, DRY, and YAGNI.

---

## SOLID Principles

### 1. Single Responsibility Principle (SRP)
- **Definition**: A class should have only one reason to change, meaning it should have only one responsibility.

#### Example
```java
class Invoice {
    private double amount;

    public double calculateTotal() {
        // logic to calculate total
    }
}

class InvoicePrinter {
    public void printInvoice(Invoice invoice) {
        // logic to print invoice
    }
}
```

#### Explanation
- The `Invoice` class handles only the calculation, while the `InvoicePrinter` class is responsible for printing. This separation adheres to SRP.

#### Scenarios
- Avoid combining database logic and UI rendering logic in the same class.

#### Interview Questions
1. **What is SRP?**
    - A class should have only one reason to change.
2. **How do you identify multiple responsibilities in a class?**
    - Look for methods performing unrelated tasks.

---

### 2. Open/Closed Principle (OCP)
- **Definition**: Software entities should be open for extension but closed for modification.

#### Example
```java
interface Shape {
    double calculateArea();
}

class Circle implements Shape {
    private double radius;
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle implements Shape {
    private double length, breadth;
    public double calculateArea() {
        return length * breadth;
    }
}
```

#### Explanation
- Adding a new shape requires implementing the `Shape` interface without modifying existing code.

#### Scenarios
- Use interfaces or abstract classes to make code extensible.

#### Interview Questions
1. **What is the Open/Closed Principle?**
    - Classes should be open to extension but closed to modification.
2. **How does OCP promote maintainability?**
    - By allowing new features to be added without altering existing code.

---

### 3. Liskov Substitution Principle (LSP)
- **Definition**: Subtypes must be substitutable for their base types.

#### Example
```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly");
    }
}
```

#### Violation Explanation
- A `Penguin` is not a proper substitute for `Bird` since it cannot fly.

#### Fix
```java
interface Flyable {
    void fly();
}

class Sparrow implements Flyable {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin {
    public void walk() {
        System.out.println("Walking");
    }
}
```

#### Interview Questions
1. **What is LSP?**
    - Subclasses should be replaceable by their base classes.
2. **How can you detect LSP violations?**
    - By checking if substituting a subclass breaks functionality.

---

### 4. Interface Segregation Principle (ISP)
- **Definition**: A class should not be forced to implement methods it does not use.

#### Example
```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

class MultifunctionPrinter implements Printer, Scanner {
    public void print() {
        System.out.println("Printing...");
    }

    public void scan() {
        System.out.println("Scanning...");
    }
}

class SimplePrinter implements Printer {
    public void print() {
        System.out.println("Printing...");
    }
}
```

#### Explanation
- Separate interfaces prevent unused methods in classes like `SimplePrinter`.

#### Interview Questions
1. **What is ISP?**
    - A class should not implement methods it does not use.
2. **How does ISP promote modularity?**
    - By splitting interfaces into smaller, more specific contracts.

---

### 5. Dependency Inversion Principle (DIP)
- **Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions.

#### Example
```java
interface MessageService {
    void sendMessage(String message);
}

class EmailService implements MessageService {
    public void sendMessage(String message) {
        System.out.println("Sending email: " + message);
    }
}

class Notification {
    private MessageService service;

    public Notification(MessageService service) {
        this.service = service;
    }

    public void notifyUser(String message) {
        service.sendMessage(message);
    }
}
```

#### Explanation
- `Notification` depends on `MessageService` abstraction, allowing flexibility.

#### Interview Questions
1. **What is DIP?**
    - High-level modules should depend on abstractions, not implementations.
2. **How does DIP improve testability?**
    - By decoupling classes, making them easier to mock for testing.

---

## Additional Principles

### 1. KISS (Keep It Simple, Stupid)
- **Definition**: Design should be simple and straightforward.

#### Example
```java
public int add(int a, int b) {
    return a + b;
}
```

#### Explanation
- Avoid unnecessary complexity by writing clear and concise code.

#### Interview Questions
1. **What is the KISS principle?**
    - Keep designs simple and avoid over-engineering.

---

### 2. DRY (Don’t Repeat Yourself)
- **Definition**: Avoid duplicating logic by abstracting reusable components.

#### Example
```java
public int calculateArea(int length, int breadth) {
    return length * breadth;
}

public int calculateVolume(int length, int breadth, int height) {
    return calculateArea(length, breadth) * height;
}
```

#### Explanation
- Common logic (`calculateArea`) is reused in `calculateVolume`.

#### Interview Questions
1. **What is DRY?**
    - A principle to eliminate code duplication.
2. **How does DRY improve maintainability?**
    - By centralizing logic, making it easier to update.

---

### 3. YAGNI (You Aren’t Gonna Need It)
- **Definition**: Do not add functionality unless it is absolutely necessary.

#### Example
```java
// Avoid writing methods or classes until they are needed.
```

#### Explanation
- Focus on current requirements and avoid speculative features.

#### Interview Questions
1. **What is YAGNI?**
    - Avoid adding unneeded functionality.

---

### 4. Separation of Concerns (SoC)
- **Definition**: Divide a program into distinct sections, each addressing a specific concern.

#### Example
```java
class DataAccess {
    public void fetchData() {
        // database logic
    }
}

class BusinessLogic {
    private DataAccess dataAccess;

    public void processData() {
        dataAccess.fetchData();
        // processing logic
    }
}

class Presentation {
    public void renderUI() {
        // UI rendering logic
    }
}
```

#### Explanation
- Each class addresses only one layer of the application.

#### Interview Questions
1. **What is Separation of Concerns?**
    - Dividing a program into sections based on functionality.

---

## Conclusion
Understanding and applying these design principles ensures clean, maintainable, and scalable code. They form the foundation for good software design and architecture.
