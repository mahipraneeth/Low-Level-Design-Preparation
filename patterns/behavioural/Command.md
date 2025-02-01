# Command Design Pattern

## Definition
The Command Design Pattern is a behavioral pattern that encapsulates a request as an object, thereby allowing parameterization of clients with different requests, queuing requests, and logging the history of executed commands. It promotes loose coupling between the sender and receiver of a request.

### Key Characteristics:
1. **Encapsulation of Requests:** Treats requests as objects, making them more flexible.
2. **Decoupling of Sender and Receiver:** The invoker does not need to know about the receiver’s implementation details.
3. **Support for Undo/Redo Operations:** By maintaining a history of executed commands, actions can be reversed.
4. **Easier Command Composition:** Commands can be queued, logged, or executed at different times.
5. **Transactional Support:** Can be used in database operations to commit or rollback transactions.
6. **Disaster Recovery:** Helps in re-executing the history of commands to restore a system's state after a failure.

---

## Real-World Scenario
**Scenario:** Remote Control System for Smart Home Devices

In a smart home system, a remote control can issue different commands to turn lights on/off, adjust thermostats, or operate smart locks. Using the Command pattern, each action (e.g., turning a light on) is encapsulated as a command object, enabling flexible execution and undo functionality.

### Benefits:
- Provides flexibility to add new commands without modifying the remote control.
- Enables macro commands that execute multiple operations at once.
- Supports undo operations for user actions.

---

## Implementation
Here is a detailed implementation of the Command pattern in Java:

### Step-by-Step Explanation

### 1. Define the Command Interface
```java
public interface Command {
    void execute();
    void undo();
}
```
**Explanation:**
- The `Command` interface declares `execute()` to perform an action and `undo()` to revert it.

### 2. Create Concrete Command Classes
```java
public class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOn();
    }

    @Override
    public void undo() {
        light.turnOff();
    }
}
```
```java
public class LightOffCommand implements Command {
    private Light light;

    public LightOffCommand(Light light) {
        this.light = light;
    }

    @Override
    public void execute() {
        light.turnOff();
    }

    @Override
    public void undo() {
        light.turnOn();
    }
}
```
**Explanation:**
- `LightOnCommand` and `LightOffCommand` encapsulate the operations of turning a light on and off.

### 3. Create the Receiver (Light Class)
```java
public class Light {
    public void turnOn() {
        System.out.println("Light is ON");
    }

    public void turnOff() {
        System.out.println("Light is OFF");
    }
}
```
**Explanation:**
- The `Light` class acts as the receiver that performs actual operations.

### 4. Implement the Invoker (Remote Control)
```java
public class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }

    public void pressUndo() {
        command.undo();
    }
}
```
**Explanation:**
- The `RemoteControl` acts as the invoker, triggering command execution without knowing the receiver’s details.

### 5. Client Code to Use the Pattern
```java
public class CommandPatternDemo {
    public static void main(String[] args) {
        Light livingRoomLight = new Light();
        Command lightOn = new LightOnCommand(livingRoomLight);
        Command lightOff = new LightOffCommand(livingRoomLight);

        RemoteControl remote = new RemoteControl();
        remote.setCommand(lightOn);
        remote.pressButton();
        remote.pressUndo();
    }
}
```
**Explanation:**
- The client creates command objects and assigns them to the `RemoteControl` for execution.

---

## Example in a Spring Boot Application
### Scenario: Job Scheduling System
A Spring Boot application can use the Command pattern to schedule and execute tasks dynamically.

```java
@Component
public class JobScheduler {
    private final List<Command> jobQueue = new ArrayList<>();

    public void addJob(Command job) {
        jobQueue.add(job);
    }

    public void runJobs() {
        for (Command job : jobQueue) {
            job.execute();
        }
    }
}
```

**Explanation:**
- The `JobScheduler` manages queued commands and executes them in sequence.

---

## Command Pattern in Transaction Management and Disaster Recovery
### Scenario: Database Transaction Handling
The Command pattern is useful in managing database transactions where each operation (insert, update, delete) is treated as a command.

```java
public class InsertCommand implements Command {
    private Database database;
    private String data;

    public InsertCommand(Database database, String data) {
        this.database = database;
        this.data = data;
    }

    @Override
    public void execute() {
        database.insert(data);
    }

    @Override
    public void undo() {
        database.rollbackLastInsert();
    }
}
```

### Scenario: Disaster Recovery
A system crash or failure can be recovered by replaying stored command history.

```java
public class CommandHistory {
    private Stack<Command> commandStack = new Stack<>();

    public void executeCommand(Command command) {
        command.execute();
        commandStack.push(command);
    }

    public void rollback() {
        while (!commandStack.isEmpty()) {
            commandStack.pop().undo();
        }
    }
}
```

---

## Advantages of Command Pattern
1. **Decouples the sender and receiver**, making code more maintainable.
2. **Supports undo functionality** for reversible actions.
3. **Allows queuing and scheduling of commands**, enabling macro commands.
4. **Enhances testability** by isolating commands as independent objects.
5. **Provides robust transaction management** with commit/rollback capabilities.
6. **Aids in disaster recovery** by re-executing command history.

## Disadvantages
1. **Increases code complexity** due to additional command classes.
2. **Potential performance overhead** if too many command objects are created.

---

## Conclusion
The Command pattern is an essential behavioral pattern that encapsulates requests as objects, providing flexibility, maintainability, and extensibility. It is widely used in GUI applications, job scheduling, transactional systems, and disaster recovery mechanisms.

