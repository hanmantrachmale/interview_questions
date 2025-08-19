# .NET (C#) Interview Questions and Answers

## Advanced

1. **What is reflection in .NET and how would you use it?**
2. **Can you explain the concept of middleware in ASP.NET Core?**
3. **Describe the Dependency Injection (DI) pattern and how it's implemented in .NET Core.**
4. **What is the purpose of the .NET Standard?**
5. **Explain the differences between .NET Core, .NET Framework, and Xamarin.**
6. **How does garbage collection work in .NET and how can you optimize it?**
7. **What are attributes in C# and how can they be used?**
8. **Can you describe the process of code compilation in .NET?**
9. **What is the Global Assembly Cache (GAC) and when should it be used?**
10. **How would you secure a web application in ASP.NET Core?**

### 1. What is reflection in .NET and how would you use it?

**Answer:** Reflection in .NET is a powerful feature that allows runtime inspection of assemblies, types, and their members (such as methods, fields, properties, and events). It enables creating instances of types, invoking methods, and accessing fields and properties dynamically, without knowing the types at compile time. Reflection is used for various purposes, including building type browsers, dynamically invoking methods, and reading custom attributes.

Reflection can be particularly useful in scenarios such as:
- Dynamically loading and using assemblies.
- Implementing object browsers or debuggers.
- Creating instances of types for dependency injection frameworks.
- Accessing and manipulating metadata for assemblies and types.

Here's a simple example demonstrating how to use reflection to inspect and invoke methods of a class dynamically:

```csharp
using System;
using System.Reflection;

public class MyClass
{
    public void MethodToInvoke()
    {
        Console.WriteLine("Method Invoked.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Obtaining the Type object for MyClass
        Type myClassType = typeof(MyClass);
        
        // Creating an instance of MyClass
        object myClassInstance = Activator.CreateInstance(myClassType);
        
        // Getting the MethodInfo object for MethodToInvoke
        MethodInfo methodInfo = myClassType.GetMethod("MethodToInvoke");
        
        // Invoking the method on the instance
        methodInfo.Invoke(myClassInstance, null);
    }
}
```

In this example, reflection is used to obtain the Type object for MyClass, create an instance of MyClass, and then retrieve and invoke the MethodToInvoke method. This demonstrates how reflection allows for dynamic type inspection and invocation, providing flexibility and power in how code interacts with objects.

Using reflection comes with a performance cost, so it should be used judiciously, especially in performance-critical paths of an application.

### 2. Can you explain the concept of middleware in ASP.NET Core?

**Answer:** Middleware in ASP.NET Core is software that's assembled into an application pipeline to handle requests and responses. Each component in the middleware pipeline is responsible for invoking the next component in the sequence or short-circuiting the chain if necessary. Middleware components can perform a variety of tasks, such as authentication, routing, session management, and logging.

Middleware enables you to customize the request pipeline in ways that are most suited to your application's needs. They are executed in the order they are added to the pipeline, allowing for precise control over how requests are processed and how responses are constructed.

Here's a simple example demonstrating how to create and use middleware in ASP.NET Core:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        // Do something with the context before the next middleware
        Console.WriteLine("Before next middleware");

        await _next(context); // Call the next middleware in the pipeline

        // Do something with the context after the next middleware
        Console.WriteLine("After next middleware");
    }
}

// Extension method used to add the middleware to the application's request pipeline
public static class CustomMiddlewareExtensions
{
    public static IApplicationBuilder UseCustomMiddleware(this IApplicationBuilder builder)
    {
        return builder.UseMiddleware<CustomMiddleware>();
    }
}

