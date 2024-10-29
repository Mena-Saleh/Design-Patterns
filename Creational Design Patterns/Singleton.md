### Singleton

The Singleton pattern ensures that only one instance of a class is created, and this instance is accessible globally. This is often used for classes that manage shared resources, like a configuration handler or a logger.

**Key Points of the Singleton Pattern in C#:**

- **Single Instance**: Only one instance of the class is created.
- **Global Access**: The instance can be accessed globally across the application.
- **Thread Safety**: Ensuring that only one instance is created, even if multiple threads attempt to create one simultaneously.

```csharp
public class Logger
{
    // Step 1: Create a private static instance of the class
    private static Logger _instance;

    // Step 2: Make the constructor private to prevent external instantiation
    private Logger() {}

    // Step 3: Provide a public static method to get the instance
    public static Logger GetInstance()
    {
        // Check if an instance already exists, if not, create one
        if (_instance == null)
        {
            _instance = new Logger();
        }
        return _instance;
    }

    // A sample method to show Singleton functionality
    public void Log(string message)
    {
        Console.WriteLine($"Log entry: {message}");
    }
}

// Test Singleton Behavior
class Program
{
    static void Main()
    {
        // Attempt to create two Logger instances
        Logger logger1 = Logger.GetInstance();
        Logger logger2 = Logger.GetInstance();

        // Test if both variables point to the same instance
        Console.WriteLine(logger1 == logger2);  // Output: True

        // Use the Singleton Logger instance
        logger1.Log("Singleton pattern in action!");
    }
}
```

**Explanation of the Code:**

- **\_instance Variable**: This static field holds the single instance of the `Logger` class.
- **Private Constructor**: By making the constructor private, we prevent any external class from directly instantiating `Logger`.
- **GetInstance Method**: This public static method checks if an instance of `Logger` already exists. If not, it creates one and returns it. If it does, it simply returns the existing instance.

**Benefits of Singleton in C#:**

- **Consistent State**: Since there’s only one instance, the state is consistent across the application.
- **Controlled Access to Shared Resources**: Ideal for situations where a resource, like a logger or configuration manager, should only have a single access point.
- **Memory Efficient**: Only one instance is created, saving memory.

**Thread-Safe Singleton (Optional)**  
To make this Singleton thread-safe (so that multiple threads don’t accidentally create multiple instances), you can add a lock:

```csharp
private static readonly object _lock = new object();

public static Logger GetInstance()
{
    if (_instance == null)
    {
        lock (_lock)
        {
            if (_instance == null)
            {
                _instance = new Logger();
            }
        }
    }
    return _instance;
}
```

This ensures that only one thread can create the instance at a time, even under heavy multithreaded use.
