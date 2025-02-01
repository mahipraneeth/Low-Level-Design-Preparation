# State Design Pattern

## Definition
The State Design Pattern allows an object to alter its behavior when its internal state changes. This pattern encapsulates state-specific behaviors within separate state objects and delegates state transitions to these objects.

### Key Characteristics:
1. **Encapsulation of States:** Each state is represented as a separate class.
2. **State Transitions:** The context delegates the behavior to the current state and transitions between states dynamically.
3. **Improved Maintainability:** Adding new states does not require modifying the context class.

---

## Real-World Scenario
**Scenario:** Order Processing System in an E-commerce Application

In an e-commerce application, an order goes through multiple states: `Pending`, `Processed`, `Shipped`, and `Delivered`. The behavior of the order (e.g., cancellation policy, notifications) varies depending on its state. By using the State pattern, we encapsulate each order state separately and allow dynamic transitions between them.

### Benefits:
- Encapsulates state-specific logic, reducing complexity in the main context class.
- Enables easy extension by adding new states without modifying existing code.
- Ensures controlled state transitions.

---

## Implementation
Here is a detailed implementation of the State pattern in Java:

### Step-by-Step Explanation

### 1. Define the State Interface
```java
public interface OrderState {
    void processOrder(OrderContext context);
}
```
**Explanation:**
- This interface defines a contract for all states.
- Each state class will implement this interface.

### 2. Implement Concrete States
```java
public class PendingState implements OrderState {
    @Override
    public void processOrder(OrderContext context) {
        System.out.println("Order is now Processed.");
        context.setState(new ProcessedState());
    }
}

public class ProcessedState implements OrderState {
    @Override
    public void processOrder(OrderContext context) {
        System.out.println("Order is now Shipped.");
        context.setState(new ShippedState());
    }
}

public class ShippedState implements OrderState {
    @Override
    public void processOrder(OrderContext context) {
        System.out.println("Order is now Delivered.");
        context.setState(new DeliveredState());
    }
}

public class DeliveredState implements OrderState {
    @Override
    public void processOrder(OrderContext context) {
        System.out.println("Order has already been delivered. No further processing.");
    }
}
```
**Explanation:**
- Each state class implements the `OrderState` interface.
- Each state defines behavior for transitioning to the next state.

### 3. Create the Context Class
```java
public class OrderContext {
    private OrderState state;

    public OrderContext() {
        this.state = new PendingState(); // Default state
    }

    public void setState(OrderState state) {
        this.state = state;
    }

    public void processOrder() {
        state.processOrder(this);
    }
}
```
**Explanation:**
- The `OrderContext` maintains the current state.
- It delegates state-specific behavior to the current state object.

### 4. Usage Example
```java
public class StatePatternDemo {
    public static void main(String[] args) {
        OrderContext order = new OrderContext();
        order.processOrder(); // Moves to Processed
        order.processOrder(); // Moves to Shipped
        order.processOrder(); // Moves to Delivered
        order.processOrder(); // Already Delivered
    }
}
```

---

## Example in a Spring Boot Application
### Scenario: Workflow Management
A workflow management system may have states such as `Draft`, `Review`, and `Published`. Using the State pattern, we can handle state transitions dynamically.

```java
@Component
public class WorkflowContext {
    private WorkflowState state;
    
    public WorkflowContext() {
        this.state = new DraftState();
    }

    public void setState(WorkflowState state) {
        this.state = state;
    }

    public void processWorkflow() {
        state.process(this);
    }
}
```
**Explanation:**
- Spring manages `WorkflowContext` as a singleton bean.
- The state logic is encapsulated, making transitions clean and maintainable.

---

## Advantages of State Pattern
1. **Encapsulation of State-Specific Logic:** Keeps state-specific behavior separate, improving maintainability.
2. **Easy State Transitions:** Allows dynamic state changes at runtime.
3. **Extensibility:** Adding new states requires minimal changes to existing code.

## Disadvantages
1. **Increased Number of Classes:** Each state requires a separate class, which may lead to class explosion.
2. **Complexity in Simple Use Cases:** Not always necessary for scenarios with limited state transitions.

---

## Common Interview Questions
1. **What is the difference between State and Strategy patterns?**
    - Answer: State pattern is used when an object's behavior changes based on its state, while Strategy pattern allows selecting an algorithm dynamically.

2. **How does the State pattern improve code maintainability?**
    - Answer: It encapsulates state-specific behavior into separate classes, making it easier to modify or extend without changing the context class.

3. **Can we implement the State pattern using Enums?**
    - Answer: Yes, for simple cases, we can use Enums with method implementations to represent states.

4. **What happens if we do not transition properly between states?**
    - Answer: The object might get stuck in an inconsistent state, leading to unexpected behavior.

---

## Conclusion
The State design pattern is an effective way to manage object behavior based on state changes. It improves maintainability by encapsulating state logic within separate classes. While it increases the number of classes, its benefits in scalability and flexibility make it a valuable design pattern for managing stateful objects effectively.

