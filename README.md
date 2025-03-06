
# C# Testing Practices 🚀  

**แนวทางปฏิบัติที่ดีที่สุดสำหรับการทดสอบโปรแกรม C# โดยใช้ xUnit และ Moq รวมถึงแนวคิดเกี่ยวกับ Unit Testing, Integration Testing และ Test Automation**  

---

## 📌 🔧 ความต้องการเบื้องต้น (Prerequisites)  
ก่อนเริ่มต้นการเขียน Unit Test ตรวจสอบให้แน่ใจว่ามีสิ่งต่อไปนี้ติดตั้งแล้ว:  

- ติดตั้ง **.NET SDK** ([ดาวน์โหลดที่นี่](https://dotnet.microsoft.com/download))  
- ติดตั้งแพ็กเกจ `xUnit` และ `Moq` ผ่าน **NuGet Package Manager** หรือใช้คำสั่งด้านล่าง  

### **ติดตั้งแพ็กเกจที่จำเป็น**
```sh
dotnet add package xunit
dotnet add package Moq
dotnet add package Microsoft.NET.Test.Sdk
dotnet add package xunit.runner.visualstudio
```

---

## 📌 📂 โครงสร้างโปรเจคที่แนะนำ  
โครงสร้างนี้ช่วยให้ **แยกโค้ดหลัก (Production Code) และโค้ดสำหรับทดสอบ (Test Code) ออกจากกัน**  

```
/MyProject
  /MyProject
    ├── Services
    │    ├── ICalculatorService.cs
    │    ├── CalculatorService.cs
    ├── Models
    │    ├── CalculationResult.cs
  /MyProject.Tests
    ├── Services
    │    ├── CalculatorServiceTests.cs
```

---

## 📌 ✅ การเขียน Unit Test  

### 1️⃣ **Basic Unit Test (ทดสอบเมธอดปกติ)**  
#### **โค้ดจริง (Production Code)**
```csharp
public interface ICalculatorService
{
    int Add(int a, int b);
}

public class CalculatorService : ICalculatorService
{
    public int Add(int a, int b) => a + b;
}
```

#### **โค้ดสำหรับทดสอบ (Unit Test)**
```csharp
using Xunit;

namespace MyProject.Tests
{
    public class CalculatorServiceTests
    {
        private readonly CalculatorService _calculator = new();

        [Fact] // ใช้ Fact Attribute สำหรับทดสอบเมธอดเดียว
        public void Add_ShouldReturnCorrectSum()
        {
            // Arrange: กำหนดค่าทดสอบ
            int a = 5;
            int b = 3;

            // Act: เรียกใช้งานเมธอด
            int result = _calculator.Add(a, b);

            // Assert: ตรวจสอบผลลัพธ์
            Assert.Equal(8, result);
        }
    }
}
```

---

### 2️⃣ **Unit Test with Moq (ทดสอบเมธอดที่เรียกใช้งานเมธอดอื่น)**  
#### **โค้ดจริง (Production Code)**
```csharp
public interface IOrderService
{
    int CalculateTotal(int price, int quantity);
}

public class OrderService : IOrderService
{
    private readonly ICalculatorService _calculatorService;

    public OrderService(ICalculatorService calculatorService)
    {
        _calculatorService = calculatorService;
    }

    public int CalculateTotal(int price, int quantity)
    {
        return _calculatorService.Add(price, quantity);
    }
}
```
#### **โค้ดสำหรับทดสอบโดยใช้ Moq**
```csharp
using Xunit;
using Moq;

namespace MyProject.Tests
{
    public class OrderServiceTests
    {
        private readonly Mock<ICalculatorService> _mockCalculator = new();
        private readonly OrderService _orderService;

        public OrderServiceTests()
        {
            _orderService = new OrderService(_mockCalculator.Object);
        }

        [Fact]
        public void CalculateTotal_ShouldCallCalculatorService()
        {
            // Arrange
            _mockCalculator.Setup(m => m.Add(100, 2)).Returns(102);

            // Act
            int result = _orderService.CalculateTotal(100, 2);

            // Assert
            Assert.Equal(102, result);
            _mockCalculator.Verify(m => m.Add(100, 2), Times.Once); // ตรวจสอบว่าเมธอด Add ถูกเรียกครั้งเดียว
        }
    }
}
```

---

## 📌 🏃‍♂️ การรัน Unit Test  
ใช้คำสั่งนี้เพื่อรันทุก Unit Test ในโปรเจค  
```sh
dotnet test
```
หรือถ้าต้องการรัน Unit Test เฉพาะเมธอด  
```sh
dotnet test --filter FullyQualifiedName=MyProject.Tests.Services.CalculatorServiceTests.Add_ShouldReturnCorrectSum
```

---

## 📌 ⚡ Best Practices (แนวทางปฏิบัติที่ดี)  
| Feature | Description |
|---------|------------|
| **Basic Test** | ใช้ `xUnit` และ `Assert.Equal` |
| **Mock Dependencies** | ใช้ `Moq` เพื่อจำลองพฤติกรรมของ Dependency |
| **Naming Conventions** | ตั้งชื่อแบบ `MethodName_ShouldDoSomething_WhenCondition` |
| **Project Structure** | แยกโค้ดหลักและโค้ดสำหรับทดสอบเป็นโฟลเดอร์แยกกัน |
| **Test Execution** | ใช้ `dotnet test` หรือ Visual Studio Test Explorer |

---

## 📌 🚀 อัปเดตในอนาคต  
✅ เพิ่มตัวอย่าง Integration Testing  
✅ เพิ่มตัวอย่าง Performance Testing  
✅ ขยายแนวทางปฏิบัติสำหรับ Test Automation  

---

**🎯 Unit Testing ทำให้โค้ดของเรามีคุณภาพดีขึ้น และช่วยลด Bug ในการพัฒนา! ✅**  

