# .NET (C#) Interview Questions and Answers

## Intermediate

11. **Explain polymorphism and its types in C#.**
12. **What are delegates and how are they used in C#?**
13. **Describe what LINQ is and give an example of where it might be used.**
14. **What is the difference between an abstract class and an interface?**
15. **How do you manage memory in .NET applications?**
16. **Explain the concept of threading in .NET.**
17. **What is async/await and how does it work?**
18. **Describe the Entity Framework and its advantages.**
19. **What are extension methods and where would you use them?**
20. **How do you handle exceptions in a method that returns a Task?**

### 11. Explain polymorphism and its types in C#.

**Answer:** Polymorphism is a core concept in object-oriented programming (OOP) that allows objects to be treated as instances of their parent class rather than their actual derived class. This enables methods to perform different tasks based on the object that invokes them, enhancing flexibility and enabling code reusability. In C#, polymorphism can be implemented in two ways: static (compile-time) polymorphism and dynamic (runtime) polymorphism.

- **Static Polymorphism:** Achieved through method overloading and operator overloading. It allows multiple methods or operators with the same name but different parameters to coexist, with the specific method or operator being invoked determined at compile time based on the arguments passed.

- **Dynamic Polymorphism:** Achieved through method overriding. It allows a method in a derived class to have the same name and signature as a method in its base class, but with different implementation details. The method that gets executed is determined at runtime, depending on the type of the object.

Here's an example demonstrating both types of polymorphism in C#:

```csharp
// Static Polymorphism (Method Overloading)
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
}

// Dynamic Polymorphism (Method Overriding)
public class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("The animal speaks");
    }
}

public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Calculator calc = new Calculator();
        Console.WriteLine(calc.Add(2, 3)); // Calls the first Add method
        Console.WriteLine(calc.Add(2, 3, 4)); // Calls the second Add method

        Animal myAnimal = new Animal();
        myAnimal.Speak(); // Output: The animal speaks

        Dog myDog = new Dog();
        myDog.Speak(); // Output: Dog barks

        Animal mySecondAnimal = new Dog();
        mySecondAnimal.Speak(); // Output: Dog barks, demonstrating dynamic polymorphism
    }
}
```

In the example above, the Calculator class demonstrates static polymorphism through method overloading, allowing the Add method to be called with different numbers of parameters. The Animal and Dog classes illustrate dynamic polymorphism, where the Speak method in the Dog class overrides the Speak method in its base class, Animal. The type of polymorphism used depends on the object reference at runtime, showcasing polymorphism's flexibility in OOP.

### 12. What are delegates and how are they used in C#?

**Answer:** Delegates in C# are type-safe function pointers or references to methods with a specific parameter list and return type. They allow methods to be passed as parameters, stored in variables, and returned by other methods, which enables flexible and extensible programming designs such as event handling and callback methods. Delegates are particularly useful in implementing the observer pattern and designing frameworks or components that need to notify other objects about events or changes without knowing the specifics of those objects.

There are three main types of delegates in C#:
- **Single-cast delegates:** Point to a single method at a time.
- **Multicast delegates:** Can point to multiple methods on a single invocation list.
- **Anonymous methods/Lambda expressions:** Allow inline methods or lambda expressions to be used wherever a delegate is expected.

Here is an example demonstrating the use of delegates in C#:

```csharp
public delegate void Operation(int num);

class Program
{
    static void Main(string[] args)
    {
        Operation op = Double;
        op(5);  // Output: 10

        op = Triple;
        op(5);  // Output: 15

        // Multicast delegate
        op = Double;
        op += Triple; // Combines Double and Triple methods
        op(5);  // Output: 10 followed by 15
    }

    static void Double(int num)
    {
        Console.WriteLine($"{num} * 2 = {num * 2}");
    }

    static void Triple(int num)
    {
        Console.WriteLine($"{num} * 3 = {num * 3}");
    }
}
```

In this example, the Operation delegate is defined to point to any method that accepts an int and returns void. Initially, op is set to the Double method, demonstrating a single-cast delegate. It is then reassigned to the Triple method, and finally, it is used as a multicast delegate to call both Double and Triple methods in sequence. This demonstrates how delegates in C# provide a flexible mechanism for method invocation and can be used to implement event handlers and callbacks.

### 13. Describe what LINQ is and give an example of where it might be used.

**Answer:** LINQ (Language Integrated Query) is a powerful feature in C# that allows developers to write expressive, readable code to query and manipulate data. It provides a uniform way to query various data sources, such as collections in memory, databases (via LINQ to SQL, LINQ to Entities), XML documents (LINQ to XML), and more. LINQ queries offer three main benefits: they are strongly typed, offer compile-time checking, and support IntelliSense, which enhances developer productivity and code maintainability.

