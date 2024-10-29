# SOLID Principles

The **SOLID** principles are a set of design guidelines that help developers create clean, maintainable, and scalable code. Each principle addresses common design issues, promoting robust and adaptable software design.

## S: Single Responsibility Principle (SRP)

**Definition**: A class should have only one reason to change, meaning each class should have a single responsibility or purpose.

**Explanation**: Each class should focus solely on a specific functionality. If a method or business logic is not directly related to the main purpose of the class, it should be moved to another class.

**Example**:
A `JobApplicationsManager` class should only handle job applications. If it also contains logic for sending emails or logging, this violates SRP. The email sending and logging logic should be moved to separate classes, and `JobApplicationsManager` can use these classes as clients.

---

## O: Open/Closed Principle (OCP)

**Definition**: A class should be open for extension but closed for modification once it reaches a somewhat final form.

**Explanation**: This principle allows you to extend a class’s functionality without modifying its existing code. This helps prevent changes in one area from causing bugs or errors in another.

**Example**:
If you have a `SalaryCalculator` class that needs to handle different employee types (hourly, monthly, intern, freelance), you shouldn't add conditions for each new type within `SalaryCalculator`. Instead, create subclasses (e.g., `HourlySalaryCalculator`) that extend `SalaryCalculator` and implement the specific logic.

**Notes**:
To achieve this, make `SalaryCalculator` an abstract class with virtual methods that can be overridden by subclasses.

---

## L: Liskov Substitution Principle (LSP)

**Definition**: Parent objects should be replaceable with their subtypes or subclasses without altering the program's correctness or causing errors.

**Explanation**: This principle ensures that subclasses can stand in for their parent classes without breaking the functionality or logic of the program.

**Example**:
Suppose you have a base class `Employee` with subclasses `SalariedEmployee`, `InternEmployee`, and `CEO`. If `Employee` has a `dismiss()` method, but `CEO` cannot be dismissed, this violates LSP. Instead, create a `DismissableEmployee` subclass with the `dismiss()` method, and have only the dismissable types inherit from it.

---

## I: Interface Segregation Principle (ISP)

**Definition**: Classes should not be forced to depend on interfaces they do not use.

**Explanation**: Large interfaces should be broken down into smaller, more specific ones to ensure that classes only implement methods that are relevant to them.

**Example**:
If you have a `Repository` pattern with an `IRepository<T>` interface containing methods like `getAll`, `getById`, `add`, `update`, and `delete`, some repositories may not need all these methods. Instead, create smaller interfaces, such as `IUpdatableRepository` and `IDeletableRepository`, to keep interfaces specific and focused.

**Notes**:
This approach promotes modularity and high cohesion, and also helps in satisfying the Liskov Substitution Principle.

---

## D: Dependency Inversion Principle (DIP)

**Definition**: High-level modules should not depend on low-level modules; both should depend on abstractions. Additionally, abstractions should not depend on details, but details should depend on abstractions.

**Explanation**: This principle aims to reduce tight coupling between components, making the system more modular and flexible.

**Example**:
If a `JobApplicationsManager` class (high-level module) depends directly on `Logger` and `EmailSender` classes (low-level modules), this creates tight coupling. Instead, `JobApplicationsManager` should depend on abstractions like `ILogger` and `IEmailSender` interfaces. The concrete implementations (e.g., `Logger`, `EmailSender`) are then injected via dependency injection.

**Notes**:
Dependency inversion is a principle, while dependency injection is a technique used to achieve this principle. Constructor injection is the most common method for implementing DIP, promoting loose coupling.

**Conclusion**:
Loose coupling (fewer dependencies) is generally better than tight coupling in a well-designed system, as it makes components easier to test, extend, and maintain.

---

By following these SOLID principles, you can write more adaptable and maintainable code, ultimately leading to higher-quality software.
