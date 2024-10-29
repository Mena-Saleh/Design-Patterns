### Strategy

The Strategy pattern is a behavioral design pattern that lets you define a family of algorithms, put each of them in a separate class, and make their objects interchangeable. This pattern allows the algorithm (or strategy) to vary independently from the client that uses it. It’s ideal when you want to choose an algorithm at runtime based on certain conditions.

#### Key Points of the Strategy Pattern

- **Encapsulates Algorithms**: Each algorithm or behavior is encapsulated in its own class, allowing easy switching between different strategies.
- **Interchangeable Strategies**: You can swap strategies dynamically at runtime without altering the client code.
- **Follows Open-Closed Principle**: The pattern allows extending or adding new strategies without modifying existing code.

#### Benefits of the Strategy Pattern

- **Easily Switch Algorithms**: Changing the behavior of a class can be done by switching out one strategy for another.
- **Improved Maintainability**: Each strategy is isolated in its own class, making code easier to understand and modify.
- **Decouples Client and Algorithm**: The client doesn’t need to know the details of each strategy, only that it has a specific interface to call.

#### Example in C#

Imagine a payment system where we want to allow customers to choose between different payment methods, like credit card, PayPal, or Google Pay. With the Strategy pattern, we can encapsulate each payment method as a separate strategy, making it easy to add or change payment methods.

1. **Define the Strategy Interface**: Each strategy implements this interface.
2. **Concrete Strategies**: Each concrete strategy defines a specific algorithm or behavior (e.g., paying with a credit card or PayPal).
3. **Context**: The client (e.g., an `Order` class) maintains a reference to a `PaymentStrategy` and delegates the payment behavior to it.

Here’s how the Strategy pattern works:

```csharp
// Step 1: Define the Strategy Interface
public interface IPaymentStrategy
{
    void Pay(decimal amount);
}

// Step 2: Implement Concrete Strategies
public class CreditCardPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount:C} using Credit Card.");
    }
}

public class PayPalPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount:C} using PayPal.");
    }
}

public class GooglePayPayment : IPaymentStrategy
{
    public void Pay(decimal amount)
    {
        Console.WriteLine($"Paid {amount:C} using Google Pay.");
    }
}

// Step 3: Create the Context class
public class Order
{
    private IPaymentStrategy _paymentStrategy;

    // Constructor that accepts a payment strategy
    public Order(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }

    // Allows setting a new payment strategy at runtime
    public void SetPaymentStrategy(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }

    public void ProcessOrder(decimal amount)
    {
        Console.WriteLine("Processing order...");
        _paymentStrategy.Pay(amount);
    }
}

// Testing the Strategy Pattern
class Program
{
    static void Main()
    {
        // Create an order with a specific payment strategy
        Order order = new Order(new CreditCardPayment());
        order.ProcessOrder(100.00m);  // Output: Paid $100.00 using Credit Card.

        // Change the payment strategy dynamically
        order.SetPaymentStrategy(new PayPalPayment());
        order.ProcessOrder(50.00m);   // Output: Paid $50.00 using PayPal.

        // Another strategy
        order.SetPaymentStrategy(new GooglePayPayment());
        order.ProcessOrder(75.00m);   // Output: Paid $75.00 using Google Pay.
    }
}
```

#### Explanation of the Code

- **IPaymentStrategy Interface**: This interface defines a `Pay` method that each payment strategy must implement. The `Order` class can call this method without needing to know which payment method is being used.
- **Concrete Strategy Classes (CreditCardPayment, PayPalPayment, GooglePayPayment)**:
  - Each of these classes implements `IPaymentStrategy` and defines the `Pay` method for a specific payment method.
  - By implementing `IPaymentStrategy`, they can be used interchangeably with any client expecting an `IPaymentStrategy`.
- **Order Class (Context)**:
  - This class holds a reference to an `IPaymentStrategy` object. It delegates the `Pay` action to whatever strategy it currently has.
  - The `SetPaymentStrategy` method allows the client to change the payment strategy dynamically at runtime.

#### Using the Strategy Pattern

In `Main`, we create an `Order` with an initial payment strategy (`CreditCardPayment`).

- The client can process the order using the chosen payment strategy. If they wish to switch payment methods, they can use `SetPaymentStrategy` to assign a different strategy, like `PayPalPayment` or `GooglePayPayment`.

#### Why Use the Strategy Pattern?

- **Flexibility to Change Behavior**: The Strategy pattern lets you change the behavior of a class at runtime by switching strategies.
- **Extensibility**: You can add new strategies without changing the client’s code, making the application open to extension but closed to modification.
- **Encapsulation of Algorithms**: By encapsulating each algorithm in its own class, you avoid complex conditional statements within the client code, improving readability and maintainability.

The Strategy pattern is commonly used when you have multiple ways to perform a specific action and want to decide on the specific approach at runtime. It’s widely used in applications like payment systems, sorting mechanisms, and even in game development where behaviors can vary based on context.
