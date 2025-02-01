# Mediator Design Pattern

## Definition
The Mediator Design Pattern is used to reduce direct dependencies between communicating objects by introducing a mediator that handles interactions. This pattern promotes loose coupling and centralizes communication between objects.

### Key Characteristics:
1. **Centralized Communication:** Mediator acts as the sole communication hub for interacting objects.
2. **Loose Coupling:** Objects do not communicate directly but through the mediator.
3. **Improved Maintainability:** Changes in communication logic affect only the mediator, not the interacting components.

---

## Real-World Scenario
**Scenario:** Chat Room System

Consider a chat room where users communicate with each other. Instead of each user handling direct messaging with every other user, a mediator (chat room) handles message routing.

### Benefits:
- Prevents direct dependencies between users.
- Centralizes message handling logic.
- Simplifies the addition of new users without modifying existing ones.

---

## Implementation
Here is a step-by-step implementation of the Mediator pattern in Java:

### Step-by-Step Explanation

### 1. Define the Mediator Interface
```java
interface ChatMediator {
    void sendMessage(String message, User user);
    void addUser(User user);
}
```
**Explanation:**
- Declares methods for sending messages and adding users.
- Acts as an abstraction for concrete mediators.

### 2. Implement the Concrete Mediator
```java
import java.util.ArrayList;
import java.util.List;

class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();
    
    @Override
    public void addUser(User user) {
        users.add(user);
    }
    
    @Override
    public void sendMessage(String message, User user) {
        for (User u : users) {
            if (u != user) {
                u.receive(message);
            }
        }
    }
}
```
**Explanation:**
- Stores a list of users.
- Forwards messages to all users except the sender.

### 3. Define the Colleague Interface
```java
abstract class User {
    protected ChatMediator mediator;
    protected String name;
    
    public User(ChatMediator mediator, String name) {
        this.mediator = mediator;
        this.name = name;
    }
    
    public abstract void send(String message);
    public abstract void receive(String message);
}
```
**Explanation:**
- Defines common properties and behaviors for users.
- Declares abstract methods for sending and receiving messages.

### 4. Implement Concrete Colleagues
```java
class ChatUser extends User {
    public ChatUser(ChatMediator mediator, String name) {
        super(mediator, name);
    }
    
    @Override
    public void send(String message) {
        System.out.println(name + " sends: " + message);
        mediator.sendMessage(message, this);
    }
    
    @Override
    public void receive(String message) {
        System.out.println(name + " receives: " + message);
    }
}
```
**Explanation:**
- Implements message sending and receiving behavior.
- Uses the mediator to communicate with other users.

### 5. Test the Mediator Pattern
```java
public class MediatorPatternDemo {
    public static void main(String[] args) {
        ChatMediator chatRoom = new ChatRoom();
        
        User user1 = new ChatUser(chatRoom, "Alice");
        User user2 = new ChatUser(chatRoom, "Bob");
        User user3 = new ChatUser(chatRoom, "Charlie");
        
        chatRoom.addUser(user1);
        chatRoom.addUser(user2);
        chatRoom.addUser(user3);
        
        user1.send("Hello everyone!");
    }
}
```
**Explanation:**
- Creates a `ChatRoom` mediator.
- Adds users and allows them to send messages.
- Demonstrates how messages are relayed via the mediator.

---

## Example in a Spring Boot Application
### Scenario: Centralized Event Handling
In a Spring Boot application, event handling can be centralized using the Mediator pattern to decouple event producers and consumers.

```java
import org.springframework.context.ApplicationEvent;
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.context.ApplicationListener;
import org.springframework.stereotype.Component;

class CustomEvent extends ApplicationEvent {
    private String message;
    public CustomEvent(Object source, String message) {
        super(source);
        this.message = message;
    }
    public String getMessage() {
        return message;
    }
}

@Component
class EventMediator {
    private final ApplicationEventPublisher publisher;
    
    public EventMediator(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }
    
    public void publishEvent(String message) {
        publisher.publishEvent(new CustomEvent(this, message));
    }
}

@Component
class EventListenerComponent implements ApplicationListener<CustomEvent> {
    @Override
    public void onApplicationEvent(CustomEvent event) {
        System.out.println("Received event: " + event.getMessage());
    }
}
```

### Usage
```java
@RestController
@RequestMapping("/events")
class EventController {
    private final EventMediator mediator;
    
    public EventController(EventMediator mediator) {
        this.mediator = mediator;
    }
    
    @PostMapping("/publish")
    public void publishEvent(@RequestBody String message) {
        mediator.publishEvent(message);
    }
}
```

**Explanation:**
- Uses Spring's `ApplicationEventPublisher` as a mediator.
- Decouples event producers (controllers) from consumers (listeners).

---

## Advantages of Mediator Pattern
1. **Reduces Coupling:** Objects interact indirectly via a mediator.
2. **Encapsulation of Communication Logic:** Centralizes message handling logic.
3. **Enhances Maintainability:** Easy to modify interactions without affecting dependent objects.

## Disadvantages
1. **Potential Bottleneck:** If the mediator handles too many responsibilities, it can become complex.
2. **Single Point of Failure:** The entire system depends on the mediator’s correct functioning.

---

## Common Interview Questions
1. **What problem does the Mediator pattern solve?**
    - Answer: It reduces direct dependencies between objects by introducing a central mediator for handling communication.

2. **How does the Mediator pattern differ from Observer pattern?**
    - Answer: The Mediator pattern centralizes communication, while the Observer pattern facilitates one-to-many dependencies.

3. **When should you avoid using the Mediator pattern?**
    - Answer: When the mediator becomes too complex, handling excessive responsibilities, leading to a maintenance burden.

4. **How does the Mediator pattern improve code maintainability?**
    - Answer: Changes in communication logic are confined to the mediator, reducing the impact on other components.

---

## Conclusion
The Mediator design pattern provides a structured way to manage communication between multiple objects, reducing coupling and improving maintainability. While beneficial in scenarios like messaging systems and event handling, care must be taken to prevent the mediator from becoming overly complex. Understanding its strengths and limitations allows developers to use it effectively in software design.

