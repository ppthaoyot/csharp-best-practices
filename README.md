
## 📌 🔧 Overview  
Repo นี้รวมแนวทางการทดสอบโปรแกรมในภาษา C# ซึ่งเน้นการใช้เครื่องมือ **xUnit** สำหรับการทดสอบหน่วย (Unit Test) และการใช้ **Moq** ในการจำลองพฤติกรรมของบริการ (Mocking) เพื่อให้สามารถทดสอบโค้ดได้อย่างอิสระจากการพึ่งพาภายนอก

ภายใน repo นี้ประกอบไปด้วยตัวอย่างโค้ดและคำแนะนำในการทำการทดสอบในหลายรูปแบบ รวมถึง **Unit Testing**, **Integration Testing**, และ **Mocking** ผ่านการใช้ **Moq**.

---

## 📌 📂 โครงสร้างของโปรเจค

```
/MyProject
  ├── /docs
  │    ├── README.md         # ไฟล์หลักสำหรับแนะนำ repo
  │    ├── unit-testing.md   # อธิบาย Unit Testing
  │    ├── integration-testing.md # อธิบาย Integration Testing
  │    ├── mocking.md        # อธิบายการใช้ Moq
  │    ├── practices.md      # แนวทางการปฏิบัติ
```

---

## 📌 🧪 เนื้อหาหลัก

1. **[Unit Testing](docs/unit-testing.md):**  
   อธิบายการเขียนและการทำงานของ **Unit Test** ด้วย **xUnit** รวมถึงตัวอย่างการทดสอบเมธอดต่างๆ เช่น เมธอดคำนวณผลรวม

2. **[Integration Testing](docs/integration-testing.md):**  
   อธิบายการทำ **Integration Test** โดยทดสอบการทำงานร่วมกันระหว่างหลายๆ โมดูลในระบบ เช่น ทดสอบการทำงานร่วมกันระหว่าง `OrderService` และ `PaymentService`

3. **[Mocking with Moq](docs/mocking.md):**  
   การจำลองพฤติกรรมของบริการหรือโมดูลที่ต้องการทดสอบโดยไม่ต้องพึ่งพาภายนอก (เช่น ฐานข้อมูลหรือ API) ด้วย **Moq**

4. **[Best Practices](docs/practices.md):**  
   แนวทางการเขียนและการทดสอบโปรแกรมที่ดีที่สุดใน C# โดยเน้นการทดสอบที่ง่ายและสามารถบำรุงรักษาได้ในระยะยาว

---

## 📌 🛠️ การตั้งค่าโปรเจค

ในการเริ่มต้นโปรเจคนี้ให้ทำการติดตั้งเครื่องมือที่จำเป็น เช่น **.NET SDK**, **xUnit**, **Moq** และติดตั้งแพ็กเกจที่เกี่ยวข้องผ่านคำสั่ง:

```sh
dotnet add package xunit
dotnet add package Moq
dotnet add package Microsoft.NET.Test.Sdk
dotnet add package xunit.runner.visualstudio
```

---

## 📌 🏃‍♂️ การทดสอบ

คุณสามารถรันการทดสอบทั้งหมดในโปรเจคนี้โดยใช้คำสั่ง:

```sh
dotnet test
```

คำสั่งนี้จะรันทุกไฟล์ทดสอบในโปรเจค และแสดงผลลัพธ์การทดสอบให้คุณทราบ

---

## 📌 💬 สรุป

ใน repo นี้คุณจะพบการทำ **Unit Testing**, **Integration Testing**, และ **Mocking** พร้อมทั้งคำแนะนำและแนวทางการเขียนการทดสอบในภาษา C# ที่ช่วยให้คุณสามารถพัฒนาโปรแกรมที่มีคุณภาพและสามารถบำรุงรักษาได้ในระยะยาว

---

📚 เรียนรู้เพิ่มเติมเกี่ยวกับแนวทางการทดสอบใน C# ได้จาก [Unit Testing](docs/unit-testing.md), [Integration Testing](docs/integration-testing.md), [Mocking with Moq](docs/mocking.md), และ [Best Practices](docs/practices.md)