LINQ can be used in a variety of scenarios, including filtering, sorting, and grouping data. It supports both method syntax and query syntax, providing flexibility in how queries are expressed.

Here is a simple example demonstrating LINQ with a list of integers:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

        // Use LINQ to find all even numbers
        var evenNumbers = from num in numbers
                          where num % 2 == 0
                          select num;

        Console.WriteLine("Even numbers:");
        foreach (var num in evenNumbers)
        {
            Console.WriteLine(num);
        }
    }
}
```

In this example, a LINQ query is used to filter a list of integers, selecting only the even numbers. The query is expressed using LINQ's query syntax, which closely resembles SQL in its readability and structure. This demonstrates how LINQ makes it easier to work with collections and other data sources by abstracting the complexity of different data manipulation operations.

### 14. What is the difference between an abstract class and an interface?

**Answer:** In C#, both abstract classes and interfaces are types that enable polymorphism, allowing objects of different classes to be treated as objects of a common super class. However, they serve different purposes and have different rules:

- **Abstract Class:**
  - Can contain implementation of methods, properties, fields, or events.
  - Can have access modifiers (public, protected, etc.).
  - A class can inherit from only one abstract class (single inheritance).
  - Can contain constructors.
  - Used when different implementations of objects have common methods or properties that can share a common implementation.

- **Interface:**
  - Cannot contain implementations, only declarations of methods, properties, events, or indexers.
  - Members of an interface are implicitly public.
  - A class or struct can implement multiple interfaces (multiple inheritance).
  - Cannot contain fields or constructors.
  - Used to define a contract for classes without imposing inheritance hierarchies.

Here is an example illustrating the use of an abstract class and an interface:

```csharp
public abstract class Animal
{
    public abstract void Eat();
    public void Sleep()
    {
        Console.WriteLine("Sleeping");
    }
}

public interface IMovable
{
    void Move();
}

public class Dog : Animal, IMovable
{
    public override void Eat()
    {
        Console.WriteLine("Dog is eating");
    }

    public void Move()
    {
        Console.WriteLine("Dog is running");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Dog myDog = new Dog();
        myDog.Eat();
        myDog.Sleep();
        myDog.Move();
    }
}
```

In this example, Animal is an abstract class that provides a default implementation of the Sleep method and an abstract Eat method that must be overridden. IMovable is an interface that defines a contract with a Move method that must be implemented. Dog inherits from Animal and implements IMovable, thereby fulfilling both the contract defined by the interface and extending the functionality provided by the abstract class.

### 15. How do you manage memory in .NET applications?

**Answer:** Memory management in .NET applications is primarily handled automatically by the Garbage Collector (GC), which provides a high-level abstraction for memory allocation and deallocation, ensuring that developers do not need to manually free memory. However, understanding and cooperating with the GC can help improve your application's performance and memory usage. Here are key aspects of memory management in .NET:

- **Garbage Collection:** Automatically reclaims memory occupied by unreachable objects, freeing developers from manually deallocating memory and helping to avoid memory leaks.

- **Dispose Pattern:** Implementing the `IDisposable` interface and the `Dispose` method allows for the cleanup of unmanaged resources (such as file handles, database connections, etc.) that the GC cannot reclaim automatically.

- **Finalizers:** Can be defined in classes to perform cleanup operations before the object is collected by the GC. However, overuse of finalizers can negatively impact performance, as it makes objects live longer than necessary.

- **Using Statements:** A syntactic sugar for calling `Dispose` on IDisposable objects, ensuring that resources are freed as soon as they are no longer needed, even if exceptions are thrown.

- **Large Object Heap (LOH) Management:** Large objects are allocated on a separate heap, and knowing how to manage large object allocations can help reduce memory fragmentation and improve performance.

Here is an example demonstrating the use of the `IDisposable` interface and `using` statement for resource management:

```csharp
public class ResourceHolder : IDisposable
{
    private bool disposed = false;

    // Simulate an unmanaged resource.
    IntPtr unmanagedResource = Marshal.AllocHGlobal(100);

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {
                // Dispose managed resources.
            }

            // Free unmanaged resources
            Marshal.FreeHGlobal(unmanagedResource);
            disposed = true;
        }
    }

    ~ResourceHolder()
    {
        Dispose(false);
    }
}

