# SOLID

Here SOLID stand for

- [S - Single Responsibility Principle](https://github.com/arifulnoman/design-patterns/tree/main?tab=readme-ov-file#s---single-responsibility-principle)
- [O - Open Closed Principle](https://github.com/arifulnoman/design-patterns/tree/main?tab=readme-ov-file#o---openclosed-principle)
- [L - Liskov Substitution Principle](https://github.com/arifulnoman/design-patterns/tree/main?tab=readme-ov-file#l---liskov-substitution-principle)
- [I - Interface Segregation Principle](https://github.com/arifulnoman/design-patterns/tree/main?tab=readme-ov-file#i---interface-segregation-principle)
- [D - Dependency Inversion Principle](https://github.com/arifulnoman/design-patterns/tree/main?tab=readme-ov-file#d---dependency-inversion-principle)

## S - Single Responsibility Principle

**Definition:** A class should have only one reason to change. In other words, a class should have one responsibility or one job.

The idea behind SRP is that a class should focus on a single task or functionality. When a class is responsible for multiple concerns, it becomes more complex, harder to maintain, and more prone to bugs. By adhering to SRP, we can separate different functionalities into different classes, making the system modular and easier to understand.

- **One Responsibility:** A class should encapsulate a single functionality. For example, a Report class should only be responsible for creating a report, not for saving it to a file or sending it over email.

- **One Reason to Change:** If a class has multiple responsibilities, any change in one of those responsibilities can potentially affect the others, leading to unintended side effects.

**Example:**

```java
public class UserService {

    public void authenticateUser(String username, String password) {
        // Logic for user authentication
    }

    public void generateReport() {
        // Logic for generating reports
    }
}
```

**Refactored Example:**

```java
public class AuthenticationService {
    public void authenticateUser(String username, String password) {
        // Logic for user authentication
    }
}

public class ReportService {
    public void generateReport() {
        // Logic for generating reports
    }
}
```

**Explanation:**

- **Initial Example:** The `UserService` class violates SRP by handling both user authentication and report generation, leading to complex and less maintainable code.

- **Refactored Example:** By splitting into `AuthenticationService` and `ReportService`, each class now has a single responsibility. This makes the code easier to maintain and extend, as changes in one class do not affect the other.

## O - Open/Closed Principle

**Definition:** Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means you should be able to extend the behavior of a class or module without altering its existing code.

The Open/Closed Principle promotes writing code that is easy to extend with new functionality but doesn’t require changes to existing code, thus reducing the risk of introducing bugs.

- **Open for Extension:** The design should allow new functionality to be added. This can be achieved through inheritance, interfaces, or other extensibility mechanisms.
- **Closed for Modification:** Existing code should remain unchanged when new features or changes are introduced, ensuring stability and reliability.

**Example:**

```java
class WhatsAppNotification {
    public void sendWhatsAppNotification() {
        System.out.println("Hey, I am WhatsApp");
    }
}

class MessengerNotification {
    public void sendMessengerNotification() {
        System.out.println("Hey, I am Messenger");
    }
}

public class Main {

    public static void main(String[] args) {

        // Directly instantiating each notification class
        WhatsAppNotification whatsAppNotification = new WhatsAppNotification();
        whatsAppNotification.sendWhatsAppNotification();

        MessengerNotification messengerNotification = new MessengerNotification();
        messengerNotification.sendMessengerNotification();

        // If we add a new notification, we have to modify this main method
        // Example: EmailNotification
        EmailNotification emailNotification = new EmailNotification();
        emailNotification.sendEmailNotification();
    }
}

class EmailNotification {
    public void sendEmailNotification() {
        System.out.println("Hey, I am Email");
    }
}
```

**Refactored Example:**

```java
package practice_open_close_principle;

interface Service {
    void sendNotification();
}

class WhatsAppNotification implements Service {
    public void sendNotification() {
        System.out.println("Hey, I am WhatsApp");
    }
}

class MessengerNotification implements Service {
    public void sendNotification() {
        System.out.println("Hey, I am Messenger");
    }
}

// New Notification Service added without modifying existing code
class EmailNotification implements Service {
    public void sendNotification() {
        System.out.println("Hey, I am Email");
    }
}

public class Main {

    public static void main(String[] args) {
        // We are using polymorphism here to make it open for extension
        sendNotification(new WhatsAppNotification());
        sendNotification(new MessengerNotification());
        sendNotification(new EmailNotification());  // New service added seamlessly
    }

    // Method to handle notifications, following the Open/Closed Principle
    public static void sendNotification(Service service) {
        service.sendNotification();
    }
}
```

**Explanation:**

- **Initial Example:** The `Main` class violates the Open/Closed Principle because it directly instantiates specific notification classes `WhatsAppNotification`, `MessengerNotification`. This requires modifying the `Main` class whenever a new notification type is added, making the code harder to maintain and extend.

- **Refactored Example:** By introducing the `Service` interface and using polymorphism, the `Main` class is now open for extension (new notification types can be added) but closed for modification (no changes needed in the existing code). This design allows for easier maintenance and scalability, as new notification services can be added without altering the core logic.

## L - Liskov Substitution Principle

**Definition:** Subtypes must be substitutable for their base types without altering the correctness of the program. In other words, objects of a derived class should be able to replace objects of the base class without affecting the functionality of the program.

The Liskov Substitution Principle ensures that a subclass can stand in for its parent class without causing unexpected behavior. This encourages the design of classes that follow consistent behavior when inherited, promoting the use of polymorphism.

- **Behavioral Consistency:** Subclasses should enhance or maintain the behavior of the base class, not contradict it.
- **No Broken Contracts:** The subclass should not violate any expectations set by the base class (e.g., method signatures, return types, or exceptions).

**Example:**

```java
class SocialMediaTasks {
    public void sendMessage() { }
    public void callInGroup() { }
    public void postPicture() { }
    public void sendPictureAndVideo() { }
}

class WhatsApp extends SocialMediaTasks {
    public void sendMessage() { /* Feature Supported */ }
    public void callInGroup() { /* Feature Supported */ }
    public void postPicture() { /* Feature Don't Support */ }
    public void sendPictureAndVideo() { /* Feature Supported */ }
}

class Instagram extends SocialMediaTasks {
    public void sendMessage() { /* Feature Supported */ }
    public void callInGroup() { /* Feature Don't Support */ }
    public void postPicture() { /* Feature Supported */ }
    public void sendPictureAndVideo() { /* Feature Supported */ }
}

class Facebook extends SocialMediaTasks {
    public void sendMessage() { /* Feature Supported */ }
    public void callInGroup() { /* Feature Supported */ }
    public void postPicture() { /* Feature Supported */ }
    public void sendPictureAndVideo() { /* Feature Supported */ }
}
```

**Refactored Example:**

```java
interface SocialMediaTasks {
    public void sendMessage();
    public void sendPictureAndVideo();
}

interface GroupCall {
    public void callInGroup();
}

interface Post {
    public void postPicture();
}

class WhatsApp implements SocialMediaTasks {
    public void sendMessage() { /* Feature Supported */ }
    public void sendPictureAndVideo() { /* Feature Supported */ }
}

class Instagram implements SocialMediaTasks, Post {
    public void sendMessage() { /* Feature Supported */ }
    public void postPicture() { /* Feature Supported */ }
    public void sendPictureAndVideo() { /* Feature Supported */ }
}

class Facebook implements SocialMediaTasks, GroupCall, Post {
    public void sendMessage() { /* Feature Supported */ }
    public void callInGroup() { /* Feature Supported */ }
    public void postPicture() { /* Feature Supported */ }
    public void sendPictureAndVideo() { /* Feature Supported */ }
}

```

**Explanation:**

- **Initial Example:** The `SocialMediaTasks` class violates the Liskov Substitution Principle (LSP) because it forces all subclasses (`WhatsApp`, `Instagram`, `Facebook`) to inherit all methods, even if they do not support certain features. This can lead to runtime errors when unsupported methods like `postPicture` in `WhatsApp` or `callInGroup` in `Instagram` are called. This violates the principle, as the derived classes cannot replace the base class without causing issues.

- **Refactored Example:** By introducing separate interfaces (`SocialMediaTasks`, `GroupCall`, `Post`), the classes now adhere to LSP. Each class (`WhatsApp`, `Instagram`, `Facebook`) implements only the interfaces that represent the features they support. This design ensures that each subclass can fully replace the base class or interfaces without leading to runtime issues. The code is now more maintainable and flexible, as changes in one feature do not impact other unrelated features.

## I - Interface Segregation Principle

**Definition:** Clients should not be forced to depend on interfaces they do not use. Instead of creating large, monolithic interfaces, it's better to create smaller, more specific ones that are tailored to the exact needs of the client. This avoids bloated interfaces that can lead to unnecessary dependencies.

The Interface Segregation Principle ensures that a class only implements the methods that are necessary for it, which reduces unnecessary code and improves maintainability.

- **Small, Specific Interfaces:** Instead of having one large interface with many methods, create smaller, more focused interfaces.
- **Client-Focused:** Design interfaces that match the specific needs of the client or implementing class.

**Example:**

```java
interface Worker {
    void work();
    void eat();
}

class Programmer implements Worker {
    public void work() {
        System.out.println("Writing code...");
    }

    public void eat() {
        System.out.println("Eating lunch...");
    }
}

class Robot implements Worker {
    public void work() {
        System.out.println("Building cars...");
    }

    public void eat() {
        // Robots don't need to eat, but are forced to implement this method
        throw new UnsupportedOperationException("Robots don't eat!");
    }
}
```

**Refactored Example:**

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Programmer implements Workable, Eatable {
    public void work() {
        System.out.println("Writing code...");
    }

    public void eat() {
        System.out.println("Eating lunch...");
    }
}

class Robot implements Workable {
    public void work() {
        System.out.println("Building cars...");
    }
}
```

**Explanation:**

- **Initial Example:** The `Worker` interface violates the Interface Segregation Principle because it forces the `Robot` class to implement the `eat()` method, even though robots don't need to eat. This leads to an unnecessary and potentially problematic method in the Robot class.

- **Refactored Example:** The interfaces are now split into `Workable` and `Eatable`, each representing a specific responsibility. The `Programmer` class implements both interfaces because it needs both functionalities, while the `Robot` class only implements the `Workable` interface since it doesn't need to eat. This adheres to ISP by ensuring that classes are not forced to implement unnecessary methods, making the code more flexible and easier to maintain.

## D - Dependency Inversion Principle

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions. Additionally, abstractions should not depend on details. Details should depend on abstractions.

The Dependency Inversion Principle encourages decoupling by ensuring that both high-level and low-level modules rely on abstractions (e.g., interfaces) rather than concrete implementations. This improves the flexibility and maintainability of the system.

- **Depend on Abstractions:** Instead of depending on concrete classes, high-level and low-level modules should depend on interfaces or abstract classes.
- **Inversion of Control:** Control over dependencies should be inverted, meaning that the creation of dependencies is handled externally, often via dependency injection.

**Example:**

```java
class PDFReport {
    public void generatePDFReport(String content) {
        System.out.println("Generating PDF report with content: " + content);
    }
}

class ExcelReport {
    public void generateExcelReport(String content) {
        System.out.println("Generating Excel report with content: " + content);
    }
}

class ReportGenerator {
    private PDFReport pdfReport;
    private ExcelReport excelReport;

    public ReportGenerator() {
        this.pdfReport = new PDFReport();
        this.excelReport = new ExcelReport();
    }

    public void generateReport(String format, String content) {
        if (format.equals("PDF")) {
            pdfReport.generatePDFReport(content); 
        } else if (format.equals("Excel")) {
            excelReport.generateExcelReport(content);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        ReportGenerator reportGenerator = new ReportGenerator();
        reportGenerator.generateReport("PDF", "Annual Report 2024");
        reportGenerator.generateReport("Excel", "Monthly Report August");
    }
}
```

**Refactored Example:**

```java
interface ReportFormat {
    void generateReport(String content);
}

class PDFReport implements ReportFormat {
    @Override
    public void generateReport(String content) {
        System.out.println("Generating PDF report with content: " + content);
    }
}

class ExcelReport implements ReportFormat {
    @Override
    public void generateReport(String content) {
        System.out.println("Generating Excel report with content: " + content);
    }
}

class ReportGenerator {
    private ReportFormat reportFormat;

    public ReportGenerator(ReportFormat reportFormat) {
        this.reportFormat = reportFormat;
    }

    public void generateReport(String content) {
        reportFormat.generateReport(content);
    }
}

public class Main {
    public static void main(String[] args) {
        ReportFormat pdfReport = new PDFReport();
        ReportGenerator pdfGenerator = new ReportGenerator(pdfReport);
        pdfGenerator.generateReport("Annual Report 2024");
        ReportFormat excelReport = new ExcelReport();
        ReportGenerator excelGenerator = new ReportGenerator(excelReport);
        excelGenerator.generateReport("Monthly Report August");
    }
}
```
**Explanation:**

- **Initial Example:** The `ReportGenerator` class depends directly on `PDFReport` and `ExcelReport`. This design forces changes in `ReportGenerator` for new report formats, violating the Dependency Inversion Principle.

- **Refactored Example:** By introducing the `ReportFormat` interface, the `ReportGenerator` class depends on an abstraction rather than concrete implementations. This allows `ReportGenerator` to handle any report format (like `PDFReport` or `ExcelReport`) without needing modifications. The system is now more flexible and adheres to the Dependency Inversion Principle.

# Design Patterns

Here are the types of design patterns:

- [Creational Design Patterns](https://github.com/arifulnoman/design-patterns/blob/main/README.md#creational-design-patterns)
- [Behavioral Design Patterns](https://github.com/arifulnoman/design-patterns/blob/main/README.md#behavioral-design-patterns)
- [Structural Design Patterns](https://github.com/arifulnoman/design-patterns/blob/main/README.md#structural-design-patterns)

## Creational Design Patterns

**Concept:**
Creational design patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. They abstract the instantiation process and provide more flexibility in deciding which objects need to be created for a given scenario. These patterns focus on handling the complexities of creating objects in a clean and efficient manner, such as controlling the object lifecycle, avoiding resource overhead, or ensuring the reuse of instances.

**Key Idea:**
They abstract how objects are created, ensuring that the system is independent of how its objects are constructed, composed, and represented.

**Common Creational Patterns:**

- Factory Method
- Abstract Factory
- Singleton
- Builder
- Prototype

### Singleton Pattern (Creational Pattern)

**Definition:** The Singleton Pattern ensures that a class has only one instance and provides a global point of access to that instance. This pattern is often used for managing shared resources like database connections, configuration settings or logging. The Singleton pattern solves two problems at the same time, violating the Single Responsibility Principle

**Key Characteristics**

- **Private Constructor:** Prevents the creation of multiple instances.
- **Static Method:** Provides a global point of access to the instance.

**Example**

```java
class DatabaseConnectionManager {

	private static DatabaseConnectionManager instance = new DatabaseConnectionManager();

	private DatabaseConnectionManager() {
		System.out.println("Database connection established.");
	}

	public static DatabaseConnectionManager getInstance() {
		return instance;
	}

	public void executeQuery(String query) {
	    System.out.println("Executing query: " + query);
	}
}
public class Singleton {

	public static void main(String[] args) {

        DatabaseConnectionManager dbManager1 = DatabaseConnectionManager.getInstance();
        dbManager1.executeQuery("SELECT * FROM users");

        DatabaseConnectionManager dbManager2 = DatabaseConnectionManager.getInstance();
        dbManager2.executeQuery("INSERT INTO users (name) VALUES ('John Doe')");

        System.out.println(dbManager1 == dbManager2);
    }
}
```

**Output:**

```
Database connection established.
Executing query: SELECT * FROM users
Executing query: INSERT INTO users (name) VALUES ('John Doe')
true
```

**Explanation:**

The Singleton Pattern in our code `DatabaseConnectionManager` class ensures that only one instance of the class is created and used throughout the application. This is achieved through the following mechanisms:

1. **Private Constructor**: The constructor is private, preventing direct instantiation of the class from outside. This ensures that new instances cannot be created using the `new` keyword.
2. **Static Instance**: A private static instance of the class (`instance`) is eagerly initialized when the class is loaded. This means the instance is created at the time of class loading, ensuring that only one instance exists throughout the application.

3. **Global Access Method**: The class provides a public static method, `getInstance()`, which returns the single instance. Since the instance is created during class loading, this method simply returns the already-created instance.

In the `main` method, both `dbManager1` and `dbManager2` refer to the same instance of `DatabaseConnectionManager`. When you call the `executeQuery()` method on either instance, it operates on the same object, demonstrating that the Singleton pattern effectively maintains a single shared instance across the application.

Finally, the comparison `System.out.println(dbManager1 == dbManager2)` returns `true`, confirming that both `dbManager1` and `dbManager2` refer to the same instance, which validates the Singleton pattern's correct implementation.

#### Use Cases of Singleton Pattern

The Singleton pattern ensures a class has only one instance and provides a global access point to it. It is useful when you need to share a single object across the entire application.

#### **Common Use Cases**

1. **Logging:** Ensure only one logging instance is used throughout the application to avoid multiple log files.
2. **Configuration Settings:** Load and use one instance of configuration data across the system.
3. **Database Connections:** Share a single database connection to optimize resource usage.
4. **Caching:** Use one cache instance to store and access frequently used data.
5. **Resource Manager:** Track and allocate limited resources (like file handles or sockets) using a single manager instance.
6. **Service Locator:** Retrieve services or dependencies from a single point of access.
7. **Global State:** Manage global data (like user preferences or app settings) in one place.
8. **Window Manager:** Control window placement and appearance in a graphical interface using a single instance.

#### **When to Avoid Singleton**

- **Testing:** Singletons can make testing harder due to shared global state.
- **Tight Coupling:** Overuse can lead to tightly connected code, reducing flexibility.
- **Concurrency:** Poor implementation can cause issues in multi-threaded environments.

#### **Conclusion**
The Singleton pattern is effective when you need one instance of a class across your application. Use it wisely to avoid potential downsides in testing, coupling, and multi-threading.


**Pros and Cons:**
| Pros | Cons |
| ----------------------------------------- | -------------------------------------------- |
| Ensures only one instance is created | Global state can make debugging harder |
| Easy access to the instance globally | Hard to test because of single instance |
| Saves memory by limiting instances | Can cause issues in multi-threaded programs |
| Useful for shared resources (e.g. logging)| Makes code less flexible and harder to change|
| Can delay creation until needed | Can become a performance bottleneck |

**Improvement:**

**Issue**: If the `DatabaseConnectionManager` instance is a heavy resource user or is not always needed, the current implementation has a drawback. The instance is eagerly initialized when the class is loaded, which can waste resources. Instead, we should use lazy initialization to create the instance only when it is actually needed.

**Solution**: Implement lazy initialization to delay the creation of the Singleton instance until `getInstance()` is called for the first time. This optimizes resource usage and ensures that the instance is only created when necessary.

**Improved Code with Lazy Initialization:**

```java
private static DatabaseConnectionManager instance;

public static synchronized DatabaseConnectionManager getInstance() {
    if (instance == null) {
        instance = new DatabaseConnectionManager();
    }
    return instance;
}
```

**Issue**: If multiple threads access the `getInstance()` method simultaneously and find that the `DatabaseConnectionManager` instance is `null`, they may both create separate instances before the instance variable is updated. This can lead to multiple instances being created, violating the Singleton pattern.

**Solution**: Use a `synchronized` block inside the `getInstance()` method to ensure that only one thread can create the instance at a time. Additionally, declare the instance variable as `volatile` to ensure proper visibility and prevent multiple instances from being created.

**Improved Code with Thread Safety: (Double-Checked Locking)**

```java
private static DatabaseConnectionManager instance;

public static DatabaseConnectionManager getInstance() {
    if (instance == null) {
        synchronized (DatabaseConnectionManager.class) {
            if (instance == null) {
                instance = new DatabaseConnectionManager();
            }
        }
    }
    return instance;
}
```

**Issue:** The double-checked locking pattern, while intended to provide a thread-safe Singleton implementation with minimal synchronization overhead, can be complex and error-prone. If not implemented correctly, it can lead to subtle concurrency issues and make the code harder to maintain.

**Solution:** Use the enum type to implement the Singleton pattern. Enums inherently provide thread safety, handle serialization correctly, and prevent reflection attacks. They simplify the implementation by leveraging the JVM’s guarantees for enum instance creation.

**Improved Code with Enum Singleton:**

```java
public enum DatabaseConnectionManager {
    INSTANCE;

    private DatabaseConnectionManager() {
        System.out.println("Database connection established.");
    }

    public static DatabaseConnectionManager getInstance() {
        return INSTANCE;
    }

    public void executeQuery(String query) {
        System.out.println("Executing query: " + query);
    }
}
```

**Explanation:**

- **Thread Safety:** The JVM (Java Virtual Machine) guarantees that the enum instance is created in a thread-safe manner, eliminating the need for explicit synchronization.
- **Serialization:** Enum instances are automatically serialized correctly, ensuring that only one instance exists even after deserialization.

## Behavioral Design Patterns

**Concept:**
Behavioral design patterns are concerned with communication between objects and how responsibilities are assigned to classes and objects. These patterns focus on the interaction between objects, making it easier for objects to communicate in a way that increases flexibility in carrying out complex tasks. They help define how objects cooperate to perform tasks and assign clear responsibilities among them.

**Key Idea:**
They focus on object interaction and responsibility. These patterns help to design efficient algorithms that ensure objects interact in ways that fulfill the system's needs.

**Common Behavioral Patterns:**

- Observer
- Strategy
- Command
- Chain of Responsibility
- Mediator
- State
- Template Method

### Chain of Responsibility (Behavioral Pattern)

**Definition:** The Chain of Responsibility Pattern allows a request to be passed along a chain of handlers. Each handler can either process the request or pass it to the next handler in the chain. This pattern promotes loose coupling between sender and receiver and provides a flexible way to handle requests.

**Key Characteristics**

- **Handler Interface:** Defines an interface for handling requests and a reference to the next handler in the chain.
- **Concrete Handlers:** Implement the handler interface and either handle the request or pass it to the next handler.
- **Client:** Initiates the request and passes it into the chain.

**Example**

```java
abstract class RequestHandler {

    protected RequestHandler next;

    public void setNext(RequestHandler next) {
        this.next = next;
    }

    public abstract void handleRequest(String requestType);
}

class OrderPlacementHandler extends RequestHandler {
    @Override
    public void handleRequest(String requestType) {

        if (requestType.equalsIgnoreCase("OrderPlacement")) {
            System.out.println("Handling Order Placement");
        }
        else if (next != null) {
            next.handleRequest(requestType);
        }
    }
}

class OrderCancellationHandler extends RequestHandler {
    @Override
    public void handleRequest(String requestType) {

        if (requestType.equalsIgnoreCase("OrderCancellation")) {
            System.out.println("Handling Order Cancellation");
        }
        else if (next != null) {
            next.handleRequest(requestType);
        }
    }
}

class RefundHandler extends RequestHandler {
    @Override
    public void handleRequest(String requestType) {

        if (requestType.equalsIgnoreCase("Refund")) {
            System.out.println("Handling Refund");
        }
        else if (next != null) {
            next.handleRequest(requestType);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        RequestHandler orderPlacementHandler = new OrderPlacementHandler();
        RequestHandler orderCancellationHandler = new OrderCancellationHandler();
        RequestHandler refundHandler = new RefundHandler();

        orderPlacementHandler.setNext(orderCancellationHandler);
        orderCancellationHandler.setNext(refundHandler);

        orderPlacementHandler.handleRequest("OrderPlacement");
        orderPlacementHandler.handleRequest("OrderCancellation");
        orderPlacementHandler.handleRequest("Refund");
    }
}
```

**Output:**

```
Handling Order Placement
Handling Order Cancellation
Handling Refund
```

**Explanation:**

In this code, the Chain of Responsibility pattern is implemented using a chain of request handlers:

1. **Abstract Handler (`RequestHandler`)**: This abstract class defines a method for handling requests and maintains a reference to the next handler in the chain.
2. **Concrete Handlers**: Classes such as `OrderPlacementHandler`, `OrderCancellationHandler`, and `RefundHandler` extend the `RequestHandler` class and implement the `handleRequest()` method. Each handler either processes the request or passes it to the next handler in the chain.

3. **Client Code**: The client creates the chain of handlers by linking them together and then sends various requests to the first handler in the chain.

This pattern allows the system to be extended by adding new handlers without modifying existing code. For example, new request types can be added by creating new handler classes and linking them into the chain.

#### Use Cases of Chain of Responsibility Pattern

The Chain of Responsibility pattern allows multiple objects to handle a request sequentially. If one handler cannot process the request, it passes the request to the next handler in the chain. This is useful for decoupling the sender of a request from its receivers.

#### **Common Use Cases**

1. **Event Handling (GUIs):** Pass events like clicks or key presses through UI components until one handles it.
2. **Middleware Processing:** Web requests pass through layers like logging, authentication and authorization, each handling its part.
3. **Exception Handling:** Exceptions move through handlers until one resolves the issue.
4. **Support Systems:** Customer issues escalate through support levels (e.g., Level 1 to Level 3) until resolved.
5. **Logging:** Log messages go through a chain of loggers, each handling specific log levels (INFO, ERROR).
6. **Approval Workflow:** Requests are passed through multiple approval levels, such as manager, director, and CFO.
7. **File Handling:** Different file formats are passed to handlers, with each handler processing the appropriate format.
8. **Command Processing:** Commands are passed through processors, each handling specific types of commands (e.g., weather, news).

#### **When to Avoid Chain of Responsibility**

- **Complexity:** If the chain becomes too long, it can lead to performance issues and difficult debugging.
- **Order Sensitivity:** If the order of handlers matters, the chain can become fragile and hard to maintain.
- **Undefined Handling:** If no handler processes the request, you need a fallback mechanism to avoid unhandled cases.

#### **Conclusion**
The Chain of Responsibility pattern is helpful for organizing multiple handlers for a task. Use it when you want flexibility in passing requests through a series of processors without tightly coupling them.


**Pros and Cons:**
| Pros | Cons |
| ----------------------------------------- | -------------------------------------------- |
| Promotes loose coupling between sender and receiver | Can make the request flow hard to follow |
| Makes it easy to add new handlers | If not careful, the chain can grow too long |
| Flexibility in order of processing requests | There might be no handler to process a request |
| Handlers can focus on specific tasks | Increased complexity with long chains |
| Supports dynamic composition of objects | Can be hard to debug and maintain |

**Improvement:**

**Issue:** The current implementation is not optimal for situations where only one handler is expected to process a request, but the request is passed down the chain even after being handled.

**Solution:** Introduce a mechanism where once a request is handled, it stops propagating down the chain. This can be done by adding a return type to the `handleRequest()` method that indicates whether the request has been processed.

**Improved Code:**

```java
abstract class RequestHandler {
    protected RequestHandler next;

    public void setNext(RequestHandler next) {
        this.next = next;
    }

    public abstract boolean handleRequest(String requestType);
}

class OrderPlacementHandler extends RequestHandler {
    @Override
    public boolean handleRequest(String requestType) {
        if (requestType.equalsIgnoreCase("OrderPlacement")) {
            System.out.println("Handling Order Placement");
            return true;  // Request handled, stop chain
        } else if (next != null) {
            return next.handleRequest(requestType);
        }
        return false;  // Request not handled
    }
}

class OrderCancellationHandler extends RequestHandler {
    @Override
    public boolean handleRequest(String requestType) {
        if (requestType.equalsIgnoreCase("OrderCancellation")) {
            System.out.println("Handling Order Cancellation");
            return true;
        } else if (next != null) {
            return next.handleRequest(requestType);
        }
        return false;
    }
}

class RefundHandler extends RequestHandler {
    @Override
    public boolean handleRequest(String requestType) {
        if (requestType.equalsIgnoreCase("Refund")) {
            System.out.println("Handling Refund");
            return true;
        } else if (next != null) {
            return next.handleRequest(requestType);
        }
        return false;
    }
}
```

## Structural Design Patterns

**Concept:**
Structural design patterns focus on how classes and objects are composed to form larger structures. These patterns simplify the design by identifying a simple way to realize relationships between entities. They help ensure that if one part of the system changes, the entire structure does not need to be modified. Structural patterns help to define relationships between classes and objects in a way that the entire structure is easier to manage and understand.

**Key Idea:**
They deal with object composition and typically help to ensure that classes and objects work together seamlessly. These patterns also help in making the system more flexible and reusable.

**Common Structural Patterns:**

- Adapter
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Proxy
