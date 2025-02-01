# Chain of Responsibility Design Pattern

## Definition
The Chain of Responsibility (CoR) pattern allows multiple objects to handle a request sequentially. Each object in the chain either processes the request or passes it to the next handler.

### Key Characteristics:
1. **Decouples sender and receiver** – The sender does not need to know which handler will process the request.
2. **Flexible request processing** – Handlers can process requests partially or fully and then pass them along.
3. **Promotes loose coupling** – New handlers can be added without modifying existing code.

---

## Real-World Scenario
**Scenario:** Request Approval System

Consider a company where an employee’s expense request needs approval. If the expense is within a certain limit, the manager approves it; otherwise, it moves to the director, and so on.

### Benefits:
- Provides a clear hierarchy for handling requests.
- Reduces duplication of approval logic.
- Makes it easy to introduce new approval levels without modifying existing code.

---

## Implementation
Here is a detailed implementation of the Chain of Responsibility pattern in Java:

### Step-by-Step Explanation

### 1. Handler Interface
```java
public interface Approver {
    void setNextApprover(Approver nextApprover);
    void processRequest(int amount);
}
```
**Explanation:**
- Defines a method to set the next handler in the chain.
- Declares a method to process the request.

### 2. Concrete Handlers
```java
public class Manager implements Approver {
    private Approver nextApprover;
    
    @Override
    public void setNextApprover(Approver nextApprover) {
        this.nextApprover = nextApprover;
    }

    @Override
    public void processRequest(int amount) {
        if (amount <= 1000) {
            System.out.println("Manager approved the request.");
        } else if (nextApprover != null) {
            nextApprover.processRequest(amount);
        }
    }
}

public class Director implements Approver {
    private Approver nextApprover;
    
    @Override
    public void setNextApprover(Approver nextApprover) {
        this.nextApprover = nextApprover;
    }

    @Override
    public void processRequest(int amount) {
        if (amount <= 5000) {
            System.out.println("Director approved the request.");
        } else if (nextApprover != null) {
            nextApprover.processRequest(amount);
        }
    }
}
```
**Explanation:**
- `Manager` approves up to $1000; otherwise, it forwards the request.
- `Director` approves up to $5000; otherwise, it forwards the request.

### 3. Client Code
```java
public class ChainOfResponsibilityDemo {
    public static void main(String[] args) {
        Approver manager = new Manager();
        Approver director = new Director();
        
        manager.setNextApprover(director);
        
        System.out.println("Requesting approval for $800:");
        manager.processRequest(800);
        
        System.out.println("Requesting approval for $3000:");
        manager.processRequest(3000);
    }
}
```
**Output:**
```
Requesting approval for $800:
Manager approved the request.

Requesting approval for $3000:
Director approved the request.
```

---

## Example in a Spring Boot Application (Using Spring AOP)
### Scenario: Handling Different Validation Levels in a Request
Spring AOP can be used to apply validation checks dynamically in a chain.

### 1. Validator Interface
```java
public interface Validator {
    void setNextValidator(Validator nextValidator);
    boolean validate(Request request);
}
```

### 2. Concrete Validators
```java
@Component
public class BasicValidation implements Validator {
    private Validator nextValidator;

    @Override
    public void setNextValidator(Validator nextValidator) {
        this.nextValidator = nextValidator;
    }

    @Override
    public boolean validate(Request request) {
        if (request.getData() == null || request.getData().isEmpty()) {
            System.out.println("BasicValidation failed: Data is missing.");
            return false;
        }
        return nextValidator == null || nextValidator.validate(request);
    }
}
```

```java
@Component
public class SecurityValidation implements Validator {
    private Validator nextValidator;

    @Override
    public void setNextValidator(Validator nextValidator) {
        this.nextValidator = nextValidator;
    }

    @Override
    public boolean validate(Request request) {
        if (!request.getUser().isAuthenticated()) {
            System.out.println("SecurityValidation failed: User is not authenticated.");
            return false;
        }
        return nextValidator == null || nextValidator.validate(request);
    }
}
```

### 3. Service Class with AOP
```java
@Aspect
@Component
public class ValidationAspect {
    @Before("execution(* com.example.service.*.*(..)) && args(request)")
    public void validateRequest(JoinPoint joinPoint, Request request) {
        System.out.println("[LOG] Validating request before execution: " + joinPoint.getSignature().getName());
    }
}
```

### 4. Client Code
```java
public class SpringAOPDemo {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        Request request = new Request("UserData", new User(true));
        
        Validator basicValidator = context.getBean(BasicValidation.class);
        Validator securityValidator = context.getBean(SecurityValidation.class);
        
        basicValidator.setNextValidator(securityValidator);
        
        boolean isValid = basicValidator.validate(request);
        if (isValid) {
            System.out.println("Request is valid and can proceed.");
        }
    }
}
```

---

## Advantages of Chain of Responsibility Pattern
1. **Reduces coupling** – The sender does not need to know which handler will process the request.
2. **Flexible Processing** – New handlers can be easily added without modifying existing code.
3. **Supports multiple request handling** – Each handler can process the request or delegate it.

## Disadvantages
1. **May impact performance** – If there are too many handlers, processing time may increase.
2. **No guaranteed handling** – If no handler processes the request, it may go unhandled.

---

## Common Interview Questions
1. **What is the purpose of the Chain of Responsibility pattern?**
    - Answer: It allows multiple objects to process a request sequentially without the sender needing to know who will handle it.

2. **How does it differ from the Strategy pattern?**
    - Answer: The Strategy pattern selects one algorithm dynamically, while CoR allows multiple handlers to process a request.

3. **How does Spring AOP use the Chain of Responsibility pattern?**
    - Answer: AOP interceptors in Spring execute in a chain, applying multiple concerns like logging, validation, and security dynamically.

---

## Conclusion
The Chain of Responsibility pattern is widely used for request handling scenarios such as approval workflows, validation pipelines, and middleware processing. In Spring, it is effectively implemented using AOP for centralized request processing. Understanding this pattern helps developers build flexible and maintainable systems.

