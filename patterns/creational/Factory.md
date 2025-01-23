# Factory Design Pattern

## Definition
The Factory Design Pattern is a creational pattern that provides an interface for creating objects in a super class, but allows subclasses to alter the type of objects that will be created. This pattern promotes loose coupling by delegating the responsibility of object creation to a separate factory class.

### Key Characteristics:
1. **Encapsulation of Object Creation:** The factory handles the creation logic, keeping the client code decoupled.
2. **Flexibility:** Easy to introduce new types of objects without modifying existing code.
3. **Abstraction:** Clients only interact with the product interface, not the implementation details.

---

## Real-World Scenario
**Scenario:** Payment Gateway Integration in a Spring Boot Application

In a Spring Boot application, you might need to handle payments through multiple gateways, such as PayPal, Stripe, or Razorpay. Each gateway has its own API and configuration, but the client code should not worry about these details. By using the Factory pattern, you can create a factory that produces the appropriate payment processor based on the user's choice.

### Benefits:
- Simplifies adding new payment gateways without modifying the client code.
- Keeps the creation logic centralized and reusable.
- Adheres to the Open/Closed Principle by allowing extension without modification.

---

## Implementation
Here is a detailed implementation of the Factory pattern in Java:

### Step-by-Step Explanation

### 1. Define the Product Interface
```java
public interface PaymentProcessor {
    void processPayment(double amount);
}
```
**Explanation:**
- This interface defines a common contract for all payment processors.
- Any new payment gateway must implement this interface.

### 2. Create Concrete Implementations
```java
public class PayPalProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + " through PayPal.");
    }
}

public class StripeProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + " through Stripe.");
    }
}

public class RazorpayProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + " through Razorpay.");
    }
}
```
**Explanation:**
- These classes implement the `PaymentProcessor` interface and provide specific logic for each payment gateway.

### 3. Create the Factory Class
```java
public class PaymentProcessorFactory {
    public static PaymentProcessor getPaymentProcessor(String type) {
        switch (type.toLowerCase()) {
            case "paypal":
                return new PayPalProcessor();
            case "stripe":
                return new StripeProcessor();
            case "razorpay":
                return new RazorpayProcessor();
            default:
                throw new IllegalArgumentException("Unknown payment type: " + type);
        }
    }
}
```
**Explanation:**
- The factory class contains the logic to decide which payment processor to instantiate based on the input parameter.
- New payment gateways can be added by extending the `switch` statement or equivalent logic without modifying the client code.

### 4. Client Code
```java
public class PaymentService {
    public void processUserPayment(String paymentType, double amount) {
        PaymentProcessor processor = PaymentProcessorFactory.getPaymentProcessor(paymentType);
        processor.processPayment(amount);
    }
}
```
**Explanation:**
- The client interacts with the factory, and the factory takes care of returning the appropriate implementation.
- The client code is not tightly coupled to specific payment processor implementations.

---

## Example in a Spring Boot Application
### Scenario: Notification Service
In a notification service, you may need to send notifications via email, SMS, or push notifications. Using the Factory pattern, you can encapsulate the creation logic for different notification types.

```java
public interface NotificationService {
    void sendNotification(String message);
}

public class EmailNotification implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending Email: " + message);
    }
}

public class SMSNotification implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

public class PushNotification implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending Push Notification: " + message);
    }
}

@Component
public class NotificationFactory {
    public NotificationService getNotificationService(String type) {
        switch (type.toLowerCase()) {
            case "email":
                return new EmailNotification();
            case "sms":
                return new SMSNotification();
            case "push":
                return new PushNotification();
            default:
                throw new IllegalArgumentException("Unknown notification type: " + type);
        }
    }
}
```
**Explanation:**
- The factory determines the appropriate notification service based on the input.
- This pattern makes it easy to add new notification types without modifying the client code.

---

## Advantages of Factory Pattern
1. Promotes loose coupling between the client code and concrete implementations.
2. Centralizes object creation logic, making the code easier to maintain and extend.
3. Adheres to the Open/Closed Principle by allowing new types to be added without altering existing code.
4. Simplifies testing by isolating the creation logic.

## Disadvantages
1. Can introduce complexity if overused for simple object creation.
2. Requires extra classes, which might increase the overall size of the codebase.

---

## Common Interview Questions
1. **What problem does the Factory Design Pattern solve?**
    - **Answer:** It decouples the client code from specific implementations by centralizing object creation logic. This promotes flexibility and makes the system easier to extend.

2. **How does the Factory pattern adhere to the Open/Closed Principle?**
    - **Answer:** New product types can be added by extending the factory logic without modifying existing client code.

3. **What is the difference between Factory Method and Abstract Factory patterns?**
    - **Answer:** The Factory Method pattern defines a method to create objects in a single class, whereas the Abstract Factory pattern provides an interface for creating families of related or dependent objects.

4. **What are the potential downsides of using the Factory pattern?**
    - **Answer:** It can increase complexity by introducing additional classes and may not be necessary for simple object creation tasks.

---

## Conclusion
The Factory Design Pattern is a powerful tool for creating objects in a decoupled and maintainable way. It is especially useful in scenarios where object creation involves complex logic or when there are multiple types of objects that share a common interface. By understanding its strengths and limitations, developers can effectively apply this pattern to build scalable and maintainable systems.

