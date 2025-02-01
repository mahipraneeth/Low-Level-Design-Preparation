# Facade Design Pattern

## Definition
The Facade Design Pattern provides a simplified and unified interface to a complex system of classes, libraries, or APIs. It acts as a single entry point, shielding clients from the complexity of underlying subsystems.

### Key Characteristics:
1. **Simplifies Complex Systems:** Provides a high-level API to a complex set of subsystems.
2. **Encapsulates Implementation Details:** Clients interact with the facade instead of multiple individual components.
3. **Reduces Coupling:** Loosely couples the client with the system components.

---

## Real-World Scenario
**Scenario:** Home Theater System

Imagine a home theater system that includes a DVD player, projector, surround sound system, and lighting. Instead of interacting with each component separately, a `HomeTheaterFacade` class can provide a single method like `watchMovie()`, which internally controls all the components.

### Benefits:
- Simplifies user interaction with the system.
- Reduces dependencies between clients and subsystems.
- Improves maintainability and readability.

---

## Implementation
Here is a detailed implementation of the Facade pattern in Java:

### Step-by-Step Explanation

### 1. Subsystem Classes
```java
class DVDPlayer {
    void turnOn() { System.out.println("DVD Player turned on"); }
    void play(String movie) { System.out.println("Playing movie: " + movie); }
    void turnOff() { System.out.println("DVD Player turned off"); }
}

class Projector {
    void turnOn() { System.out.println("Projector turned on"); }
    void setInput(String source) { System.out.println("Projector source set to: " + source); }
    void turnOff() { System.out.println("Projector turned off"); }
}

class SoundSystem {
    void turnOn() { System.out.println("Sound System turned on"); }
    void setVolume(int level) { System.out.println("Volume set to: " + level); }
    void turnOff() { System.out.println("Sound System turned off"); }
}
```
**Explanation:**
- These classes represent individual subsystems.

### 2. Facade Class
```java
class HomeTheaterFacade {
    private DVDPlayer dvdPlayer;
    private Projector projector;
    private SoundSystem soundSystem;

    public HomeTheaterFacade(DVDPlayer dvdPlayer, Projector projector, SoundSystem soundSystem) {
        this.dvdPlayer = dvdPlayer;
        this.projector = projector;
        this.soundSystem = soundSystem;
    }

    public void watchMovie(String movie) {
        System.out.println("Setting up movie experience...");
        projector.turnOn();
        projector.setInput("DVD");
        soundSystem.turnOn();
        soundSystem.setVolume(10);
        dvdPlayer.turnOn();
        dvdPlayer.play(movie);
    }

    public void endMovie() {
        System.out.println("Shutting down home theater...");
        dvdPlayer.turnOff();
        soundSystem.turnOff();
        projector.turnOff();
    }
}
```
**Explanation:**
- The `HomeTheaterFacade` provides a simple interface to the subsystems.
- Methods like `watchMovie()` and `endMovie()` encapsulate complex operations.

### 3. Client Code
```java
public class HomeTheaterTest {
    public static void main(String[] args) {
        DVDPlayer dvd = new DVDPlayer();
        Projector projector = new Projector();
        SoundSystem sound = new SoundSystem();

        HomeTheaterFacade homeTheater = new HomeTheaterFacade(dvd, projector, sound);
        homeTheater.watchMovie("Inception");
        homeTheater.endMovie();
    }
}
```
**Output:**
```
Setting up movie experience...
Projector turned on
Projector source set to: DVD
Sound System turned on
Volume set to: 10
DVD Player turned on
Playing movie: Inception
Shutting down home theater...
DVD Player turned off
Sound System turned off
Projector turned off
```

---

## Example in a Spring Boot Application
### Scenario: Simplifying Database Access with a Facade
```java
@Service
public class DatabaseFacade {
    private final UserRepository userRepository;
    private final OrderRepository orderRepository;

    public DatabaseFacade(UserRepository userRepository, OrderRepository orderRepository) {
        this.userRepository = userRepository;
        this.orderRepository = orderRepository;
    }

    public User getUserWithOrders(Long userId) {
        User user = userRepository.findById(userId).orElseThrow();
        user.setOrders(orderRepository.findByUserId(userId));
        return user;
    }
}
```
**Explanation:**
- The `DatabaseFacade` provides a single method to retrieve a user and their orders, abstracting multiple repository calls.

---

## Advantages of Facade Pattern
1. **Simplifies Client Interaction:** Reduces dependencies and provides a clear API.
2. **Encapsulates Complexity:** Clients do not need to understand the internal workings of the system.
3. **Improves Maintainability:** Changes in subsystems do not directly impact clients.

## Disadvantages
1. **Limited Flexibility:** If additional functionality is needed, clients may need to bypass the facade.
2. **Potential Performance Overhead:** If the facade adds extra layers without optimization, it may reduce performance.

---

## Common Interview Questions
1. **What is the primary purpose of the Facade pattern?**
    - Answer: To provide a simplified interface to a complex subsystem.

2. **How does the Facade pattern improve maintainability?**
    - Answer: Changes in subsystem components do not directly affect clients as they interact only with the facade.

3. **What is the difference between a Facade and a Decorator?**
    - Answer: A Facade simplifies access to a subsystem, while a Decorator dynamically adds functionality to objects.

4. **Can a Facade be used with a Singleton?**
    - Answer: Yes, a facade can be implemented as a Singleton to ensure a single entry point.

---

## Conclusion
The Facade pattern is a great choice for managing complexity by providing a unified and simplified API. It improves maintainability, reduces coupling, and makes large systems easier to use.