public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.UseCustomMiddleware();
        // Other middleware registrations
    }
}
```

In this example, CustomMiddleware is defined with an InvokeAsync method that ASP.NET Core calls to process the HTTP request. The UseCustomMiddleware extension method adds the middleware to the application's request pipeline, and it's registered in the Configure method of the Startup class.

Middleware components in ASP.NET Core provide a powerful way to compose your application's request-handling pipeline, allowing for modular and reusable components that can encapsulate request-processing logic.

### 3. Describe the Dependency Injection (DI) pattern and how it's implemented in .NET Core.

**Answer:** Dependency Injection (DI) is a design pattern that facilitates loose coupling between software components by removing the direct dependencies among them. Instead of instantiating dependencies directly, components receive their dependencies from an external source (often an inversion of control container). DI makes your code more modular, easier to test, maintain, and extend.

.NET Core has built-in support for dependency injection, which allows services to be registered and resolved through an IoC (Inversion of Control) container. The container manages object creation and injects dependencies where required. This mechanism is central to ASP.NET Core applications, enabling features like middleware, controllers, views, and other services to be provided and managed by the framework.

The core concepts in .NET Core's DI system include:
- **Service registration:** Services are registered with the DI container, typically in the `Startup.ConfigureServices` method, specifying their lifetime (singleton, scoped, or transient).
- **Service resolution:** Services are resolved either through constructor injection, method call injection, or property injection, with constructor injection being the most common approach.

Here's a simple example demonstrating how DI is used in an ASP.NET Core application:

```csharp
public interface IGreetingService
{
    string Greet(string name);
}

public class GreetingService : IGreetingService
{
    public string Greet(string name)
    {
        return $"Hello, {name}!";
    }
}

public class HomeController : Controller
{
    private readonly IGreetingService _greetingService;

    public HomeController(IGreetingService greetingService)
    {
        _greetingService = greetingService;
    }

    public IActionResult Index()
    {
        var greeting = _greetingService.Greet("World");
        return Content(greeting);
    }
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllersWithViews();
        services.AddTransient<IGreetingService, GreetingService>();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        // Configure the request pipeline.
        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapDefaultControllerRoute();
        });
    }
}
```

In this example, IGreetingService is an interface defining a service contract, and GreetingService is its implementation. The service is registered with the DI container in the Startup.ConfigureServices method using services.AddTransient<IGreetingService, GreetingService>();. The HomeController receives an instance of IGreetingService through constructor injection, provided by the DI container. This demonstrates how DI facilitates loose coupling and enhances the testability and maintainability of the application.

Dependency Injection in .NET Core is a foundational feature that supports the development of decoupled and easily testable applications.

### 4. What is the purpose of the .NET Standard?

**Answer:** The .NET Standard is a formal specification of .NET APIs that are intended to be available on all .NET implementations. The goal of the .NET Standard is to establish greater uniformity in the .NET ecosystem. It enables developers to create libraries that are compatible across different .NET platforms, such as .NET Core, .NET Framework, Xamarin, and others, with a single codebase. This simplifies the development process and enhances code reuse across projects and platforms.

The .NET Standard is versioned, with each version building upon the previous one and adding more APIs. Higher versions of the .NET Standard support newer APIs but reduce the number of platforms that support that standard because older platforms may not implement the newer APIs.

Here's how the .NET Standard serves different roles in the .NET ecosystem:
- For library developers: It provides a base set of APIs that are guaranteed to be available on all .NET platforms, enabling the creation of portable libraries that can run on any .NET implementation.
- For application developers: It assures that any library targeting a version of the .NET Standard can be used in their application, regardless of the .NET platform it runs on.
- For .NET implementations: It establishes a baseline of APIs that must be implemented, ensuring compatibility and unification across different .NET platforms.

An example of targeting the .NET Standard in a class library project file (`csproj`):

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
  </PropertyGroup>

</Project>
```

In this example, the class library targets .NET Standard 2.0, meaning it can run on any .NET platform that supports .NET Standard 2.0 or higher. This includes .NET Core 2.0+, .NET Framework 4.6.1+, Xamarin, and others, making the library highly reusable across a wide range of applications and platforms.

The .NET Standard facilitates the development of portable libraries and helps unify the .NET ecosystem, making it easier for developers to share and reuse code across different .NET platforms.

