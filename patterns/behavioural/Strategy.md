# Strategy Design Pattern

## Definition
The **Strategy Design Pattern** defines a family of algorithms, encapsulates each one, and makes them interchangeable. This pattern allows the algorithm to be selected at runtime, promoting flexibility and reusability.

### Key Characteristics:
1. **Encapsulation of Algorithms**: Each algorithm is encapsulated in a separate class.
2. **Interchangeability**: Strategies can be swapped dynamically without modifying the client code.
3. **Open/Closed Principle**: New strategies can be added without modifying existing code.
4. **Separation of Concerns**: The strategy pattern separates the selection of an algorithm from its implementation.

---

## Real-World Scenario
**Scenario:** Payment Processing System in an E-Commerce Application

Consider an e-commerce platform that supports multiple payment methods, such as Credit Card, PayPal, and Google Pay. Instead of embedding the payment logic within the order processing code, we can use the Strategy pattern to dynamically select the appropriate payment method at runtime.

### Benefits:
- Enables new payment methods to be added without modifying existing payment logic.
- Allows customers to choose their preferred payment option dynamically.
- Enhances code maintainability and scalability.

---

## Implementation
Below is a step-by-step implementation of the Strategy pattern in Java.

### Step 1: Define a Strategy Interface
```java
public interface PaymentStrategy {
    void pay(double amount);
}
```

### Step 2: Implement Concrete Strategies
#### Credit Card Payment Strategy
```java
public class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    private String cardHolderName;

    public CreditCardPayment(String cardNumber, String cardHolderName) {
        this.cardNumber = cardNumber;
        this.cardHolderName = cardHolderName;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using Credit Card.");
    }
}
```

#### PayPal Payment Strategy
```java
public class PayPalPayment implements PaymentStrategy {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using PayPal.");
    }
}
```

#### Google Pay Payment Strategy
```java
public class GooglePayPayment implements PaymentStrategy {
    private String phoneNumber;

    public GooglePayPayment(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using Google Pay.");
    }
}
```

### Step 3: Create a Context Class
```java
public class PaymentContext {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    public void processPayment(double amount) {
        if (paymentStrategy == null) {
            throw new IllegalStateException("Payment strategy is not set");
        }
        paymentStrategy.pay(amount);
    }
}
```

### Step 4: Test the Strategy Pattern
```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
        PaymentContext context = new PaymentContext();

        // Pay using Credit Card
        context.setPaymentStrategy(new CreditCardPayment("1234-5678-9876-5432", "John Doe"));
        context.processPayment(100.0);

        // Pay using PayPal
        context.setPaymentStrategy(new PayPalPayment("john.doe@example.com"));
        context.processPayment(200.0);

        // Pay using Google Pay
        context.setPaymentStrategy(new GooglePayPayment("+1234567890"));
        context.processPayment(150.0);
    }
}
```

**Output:**
```
Paid 100.0 using Credit Card.
Paid 200.0 using PayPal.
Paid 150.0 using Google Pay.
```

---

## Example in a Spring Boot Application
### Scenario: Dynamic Logging Strategy
In a Spring Boot application, different logging strategies (e.g., Console, File, Database) can be implemented using the Strategy pattern.

#### Define Logging Strategy Interface
```java
public interface LoggingStrategy {
    void log(String message);
}
```

#### Implement Concrete Logging Strategies
```java
@Component("consoleLogger")
public class ConsoleLogging implements LoggingStrategy {
    @Override
    public void log(String message) {
        System.out.println("Console Log: " + message);
    }
}
```

```java
@Component("fileLogger")
public class FileLogging implements LoggingStrategy {
    @Override
    public void log(String message) {
        System.out.println("Logging to file: " + message);
    }
}
```

#### Create Logging Context
```java
@Component
public class LoggerContext {
    private LoggingStrategy loggingStrategy;

    @Autowired
    private ApplicationContext context;

    public void setLoggingStrategy(String strategyBeanName) {
        this.loggingStrategy = context.getBean(strategyBeanName, LoggingStrategy.class);
    }

    public void logMessage(String message) {
        if (loggingStrategy == null) {
            throw new IllegalStateException("Logging strategy is not set");
        }
        loggingStrategy.log(message);
    }
}
```

#### Use the Logger Context
```java
@RestController
@RequestMapping("/log")
public class LoggingController {
    @Autowired
    private LoggerContext loggerContext;

    @GetMapping("/logMessage")
    public ResponseEntity<String> logMessage(@RequestParam String strategy, @RequestParam String message) {
        loggerContext.setLoggingStrategy(strategy);
        loggerContext.logMessage(message);
        return ResponseEntity.ok("Logged successfully using " + strategy);
    }
}
```

---

## Advantages of Strategy Pattern
1. **Improves Code Maintainability**: Avoids long if-else statements by encapsulating algorithms in separate classes.
2. **Enhances Scalability**: New strategies can be added without modifying existing code.
3. **Supports Open/Closed Principle**: Allows extension without modifying existing implementations.
4. **Promotes Loose Coupling**: The client code depends only on the strategy interface, not concrete implementations.

## Disadvantages
1. **Increased Complexity**: Introduces additional classes, making code structure more complex.
2. **Requires Careful Design**: Managing multiple strategy classes might be challenging if not well-structured.

---

## Common Interview Questions
1. **What problem does the Strategy pattern solve?**
    - It solves the problem of selecting an algorithm dynamically at runtime.

2. **How does the Strategy pattern follow the Open/Closed Principle?**
    - New strategies can be introduced without modifying the existing code.

3. **How is Strategy different from State Pattern?**
    - Strategy pattern is used for interchangeable behaviors, while the State pattern is used to represent different states of an object.

4. **Can Strategy Pattern be implemented using Functional Interfaces?**
    - Yes, in Java 8+, strategies can be implemented using lambda expressions to avoid creating separate classes.

---

## Conclusion
The Strategy pattern is a powerful design pattern that promotes flexibility, scalability, and maintainability. It is widely used in scenarios where multiple algorithms or behaviors need to be selected dynamically at runtime, such as payment processing, logging, and validation in enterprise applications.