class Program
{
    static void Main(string[] args)
    {
        using (ResourceHolder holder = new ResourceHolder())
        {
            // Use the resource
        } // Automatic disposal here
    }
}
```

In this example, ResourceHolder implements IDisposable to properly manage both managed and unmanaged resources. The using statement ensures that Dispose is called automatically, providing a robust pattern for resource management in .NET applications.

### 16. Explain the concept of threading in .NET.

**Answer:** Threading in .NET allows for the execution of multiple operations simultaneously within the same process. It enables applications to perform background tasks, UI responsiveness, and parallel computations, improving overall application performance and efficiency. The .NET framework provides several ways to create and manage threads:

- **System.Threading.Thread:** A low-level approach to create and manage threads directly. This class offers fine-grained control over thread behavior.

- **ThreadPool:** A collection of worker threads maintained by the .NET runtime, offering efficient execution of short-lived background tasks without the overhead of creating individual threads for each task.

- **Task Parallel Library (TPL):** Provides a higher-level abstraction over threading, using tasks that represent asynchronous operations. TPL uses the ThreadPool internally and supports features like task chaining, cancellation, and continuation.

- **async and await:** Keywords that simplify asynchronous programming, making it easier to write asynchronous code that's as straightforward as synchronous code.

Here is an example demonstrating the use of the `System.Threading.Thread` class:

```csharp
using System;
using System.Threading;

class Program
{
    static void Main(string[] args)
    {
        Thread thread = new Thread(new ThreadStart(DoWork));
        thread.Start();

        Console.WriteLine("Main thread does some work, then waits.");
        thread.Join();
        Console.WriteLine("Background thread has completed. Main thread ends.");
    }

    static void DoWork()
    {
        Console.WriteLine("Background thread is working.");
        Thread.Sleep(1000); // Simulates doing work
        Console.WriteLine("Background thread has finished.");
    }
}
```

In this example, a new thread is created and started to execute the DoWork method in parallel to the main thread. The main thread then waits for the background thread to complete using the Join method before it proceeds to finish. This demonstrates the basic use of threading to perform operations concurrently.

Using threads can significantly improve the responsiveness and performance of your application but also introduces complexity, such as the need for thread synchronization to avoid race conditions and deadlocks. Proper understanding and careful management are essential when working with threads in .NET.

### 17. What is async/await and how does it work?

**Answer:** In C#, `async` and `await` are keywords that simplify writing asynchronous code, making it more readable and maintainable. This feature allows developers to perform non-blocking operations without the complex code traditionally associated with asynchronous programming, such as callbacks or manual thread management. The `async` modifier indicates that a method is asynchronous and may contain one or more `await` expressions. The `await` keyword is applied to a task, indicating that the method should pause until the awaited task completes, allowing other operations to run concurrently without blocking the main thread.

Key points about async/await:
- Improves application responsiveness, particularly important for UI applications where long-running operations can freeze the user interface.
- Enables server applications to handle more concurrent requests by freeing up threads while waiting for operations to complete.
- Simplifies error handling in asynchronous code with try-catch blocks.

Here's a simple example demonstrating async/await:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        string result = await DownloadContentAsync();
        Console.WriteLine(result);
    }

    static async Task<string> DownloadContentAsync()
    {
        using HttpClient client = new HttpClient();
        string result = await client.GetStringAsync("http://example.com");
        return result;
    }
}
```

In this example, DownloadContentAsync is an asynchronous method that downloads content from a web address. It uses await to asynchronously wait for the HTTP request to complete without blocking the main thread. The Main method is also marked with async and awaits the completion of DownloadContentAsync. This approach keeps the application responsive, as the thread that started the operation can perform other work while waiting for the HTTP request to complete.

Async/await simplifies asynchronous programming by allowing developers to write code that's both easy to read and maintain, resembling synchronous code while providing the benefits of asynchronous execution.

### 18. Describe the Entity Framework and its advantages.

**Answer:** Entity Framework (EF) is an open-source object-relational mapping (ORM) framework for .NET. It enables developers to work with databases using .NET objects, eliminating the need for most of the data-access code that developers usually need to write. Entity Framework provides a high-level abstraction over database connections and operations, allowing developers to perform CRUD (Create, Read, Update, Delete) operations without having to deal with the underlying database SQL commands directly.

Advantages of using Entity Framework include:
- **Increased Productivity:** Automatically generates classes based on database schemas, reducing the amount of manual coding required for data access.
- **Maintainability:** Changes to the database schema can be easily propagated to the code through migrations, helping maintain code and database schema synchronicity.
- **Support for LINQ:** Enables developers to use LINQ queries to access and manipulate data, providing a type-safe way to query databases that integrates with the C# language.
- **Database Agnostic:** EF can work with various databases, including SQL Server, MySQL, Oracle, and more, by changing the database provider with minimal changes to the code.
- **Caching, Lazy Loading, Eager Loading:** Offers built-in support for caching, and configurable loading options like lazy loading and eager loading, improving application performance.

Here is a simple example demonstrating the use of Entity Framework to query a database:

```csharp
using System;
using System.Linq;
using System.Data.Entity;

public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();
            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }
        }
    }
}
```