### 5. Explain the differences between .NET Core, .NET Framework, and Xamarin.

**Answer:** .NET Core, .NET Framework, and Xamarin are all part of the .NET ecosystem, but they serve different purposes and are used in different types of projects. Understanding the differences between them can help you choose the right technology for your specific needs.

- **.NET Framework:**
  - The original .NET implementation, launched in 2002.
  - Primarily runs on Windows and is used for developing Windows desktop applications and ASP.NET web applications.
  - Provides a vast library of pre-built functionality, including Windows Forms, WPF (Windows Presentation Foundation) for desktop applications, and ASP.NET for web applications.
  - Moving forward, it will receive only critical security updates and bug fixes.

- **.NET Core:**
  - A cross-platform, open-source reimplementation of .NET that runs on Windows, macOS, and Linux.
  - Designed to be modular and lightweight, making it suitable for web applications, microservices, and cloud applications.
  - Supports development of console applications, ASP.NET Core web applications, and libraries.
  - With the release of .NET 5 and beyond, .NET Core has evolved into simply ".NET," unifying the .NET platform and serving as the future of the .NET ecosystem.

- **Xamarin:**
  - A .NET platform for building mobile applications that can run on iOS, Android, and Windows devices.
  - Allows developers to write mobile applications using C# and .NET libraries while providing native performance and look-and-feel on each platform.
  - Integrates with Visual Studio, providing a rich development environment.
  - Xamarin.Forms allows for the creation of UIs from a single, shared codebase, while Xamarin.iOS and Xamarin.Android provide access to platform-specific APIs.

Here's a simple comparison:

| Feature/Platform | .NET Framework | .NET Core           | Xamarin             |
|------------------|----------------|---------------------|---------------------|
| Platform         | Windows        | Cross-platform      | Mobile (iOS, Android, Windows) |
| Use Cases        | Desktop, Web   | Web, Microservices, Console | Mobile apps          |
| Development      | Visual Studio  | Visual Studio, VS Code, Others | Visual Studio, Visual Studio for Mac |
| Open Source      | No             | Yes                 | Yes                 |

.NET 5 and onwards (rebranded from .NET Core) aim to unify these platforms under a single .NET runtime and framework that can be used everywhere and that supports all types of application development.

### 6. How does garbage collection work in .NET and how can you optimize it?

**Answer:** Garbage Collection (GC) in .NET is an automatic memory management feature that helps in reclaiming the memory used by objects that are no longer accessible in the application. It eliminates the need for manual memory management, reducing the risks of memory leaks and other memory-related issues.

**How Garbage Collection Works:**
- **Mark:** The GC traverses the object graph to mark objects that are reachable, starting from root references. Reachable objects are considered "live".
- **Compact:** To reclaim the memory occupied by unreachable objects, the GC compacts the remaining objects, thus reducing the space used on the managed heap and making room for new objects.
- **Generations:** The GC uses a generational approach to manage memory collections, dividing the heap into three generations (0, 1, and 2) for more efficient garbage collection. Generation 0 is for short-lived objects, Generation 1 for medium-lived objects, and Generation 2 for long-lived objects. Collecting younger generations more frequently reduces the need to perform expensive collections on older generations.

**Optimizing Garbage Collection:**
1. **Minimize Allocations:** Be mindful of unnecessary allocations, especially in performance-critical paths. Reuse objects when possible, and consider using object pools for frequently created and destroyed objects.
2. **Understand Generations:** Knowing how generations work can help you write code that interacts more efficiently with the GC. For example, large objects are placed directly into Generation 2, so their allocation and deallocation can be expensive.
3. **Use Structs Judiciously:** Structs are value types and are allocated on the stack (when declared outside of a class). When used appropriately, they can reduce heap allocations. However, excessively large structs or inappropriate use can lead to performance issues.
4. **Implement IDisposable:** For classes that use unmanaged resources (like file handles or database connections), implement the `IDisposable` interface and dispose of those resources when they are no longer needed to free up resources promptly.
5. **Monitor and Analyze:** Use profiling tools to monitor your application's memory usage and GC behavior. Tools like Visual Studio Diagnostic Tools, dotMemory, and the GC API (`System.GC`) can provide insights into how your application interacts with the garbage collector.

