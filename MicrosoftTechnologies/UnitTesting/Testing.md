# .NET (UnitTesting) Interview Questions and Answers

1. **What is Unit Testing in .NET?**
2. **How do you mock dependencies in unit tests using .NET?**


## 1. What is Unit Testing in .NET?
Unit testing is the practice of testing individual components of an application in isolation.

### Example: NUnit Unit Test
```csharp
[Test]
public void TestSum()
{
    int result = Calculator.Sum(2, 3);
    Assert.AreEqual(5, result);
}
```
---

## 2. How do you mock dependencies in unit tests using .NET?
Mocking is used to simulate dependencies in unit tests using **Moq**.

### Example:
```csharp
var mockRepo = new Mock<IRepository>();
mockRepo.Setup(repo => repo.GetData()).Returns("Mock Data");
```
---