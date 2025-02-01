# Visitor Design Pattern

## Definition
The **Visitor Pattern** is a behavioral design pattern that allows adding further operations to objects without modifying their structures. It helps separate an algorithm from the object structure on which it operates.

### Key Characteristics:
1. **Separation of Concerns:** Encapsulates operations in a separate class, keeping the object structure untouched.
2. **Extensibility:** New operations can be added without modifying existing object classes.
3. **Double Dispatch:** Ensures the correct method is called based on both the visitor and the element it visits.

---

## Real-World Scenario
### Scenario: Tax Calculation System
Imagine an e-commerce system where different types of items have different tax rates. Instead of modifying each item class when tax rules change, we use the Visitor Pattern to encapsulate tax calculations in a separate class.

### Benefits:
- Separates tax logic from item classes.
- Easily extendable for new tax types.
- Reduces code duplication.

---

## Implementation
Here’s how we implement the Visitor Pattern in Java:

### Step 1: Define the `Visitable` Interface
```java
public interface Visitable {
    void accept(Visitor visitor);
}
```

### Step 2: Create Concrete Elements
```java
public class Book implements Visitable {
    private double price;
    
    public Book(double price) {
        this.price = price;
    }
    
    public double getPrice() {
        return price;
    }
    
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

public class Electronics implements Visitable {
    private double price;
    
    public Electronics(double price) {
        this.price = price;
    }
    
    public double getPrice() {
        return price;
    }
    
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

### Step 3: Define the `Visitor` Interface
```java
public interface Visitor {
    void visit(Book book);
    void visit(Electronics electronics);
}
```

### Step 4: Create Concrete Visitors
```java
public class TaxVisitor implements Visitor {
    @Override
    public void visit(Book book) {
        System.out.println("Tax for Book: " + book.getPrice() * 0.1);
    }

    @Override
    public void visit(Electronics electronics) {
        System.out.println("Tax for Electronics: " + electronics.getPrice() * 0.2);
    }
}

public class DiscountVisitor implements Visitor {
    @Override
    public void visit(Book book) {
        System.out.println("Discount on Book: " + book.getPrice() * 0.05);
    }

    @Override
    public void visit(Electronics electronics) {
        System.out.println("Discount on Electronics: " + electronics.getPrice() * 0.1);
    }
}
```

### Step 5: Using the Visitor Pattern
```java
public class VisitorPatternDemo {
    public static void main(String[] args) {
        Visitable book = new Book(100);
        Visitable electronics = new Electronics(200);
        
        Visitor taxVisitor = new TaxVisitor();
        Visitor discountVisitor = new DiscountVisitor();
        
        book.accept(taxVisitor);
        electronics.accept(taxVisitor);
        
        book.accept(discountVisitor);
        electronics.accept(discountVisitor);
    }
}
```

---

## Additional Example: Document Processing System
### Scenario
In a document processing system, we have different types of documents (PDF, Word, Excel), and we want to perform operations like printing and encryption without modifying document classes.

### Step 1: Define the Document Interface
```java
public interface Document {
    void accept(DocumentVisitor visitor);
}
```

### Step 2: Implement Concrete Documents
```java
public class PDFDocument implements Document {
    @Override
    public void accept(DocumentVisitor visitor) {
        visitor.visit(this);
    }
}

public class WordDocument implements Document {
    @Override
    public void accept(DocumentVisitor visitor) {
        visitor.visit(this);
    }
}
```

### Step 3: Define the Visitor Interface
```java
public interface DocumentVisitor {
    void visit(PDFDocument pdf);
    void visit(WordDocument word);
}
```

### Step 4: Implement Concrete Visitors
```java
public class PrintVisitor implements DocumentVisitor {
    @Override
    public void visit(PDFDocument pdf) {
        System.out.println("Printing PDF Document");
    }

    @Override
    public void visit(WordDocument word) {
        System.out.println("Printing Word Document");
    }
}

public class EncryptVisitor implements DocumentVisitor {
    @Override
    public void visit(PDFDocument pdf) {
        System.out.println("Encrypting PDF Document");
    }

    @Override
    public void visit(WordDocument word) {
        System.out.println("Encrypting Word Document");
    }
}
```

### Step 5: Using the Visitor Pattern
```java
public class DocumentProcessingDemo {
    public static void main(String[] args) {
        Document pdf = new PDFDocument();
        Document word = new WordDocument();
        
        DocumentVisitor printVisitor = new PrintVisitor();
        DocumentVisitor encryptVisitor = new EncryptVisitor();
        
        pdf.accept(printVisitor);
        word.accept(printVisitor);
        
        pdf.accept(encryptVisitor);
        word.accept(encryptVisitor);
    }
}
```

---

## Advantages of Visitor Pattern
1. **Separation of Concerns:** Business logic is separated from object structure.
2. **Extensibility:** New operations can be added without modifying existing classes.
3. **Open/Closed Principle:** New visitors can be added without altering elements.

## Disadvantages
1. **Complexity:** Increases code complexity, especially for large hierarchies.
2. **Violation of Encapsulation:** The visitor needs access to internal details of elements.
3. **Dependency on Concrete Classes:** The pattern relies on concrete element types.

---

## Common Interview Questions
1. **When should you use the Visitor Pattern?**
    - When operations on an object hierarchy need to be extended without modifying the elements.
2. **How does the Visitor Pattern support Double Dispatch?**
    - It ensures that the method executed depends on both the visitor and the element visited.
3. **What are alternatives to the Visitor Pattern?**
    - Strategy Pattern or method overloading can sometimes be used instead.
4. **Can the Visitor Pattern be used with unrelated classes?**
    - No, it is meant for hierarchies where elements share a common interface.

---

## Conclusion
The **Visitor Pattern** is a powerful technique to extend functionalities without modifying existing object structures. It is widely used in scenarios like **tax calculation**, **document processing**, and **compiler design**. However, it should be used carefully due to its complexity and dependency on object structures.

