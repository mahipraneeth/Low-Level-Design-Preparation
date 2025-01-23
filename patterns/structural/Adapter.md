# Adapter Design Pattern

## Definition
The Adapter Design Pattern is a structural pattern that acts as a bridge between two incompatible interfaces. It allows an existing class or system to work with another class or system without modifying their source code. The adapter converts the interface of one class into another interface that the client expects.

### Key Characteristics:
1. **Interface Bridging:** The pattern connects incompatible interfaces to enable collaboration.
2. **Reusability:** Promotes reuse of existing functionality without modification.
3. **Single Responsibility:** Ensures the adapter handles the responsibility of interface conversion without impacting the existing system.

---

## Real-World Scenario

### Scenario 1: Payment Gateway Integration in a Spring Boot Application

Imagine a Spring Boot application that needs to integrate with multiple payment gateways (e.g., PayPal, Stripe, and Square). Each payment gateway has its own API with different interfaces. The Adapter pattern can be used to create a common interface (e.g., `PaymentGateway`) that all payment gateways implement via adapters.

#### Benefits:
- Simplifies integration by providing a uniform interface.
- Decouples the application logic from specific payment gateway APIs.

### Scenario 2: Legacy System Integration

A legacy system generates reports in XML format, but a modern reporting tool expects JSON input. An adapter can be used to convert the XML output into JSON, enabling seamless integration without modifying the legacy system or the modern tool.

#### Benefits:
- Facilitates interoperability between old and new systems.
- Avoids costly refactoring of the legacy system.

---

## Implementation

### Step-by-Step Explanation

### 1. Define the Target Interface

```java
public interface PaymentGateway {
    void processPayment(double amount);
}
```

**Explanation:**
- The `PaymentGateway` interface represents the expected interface that the client code will use.

---

### 2. Implement Adaptee Classes

#### Example: PayPal Payment

```java
public class PayPal {
    public void makePayment(double amount) {
        System.out.println("Payment of $" + amount + " processed via PayPal.");
    }
}
```

#### Example: Stripe Payment

```java
public class Stripe {
    public void completePayment(double amount) {
        System.out.println("Payment of $" + amount + " processed via Stripe.");
    }
}
```

**Explanation:**
- `PayPal` and `Stripe` classes are the existing implementations with their own unique methods.

---

### 3. Create Adapter Classes

#### Example: PayPal Adapter

```java
public class PayPalAdapter implements PaymentGateway {
    private PayPal payPal;

    public PayPalAdapter(PayPal payPal) {
        this.payPal = payPal;
    }

    @Override
    public void processPayment(double amount) {
        payPal.makePayment(amount);
    }
}
```

#### Example: Stripe Adapter

```java
public class StripeAdapter implements PaymentGateway {
    private Stripe stripe;

    public StripeAdapter(Stripe stripe) {
        this.stripe = stripe;
    }

    @Override
    public void processPayment(double amount) {
        stripe.completePayment(amount);
    }
}
```

**Explanation:**
- The adapter classes implement the `PaymentGateway` interface and translate its methods into calls to the adaptee classes' methods.

---

### 4. Client Code

```java
public class PaymentProcessor {
    public static void main(String[] args) {
        PaymentGateway payPalGateway = new PayPalAdapter(new PayPal());
        PaymentGateway stripeGateway = new StripeAdapter(new Stripe());

        payPalGateway.processPayment(100.00);
        stripeGateway.processPayment(200.00);
    }
}
```

**Output:**
```
Payment of $100.0 processed via PayPal.
Payment of $200.0 processed via Stripe.
```

**Explanation:**
- The client code interacts with the `PaymentGateway` interface, making it independent of specific payment gateway implementations.

---

## Advantages of Adapter Pattern

1. **Improved Reusability:** Allows existing code to work with new interfaces without modification.
2. **Decoupling:** Decouples the client code from the specific implementation of dependencies.
3. **Interoperability:** Enables integration of incompatible systems.

## Disadvantages

1. **Increased Complexity:** Adds extra layers of abstraction, which may increase complexity.
2. **Performance Overhead:** The adapter layer may introduce slight performance overhead due to method delegation.

---

## Adapter vs. Factory Pattern

### Key Differences:
| **Aspect**             | **Adapter Pattern**                                        | **Factory Pattern**                                     |
|-------------------------|-----------------------------------------------------------|--------------------------------------------------------|
| **Purpose**             | Converts an interface to another expected by the client.  | Abstract and centralize object creation.               |
| **Focus**               | Interfacing between systems.                              | Controlling and standardizing object instantiation.    |
| **Type of Pattern**     | Structural pattern (focuses on object composition).       | Creational pattern (focuses on object creation).       |
| **Role**                | Modifies the interface of an existing class.              | Creates objects based on conditions or input.          |
| **Example Use Case**    | Integrating third-party APIs with mismatched interfaces.  | Creating different `Notification` types at runtime.    |

---

## Common Interview Questions

1. **What is the Adapter Design Pattern?**
    - **Answer:** It is a structural design pattern that bridges incompatible interfaces, allowing systems to work together without modifying their source code.

2. **When would you use the Adapter pattern?**
    - **Answer:** When integrating with third-party libraries, legacy systems, or APIs that have interfaces different from what the client expects.

3. **What are the advantages of using the Adapter pattern?**
    - **Answer:** It promotes code reuse, improves compatibility, and decouples the client from specific implementations.

4. **What are the different types of Adapters?**
    - **Answer:** Class Adapter (uses inheritance) and Object Adapter (uses composition).

5. **Can you explain the difference between Adapter and Factory patterns?**
    - **Answer:** The Adapter pattern focuses on interface compatibility, while the Factory pattern abstracts and centralizes object creation. Adapter ensures systems work together, while Factory simplifies object creation logic.

6. **Can you provide a real-world example of the Adapter pattern?**
    - **Answer:** Payment gateway integration, where each gateway has a unique API, and an adapter provides a common interface for the application.

---

## Conclusion
The Adapter Design Pattern is a powerful tool for achieving compatibility between systems with incompatible interfaces. By leveraging this pattern, developers can reuse existing code, simplify integration, and ensure flexibility in evolving software systems.