Here's an example of implementing `IDisposable`:

```csharp
public class ResourceWrapper : IDisposable
{
    private bool disposed = false;

    // Assume _resource is an unmanaged resource.
    private IntPtr _resource;

    public ResourceWrapper()
    {
        _resource = // Allocate the resource
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
            if (_resource != IntPtr.Zero)
            {
                // Free the resource
                _resource = IntPtr.Zero;
            }

            disposed = true;
        }
    }

    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }

    ~ResourceWrapper()
    {
        Dispose(false);
    }
}
```

By understanding and optimizing the .NET garbage collector, developers can improve their applications' performance and reliability.


### 7. What are attributes in C# and how can they be used?

**Answer:** Attributes in C# are a powerful way to add declarative information to your code. They are used to add metadata, such as compiler instructions, annotations, or custom information, to program elements (classes, methods, properties, etc.). Attributes can influence the behavior of certain components at runtime or compile time, and they can be queried through reflection.

**Common Uses of Attributes:**
- Marking methods as test methods in a unit testing framework (e.g., `[TestMethod]` in MSTest).
- Specifying serialization rules (e.g., `[Serializable]`, `[DataMember]`).
- Controlling binding and model validation in ASP.NET Core (e.g., `[Required]`, `[Bind]`).
- Defining aspects of web service behaviors (e.g., `[WebMethod]`).
- Custom attributes for domain-specific purposes.

**Example of Using an Attribute:**

A common use case is data validation in ASP.NET Core models. The `[Required]` attribute indicates that a property must have a value; if a model is passed to a controller method without this property set, model validation fails.

```csharp
using System.ComponentModel.DataAnnotations;

public class Person
{
    [Required]
    public string Name { get; set; }

    [Range(1, 120)]
    public int Age { get; set; }
}
```

**Creating Custom Attributes:**

You can also define custom attributes for specific needs. Here's a simple example:

```csharp

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false)]
public class MyCustomAttribute : Attribute
{
    public string Description { get; set; }

    public MyCustomAttribute(string description)
    {
        Description = description;
    }
}

[MyCustomAttribute("This is a class description.")]
public class MyClass
{
    [MyCustomAttribute("This is a method description.")]
    public void MyMethod() { }
}
```

In this example, MyCustomAttribute is defined with a property Description. It can be attached to classes or methods, providing custom descriptive metadata. The AttributeUsage attribute specifies where this attribute can be applied (Class or Method) and whether it can be used multiple times on the same element (AllowMultiple).

Attributes extend the capabilities of C# by allowing you to define additional information about the behavior and structure of your code in a declarative manner. They are a key part of many .NET frameworks and libraries, enabling various runtime behaviors and compile-time checks.

### 8. Can you describe the process of code compilation in .NET?

