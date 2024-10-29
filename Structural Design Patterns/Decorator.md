### Decorator

The Decorator pattern is a structural design pattern that allows you to dynamically add new functionality to an object without altering its structure. Instead of modifying the class itself, you "wrap" it in decorator classes that add the desired behaviors. This pattern is especially useful when you want to add responsibilities to objects without creating an extensive hierarchy of subclasses.

#### Key Points of the Decorator Pattern

- **Adds Responsibilities Dynamically**: Allows adding new functionality at runtime, instead of through inheritance, making the design flexible and scalable.
- **Wraps Original Objects**: A decorator wraps the original object and provides additional features without altering the original object's class.
- **Supports Open-Closed Principle**: Keeps classes open for extension but closed for modification, aligning with clean design principles.

#### Benefits of the Decorator Pattern

- **Flexibility**: You can add or remove functionalities at runtime without modifying the object's class.
- **Composability**: You can combine multiple decorators to add various features, making the object very flexible.
- **Avoids Subclass Explosion**: Instead of creating subclasses for each combination of features, decorators allow for creating combinations at runtime, saving time and reducing code complexity.

#### Example in C#

Let’s consider an example of a simple notification system. We have an `INotification` interface that represents a notification type. We want to add features to send notifications via multiple channels like SMS or email, but we don’t want to modify the existing `Notification` class directly. Instead, we’ll use decorators to add these features.

1. **Define the Interface**: The `INotification` interface defines the main method.
2. **Concrete Component**: The `BasicNotification` class implements the interface and represents the core functionality.
3. **Decorator Classes**: Additional decorators like `EmailDecorator` and `SMSDecorator` wrap the base notification to add new functionality.

Here’s how the Decorator pattern works in C#:

```csharp
// Step 1: Define the INotification interface
public interface INotification
{
    void Send(string message);
}

// Step 2: Create the Concrete Component implementing INotification
public class BasicNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine("Sending basic notification: " + message);
    }
}

// Step 3: Create the abstract Decorator class
public abstract class NotificationDecorator : INotification
{
    protected INotification _notification;

    public NotificationDecorator(INotification notification)
    {
        _notification = notification;
    }

    public virtual void Send(string message)
    {
        _notification.Send(message);
    }
}

// Step 4: Create Concrete Decorators
public class EmailDecorator : NotificationDecorator
{
    public EmailDecorator(INotification notification) : base(notification) {}

    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine("Sending email notification: " + message);
    }
}

public class SMSDecorator : NotificationDecorator
{
    public SMSDecorator(INotification notification) : base(notification) {}

    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine("Sending SMS notification: " + message);
    }
}

// Testing the Decorator
class Program
{
    static void Main()
    {
        // Basic notification
        INotification notification = new BasicNotification();

        // Adding email functionality
        notification = new EmailDecorator(notification);

        // Adding SMS functionality on top
        notification = new SMSDecorator(notification);

        // Now it sends basic, email, and SMS notifications
        notification.Send("Hello World!");

        // Output:
        // Sending basic notification: Hello World!
        // Sending email notification: Hello World!
        // Sending SMS notification: Hello World!
    }
}
```

#### Explanation of the Code

- **INotification Interface**: This interface has a `Send` method that any notification type must implement. It represents the base functionality.
- **BasicNotification Class**: This is the concrete component that implements `INotification`. Its `Send` method outputs a simple message, representing the core functionality.
- **NotificationDecorator Abstract Class**:
  - This class implements `INotification` and wraps another `INotification` instance (passed through the constructor).
  - It holds a reference to the base component (`_notification`) and delegates the `Send` call to it. It acts as a foundation for any decorator that wants to add functionality to `INotification`.
- **EmailDecorator and SMSDecorator Classes**:
  - These concrete decorators extend `NotificationDecorator`.
  - Each one overrides the `Send` method, calling `base.Send(message)` to execute any previously added decorators and then adding its specific behavior (sending an email or SMS).

#### Using the Decorators

In `Main`, we start with a `BasicNotification` instance.

- We then wrap it with an `EmailDecorator`, adding email functionality.
- Finally, we wrap it with an `SMSDecorator`, adding SMS functionality. Each decorator builds on the previous, adding a new layer of behavior.

#### Why Use the Decorator Pattern?

- **Add Functionality Without Modifying the Base Class**: Instead of modifying `BasicNotification` directly, we add new behavior via decorators, making it more maintainable.
- **Combine Multiple Behaviors**: Decorators allow us to stack multiple behaviors dynamically, creating customized notifications (e.g., Basic + Email + SMS).
- **Adheres to Open-Closed Principle**: We extend the `INotification` functionality by creating decorators rather than changing the base class, which keeps the code flexible and open for future extensions.

The Decorator pattern is useful when you need dynamic behavior or want to add/remove features without altering the original class or relying on complex inheritance structures.
