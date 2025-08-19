# .NET (Best Practices & Architecture) Interview Questions and Answers

1. **Can you explain SOLID principles?**
2. **What is Continuous Integration/Continuous Deployment (CI/CD) and how does it apply to .NET development?**
3. **How do you ensure your C# code is secure?**
4. **What are some common performance issues in .NET applications and how do you address them?**
5. **Describe the Repository pattern and its benefits.**
6. **How do you handle database migrations in Entity Framework?**
7. **What tools do you use for debugging and profiling .NET applications?**
8. **How do you stay updated with the latest .NET technologies and practices?**

## 1. Can you explain SOLID principles?
**SOLID** is a set of five principles for writing maintainable code:

1. **S**ingle Responsibility Principle

2. **O**pen/Closed Principle

3. **L**iskov Substitution Principle

4. **I**nterface Segregation Principle

5. **D**ependency Inversion Principle
---

## 2. What is Continuous Integration/Continuous Deployment (CI/CD)?
CI/CD automates code integration, testing, and deployment.

### Example: GitHub Actions for CI/CD
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2
      - name: Build and Test
        run: dotnet test
```
## 3. How do you ensure your C# code is secure?
Writing secure C# code requires following best practices to prevent vulnerabilities such as SQL injection, cross-site scripting (XSS), and unauthorized access.

### Key Security Measures:
- **Use parameterized queries** to prevent SQL injection.
- **Hash and salt passwords** using `BCrypt` or `PBKDF2`.
- **Enable HTTPS** to encrypt communication.
- **Validate user input** to prevent injection attacks.
- **Use dependency injection (DI)** to avoid hardcoded secrets.

### Example: Preventing SQL Injection
```csharp
using System.Data.SqlClient;

public void GetUser(int userId)
{
    using (SqlConnection conn = new SqlConnection("connectionString"))
    {
        string query = "SELECT * FROM Users WHERE Id = @userId";
        using (SqlCommand cmd = new SqlCommand(query, conn))
        {
            cmd.Parameters.AddWithValue("@userId", userId);
            conn.Open();
            var reader = cmd.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(reader["Name"]);
            }
        }
    }
}
```
Here, **parameterized queries** prevent SQL injection.

---

## 4. What are some common performance issues in .NET applications and how do you address them?
Performance issues in .NET applications often arise due to inefficient memory usage, slow database queries, or excessive allocations.

### Common Performance Issues and Fixes:
- **Memory Leaks** → Use `IDisposable` and `using` statements.
- **Slow Queries** → Optimize SQL queries and indexing.
- **Excessive Object Allocations** → Use object pooling and struct types.

### Example: Using Caching to Improve Performance
```csharp
public class DataService
{
    private readonly IMemoryCache _cache;
    public DataService(IMemoryCache cache)
    {
        _cache = cache;
    }

    public string GetData()
    {
        return _cache.GetOrCreate("cachedData", entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
            return "Cached Data";
        });
    }
}
```
Using **in-memory caching** reduces expensive database calls.

---

## 5. Describe the Repository pattern and its benefits.
The **Repository Pattern** abstracts the data layer, providing a clean way to manage database operations.

### Benefits:
- **Separation of concerns** → Keeps data access logic separate.
- **Easier testing** → Allows mocking of database operations.
- **Code reusability** → Centralizes data access logic.

### Example: Repository Pattern in ASP.NET Core
```csharp
public interface IUserRepository
{
    IEnumerable<User> GetAllUsers();
    User GetUserById(int id);
}

public class UserRepository : IUserRepository
{
    private readonly AppDbContext _context;
    public UserRepository(AppDbContext context) { _context = context; }

    public IEnumerable<User> GetAllUsers() => _context.Users.ToList();

    public User GetUserById(int id) => _context.Users.Find(id);
}
```
Here, the `UserRepository` encapsulates database operations.

---

## 6. How do you handle database migrations in Entity Framework?
Migrations in Entity Framework allow changes to the database schema while preserving existing data.

### Steps to Apply Migrations:
1. **Add a new migration:**
   ```shell
   dotnet ef migrations add InitialCreate
   ```
2. **Apply the migration to the database:**
   ```shell
   dotnet ef database update
   ```

### Example: Defining a Model with Migrations
```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}
```
Adding a migration for this model will create a corresponding database table.

---

## 7. What tools do you use for debugging and profiling .NET applications?
Effective debugging and profiling tools help identify performance bottlenecks and runtime errors.

### Popular Tools:
- **Visual Studio Debugger** → Step-through debugging.
- **dotTrace** → Identifies CPU-intensive operations.
- **BenchmarkDotNet** → Measures code performance.
- **dotMemory** → Finds memory leaks.

### Example: Using BenchmarkDotNet to Measure Performance
```csharp
[MemoryDiagnoser]
public class PerformanceTests
{
    [Benchmark]
    public void TestMethod()
    {
        var list = new List<int>();
        for (int i = 0; i < 1000; i++)
        {
            list.Add(i);
        }
    }
}
```
Here, **BenchmarkDotNet** measures memory usage and execution time.

---

## 8. How do you stay updated with the latest .NET technologies and practices?
Staying current with .NET advancements ensures that you use the best tools and frameworks.

### Recommended Ways to Stay Updated:
- **Follow Microsoft’s .NET Blog** → [https://devblogs.microsoft.com/dotnet/](https://devblogs.microsoft.com/dotnet/)
- **Attend Conferences/Webinars** → .NET Conf, Microsoft Build
- **Join Online Communities** → GitHub, Stack Overflow, Twitter

---
