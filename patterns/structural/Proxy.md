# Proxy Design Pattern

## Definition
The Proxy Design Pattern provides a surrogate or placeholder for another object to control access to it. This pattern is useful when dealing with resource-intensive objects, security controls, or remote method invocations.

### Key Characteristics:
1. **Access Control:** Controls access to the real object based on permissions or conditions.
2. **Lazy Initialization:** Creates objects only when they are needed to optimize performance.
3. **Logging and Monitoring:** Intercepts method calls to add logging, authentication, or caching.

---
## Real-World Scenario
### Scenario: Image Loading Optimization
Imagine an application that loads high-resolution images. Instead of loading the full image immediately, a proxy can load a placeholder and fetch the actual image when required.

### Benefits:
- Reduces unnecessary memory consumption.
- Improves application startup time.
- Allows access control and logging.

---

## Implementation
### Step-by-Step Explanation

### 1. Subject Interface
```java
public interface Image {
    void display();
}
```
**Explanation:**
- Defines a common interface for both real and proxy objects.

### 2. Real Object
```java
public class RealImage implements Image {
    private String fileName;
    
    public RealImage(String fileName) {
        this.fileName = fileName;
        loadImageFromDisk();
    }
    
    private void loadImageFromDisk() {
        System.out.println("Loading " + fileName);
    }
    
    @Override
    public void display() {
        System.out.println("Displaying " + fileName);
    }
}
```
**Explanation:**
- Represents the actual object performing the expensive operation.
- Loads the image from disk when instantiated.

### 3. Proxy Object
```java
public class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;
    
    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }
    
    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName);
        }
        realImage.display();
    }
}
```
**Explanation:**
- Creates the real object only when `display()` is called.
- Reduces unnecessary object creation.

### 4. Client Code
```java
public class ProxyPatternDemo {
    public static void main(String[] args) {
        Image image = new ProxyImage("test_image.jpg");
        
        // First call: Loads from disk
        image.display();
        
        // Second call: Uses cached image
        image.display();
    }
}
```
**Output:**
```
Loading test_image.jpg
Displaying test_image.jpg
Displaying test_image.jpg
```

---

## Example in a Spring Boot Application
### Scenario: Proxy for a Secure Service
```java
public interface UserService {
    void getUserDetails(String userId);
}
```

#### Real Implementation
```java
public class RealUserService implements UserService {
    @Override
    public void getUserDetails(String userId) {
        System.out.println("Fetching details for user: " + userId);
    }
}
```

#### Proxy with Access Control
```java
public class UserServiceProxy implements UserService {
    private RealUserService realUserService = new RealUserService();
    private boolean isAdmin;
    
    public UserServiceProxy(boolean isAdmin) {
        this.isAdmin = isAdmin;
    }
    
    @Override
    public void getUserDetails(String userId) {
        if (isAdmin) {
            realUserService.getUserDetails(userId);
        } else {
            System.out.println("Access Denied: Insufficient permissions");
        }
    }
}
```
#### Using the Proxy in Spring Boot
```java
@Service
public class UserServiceFactory {
    public static UserService getUserService(boolean isAdmin) {
        return new UserServiceProxy(isAdmin);
    }
}
```
```java
public class ProxyDemo {
    public static void main(String[] args) {
        UserService userService = UserServiceFactory.getUserService(true);
        userService.getUserDetails("JohnDoe");
        
        UserService restrictedService = UserServiceFactory.getUserService(false);
        restrictedService.getUserDetails("JohnDoe");
    }
}
```
**Output:**
```
Fetching details for user: JohnDoe
Access Denied: Insufficient permissions
```

**Scenario:** Secure Database Access

Imagine a system where database access should only be granted to authenticated users. Instead of directly accessing the database, a proxy can be used to enforce authentication before delegating requests to the actual database handler.

### Benefits:
- Enhances security by restricting unauthorized access.
- Reduces resource consumption by creating objects only when necessary.
- Adds logging, caching, and monitoring without modifying the original class.

---

## Implementation
Here is a detailed implementation of the Proxy pattern in Java:

### Step-by-Step Explanation

### 1. Subject Interface
```java
public interface DatabaseAccessor {
    void executeQuery(String query);
}
```
**Explanation:**
- Defines the common interface for both the real object and the proxy.

### 2. Real Subject
```java
public class RealDatabaseAccessor implements DatabaseAccessor {
    @Override
    public void executeQuery(String query) {
        System.out.println("Executing query: " + query);
    }
}
```
**Explanation:**
- Implements the actual logic for database query execution.