In this example, BloggingContext is the database context that manages the database connection and provides DbSet properties for querying and saving instances of the Blog class. The example demonstrates creating a new Blog instance, saving it to the database, and then querying and displaying all blogs. Entity Framework handles all the database interactions, allowing the developer to work at a higher level of abstraction.

Entity Framework significantly simplifies data access in .NET applications, making it an essential tool for rapid development while ensuring applications are maintainable and scalable.

### 19. What are extension methods and where would you use them?

**Answer:** Extension methods in C# allow developers to add new methods to existing types without modifying, deriving from, or recompiling the original types. They are static methods defined in a static class, but called as if they were instance methods on the extended type. Extension methods provide a flexible way to extend the functionality of a class or interface.

To use extension methods, the static class containing the extension method must be in scope, which usually requires a `using` directive for the namespace of the class.

Advantages of using extension methods include:
- Enhancing the functionality of third-party libraries or built-in .NET types without access to the original source code.
- Keeping code cleaner and more readable by encapsulating complex operations into methods.
- Facilitating a fluent interface style of coding, which can make code more expressive.

Here is a simple example demonstrating how to define and use an extension method:

```csharp
using System;

namespace ExtensionMethods
{
    public static class StringExtensions
    {
        // Extension method for the String class
        public static string ToPascalCase(this string input)
        {
            if (string.IsNullOrEmpty(input))
                return input;

            string[] words = input.Split(new char[] { ' ', '-' }, StringSplitOptions.RemoveEmptyEntries);
            for (int i = 0; i < words.Length; i++)
            {
                words[i] = char.ToUpper(words[i][0]) + words[i].Substring(1).ToLower();
            }
            return string.Join("", words);
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        string title = "the quick-brown fox";
        string pascalCaseTitle = title.ToPascalCase();
        Console.WriteLine(pascalCaseTitle); // Outputs: TheQuickBrownFox
    }
}
```

In this example, ToPascalCase is an extension method defined for the String class. It converts a given string to Pascal case format. To use this extension method, simply call it on a string instance, as shown in the Main method. This demonstrates how extension methods can add useful functionality to existing types in a clean and natural syntax.

Extension methods are a powerful feature for extending the capabilities of types, especially when direct modifications to the class are not possible or desirable.

### 20. How do you handle exceptions in a method that returns a Task?

**Answer:** In asynchronous programming with C#, when a method returns a `Task` or `Task<T>`, exceptions should be handled within the task to avoid unhandled exceptions that can crash the application. Exceptions thrown in a task are captured and placed on the returned task object. To handle these exceptions, you can use a try-catch block within the asynchronous method, or you can inspect the task after it has completed for any exceptions.

There are several approaches to handle exceptions in tasks:

1. **Inside the Asynchronous Method:**
   Use a try-catch block inside the async method to catch exceptions directly.

```csharp
public async Task PerformOperationAsync()
{
    try
    {
        // Async operation that may throw an exception
    }
    catch (Exception ex)
    {
        // Handle exception
    }
}
```

2. When Awaiting the Task:
Await the task inside a try-catch block to catch exceptions when the task is awaited.

```csharp

try
{
    await PerformOperationAsync();
}
catch (Exception ex)
{
    // Handle exception
}
```

3. Using Task.ContinueWith:
Use the ContinueWith method to attach a continuation task that can handle exceptions.

```csharp
PerformOperationAsync().ContinueWith(task =>
{
    if (task.Exception != null)
    {
        // Handle exception
        var exception = task.Exception.InnerException;
    }
}, TaskContinuationOptions.OnlyOnFaulted);
```

4. Using Task.WhenAny:
Useful for handling exceptions from multiple tasks.

```csharp

var task = PerformOperationAsync();
await Task.WhenAny(task); // Wait for task to complete

if (task.IsFaulted)
{
    // Handle exception
    var exception = task.Exception.InnerException;
}

```

Here is an example demonstrating handling exceptions for a method that returns a Task:

```csharp

public async Task<int> DivideAsync(int numerator, int denominator)
{
    return await Task.Run(() =>
    {
        if (denominator == 0)
            throw new DivideByZeroException("Denominator cannot be zero.");

        return numerator / denominator;
    });
}

public async Task ExecuteAsync()
{
    try
    {
        int result = await DivideAsync(10, 0);
        Console.WriteLine($"Result: {result}");
    }
    catch (DivideByZeroException ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }
}
```

In this example, DivideAsync performs a division operation asynchronously and may throw a DivideByZeroException. The exception is handled in the ExecuteAsync method, demonstrating how to properly handle exceptions for tasks in asynchronous methods.

Handling exceptions in tasks is crucial for writing robust and error-resistant asynchronous C# applications, ensuring that your application can gracefully recover from errors encountered during asynchronous operations.
