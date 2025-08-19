# .NET (MVC) Interview Questions and Answers

1. **What is MVC (Model-View-Controller)?**
2. **Can you explain the difference between Razor Pages and MVC in ASP.NET Core?**
3. **How do you perform validations in ASP.NET Core?**
4. **Describe SignalR and its use cases.**
5. **What are the benefits of using Blazor over traditional web technologies?**
6. **How do you implement Web API versioning in ASP.NET Core?**
7. **Explain the role of IApplicationBuilder in ASP.NET Core.**
8. **What are Areas in ASP.NET Core and how do you use them?**
9. **How do you manage sessions in ASP.NET Core applications?**
10. **Describe how to implement caching in ASP.NET Core.**


## 1. What is MVC (Model-View-Controller)?
MVC is a software design pattern that separates an application into three components:

- **Model**: Represents the data and business logic.

- **View**: Handles the presentation layer.

- **Controller**: Manages user input and updates the model.

### Example: A Simple MVC Controller in ASP.NET Core
```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }
}
```
---

## 2. Can you explain the difference between Razor Pages and MVC in ASP.NET Core?
- **MVC** uses a Controller to handle requests, while **Razor Pages** has built-in page handlers (`OnGet`, `OnPost`).
- Razor Pages is more suitable for simple, page-focused applications, whereas MVC is better for larger applications.

### Example: Razor Page Handler
```csharp
public class IndexModel : PageModel
{
    public void OnGet()
    {
        // Handle GET request
    }
}
```
---

## 3. How do you perform validations in ASP.NET Core?
ASP.NET Core provides validation using **Data Annotations** and **Fluent Validation**.

### Example: Using Data Annotations
```csharp
public class UserModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [MinLength(6)]
    public string Password { get; set; }
}
```
---

## 4. Describe SignalR and its use cases.
SignalR is a real-time communication library in ASP.NET Core that enables WebSockets for interactive web applications.

### Example: SignalR Hub
```csharp
public class ChatHub : Hub
{
    public async Task SendMessage(string user, string message)
    {
        await Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```
---

## 5. What are the benefits of using Blazor over traditional web technologies?
Blazor allows building web applications using **C# and .NET** instead of JavaScript.

### Example: Blazor Component
```csharp
@code {
    string message = "Hello, Blazor!";
}
<p>@message</p>
```
---

## 6. How do you implement Web API versioning in ASP.NET Core?
API versioning ensures backward compatibility for REST APIs.

### Example: Using API Versioning Middleware
```csharp
services.AddApiVersioning(o =>
{
    o.ReportApiVersions = true;
    o.AssumeDefaultVersionWhenUnspecified = true;
    o.DefaultApiVersion = new ApiVersion(1, 0);
});
```
---

## 7. Explain the role of `IApplicationBuilder` in ASP.NET Core.
`IApplicationBuilder` is used in `Startup.Configure()` to define the middleware pipeline.

### Example:
```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    app.UseAuthorization();
    app.UseEndpoints(endpoints => endpoints.MapControllers());
}
```
---

## 8. What are Areas in ASP.NET Core and how do you use them?
Areas help organize large MVC applications by grouping controllers, views, and models.

### Example: Defining an Area
```csharp
[Area("Admin")]
public class DashboardController : Controller
{
    public IActionResult Index() => View();
}
```
---

## 9. How do you manage sessions in ASP.NET Core applications?
ASP.NET Core provides session state management using `ISession`.

### Example:
```csharp
services.AddSession();

app.UseSession();

HttpContext.Session.SetString("User", "Admin");
string user = HttpContext.Session.GetString("User");
```
---

## 10. Describe how to implement caching in ASP.NET Core.
ASP.NET Core supports memory caching and distributed caching.

### Example: In-Memory Caching
```csharp
services.AddMemoryCache();

public class MyService
{
    private readonly IMemoryCache _cache;
    public MyService(IMemoryCache cache) { _cache = cache; }

    public string GetData()
    {
        return _cache.GetOrCreate("cachedData", entry => "Hello, Cache!"); 
    }
}
```
---
