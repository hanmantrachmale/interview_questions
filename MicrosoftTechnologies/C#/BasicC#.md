# .NET (C#) Interview Questions and Answers

## Basic C# Questions

1. **What is .NET?**
2. **Can you explain the Common Language Runtime (CLR)?**
3. **What is the difference between managed and unmanaged code?**
4. **Explain the basic structure of a C# program.**
5. **What are Value Types and Reference Types in C#?**
6. **What is garbage collection in .NET?**
7. **Explain the concept of exception handling in C#.**
8. **What are the different types of classes in C#?**
9. **Can you describe what a namespace is and how it is used in C#?**
10. **What is encapsulation?**

## Basic

### 1. What is .NET?

**Answer:** .NET is a comprehensive development platform used for building a wide variety of applications, including web, mobile, desktop, and gaming. It supports multiple programming languages, such as C#, F#, and Visual Basic. .NET provides a large class library called Framework Class Library (FCL) and runs on a Common Language Runtime (CLR) which offers services like memory management, security, and exception handling.

### 2. Can you explain the Common Language Runtime (CLR)?

**Answer:** The CLR is a virtual machine component of the .NET framework that manages the execution of .NET programs. It provides important services such as memory management, type safety, exception handling, garbage collection, and thread management. The CLR converts Intermediate Language (IL) code into native machine code through a process called Just-In-Time (JIT) compilation. This ensures that .NET applications can run on any device or platform that supports the .NET framework.

### 3. What is the difference between managed and unmanaged code?

**Answer:** Managed code is executed by the CLR, which provides services like garbage collection, exception handling, and type checking. It's called "managed" because the CLR manages a lot of the functionalities that developers would otherwise need to implement themselves. Unmanaged code, on the other hand, is executed directly by the operating system, and all memory allocation, type safety, and security must be handled by the programmer. Examples of unmanaged code include applications written in C or C++.

### 4. Explain the basic structure of a C# program.

**Answer:** A basic C# program consists of the following elements:

- **Namespace declaration:** A way to organize code and control the scope of classes and methods in larger projects.
- **Class declaration:** Defines a new type with data members (fields, properties) and function members (methods, constructors).
- **Main method:** The entry point for the program where execution begins and ends.
- **Statements and expressions:** Perform actions with variables, calling methods, looping through collections, etc.

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

### 5. What are Value Types and Reference Types in C#?

**Answer:** In C#, data types are divided into two categories: Value Types and Reference Types. This distinction affects how values are stored and manipulated within memory.

- **Value Types:** Store data directly and are allocated on the stack. This means that when you assign one value type to another, a direct copy of the value is created. Basic data types (`int`, `double`, `bool`, etc.) and structs are examples of value types. Operations on value types are generally faster due to stack allocation.

- **Reference Types:** Store a reference (or pointer) to the actual data, which is allocated on the heap. When you assign one reference type to another, both refer to the same object in memory; changes made through one reference are reflected in the other. Classes, arrays, delegates, and strings are examples of reference types.

Here's a simple example to illustrate the difference:

```csharp
// Value type example
int a = 10;
int b = a;
b = 20;
Console.WriteLine(a); // Output: 10
Console.WriteLine(b); // Output: 20

// Reference type example
var list1 = new List<int> { 1, 2, 3 };
var list2 = list1;
list2.Add(4);
Console.WriteLine(list1.Count); // Output: 4
Console.WriteLine(list2.Count); // Output: 4
```

In the value type example, changing b does not affect a because b is a separate copy. In the reference type example, list2 is not a separate copy; it's another reference to the same list object as list1, so changes made through list2 are visible when accessing list1.

### 6. What is garbage collection in .NET?

**Answer:** Garbage collection (GC) in .NET is an automatic memory management feature that frees up memory used by objects that are no longer accessible in the program. It eliminates the need for developers to manually release memory, thereby reducing memory leaks and other memory-related errors. The GC operates on a separate thread and works in three phases: marking, relocating, and compacting. During the marking phase, it identifies which objects in the heap are still in use. During the relocating phase, it updates the references to objects that will be compacted. Finally, during the compacting phase, it reclaims the space occupied by the garbage objects and compacts the remaining objects to make memory allocation more efficient.

### 7. Explain the concept of exception handling in C#.

**Answer:** Exception handling in C# is a mechanism to handle runtime errors, allowing a program to continue running or fail gracefully instead of crashing. It is done using the try, catch, and finally blocks. The try block contains code that might throw an exception, while catch blocks are used to handle the exception. The finally block contains code that is executed whether an exception is thrown or not, often for cleanup purposes.

```csharp
try {
    // Code that may cause an exception
    int divide = 10 / 0;
}
catch (DivideByZeroException ex) {
    // Code to handle the exception
    Console.WriteLine("Cannot divide by zero. Please try again.");
}
finally {
    // Code that executes after try/catch, regardless of an exception
    Console.WriteLine("Operation completed.");
}
```

### 8. What are the different types of classes in C#?

**Answer:** In C#, classes can be categorized based on their functionality and accessibility:

- **Static classes:** Cannot be instantiated and can only contain static members.
- **Sealed classes:** Cannot be inherited from.
- **Abstract classes:** Cannot be instantiated and are meant to be inherited from.
- **Partial classes:** Allow the splitting of a class definition across multiple files.
- **Generic classes:** Allow the definition of classes with placeholders for the type of its fields, methods, parameters, etc.

Each type serves different purposes in the context of object-oriented programming and design patterns.

### 9. Can you describe what a namespace is and how it is used in C#?

**Answer:** A namespace in C# is used to organize code into a hierarchical structure. It allows the grouping of logically related classes, structs, interfaces, enums, and delegates. Namespaces help avoid naming conflicts by qualifying the uniqueness of each type. For example, the `System` namespace in .NET includes classes for basic system operations, such as console input/output, file reading/writing, and data manipulation.

```csharp
using System;

namespace MyApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }
    }
}
```

In this example, the System namespace is used to access the Console class, and MyApplication is a custom namespace for organizing the application's code. Namespaces are essential for managing the scope of names in larger programming projects to avoid name collisions.

### 10. What is encapsulation?

**Answer:** Encapsulation is a fundamental principle of object-oriented programming (OOP) that involves bundling the data (attributes) and methods (operations) that operate on the data into a single unit, or class, and restricting access to the internals of that class. This is typically achieved through the use of access modifiers such as `private`, `public`, `protected`, and `internal`. Encapsulation helps to protect an object's internal state from unauthorized access and modification by external code, promoting data integrity and security.

Encapsulation allows the internal representation of an object to be hidden from the outside, only allowing access through a public interface. This concept is also known as data hiding. By controlling how data is accessed and modified, encapsulation helps to reduce complexity and increase reusability of code.

Here is a simple example demonstrating encapsulation in C#:

```csharp
public class Person
{
    private string name; // Private field, encapsulated data

    public string Name // Public property, access to the name field
    {
        get { return name; }
        set { name = value; }
    }

    public Person(string name) // Constructor
    {
        this.name = name;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Person person = new Person("John");
        Console.WriteLine(person.Name); // Accessing name through a public property
    }
}
```

In this example, the name field of the Person class is encapsulated and only accessible via the Name property. This approach allows the Person class to control how the name field is accessed and modified, ensuring that any rules or validations about the data can be applied within the class itself.

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

