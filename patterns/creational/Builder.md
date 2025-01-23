# Builder Design Pattern

## Definition
The Builder Design Pattern is a creational pattern that provides a way to construct complex objects step-by-step. Unlike other creational patterns, the Builder pattern separates the construction of an object from its representation, allowing the same construction process to create different representations.

### Key Characteristics:
1. **Step-by-Step Construction:** Allows building an object piece by piece.
2. **Control Over Object Creation:** Provides fine-grained control over how the object is constructed.
3. **Immutable or Configurable Objects:** Useful for creating immutable objects or objects with multiple optional attributes.

---

## Real-World Scenario
### Scenario 1: Building Configurable HTTP Requests in a Spring Boot Application
Imagine you need to send HTTP requests with varying headers, query parameters, and body content in a Spring Boot application. Instead of creating a complex constructor or multiple setter methods, you can use the Builder pattern to configure and create the request step by step.

### Benefits:
- Simplifies the creation of complex objects.
- Ensures the immutability of the constructed object.
- Makes the code more readable and maintainable.

---

## Implementation

### Step-by-Step Explanation

### 1. Define the Product Class
```java
public class HttpRequest {
    private String url;
    private String method;
    private Map<String, String> headers;
    private String body;

    // Private constructor to enforce object creation through the builder
    private HttpRequest(HttpRequestBuilder builder) {
        this.url = builder.url;
        this.method = builder.method;
        this.headers = builder.headers;
        this.body = builder.body;
    }

    // Getters
    public String getUrl() { return url; }
    public String getMethod() { return method; }
    public Map<String, String> getHeaders() { return headers; }
    public String getBody() { return body; }

    @Override
    public String toString() {
        return "HttpRequest{" +
                "url='" + url + '\'' +
                ", method='" + method + '\'' +
                ", headers=" + headers +
                ", body='" + body + '\'' +
                '}';
    }

    // Builder Class
    public static class HttpRequestBuilder {
        private String url;
        private String method;
        private Map<String, String> headers = new HashMap<>();
        private String body;

        public HttpRequestBuilder setUrl(String url) {
            this.url = url;
            return this;
        }

        public HttpRequestBuilder setMethod(String method) {
            this.method = method;
            return this;
        }

        public HttpRequestBuilder addHeader(String key, String value) {
            this.headers.put(key, value);
            return this;
        }

        public HttpRequestBuilder setBody(String body) {
            this.body = body;
            return this;
        }

        public HttpRequest build() {
            return new HttpRequest(this);
        }
    }
}
```
**Explanation:**
- The `HttpRequest` class has private attributes and a private constructor to enforce immutability.
- The `HttpRequestBuilder` class provides methods to set individual attributes and a `build()` method to create the `HttpRequest` object.

### 2. Client Code
```java
public class Main {
    public static void main(String[] args) {
        HttpRequest request = new HttpRequest.HttpRequestBuilder()
                .setUrl("https://example.com/api")
                .setMethod("POST")
                .addHeader("Content-Type", "application/json")
                .addHeader("Authorization", "Bearer token")
                .setBody("{\"key\": \"value\"}")
                .build();

        System.out.println(request);
    }
}
```
**Output:**
```
HttpRequest{url='https://example.com/api', method='POST', headers={Content-Type=application/json, Authorization=Bearer token}, body='{"key": "value"}'}
```

**Explanation:**
- The client uses the `HttpRequestBuilder` to create an `HttpRequest` object step by step.
- The final object is immutable and ready to use.

---

### Scenario 2: Building Complex Reports in a Reporting System
In a reporting system, you might need to generate reports with different sections, charts, and tables. Using the Builder pattern, you can build reports step by step, customizing them as needed.

#### Benefits:
- Simplifies the creation of complex reports.
- Makes the report generation process flexible and reusable.

---

## Advantages of Builder Pattern
1. Simplifies the construction of complex objects with many optional parameters.
2. Ensures immutability and thread safety for the constructed object.
3. Improves code readability and maintainability.
4. Allows the reuse of the same building process for different representations of the object.

## Disadvantages
1. Can increase the number of classes in the codebase.
2. Might introduce complexity if overused for simple objects.

---

## Common Interview Questions
1. **What is the purpose of the Builder Design Pattern?**
    - **Answer:** It separates the construction of a complex object from its representation, allowing step-by-step creation and making the object immutable and easier to configure.

2. **How does the Builder pattern differ from the Factory pattern?**
    - **Answer:** The Factory pattern focuses on creating objects in a single step, while the Builder pattern constructs complex objects step-by-step, especially useful for objects with many optional attributes.

3. **Can you provide a real-world example of using the Builder pattern?**
    - **Answer:** The Builder pattern can be used to construct HTTP requests, complex reports, or configurable UI components.

4. **What are the benefits of the Builder pattern?**
    - **Answer:** Simplifies the creation process, ensures immutability, and improves code readability and maintainability.

5. **What are the trade-offs of using the Builder pattern?**
    - **Answer:** It can increase the number of classes and might introduce unnecessary complexity for simple object creation.

---

## Conclusion
The Builder Design Pattern is a powerful tool for constructing complex objects in a controlled and flexible way. By understanding its use cases and benefits, developers can apply this pattern effectively to create scalable and maintainable systems.

