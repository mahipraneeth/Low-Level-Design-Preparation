# Decorator Design Pattern

## Definition
The Decorator Design Pattern allows behavior to be dynamically added to an individual object, without affecting the behavior of other objects from the same class. This pattern is useful when subclassing is not a viable option to extend functionality.

### Key Characteristics:
1. **Flexible Extensibility:** New functionalities can be added dynamically at runtime.
2. **Follows Open-Closed Principle:** Classes remain open for extension but closed for modification.
3. **Wrapper-Based Implementation:** Uses composition instead of inheritance to extend behavior.

---

## Real-World Scenario
**Scenario:** Adding Features to Coffee in a Coffee Shop

Imagine a coffee shop where customers can customize their coffee by adding ingredients like milk, sugar, and caramel. Instead of creating multiple subclasses for each combination, we can use the Decorator pattern to dynamically add features.

### Benefits:
- Avoids subclass explosion by eliminating the need for multiple subclasses.
- Provides dynamic feature addition at runtime.
- Encourages modular and reusable design.

---

## Implementation
Here is a detailed implementation of the Decorator pattern in Java:

### Step-by-Step Explanation

### 1. Base Component Interface
```java
public interface Coffee {
    String getDescription();
    double getCost();
}
```
**Explanation:**
- This interface defines the common behavior of all coffee types.

### 2. Concrete Component
```java
public class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple Coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}
```
**Explanation:**
- Represents a basic coffee without additional features.

### 3. Abstract Decorator
```java
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost();
    }
}
```
**Explanation:**
- Implements the same interface and contains a reference to the wrapped object.

### 4. Concrete Decorators
```java
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Milk";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 1.5;
    }
}

public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", Sugar";
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost() + 0.5;
    }
}
```
**Explanation:**
- These decorators modify the behavior of the `Coffee` object dynamically.

### 5. Client Code
```java
public class CoffeeShop {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " : $" + coffee.getCost());

        coffee = new MilkDecorator(coffee);
        System.out.println(coffee.getDescription() + " : $" + coffee.getCost());

        coffee = new SugarDecorator(coffee);
        System.out.println(coffee.getDescription() + " : $" + coffee.getCost());
    }
}
```
**Output:**
```
Simple Coffee : $5.0
Simple Coffee, Milk : $6.5
Simple Coffee, Milk, Sugar : $7.0
```

---

## Example in a Spring Boot Application
### Scenario: Logging Decorator for a Service Layer
```java
@Service
public class UserService {
    public void createUser(String username) {
        System.out.println("User " + username + " created.");
    }
}
```
#### Creating a Logging Decorator
```java
public class LoggingUserServiceDecorator extends UserService {
    private final UserService userService;

    public LoggingUserServiceDecorator(UserService userService) {
        this.userService = userService;
    }

    @Override
    public void createUser(String username) {
        System.out.println("[LOG] Creating user: " + username);
        userService.createUser(username);
    }
}
```
#### Registering the Decorator as a Bean
```java
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new LoggingUserServiceDecorator(new UserService());
    }
}
```

---

## Advantages of Decorator Pattern
1. **Flexible Runtime Behavior:** Enables dynamic modification of behavior.
2. **Prevents Class Explosion:** Avoids creating multiple subclasses for each combination of features.
3. **Promotes Single Responsibility:** Each decorator adds only a specific responsibility.

## Disadvantages
1. **Complexity Increases:** Multiple decorators can make debugging difficult.
2. **Hard to Manage Order of Execution:** The order in which decorators are applied affects the final result.

---

## Common Interview Questions
1. **How is the Decorator pattern different from Inheritance?**
    - Answer: Inheritance adds behavior at compile time, whereas the Decorator pattern adds behavior at runtime dynamically.

2. **When should you use the Decorator pattern?**
    - Answer: When you need to extend the behavior of objects dynamically without modifying existing code.

3. **Can a decorator modify multiple methods of a class?**
    - Answer: Yes, decorators can override multiple methods of the wrapped class to alter behavior as needed.

4. **How does the Decorator pattern follow the Open-Closed Principle?**
    - Answer: It allows extending functionality without modifying the original class.

---

## Conclusion
The Decorator design pattern is a powerful alternative to inheritance for extending behavior. It enables dynamic behavior addition while keeping the base class unchanged. By using this pattern correctly, developers can write modular and flexible code that adheres to design principles.