### 3. Proxy Class
```java
public class DatabaseProxy implements DatabaseAccessor {
    private RealDatabaseAccessor realDatabaseAccessor;
    private boolean isAuthenticated;

    public DatabaseProxy(boolean isAuthenticated) {
        this.isAuthenticated = isAuthenticated;
    }

    @Override
    public void executeQuery(String query) {
        if (!isAuthenticated) {
            System.out.println("Access Denied: User is not authenticated.");
            return;
        }
        if (realDatabaseAccessor == null) {
            realDatabaseAccessor = new RealDatabaseAccessor();
        }
        realDatabaseAccessor.executeQuery(query);
    }
}
```
**Explanation:**
- Controls access to `RealDatabaseAccessor` by checking authentication before delegating the request.
- Implements lazy initialization by creating the real object only when required.

### 4. Client Code
```java
public class ProxyPatternDemo {
    public static void main(String[] args) {
        DatabaseAccessor unauthorizedAccess = new DatabaseProxy(false);
        unauthorizedAccess.executeQuery("SELECT * FROM users");

        DatabaseAccessor authorizedAccess = new DatabaseProxy(true);
        authorizedAccess.executeQuery("SELECT * FROM users");
    }
}
```
**Output:**
```
Access Denied: User is not authenticated.
Executing query: SELECT * FROM users
```

---

## Example in a Spring Boot Application (Using Spring AOP)
### Scenario: Logging with Proxy (Aspect-Oriented Programming)
Spring AOP (Aspect-Oriented Programming) is a practical implementation of the Proxy pattern, where additional behavior (like logging, security, or transactions) can be added dynamically.

### 1. Service Class
```java
@Service
public class UserService {
    public void createUser(String username) {
        System.out.println("User " + username + " created.");
    }
}
```
**Explanation:**
- This is the target class where we want to add logging behavior dynamically.

### 2. Logging Aspect (Proxy Implementation)
```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("[LOG] Executing method: " + joinPoint.getSignature().getName());
    }
}
```
**Explanation:**
- The `@Aspect` class acts as a proxy that intercepts method calls and logs before execution.
- `@Before` executes custom logic before the target method runs.

### 3. Configuration to Enable Proxy (AOP)
```java
@EnableAspectJAutoProxy
@Configuration
public class AppConfig {}
```
**Explanation:**
- `@EnableAspectJAutoProxy` enables Spring to create proxies around beans to apply AOP behaviors.

### 4. Client Code
```java
public class SpringAOPDemo {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        UserService userService = context.getBean(UserService.class);
        userService.createUser("JohnDoe");
    }
}
```
**Output:**
```
[LOG] Executing method: createUser
User JohnDoe created.
```

---

## Advantages of Proxy Pattern
1. **Security & Access Control:** Prevents unauthorized access by restricting object creation.
2. **Performance Optimization:** Uses lazy initialization to improve efficiency.
3. **Logging & Monitoring:** Enables centralized logging, caching, and auditing without modifying the core logic.

## Disadvantages
1. **Overhead:** Adds an extra layer of indirection, which might impact performance.
2. **Complexity:** Managing multiple proxy layers can make debugging difficult.

---

## Common Interview Questions
1. **What is the purpose of the Proxy pattern?**
    - Answer: It controls access to another object, adding security, logging, caching, or lazy initialization.

2. **How does Spring AOP use the Proxy pattern?**
    - Answer: Spring AOP dynamically creates proxies around beans to add cross-cutting concerns like logging, transactions, and security.
   
3. **How is the Proxy pattern different from the Decorator pattern?**
    - Answer: The Proxy pattern controls access to an object, while the Decorator pattern adds behavior dynamically.

4. **When should you use the Proxy pattern?**
    - Answer: When you need controlled access to an object, such as for security, logging, or performance reasons.

5. **Can a Proxy be used with multiple real objects?**
    - Answer: Yes, a proxy can manage access to multiple objects if needed.

6. **What are different types of Proxies?**
    - Answer: Virtual Proxy (lazy loading), Protection Proxy (security), Remote Proxy (network communication), and Caching Proxy (performance optimization).

---

## Conclusion
The Proxy design pattern is widely used in security, performance optimizations, and monitoring. In modern applications, it plays a crucial role in frameworks like Spring AOP, where it enables dynamic behavior injection without modifying core logic. By understanding this pattern, developers can build scalable and maintainable applications efficiently.

The Proxy pattern is useful when direct access to an object is costly or needs to be controlled. By implementing proxies, developers can enforce security, improve performance, and introduce additional layers like logging without modifying the original class. This makes the Proxy pattern a powerful tool in enterprise applications.

