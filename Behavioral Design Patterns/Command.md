### Command

The Command pattern is a behavioral design pattern that turns a request or an action into a stand-alone object called a "command." This object contains all the information needed to perform the action, including method parameters and the receiver object. This approach enables requests to be queued, logged, and even undone if necessary, making it especially useful in applications that require a flexible and extensible way to handle operations, like GUI applications, undo functionality, or macro recording.

#### Key Points of the Command Pattern

- **Encapsulates Requests**: Each action is encapsulated as a command object, allowing the client to execute, undo, or log requests independently.
- **Separates Command and Receiver**: The pattern decouples the object that invokes the action (Invoker) from the object that performs it (Receiver).
- **Supports Undo/Redo**: Since each command is a standalone object, it’s easier to implement undo and redo operations by keeping track of commands.

#### Benefits of the Command Pattern

- **Decoupling of Sender and Receiver**: The pattern allows the sender to trigger an action without needing to know details of the receiver.
- **Easier to Add/Modify Commands**: New commands can be added independently, improving scalability and maintainability.
- **Supports Undo/Redo Functionality**: Commands can be stored and reversed, enabling undo/redo operations.

#### Example in C#

Let’s consider a text editor where you can perform actions like writing text, undoing, and redoing. Each action (like typing, undo, redo) can be encapsulated as a command, allowing us to execute and manage these commands more flexibly.

1. **Define the Command Interface**: This interface has an `Execute` method that all commands must implement.
2. **Concrete Command Classes**: Each command (e.g., `WriteTextCommand`) implements the command interface.
3. **Invoker**: The invoker (e.g., a button or a menu item) keeps a reference to a command and can call `Execute` to perform the command.
4. **Receiver**: The object that actually performs the action, such as a `TextEditor` class.

Here’s how the Command pattern works in this example:

```csharp
// Step 1: Define the Command Interface
public interface ICommand
{
    void Execute();
    void Undo();
}

// Step 2: Create the Receiver class (TextEditor)
public class TextEditor
{
    private string _text = "";

    public void Write(string newText)
    {
        _text += newText;
        Console.WriteLine($"TextEditor: {_text}");
    }

    public void EraseLast(int length)
    {
        if (_text.Length >= length)
        {
            _text = _text.Substring(0, _text.Length - length);
            Console.WriteLine($"TextEditor: {_text}");
        }
    }
}

// Step 3: Implement Concrete Commands
public class WriteTextCommand : ICommand
{
    private readonly TextEditor _textEditor;
    private readonly string _text;

    public WriteTextCommand(TextEditor textEditor, string text)
    {
        _textEditor = textEditor;
        _text = text;
    }

    public void Execute()
    {
        _textEditor.Write(_text);
    }

    public void Undo()
    {
        _textEditor.EraseLast(_text.Length);
    }
}

// Step 4: Create the Invoker (e.g., a button or a menu item)
public class CommandInvoker
{
    private readonly Stack<ICommand> _commandHistory = new Stack<ICommand>();

    public void ExecuteCommand(ICommand command)
    {
        command.Execute();
        _commandHistory.Push(command);
    }

    public void UndoCommand()
    {
        if (_commandHistory.Count > 0)
        {
            ICommand command = _commandHistory.Pop();
            command.Undo();
        }
    }
}

// Testing the Command Pattern
class Program
{
    static void Main()
    {
        // Create the receiver
        TextEditor textEditor = new TextEditor();

        // Create the invoker
        CommandInvoker invoker = new CommandInvoker();

        // Create and execute commands
        ICommand writeHello = new WriteTextCommand(textEditor, "Hello ");
        ICommand writeWorld = new WriteTextCommand(textEditor, "World!");

        invoker.ExecuteCommand(writeHello);  // Output: TextEditor: Hello
        invoker.ExecuteCommand(writeWorld);  // Output: TextEditor: Hello World!

        // Undo last command
        invoker.UndoCommand();               // Output: TextEditor: Hello
        invoker.UndoCommand();               // Output: TextEditor:
    }
}
```

#### Explanation of the Code

- **ICommand Interface**: This interface has two methods, `Execute` and `Undo`, which each concrete command must implement. This standardizes how commands behave, making them interchangeable.
- **TextEditor Class (Receiver)**:
  - The `TextEditor` class has methods to perform the actual actions (writing and erasing text). These methods are called by the concrete commands.
  - It maintains the current state of text in `_text`.
- **WriteTextCommand Class (Concrete Command)**:
  - `WriteTextCommand` encapsulates the action of adding text to `TextEditor`.
  - In `Execute`, it calls `Write` on `TextEditor`, and in `Undo`, it calls `EraseLast` to remove the last part of the text added.
- **CommandInvoker Class (Invoker)**:
  - The `CommandInvoker` class keeps a history of commands so it can undo them.
  - The `ExecuteCommand` method executes a command and pushes it onto the history stack.
  - The `UndoCommand` method pops the last command from the stack and calls `Undo` on it, reversing the action.

#### Using the Command Pattern

In `Main`, we create a `TextEditor` and a `CommandInvoker`.

- We create commands to write text and pass them to the invoker.
- The invoker executes each command, updating the text editor’s state.
- If we want to undo actions, the invoker calls `UndoCommand`, which reverses each command in the order they were executed (LIFO order).

#### Why Use the Command Pattern?

- **Easily Add New Commands**: New commands can be added without modifying existing code. Just create a new class implementing `ICommand`.
- **Supports Undo/Redo**: Command history allows for undo and redo functionality, which is valuable in applications like text editors or drawing software.
- **Simplifies Client and Receiver Interaction**: The client only knows about the command, not the details of how the receiver performs actions, making it easier to manage complex systems with multiple actions.

The Command pattern is commonly used in applications that require action history, such as GUIs, macro recording systems, and even game development, where various commands (like move, attack) can be queued, executed, or undone.
