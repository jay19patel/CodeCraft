# SOLID Principles in Object-Oriented Programming (OOP)

SOLID is a set of five design principles that improve software quality, making it **scalable, maintainable, and flexible**. These principles were introduced by **Robert C. Martin (Uncle Bob)**.

---

## **1. S – Single Responsibility Principle (SRP)**
### "A class should have only one reason to change."

Each class should focus on a **single task** or responsibility.

### ✅ **Example (Correct Implementation)**
```python
class ReportGenerator:
    def generate_pdf(self, data):
        pass  # Generates PDF report

class ReportSaver:
    def save_to_database(self, report):
        pass  # Saves report to database
```
👉 Here, `ReportGenerator` only **creates** the report, while `ReportSaver` **stores** it.

### ❌ **Example (Violating SRP)**
```python
class Report:
    def generate_pdf(self, data):
        pass

    def save_to_database(self, report):
        pass  # Handles multiple responsibilities (BAD PRACTICE)
```

---

## **2. O – Open/Closed Principle (OCP)**
### "Software should be open for extension, but closed for modification."

New functionality should be added **without modifying** existing code.

### ✅ **Example (Using Polymorphism)**
```python
class PaymentProcessor:
    def process_payment(self, amount):
        pass

class PayPalPayment(PaymentProcessor):
    def process_payment(self, amount):
        print(f"Processing ${amount} via PayPal")

class StripePayment(PaymentProcessor):
    def process_payment(self, amount):
        print(f"Processing ${amount} via Stripe")
```
👉 Adding new payment methods does **not** require modifying existing code.

### ❌ **Example (Violating OCP)**
```python
class PaymentProcessor:
    def process_payment(self, amount, method):
        if method == "paypal":
            print(f"Processing ${amount} via PayPal")
        elif method == "stripe":
            print(f"Processing ${amount} via Stripe")
```
🚨 If a new payment method is added, the **existing code must be modified**.

---

## **3. L – Liskov Substitution Principle (LSP)**
### "Subtypes must be substitutable for their base types."

A subclass should be **replaceable** for its parent class without breaking the program.

### ❌ **Bad Example (Violating LSP)**
```python
class Bird:
    def fly(self):
        pass

class Sparrow(Bird):
    def fly(self):
        print("Sparrow is flying")

class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins cannot fly")  # Breaks LSP
```
🚨 `Penguin` **inherits** `Bird` but **cannot fly**, violating LSP.

### ✅ **Correct Approach**
```python
class Bird:
    pass

class FlyingBird(Bird):
    def fly(self):
        pass

class Sparrow(FlyingBird):
    def fly(self):
        print("Sparrow is flying")

class Penguin(Bird):
    def swim(self):
        print("Penguin is swimming")
```
👉 `Penguin` no longer has a `fly()` method, fixing the issue.

---

## **4. I – Interface Segregation Principle (ISP)**
### "Clients should not be forced to depend on methods they do not use."

Instead of a large interface, create **multiple specific interfaces**.

### ❌ **Bad Example (Violating ISP)**
```python
class Animal:
    def fly(self):
        pass

    def swim(self):
        pass

class Dog(Animal):
    def swim(self):
        print("Dog is swimming")
    
    def fly(self):
        raise Exception("Dogs cannot fly")  # Violates ISP
```
🚨 `Dog` **must implement** `fly()`, which it does **not need**.

### ✅ **Correct Approach (Using Multiple Interfaces)**
```python
class Swimmable:
    def swim(self):
        pass

class Flyable:
    def fly(self):
        pass

class Dog(Swimmable):
    def swim(self):
        print("Dog is swimming")

class Bird(Flyable):
    def fly(self):
        print("Bird is flying")
```
👉 `Dog` only implements **swimming**, and `Bird` only implements **flying**.

---

## **5. D – Dependency Inversion Principle (DIP)**
### "Depend on abstractions, not concrete implementations."

A class should depend on an **interface (abstraction)** rather than a specific implementation.

### ❌ **Bad Example (Violating DIP – Tight Coupling)**
```python
class MySQLDatabase:
    def connect(self):
        print("Connecting to MySQL")

class UserService:
    def __init__(self):
        self.db = MySQLDatabase()  # ❌ Direct dependency

    def get_user(self):
        self.db.connect()
```
🚨 If we switch to PostgreSQL, `UserService` **must be modified**.

### ✅ **Correct Approach (Using Abstraction – Loose Coupling)**
```python
class Database:
    def connect(self):
        pass

class MySQLDatabase(Database):
    def connect(self):
        print("Connecting to MySQL")

class PostgreSQLDatabase(Database):
    def connect(self):
        print("Connecting to PostgreSQL")

class UserService:
    def __init__(self, db: Database):  # ✅ Inject dependency
        self.db = db

    def get_user(self):
        self.db.connect()

# Now, we can easily switch databases
service = UserService(PostgreSQLDatabase())
service.get_user()  # Output: Connecting to PostgreSQL
```
👉 `UserService` now **depends on an abstraction**, making it flexible.

---

## **Summary of SOLID Principles**
| **Principle** | **Definition** | **Benefit** |
|--------------|--------------|-----------|
| **S** – Single Responsibility | A class should have **one** responsibility. | Easier maintenance & debugging |
| **O** – Open/Closed | Open for extension, closed for modification. | Enhances flexibility & reduces risk |
| **L** – Liskov Substitution | Subtypes must be **replaceable** for their base types. | Prevents unexpected behavior |
| **I** – Interface Segregation | A class should not **implement unnecessary methods**. | Reduces unnecessary dependencies |
| **D** – Dependency Inversion | Depend on **abstractions**, not concrete implementations. | Improves testability & maintainability |

---

### 🚀 Following **SOLID Principles** makes your code **clean, reusable, and scalable**! 🚀