**Answer:** The process of code compilation in .NET involves converting high-level code (such as C#) into a platform-independent Intermediate Language (IL) and then into platform-specific machine code. This process is facilitated by the .NET Compiler Platform ("Roslyn" for C# and Visual Basic) and the Common Language Runtime (CLR). Here's an overview of the steps involved:

1. **Source Code to Intermediate Language (IL):**
   - When you compile a .NET application, the .NET compiler for your language (e.g., csc.exe for C#) compiles the source code into Intermediate Language (IL). IL is a CPU-independent set of instructions that can be efficiently converted into native machine code.
   - Along with IL, metadata is generated, which includes information about the types, members, references, and other data that the IL code uses.

2. **IL to Native Code:**
   - At runtime, the .NET application is executed by the CLR. The CLR's Just-In-Time (JIT) compiler converts the IL code into native machine code specific to the architecture of the target machine.
   - This conversion happens on a need-to-run basis; that is, each method's IL is converted to native code the first time the method is called. The native code is then cached, so subsequent calls to the same method do not require re-compilation.

3. **Execution:**
   - The native code is executed directly by the hardware of the target machine, leading to the execution of the .NET application.

**Aspects of .NET Compilation:**

- **Assembly:** The compiled code and resources are packed into assemblies (`.dll` or `.exe` files), which are the units of deployment and versioning in .NET. Assemblies contain the IL code and metadata.
- **Metadata:** Metadata describes the assembly itself and the types defined within the assembly, including their methods and properties. This information is used by the CLR during execution and supports features like reflection.
- **Strong Naming and GAC:** Assemblies can be strong-named to ensure their uniqueness and integrity. Strong-named assemblies can be placed in the Global Assembly Cache (GAC) for shared use by multiple applications.

**Optimizations and NGEN:**

- The JIT compiler applies various optimizations while converting IL to native code to improve runtime performance. Additionally, developers can use the Native Image Generator (NGEN) to pre-compile assemblies into native images at install time, reducing JIT compilation time at runtime but at the cost of losing some JIT optimizations specific to the end-user's machine.

```csharp
// Example C# code
public class Program
{
    public static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

This simple program is compiled to IL when built and then JIT-compiled to native code by the CLR when executed.
Understanding the compilation process in .NET is crucial for grasping how .NET applications are built, deployed, and executed across different environments and platforms.

### 9. What is the Global Assembly Cache (GAC) and when should it be used?

**Answer:** The Global Assembly Cache (GAC) is a machine-wide code cache for the Common Language Runtime (CLR) in the .NET Framework. It stores assemblies specifically designated to be shared by several applications on the computer. The GAC is used to store shared .NET assemblies that have strong names (which are unique identifiers consisting of a name, version number, culture information, and a public key token).

**Key Points about the GAC:**

- **Sharing Assemblies:** The GAC allows multiple applications to share the same library, reducing memory usage and ensuring consistency across applications that use common components.
- **Strong Naming:** Only assemblies with strong names (signed with a public/private key pair) can be added to the GAC. Strong naming guarantees the uniqueness of the assembly's identity, allowing side-by-side hosting of different versions.
- **Versioning:** The GAC supports side-by-side execution of different versions of the same assembly. This enables applications to specify and use the version of an assembly they were built with, even if newer versions are installed on the system.

**When to Use the GAC:**

- **Shared Libraries:** Use the GAC for assemblies that need to be shared by multiple applications on the same machine. Common libraries or frameworks that are stable and versioned are good candidates.
- **Side-by-Side Hosting:** When different versions of the same assembly need to be used by different applications on the same system, deploying these assemblies to the GAC can manage versioning complexities.
- **Security:** Assemblies in the GAC are generally more secure, as only users with administrative privileges can add or remove assemblies. This control can prevent malicious code from being inadvertently added to the cache.

**Example of Adding an Assembly to the GAC:**

To add an assembly to the GAC, you can use the `gacutil` tool provided with the .NET Framework SDK:

```bash
gacutil -i MyAssembly.dll
```

This command installs MyAssembly.dll into the GAC.

Note: With the introduction of .NET Core and its focus on application-local deployment models, the GAC is less emphasized and is specific to the .NET Framework. .NET Core and .NET 5+ applications typically rely on package management systems like NuGet to handle dependencies and do not use the GAC.

The GAC plays a critical role in assembly sharing and versioning in the .NET Framework, facilitating the management of common libraries across applications on a single machine.

## 10. How would you secure a web application in ASP.NET Core?
Securing an ASP.NET Core web application involves multiple strategies, including authentication, authorization, data protection, and HTTPS enforcement.

### Example: Enforcing HTTPS in ASP.NET Core
```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseHttpsRedirection(); // Redirect HTTP requests to HTTPS
    app.UseAuthentication();   // Enable authentication middleware
    app.UseAuthorization();    // Enable authorization middleware
}
```
---