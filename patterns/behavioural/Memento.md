# Memento Design Pattern

## Definition
The Memento Design Pattern is used to capture and restore an object's internal state without exposing its implementation details. This pattern is useful when implementing undo/redo functionality in applications.

### Key Characteristics:
1. **State Preservation:** Allows an object's state to be saved and restored.
2. **Encapsulation:** The saved state is kept hidden from external modifications.
3. **Separation of Concerns:** The originator, caretaker, and memento roles are clearly defined.

---

## Real-World Scenario
**Scenario:** Undo Functionality in a Text Editor

Consider a text editor that allows users to undo and redo actions. By using the Memento pattern, we can save snapshots of the text at different points and restore them as needed.

### Benefits:
- Provides an easy way to implement undo/redo functionality.
- Maintains encapsulation by keeping the internal state hidden.
- Enhances user experience by allowing error recovery.

---

## Implementation
Here is a step-by-step implementation of the Memento pattern in Java:

### Step-by-Step Explanation

### 1. Define the Memento Class
```java
class Memento {
    private final String state;
    
    public Memento(String state) {
        this.state = state;
    }
    
    public String getState() {
        return state;
    }
}
```
**Explanation:**
- The `Memento` class stores the state of an object.
- It provides a getter method to retrieve the saved state.

### 2. Implement the Originator
```java
class TextEditor {
    private String text;
    
    public void setText(String text) {
        this.text = text;
    }
    
    public String getText() {
        return text;
    }
    
    public Memento save() {
        return new Memento(text);
    }
    
    public void restore(Memento memento) {
        this.text = memento.getState();
    }
}
```
**Explanation:**
- The `TextEditor` (Originator) maintains the text state.
- It provides methods to save and restore the state using a `Memento`.

### 3. Implement the Caretaker
```java
import java.util.Stack;

class History {
    private Stack<Memento> history = new Stack<>();
    
    public void saveState(Memento memento) {
        history.push(memento);
    }
    
    public Memento undo() {
        return history.isEmpty() ? null : history.pop();
    }
}
```
**Explanation:**
- The `History` class (Caretaker) manages saved states.
- It allows storing and retrieving past states using a stack.

### 4. Test the Memento Pattern
```java
public class MementoPatternDemo {
    public static void main(String[] args) {
        TextEditor editor = new TextEditor();
        History history = new History();
        
        editor.setText("Hello World");
        history.saveState(editor.save());
        
        editor.setText("Design Patterns are awesome!");
        history.saveState(editor.save());
        
        System.out.println("Current Text: " + editor.getText());
        
        editor.restore(history.undo());
        System.out.println("After Undo: " + editor.getText());
    }
}
```
**Explanation:**
- Saves text states at different points.
- Restores a previous state when undo is performed.

---

## Example in a Spring Boot Application
### Scenario: Saving and Restoring Application Configurations
In a Spring Boot application, we may need to save and restore configuration settings dynamically.

```java
import org.springframework.stereotype.Component;
import java.util.Stack;

@Component
class ConfigManager {
    private String currentConfig;
    private Stack<Memento> history = new Stack<>();
    
    public void setConfig(String config) {
        history.push(new Memento(currentConfig));
        this.currentConfig = config;
    }
    
    public String getConfig() {
        return currentConfig;
    }
    
    public void restorePreviousConfig() {
        if (!history.isEmpty()) {
            this.currentConfig = history.pop().getState();
        }
    }
}
```
### Usage
```java
@RestController
@RequestMapping("/config")
class ConfigController {
    private final ConfigManager configManager;
    
    public ConfigController(ConfigManager configManager) {
        this.configManager = configManager;
    }
    
    @PostMapping("/update")
    public void updateConfig(@RequestBody String newConfig) {
        configManager.setConfig(newConfig);
    }
    
    @GetMapping("/current")
    public String getCurrentConfig() {
        return configManager.getConfig();
    }
    
    @PostMapping("/undo")
    public void undoConfigChange() {
        configManager.restorePreviousConfig();
    }
}
```
**Explanation:**
- Uses Memento to save and restore configuration settings.
- Provides endpoints to update, retrieve, and undo configuration changes.

---

## Advantages of Memento Pattern
1. **Encapsulation:** Preserves the integrity of an object's state without exposing its details.
2. **Undo/Redo Implementation:** Provides a clean way to implement undo/redo functionality.
3. **State Preservation:** Allows restoration of previous states efficiently.

## Disadvantages
1. **Memory Overhead:** Storing multiple states can consume a lot of memory.
2. **Complexity:** Managing multiple mementos can add implementation complexity.

---

## Common Interview Questions
1. **What problem does the Memento pattern solve?**
    - Answer: It allows an object to save and restore its state without exposing internal details.

2. **How does the Memento pattern differ from the Command pattern?**
    - Answer: The Memento pattern captures and restores state, while the Command pattern encapsulates actions.

3. **When should you avoid using the Memento pattern?**
    - Answer: When storing large amounts of state data, as it may lead to memory overhead.

4. **How can we optimize the Memento pattern for large objects?**
    - Answer: By storing only deltas (changes) instead of full object snapshots.

---

## Conclusion
The Memento pattern is a powerful way to manage object state changes, making it ideal for undo/redo functionality, configuration management, and history tracking. However, developers must balance its advantages against potential memory and complexity issues to use it effectively.

# Memento Pattern vs Command Pattern

| Feature           | Memento Pattern                                       | Command Pattern                                       |
|-------------------|-------------------------------------------------------|-------------------------------------------------------|
| **Purpose**       | Captures and restores an object's state.              | Encapsulates an operation as an object to be executed later. |
| **Focus**         | State preservation and undo/redo functionality.       | Action execution, queuing, and transaction management. |
| **Encapsulation** | Stores internal state without exposing details.       | Encapsulates a request as an object.                  |
| **Undo Mechanism**| Restores a previous state.                            | Executes an inverse operation (if implemented).        |
| **Usage**         | Undo/redo in text editors, restoring configurations, snapshots. | Logging, macro recording, task scheduling, transactional operations. |
| **Example**       | Saving and restoring text in a text editor.           | Command execution in a menu-driven system (e.g., copy-paste in an editor). |

## Key Differences:
- **Memento** focuses on storing state and restoring it when needed.
- **Command** encapsulates actions as objects, allowing them to be queued, logged, or undone via an inverse operation.
