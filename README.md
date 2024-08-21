# SOLID

Here SOLID stand for
* S - Single Responsibility Principle
* O - Open Closed Principle
* L - Liskov Substitution Principle
* I - Interface Segregation Principle
* D - Dependency Inversion Principle

## S - Single Responsibility Principle

**Definition:** A class should have only one reason to change. In other words, a class should have one responsibility or one job.  

The idea behind SRP is that a class should focus on a single task or functionality. When a class is responsible for multiple concerns, it becomes more complex, harder to maintain, and more prone to bugs. By adhering to SRP, we can separate different functionalities into different classes, making the system modular and easier to understand.

* **One Responsibility:** A class should encapsulate a single functionality. For example, a Report class should only be responsible for creating a report, not for saving it to a file or sending it over email.

* **One Reason to Change:** If a class has multiple responsibilities, any change in one of those responsibilities can potentially affect the others, leading to unintended side effects.

**Example:**
```
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
```
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
* **Initial Example:** The `UserService` class violates SRP by handling both user authentication and report generation, leading to complex and less maintainable code.

* **Refactored Example:** By splitting into `AuthenticationService` and `ReportService`, each class now has a single responsibility. This makes the code easier to maintain and extend, as changes in one class do not affect the other.

## O - Open/Closed Principle

**Definition:** Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means you should be able to extend the behavior of a class or module without altering its existing code.

The Open/Closed Principle promotes writing code that is easy to extend with new functionality but doesn’t require changes to existing code, thus reducing the risk of introducing bugs.

* **Open for Extension:** The design should allow new functionality to be added. This can be achieved through inheritance, interfaces, or other extensibility mechanisms.
* **Closed for Modification:** Existing code should remain unchanged when new features or changes are introduced, ensuring stability and reliability.

**Example:**

```
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

```
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

* **Initial Example:** The `Main` class violates the Open/Closed Principle because it directly instantiates specific notification classes `WhatsAppNotification`, `MessengerNotification`. This requires modifying the `Main` class whenever a new notification type is added, making the code harder to maintain and extend.

* **Refactored Example:** By introducing the `Service` interface and using polymorphism, the `Main` class is now open for extension (new notification types can be added) but closed for modification (no changes needed in the existing code). This design allows for easier maintenance and scalability, as new notification services can be added without altering the core logic.

## L - Liskov Substitution Principle

**Definition:** Subtypes must be substitutable for their base types without altering the correctness of the program. In other words, objects of a derived class should be able to replace objects of the base class without affecting the functionality of the program.

The Liskov Substitution Principle ensures that a subclass can stand in for its parent class without causing unexpected behavior. This encourages the design of classes that follow consistent behavior when inherited, promoting the use of polymorphism.

* **Behavioral Consistency:** Subclasses should enhance or maintain the behavior of the base class, not contradict it.
* **No Broken Contracts:** The subclass should not violate any expectations set by the base class (e.g., method signatures, return types, or exceptions).

**Example:**

```
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
```
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
* **Initial Example:** The `SocialMediaTasks` class violates the Liskov Substitution Principle (LSP) because it forces all subclasses (`WhatsApp`, `Instagram`, `Facebook`) to inherit all methods, even if they do not support certain features. This can lead to runtime errors when unsupported methods like `postPicture` in `WhatsApp` or `callInGroup` in `Instagram` are called. This violates the principle, as the derived classes cannot replace the base class without causing issues.

* **Refactored Example:** By introducing separate interfaces (`SocialMediaTasks`, `GroupCall`, `Post`), the classes now adhere to LSP. Each class (`WhatsApp`, `Instagram`, `Facebook`) implements only the interfaces that represent the features they support. This design ensures that each subclass can fully replace the base class or interfaces without leading to runtime issues. The code is now more maintainable and flexible, as changes in one feature do not impact other unrelated features.

## I - Interface Segregation Principle

**Definition:** Clients should not be forced to depend on interfaces they do not use. Instead of creating large, monolithic interfaces, it's better to create smaller, more specific ones that are tailored to the exact needs of the client. This avoids bloated interfaces that can lead to unnecessary dependencies.

The Interface Segregation Principle ensures that a class only implements the methods that are necessary for it, which reduces unnecessary code and improves maintainability.

* **Small, Specific Interfaces:** Instead of having one large interface with many methods, create smaller, more focused interfaces.
* **Client-Focused:** Design interfaces that match the specific needs of the client or implementing class.

**Example:**

```
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
```
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
* **Initial Example:** The `Worker` interface violates the Interface Segregation Principle because it forces the `Robot` class to implement the `eat()` method, even though robots don't need to eat. This leads to an unnecessary and potentially problematic method in the Robot class.

* **Refactored Example:** The interfaces are now split into `Workable` and `Eatable`, each representing a specific responsibility. The `Programmer` class implements both interfaces because it needs both functionalities, while the `Robot` class only implements the `Workable` interface since it doesn't need to eat. This adheres to ISP by ensuring that classes are not forced to implement unnecessary methods, making the code more flexible and easier to maintain.

## L - Liskov Substitution Principle

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions. Additionally, abstractions should not depend on details. Details should depend on abstractions.

The Dependency Inversion Principle encourages decoupling by ensuring that both high-level and low-level modules rely on abstractions (e.g., interfaces) rather than concrete implementations. This improves the flexibility and maintainability of the system.

* **Depend on Abstractions:** Instead of depending on concrete classes, high-level and low-level modules should depend on interfaces or abstract classes.
* **Inversion of Control:** Control over dependencies should be inverted, meaning that the creation of dependencies is handled externally, often via dependency injection.

**Example:**

```
class LightBulb {
    public void turnOn() {
        System.out.println("LightBulb turned on");
    }

    public void turnOff() {
        System.out.println("LightBulb turned off");
    }
}

class Switch {
    private LightBulb bulb;

    public Switch(LightBulb bulb) {
        this.bulb = bulb;
    }

    public void operate(boolean on) {
        if (on) {
            bulb.turnOn();
        } else {
            bulb.turnOff();
        }
    }
}
```
**Refactored Example:**
```
interface Switchable {
    void turnOn();
    void turnOff();
}

class LightBulb implements Switchable {
    public void turnOn() {
        System.out.println("LightBulb turned on");
    }

    public void turnOff() {
        System.out.println("LightBulb turned off");
    }
}

class Fan implements Switchable {
    public void turnOn() {
        System.out.println("Fan turned on");
    }

    public void turnOff() {
        System.out.println("Fan turned off");
    }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate(boolean on) {
        if (on) {
            device.turnOn();
        } else {
            device.turnOff();
        }
    }
}
```

**Explanation:**  
* **Initial Example:** In this example, the `Switch` class directly depends on the `LightBulb` class. This means that if you need to change or replace the `LightBulb` with a different type of bulb or device, you would have to modify the `Switch` class. This tight coupling violates the Dependency Inversion Principle because the high-level module (`Switch`) depends directly on a low-level module (`LightBulb`).

* **Refactored Example:** The refactored code adheres to the Dependency Inversion Principle by introducing the Switchable interface, which acts as an abstraction for the devices that can be controlled. The `Switch` class now depends on the `Switchable` abstraction rather than a concrete implementation like `LightBulb`. This way, `Switch` can operate any device that implements the `Switchable` interface (such as `LightBulb` or `Fan`), without needing to know the specifics of those devices.
