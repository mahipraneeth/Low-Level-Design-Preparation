# Template Method Design Pattern

## Definition
The **Template Method Pattern** is a behavioral design pattern that defines the **skeleton of an algorithm** in a base class but allows subclasses to override specific steps without changing the overall structure. This promotes **code reusability** and ensures consistency across implementations.

### Key Characteristics:
1. **Defines a Template Method**: A base class provides a structured method that outlines an algorithm, delegating specific steps to subclasses.
2. **Encapsulates Common Behavior**: The base class contains the core logic that is common across implementations.
3. **Allows Customization**: Subclasses can override specific steps to provide different implementations.
4. **Follows the Hollywood Principle**: *"Don't call us, we'll call you"* — meaning the base class controls the flow, and subclasses only provide custom implementations.

---

## Real-World Scenario
**Scenario:** Report Generation System

A report generation system needs to generate reports in different formats (PDF, Excel, CSV). Each format requires a common sequence:
1. Fetching Data
2. Formatting Data
3. Exporting the Report

The **Template Method Pattern** allows defining a base `ReportGenerator` class with a template method that outlines this sequence while letting subclasses implement the format-specific details.

### Benefits:
- Ensures a consistent structure for report generation.
- Reduces code duplication by handling common steps in a base class.
- Allows flexibility to customize report formats.

---

## Implementation

### Step-by-Step Explanation

### 1. Abstract Base Class (Template Method)
The base class defines the **template method** and declares abstract methods for steps that subclasses will implement.

```java
public abstract class ReportGenerator {
    // Template Method defining the algorithm skeleton
    public final void generateReport() {
        fetchData();
        processData();
        exportReport();
    }

    // Steps to be implemented by subclasses
    protected abstract void fetchData();
    protected abstract void processData();
    protected abstract void exportReport();
}
```

### 2. Concrete Implementations
Each subclass implements specific steps based on the required format.

#### PDF Report Implementation
```java
public class PdfReportGenerator extends ReportGenerator {
    @Override
    protected void fetchData() {
        System.out.println("Fetching data for PDF report...");
    }

    @Override
    protected void processData() {
        System.out.println("Processing data into PDF format...");
    }

    @Override
    protected void exportReport() {
        System.out.println("Exporting PDF report...");
    }
}
```

#### Excel Report Implementation
```java
public class ExcelReportGenerator extends ReportGenerator {
    @Override
    protected void fetchData() {
        System.out.println("Fetching data for Excel report...");
    }

    @Override
    protected void processData() {
        System.out.println("Processing data into Excel format...");
    }

    @Override
    protected void exportReport() {
        System.out.println("Exporting Excel report...");
    }
}
```

### 3. Usage Example
```java
public class TemplateMethodDemo {
    public static void main(String[] args) {
        ReportGenerator pdfReport = new PdfReportGenerator();
        pdfReport.generateReport();

        System.out.println("--------------------------------");

        ReportGenerator excelReport = new ExcelReportGenerator();
        excelReport.generateReport();
    }
}
```

**Output:**
```
Fetching data for PDF report...
Processing data into PDF format...
Exporting PDF report...
--------------------------------
Fetching data for Excel report...
Processing data into Excel format...
Exporting Excel report...
```

---

## Example in a Spring Boot Application
### Scenario: Payment Processing System
A payment system processes different payment methods (Credit Card, PayPal, UPI) using a common structure.

#### Step 1: Define the Abstract Base Class
```java
public abstract class PaymentProcessor {
    public final void processPayment() {
        validatePayment();
        executeTransaction();
        sendReceipt();
    }

    protected abstract void validatePayment();
    protected abstract void executeTransaction();
    protected void sendReceipt() {
        System.out.println("Sending payment receipt...");
    }
}
```

#### Step 2: Implement Concrete Classes
```java
public class CreditCardPayment extends PaymentProcessor {
    @Override
    protected void validatePayment() {
        System.out.println("Validating credit card details...");
    }

    @Override
    protected void executeTransaction() {
        System.out.println("Processing credit card transaction...");
    }
}
```

```java
public class PayPalPayment extends PaymentProcessor {
    @Override
    protected void validatePayment() {
        System.out.println("Validating PayPal account...");
    }

    @Override
    protected void executeTransaction() {
        System.out.println("Processing PayPal transaction...");
    }
}
```

#### Step 3: Using the Payment Processors
```java
public class PaymentDemo {
    public static void main(String[] args) {
        PaymentProcessor creditCard = new CreditCardPayment();
        creditCard.processPayment();

        System.out.println("--------------------------------");

        PaymentProcessor payPal = new PayPalPayment();
        payPal.processPayment();
    }
}
```

**Output:**
```
Validating credit card details...
Processing credit card transaction...
Sending payment receipt...
--------------------------------
Validating PayPal account...
Processing PayPal transaction...
Sending payment receipt...
```

---

## Advantages of Template Method Pattern
1. **Code Reusability**: Common logic is placed in the base class, reducing duplication.
2. **Consistent Algorithm Structure**: Ensures all subclasses follow the same execution flow.
3. **Encapsulation of Varying Behavior**: Subclasses define only the required behavior without modifying the overall process.
4. **Easier Maintenance**: Changes in the algorithm’s structure only affect the base class.

## Disadvantages
1. **Limited Flexibility**: The fixed algorithm structure may not suit all scenarios.
2. **Tightly Coupled Hierarchy**: Changes in the base class can impact all subclasses.

---

## Common Interview Questions
1. **What is the Template Method Pattern?**
    - Answer: It is a design pattern that defines the structure of an algorithm in a base class and lets subclasses override specific steps.

2. **How does the Template Method Pattern promote code reusability?**
    - Answer: Common behavior is placed in the base class, allowing subclasses to reuse it without duplication.

3. **How does it differ from the Strategy Pattern?**
    - Answer: The Template Method defines an algorithm structure, while the Strategy Pattern encapsulates interchangeable behaviors within a class.

4. **Can the Template Method Pattern be used without inheritance?**
    - Answer: Typically, it relies on inheritance, but it can also be implemented using interfaces and composition.

---

## Conclusion
The **Template Method Pattern** is useful when a structured process needs to be enforced while allowing flexibility in certain steps. It promotes **reusability, consistency, and maintainability**, making it a powerful tool in software design. However, careful consideration is needed to avoid overuse, which may lead to rigid hierarchies.

By leveraging this pattern, developers can create flexible yet structured systems that adapt to various business requirements. 🚀

