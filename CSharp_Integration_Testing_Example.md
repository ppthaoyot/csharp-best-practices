
# C# Integration Testing Example 🚀

**ตัวอย่างการทดสอบ Integration Testing โดยใช้ xUnit และ Moq ในการทดสอบการทำงานร่วมกันของหลายๆ โมดูล**

---

## 📌 🎯 Overview  
**Integration Testing** ใช้เพื่อทดสอบว่าแต่ละโมดูลของระบบสามารถทำงานร่วมกันได้อย่างถูกต้อง ตัวอย่างนี้จะใช้ `OrderService` และ `PaymentService` ซึ่ง `OrderService` ต้องเรียก `PaymentService` เพื่อทำการชำระเงิน

---

## 📌 🛠️ Production Code  
### 1️⃣ **PaymentService**
```csharp
public interface IPaymentService
{
    bool ProcessPayment(int amount);
}

public class PaymentService : IPaymentService
{
    public bool ProcessPayment(int amount)
    {
        // Logic for processing payment (for example, call to an external API)
        return amount > 0; // Assuming a payment is valid if the amount is greater than 0
    }
}
```

### 2️⃣ **OrderService**
```csharp
public interface IOrderService
{
    bool PlaceOrder(int amount);
}

public class OrderService : IOrderService
{
    private readonly IPaymentService _paymentService;

    public OrderService(IPaymentService paymentService)
    {
        _paymentService = paymentService;
    }

    public bool PlaceOrder(int amount)
    {
        if (_paymentService.ProcessPayment(amount))
        {
            // Logic to place order
            return true;
        }
        return false;
    }
}
```

---

## 📌 🧪 Integration Test

### **ทดสอบการทำงานร่วมกันระหว่าง `OrderService` และ `PaymentService` โดยใช้ Moq**
```csharp
using Xunit;
using Moq;

namespace MyProject.Tests
{
    public class OrderServiceIntegrationTests
    {
        private readonly Mock<IPaymentService> _mockPaymentService;
        private readonly OrderService _orderService;

        public OrderServiceIntegrationTests()
        {
            // สร้าง Mock ของ PaymentService
            _mockPaymentService = new Mock<IPaymentService>();
            // สร้างตัวอย่างของ OrderService โดยส่ง PaymentService เข้าไป
            _orderService = new OrderService(_mockPaymentService.Object);
        }

        [Fact]
        public void PlaceOrder_ShouldReturnTrue_WhenPaymentIsSuccessful()
        {
            // Arrange
            int amount = 100;
            _mockPaymentService.Setup(ps => ps.ProcessPayment(amount)).Returns(true); // จำลองว่า Payment สำเร็จ

            // Act
            bool result = _orderService.PlaceOrder(amount);

            // Assert
            Assert.True(result); // คาดว่า Order จะถูกดำเนินการ
            _mockPaymentService.Verify(ps => ps.ProcessPayment(amount), Times.Once); // ตรวจสอบว่า Payment ถูกเรียกแค่ครั้งเดียว
        }

        [Fact]
        public void PlaceOrder_ShouldReturnFalse_WhenPaymentFails()
        {
            // Arrange
            int amount = 0;
            _mockPaymentService.Setup(ps => ps.ProcessPayment(amount)).Returns(false); // จำลองว่า Payment ล้มเหลว

            // Act
            bool result = _orderService.PlaceOrder(amount);

            // Assert
            Assert.False(result); // คาดว่า Order จะไม่ถูกดำเนินการ
            _mockPaymentService.Verify(ps => ps.ProcessPayment(amount), Times.Once); // ตรวจสอบว่า Payment ถูกเรียกแค่ครั้งเดียว
        }
    }
}
```

---

## 📌 🏃‍♂️ การรัน Integration Test  
ใช้คำสั่งนี้เพื่อรันทุก Unit Test รวมถึง Integration Test  
```sh
dotnet test
```

---

## 📌 ⚡ อธิบายการทดสอบ  
- **Arrange:** เรากำหนดค่าทดสอบ (ในที่นี้คือจำนวนเงิน `amount` ที่ต้องการใช้ในการทดสอบ) และจำลองพฤติกรรมของ `PaymentService` ด้วย `Moq` ว่ามันจะคืนค่าผลลัพธ์ว่า **สำเร็จหรือไม่** ตามที่กำหนด
- **Act:** เราทำการทดสอบฟังก์ชัน `PlaceOrder` ใน `OrderService`
- **Assert:** ตรวจสอบว่า `PlaceOrder` ทำงานถูกต้องตามที่คาดหวัง (ตรวจสอบว่า Order ถูกวางหรือไม่) และตรวจสอบว่า `ProcessPayment` ถูกเรียกด้วยค่าที่ถูกต้อง

---

🔥 **Integration Testing ช่วยให้มั่นใจได้ว่าโมดูลต่างๆ ในระบบสามารถทำงานร่วมกันได้ตามที่คาดหวัง!**
