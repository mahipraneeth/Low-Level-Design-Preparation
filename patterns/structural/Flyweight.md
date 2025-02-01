# Flyweight Design Pattern

## Definition
The Flyweight Design Pattern is a structural pattern that minimizes memory usage by sharing as much data as possible between similar objects. This pattern is particularly useful when working with a large number of objects that share common properties.

### Key Characteristics:
1. **Reduces Memory Usage:** By sharing intrinsic data across multiple instances.
2. **Improves Performance:** Reduces object creation overhead and memory footprint.
3. **Intrinsic vs. Extrinsic State:** Separates shared data (intrinsic) from unique data (extrinsic).

---

## Real-World Scenario
**Scenario:** Managing Tree Objects in a Forest Simulation

Imagine a forest simulation where thousands of trees are displayed. Instead of creating separate instances for each tree, we use the Flyweight pattern to share common properties like tree type, texture, and color while storing unique properties like coordinates separately.

### Benefits:
- Avoids redundant object creation.
- Enhances performance and reduces memory consumption.
- Promotes object sharing and reusability.

---

## Implementation
Here is a detailed implementation of the Flyweight pattern in Java:

### Step-by-Step Explanation

### 1. Flyweight Interface
```java
public interface Tree {
    void display(int x, int y);
}
```
**Explanation:**
- This interface defines the common behavior of all tree objects.

### 2. Concrete Flyweight Class
```java
public class TreeType implements Tree {
    private final String name;
    private final String color;
    private final String texture;

    public TreeType(String name, String color, String texture) {
        this.name = name;
        this.color = color;
        this.texture = texture;
    }

    @Override
    public void display(int x, int y) {
        System.out.println("Displaying " + name + " tree at (" + x + ", " + y + ") with color " + color);
    }
}
```
**Explanation:**
- This class represents the shared state (intrinsic properties like name, color, and texture).

### 3. Flyweight Factory
```java
import java.util.HashMap;
import java.util.Map;

public class TreeFactory {
    private static final Map<String, TreeType> treeTypes = new HashMap<>();

    public static TreeType getTreeType(String name, String color, String texture) {
        String key = name + "-" + color + "-" + texture;
        if (!treeTypes.containsKey(key)) {
            treeTypes.put(key, new TreeType(name, color, texture));
            System.out.println("Creating new TreeType: " + key);
        }
        return treeTypes.get(key);
    }
}
```
**Explanation:**
- The factory ensures that the same tree type is reused instead of creating new instances.

### 4. Client Code
```java
import java.util.ArrayList;
import java.util.List;

public class Forest {
    private final List<Tree> trees = new ArrayList<>();

    public void plantTree(String name, String color, String texture, int x, int y) {
        TreeType treeType = TreeFactory.getTreeType(name, color, texture);
        trees.add(() -> treeType.display(x, y));
    }

    public void displayTrees() {
        for (Tree tree : trees) {
            tree.display(0, 0);
        }
    }

    public static void main(String[] args) {
        Forest forest = new Forest();
        forest.plantTree("Oak", "Green", "Rough", 10, 20);
        forest.plantTree("Oak", "Green", "Rough", 30, 40);
        forest.plantTree("Pine", "Dark Green", "Smooth", 50, 60);
        forest.displayTrees();
    }
}
```
**Output:**
```
Creating new TreeType: Oak-Green-Rough
Creating new TreeType: Pine-Dark Green-Smooth
Displaying Oak tree at (0, 0) with color Green
Displaying Oak tree at (0, 0) with color Green
Displaying Pine tree at (0, 0) with color Dark Green
```

---

## Example in a Spring Boot Application
### Scenario: Database Connection Pooling
Instead of creating a new database connection for each request, we use a connection pool where connections are shared.

#### Connection Flyweight
```java
public class DatabaseConnection {
    private final String url;

    public DatabaseConnection(String url) {
        this.url = url;
    }

    public void connect() {
        System.out.println("Connecting to " + url);
    }
}
```

#### Connection Factory
```java
import java.util.HashMap;
import java.util.Map;

public class ConnectionPool {
    private static final Map<String, DatabaseConnection> pool = new HashMap<>();

    public static DatabaseConnection getConnection(String url) {
        pool.putIfAbsent(url, new DatabaseConnection(url));
        return pool.get(url);
    }
}
```

#### Client Code
```java
public class App {
    public static void main(String[] args) {
        DatabaseConnection conn1 = ConnectionPool.getConnection("jdbc:mysql://localhost:3306/db1");
        DatabaseConnection conn2 = ConnectionPool.getConnection("jdbc:mysql://localhost:3306/db1");
        DatabaseConnection conn3 = ConnectionPool.getConnection("jdbc:mysql://localhost:3306/db2");

        conn1.connect();
        conn2.connect();
        conn3.connect();
    }
}
```

**Output:**
```
Connecting to jdbc:mysql://localhost:3306/db1
Connecting to jdbc:mysql://localhost:3306/db2
```

---

## Advantages of Flyweight Pattern
1. **Efficient Memory Usage:** Reduces object duplication.
2. **Improves Performance:** Reduces object creation overhead.
3. **Encourages Object Sharing:** Promotes reuse of common objects.

## Disadvantages
1. **Complexity Increases:** Managing shared state can be challenging.
2. **Not Suitable for All Cases:** Works best when a large number of objects share common data.

---

## Common Interview Questions
1. **How does the Flyweight pattern improve performance?**
    - Answer: By reducing memory usage through object sharing, minimizing the number of new objects created.

2. **What is the difference between intrinsic and extrinsic state?**
    - Answer: Intrinsic state is shared among objects, while extrinsic state is unique to each instance.

3. **Can the Flyweight pattern be used with immutable objects?**
    - Answer: Yes, Flyweight objects are often immutable to ensure shared state remains consistent.

4. **How does the Flyweight pattern differ from caching?**
    - Answer: Flyweight focuses on reusing objects with shared state, while caching stores frequently accessed data.

---

## Conclusion
The Flyweight pattern is an effective way to manage memory and improve performance in applications requiring a large number of similar objects. By separating intrinsic and extrinsic data, it allows efficient object reuse, making it a valuable pattern in scenarios like UI rendering, connection pooling, and data compression.

