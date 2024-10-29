### Adapter

The Adapter pattern is a structural design pattern used to allow incompatible interfaces to work together. It acts as a bridge between two incompatible classes by "adapting" one interface to another expected by the client. This pattern is useful when you have a class that doesn’t match the required interface but you want to make it usable by adapting it.

#### Key Points of the Adapter Pattern

- **Compatibility**: The Adapter pattern allows you to make an existing class compatible with another class or interface without modifying either class.
- **Single Responsibility**: The adapter handles the conversion, keeping each class focused on its main task.
- **Flexibility**: It allows you to work with different types of incompatible objects without changing existing code.

#### Benefits of the Adapter Pattern

- **Reuse Existing Classes**: Allows you to use existing classes with a new interface, saving time and effort compared to rewriting or reworking them.
- **Interchangeability**: Different adapters can be created to make the same class compatible with multiple interfaces.
- **Improved Maintainability**: The adapter separates conversion logic from the main application logic, keeping the code clean and easy to maintain.

#### Example in C#

Imagine a scenario where we have an application that expects an `ITarget` interface, but we have a legacy class (`LegacySystem`) that does not implement it. We’ll use the Adapter pattern to make `LegacySystem` compatible with the `ITarget` interface.

1. **Define the Target Interface**: This is the interface our application expects.
2. **Legacy System**: This is an existing class that we want to use but doesn’t match the target interface.
3. **Adapter**: The Adapter class implements the target interface and wraps the legacy system, converting its interface to match the target.

Here’s how it looks:

```csharp
// Step 1: Define the Target Interface that the application expects
public interface ITarget
{
    void Request();
}

// Step 2: Create the Legacy System with an incompatible interface
public class LegacySystem
{
    public void SpecificRequest()
    {
        Console.WriteLine("Legacy System: Handling request in a specific way.");
    }
}

// Step 3: Create the Adapter class that implements ITarget and wraps LegacySystem
public class Adapter : ITarget
{
    private readonly LegacySystem _legacySystem;

    public Adapter(LegacySystem legacySystem)
    {
        _legacySystem = legacySystem;
    }

    public void Request()
    {
        // Call the legacy system's method in a way that matches ITarget's Request method
        _legacySystem.SpecificRequest();
    }
}

// Testing the Adapter
class Program
{
    static void Main()
    {
        // Legacy system without adapter would be incompatible with ITarget
        LegacySystem legacySystem = new LegacySystem();

        // Use the adapter to make the legacy system compatible
        ITarget adapter = new Adapter(legacySystem);

        // Now we can use the legacy system through the adapter
        adapter.Request();  // Output: Legacy System: Handling request in a specific way.
    }
}
```

#### Explanation of the Code

- **ITarget Interface**: This is the interface our application expects. It has a `Request` method, which represents the action we want to perform. Any class implementing `ITarget` is expected to have this `Request` method.
- **LegacySystem Class**: This is an existing class with a method `SpecificRequest` that performs a similar function to what we need, but its interface does not match `ITarget`.
  - Directly using `LegacySystem` with code expecting `ITarget` would cause incompatibility, as `SpecificRequest` does not match `Request`.
- **Adapter Class**: The `Adapter` class implements the `ITarget` interface, making it compatible with any code that expects an `ITarget`.
  - `Adapter` has a private field `_legacySystem` that stores a reference to the `LegacySystem`.
  - In the `Request` method, `Adapter` calls `SpecificRequest` on the `LegacySystem` instance, allowing `LegacySystem` to work as if it implements `ITarget`.

#### Testing the Adapter

In `Main`, we create an instance of `LegacySystem` and wrap it with `Adapter`. We can now interact with `LegacySystem` using the `Request` method through the `ITarget` interface, even though `LegacySystem` doesn’t directly implement `ITarget`.

#### Why Use the Adapter Pattern?

- **Compatibility without Modification**: You can make two incompatible interfaces work together without changing the original classes.
- **Extensibility**: You can create additional adapters to make `LegacySystem` compatible with more interfaces, if needed, without modifying the original class.
- **Code Reusability**: The Adapter pattern allows you to reuse existing code, even if it wasn’t designed with the current application’s interface requirements in mind.

The Adapter pattern is helpful whenever you need to integrate a new system or use an existing system with a different interface.
