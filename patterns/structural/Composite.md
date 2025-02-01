# Composite Design Pattern

## Definition
The Composite Design Pattern is a structural pattern that allows individual objects and compositions of objects to be treated uniformly. It is useful when representing part-whole hierarchies, enabling clients to interact with individual objects and groups of objects in the same way.

### Key Characteristics:
1. **Hierarchical Structure:** Represents objects in a tree structure where individual objects and compositions can be treated uniformly.
2. **Uniform Access:** Provides a common interface for both individual and composite objects.
3. **Scalability:** Easily extends to support new types of components without modifying existing code.

---

## Real-World Scenario
### **Scenario: File System Representation**
In a file system, both files and directories exist. A directory can contain files or other directories, forming a tree-like hierarchy. The Composite pattern allows treating both files and directories uniformly, enabling operations such as listing contents, calculating total size, or deleting elements.

### **Benefits:**
- Simplifies client code by providing a unified way to handle individual and grouped objects.
- Supports dynamic structure modifications (e.g., adding/removing files or directories at runtime).
- Improves maintainability by following the Open/Closed Principle.

---

## Implementation
Here is a detailed implementation of the Composite pattern in Java:

### Step-by-Step Explanation

### **1. Component Interface**
Defines a common interface for both individual and composite objects.
```java
interface FileSystemComponent {
    void showDetails();
}
```

### **2. Leaf Class (File)**
Represents individual objects that do not contain other objects.
```java
class File implements FileSystemComponent {
    private String name;
    
    public File(String name) {
        this.name = name;
    }
    
    @Override
    public void showDetails() {
        System.out.println("File: " + name);
    }
}
```

### **3. Composite Class (Directory)**
Represents composite objects that can contain multiple components (both files and directories).
```java
import java.util.ArrayList;
import java.util.List;

class Directory implements FileSystemComponent {
    private String name;
    private List<FileSystemComponent> components = new ArrayList<>();
    
    public Directory(String name) {
        this.name = name;
    }
    
    public void addComponent(FileSystemComponent component) {
        components.add(component);
    }
    
    public void removeComponent(FileSystemComponent component) {
        components.remove(component);
    }
    
    @Override
    public void showDetails() {
        System.out.println("Directory: " + name);
        for (FileSystemComponent component : components) {
            component.showDetails();
        }
    }
}
```

### **4. Client Code**
```java
public class CompositePatternDemo {
    public static void main(String[] args) {
        File file1 = new File("document.txt");
        File file2 = new File("image.png");
        
        Directory folder = new Directory("MyFolder");
        folder.addComponent(file1);
        folder.addComponent(file2);
        
        Directory rootDirectory = new Directory("Root");
        rootDirectory.addComponent(folder);
        rootDirectory.showDetails();
    }
}
```

### **Output:**
```
Directory: Root
Directory: MyFolder
File: document.txt
File: image.png
```

---

## Example in a Spring Boot Application
### **Scenario: Organization Hierarchy Management**
In an enterprise application, we may need to manage an organization hierarchy where each employee can be either an individual contributor (leaf) or a manager (composite) who oversees multiple employees.

```java
interface Employee {
    void showEmployeeDetails();
}
```

#### **Leaf Class (Developer)**
```java
class Developer implements Employee {
    private String name;
    private String role;
    
    public Developer(String name, String role) {
        this.name = name;
        this.role = role;
    }
    
    @Override
    public void showEmployeeDetails() {
        System.out.println("Developer: " + name + ", Role: " + role);
    }
}
```

#### **Composite Class (Manager)**
```java
import java.util.ArrayList;
import java.util.List;

class Manager implements Employee {
    private String name;
    private List<Employee> subordinates = new ArrayList<>();
    
    public Manager(String name) {
        this.name = name;
    }
    
    public void addEmployee(Employee employee) {
        subordinates.add(employee);
    }
    
    public void removeEmployee(Employee employee) {
        subordinates.remove(employee);
    }
    
    @Override
    public void showEmployeeDetails() {
        System.out.println("Manager: " + name);
        for (Employee employee : subordinates) {
            employee.showEmployeeDetails();
        }
    }
}
```

#### **Client Code**
```java
public class OrganizationStructure {
    public static void main(String[] args) {
        Developer dev1 = new Developer("Alice", "Backend Developer");
        Developer dev2 = new Developer("Bob", "Frontend Developer");
        
        Manager techLead = new Manager("Charlie");
        techLead.addEmployee(dev1);
        techLead.addEmployee(dev2);
        
        Manager ceo = new Manager("David");
        ceo.addEmployee(techLead);
        
        ceo.showEmployeeDetails();
    }
}
```

### **Output:**
```
Manager: David
Manager: Charlie
Developer: Alice, Role: Backend Developer
Developer: Bob, Role: Frontend Developer
```

---

## Advantages of Composite Pattern
1. Simplifies client code by providing a uniform way to work with both individual and composite objects.
2. Supports scalability as new components can be easily added without modifying existing code.
3. Promotes better code organization by maintaining a clear hierarchical structure.

## Disadvantages
1. Can make debugging complex as the structure grows larger.
2. Overhead in maintaining parent-child relationships in complex hierarchies.
3. Operations on the tree may require extra care to ensure consistency.

---

## Common Interview Questions
1. **What problem does the Composite pattern solve?**
    - Answer: It allows treating individual objects and compositions uniformly, simplifying hierarchical structure management.

2. **How does Composite differ from Decorator pattern?**
    - Answer: The Composite pattern represents part-whole hierarchies, while the Decorator pattern adds behavior dynamically to individual objects.

3. **Can a Composite contain other Composites?**
    - Answer: Yes, a Composite can contain other Composites, forming a recursive tree structure.

4. **How does the Composite pattern align with the Open/Closed Principle?**
    - Answer: New components can be added without modifying existing code, following the Open/Closed Principle.

---

## Conclusion
The Composite pattern is a powerful structural pattern that enables handling complex hierarchical structures in a uniform way. It is widely used in UI frameworks, file systems, and organizational hierarchies. While it simplifies client interaction, careful design is required to manage large-scale composite structures effectively.

