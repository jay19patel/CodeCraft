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



---
---
---
#  **SOLID Principle**

    - Single Responsibility Principle (SRP)
    - Open/Closed Principle (OCP)
    - Liskov Substitution Principle (LSP)
    - Interface Segregation Principle (ISP)
    - Dependency Inversion Principle (DIP)


## User Class

### Unorganized Code
```python
from argon2 import PasswordHasher

ph = PasswordHasher()

class User:
    def __init__(self,name,email):
        self.id = 1
        self.name=name
        self.email=email
        self.is_authenticated=False
    def __repr__(self):
        return f"User(id={self.id},name={self.name},email={self.email})"
    
    def create_password(self,password):
        self.password = ph.hash(password)

    def authentication(self,password):
        try:
            ph.verify(self.password, password)
        except Exception as e:
            raise ValueError("Password not match!",e)
        self.is_authenticated = True
        return True
    
user = User("John Doe", "john.doe@example.com")
user.create_password("123456")
print(user.authentication("123456"))
print(user)
```

### SOLID-Compliant Code
```python
from argon2 import PasswordHasher

class AuthService:
    def __init__(self):
        self.hasher = PasswordHasher()

    def hash_password(self, password):
        return self.hasher.hash(password)

    def verify_password(self, hashed_password, input_password):
        try:
            return self.hasher.verify(hashed_password, input_password)
        except Exception:
            return False

class User:
    def __init__(self, user_id, name, email, auth_service):
        self.id = user_id
        self.name = name
        self.email = email
        self.is_authenticated = False
        self._auth_service = auth_service
        self._password = None

    def __repr__(self):
        return f"User(id={self.id}, name={self.name}, email={self.email})"

    def set_password(self, password):
        self._password = self._auth_service.hash_password(password)

    def authenticate(self, password):
        if self._password and self._auth_service.verify_password(self._password, password):
            self.is_authenticated = True
            return True
        return False


auth_service = AuthService()

# Create a user
user = User(user_id=1, name="Jay Patel", email="justjayy19@example.com", auth_service=auth_service)
user.set_password("123456")

# Authenticate user
print(user.authenticate("123456"))  # ✅ True
print(user)  # ✅ User object representation
# User(id=1, name=Jay Patel, email=justjayy19@example.com)
```

### Final Benefits of This Approach
| Principle | Fix |
|-----------|-----|
| ✅ **SRP** (Single Responsibility) | User class only manages user data; `AuthService` handles passwords. |
| ✅ **OCP** (Open-Closed) | Can switch hashing methods without modifying `User`. |
| ✅ **LSP** (Liskov Substitution) | Future user types can inherit from `User` without breaking authentication. |
| ✅ **ISP** (Interface Segregation) | `User` does not handle unnecessary authentication logic. |
| ✅ **DIP** (Dependency Inversion) | `User` depends on `AuthService`, not a specific hashing library. |



## Product Class

### Unorganized Product Code
```python
class Product:
    def __init__(self,name,price,stock):
        self.id = 1
        self.name = name
        self.price = price
        self.stock = stock
    
    def __repr__(self):
        return f"Product(id={self.id},name={self.name},price={self.price})"
    
    def is_available(self):
        return self.stock > 0
    
    def add_stock(self,stock):
        self.stock = stock
```

### SOLID-Compliant Product Code (Production-Ready)
```python
from typing import Optional

class Product:
    def __init__(self, name: str, price: float, stock: int, category: Optional[str] = None, description: Optional[str] = None, discount: float = 0.0):
        self.id = 1
        self.name = name
        self.price = max(0, price)  # Prevent negative prices
        self.stock = max(0, stock)  # Prevent negative stock
        self.category = category
        self.description = description
        self.discount = discount
    
    def __repr__(self) -> str:
        return f"Product(id={self.id}, name={self.name}, price={self.get_final_price()}, stock={self.stock})"
    
    def is_available(self) -> bool:
        return self.stock > 0
    
    def add_stock(self, quantity: int) -> None:
        if quantity > 0:
            self.stock += quantity
    
    def reduce_stock(self, quantity: int) -> bool:
        if 0 < quantity <= self.stock:
            self.stock -= quantity
            return True
        return False
    
    def apply_discount(self, discount_percentage: float) -> None:
        if 0 <= discount_percentage <= 100:
            self.discount = discount_percentage
    
    def get_final_price(self) -> float:
        return round(self.price * (1 - self.discount / 100), 2)

class PhysicalProduct(Product):
    def __init__(self, name: str, price: float, stock: int, weight: float, dimensions: str, **kwargs):
        super().__init__(name, price, stock, **kwargs)
        self.weight = weight
        self.dimensions = dimensions
    
    def __repr__(self) -> str:
        return f"PhysicalProduct(id={self.id}, name={self.name}, weight={self.weight}kg, dimensions={self.dimensions}, price={self.get_final_price()}, stock={self.stock})"

class DigitalProduct(Product):
    def __init__(self, name: str, price: float, stock: int, file_size: float, download_link: str, **kwargs):
        super().__init__(name, price, stock, **kwargs)
        self.file_size = file_size
        self.download_link = download_link
    
    def __repr__(self) -> str:
        return f"DigitalProduct(id={self.id}, name={self.name}, file_size={self.file_size}MB, price={self.get_final_price()}, stock={self.stock})"
    



laptop = PhysicalProduct(name="Laptop", price=1500.0, stock=5, weight=2.5, dimensions="35x25x2 cm")
ebook = DigitalProduct(name="Python Guide", price=50.0, stock=100, file_size=5.0, download_link="https://example.com/python-guide")

print(laptop)
print(ebook)

# Applying discount
laptop.apply_discount(10)
print(f"After discount: {laptop}")

# PhysicalProduct(id=1, name=Laptop, weight=2.5kg, dimensions=35x25x2 cm, price=1500.0, stock=5)
# DigitalProduct(id=1, name=Python Guide, file_size=5.0MB, price=50.0, stock=100)
# After discount: PhysicalProduct(id=1, name=Laptop, weight=2.5kg, dimensions=35x25x2 cm, price=1350.0, stock=5)
```

### Final Benefits of This Approach
| Principle | Fix |
|-----------|-----|
| ✅ **SRP** (Single Responsibility) | Product class only manages product data and logic. |
| ✅ **OCP** (Open-Closed) | Supports new features like categories, descriptions, and discounts without modification. |
| ✅ **LSP** (Liskov Substitution) | Can extend the `Product` class for different types (e.g., `DigitalProduct`, `PhysicalProduct`). |
| ✅ **ISP** (Interface Segregation) | Product class does not manage unrelated concerns like order processing. |
| ✅ **DIP** (Dependency Inversion) | No dependency on fixed values; ID and stock management follow flexible logic. |




## Cart And Order Class

### Unorganized Order Code
```python
from models.base_id import IDGenerator
from services.email_service import EmailService
from services.sms_service import SMSService

class Order:
    def __init__(self, cart):
        self.order_id = IDGenerator.generate_id("ORD")
        self.user = cart.user
        self.items = cart.items
        self.total_price = cart.get_total()
        self.status = "Pending"

    def process_order(self):
        self.status = "Confirmed"
        EmailService.send_email(self.user.email, "Order Confirmation", f"Your order {self.order_id} is confirmed!")
        SMSService.send_sms(self.user.phone, f"Order {self.order_id} confirmed! Total: ${self.total_price}")

    def __str__(self):
        return f"Order {self.order_id} - {self.status} - Total: ${self.total_price}"
```

### SOLID-Compliant Order Code (Production-Ready)
```python
from services.utilities import IDGenerator
from services.notification_service import NotificationService

class Order:
    def __init__(self, user, cart_items, total_price):
        self.order_id = IDGenerator.generate_id("ORD")
        self.user = user
        self.items = cart_items
        self.total_price = total_price
        self.status = "Pending"

    def process_order(self):
        self.status = "Confirmed"
        self.send_notifications()
    
    def send_notifications(self):
        NotificationService.send_email(self.user.email, "Order Confirmation", f"Your order {self.order_id} is confirmed!")
        NotificationService.send_sms(self.user.phone, f"Order {self.order_id} confirmed! Total: ${self.total_price}")
    
    def cancel_order(self):
        if self.status == "Pending":
            self.status = "Cancelled"
            self.send_notifications()
        else:
            raise ValueError("Order cannot be cancelled after confirmation.")
    
    def __str__(self):
        return f"Order {self.order_id} - {self.status} - Total: ${self.total_price}"
```

### Unorganized Cart Code
```python
from models.base_id import IDGenerator

class Cart:
    def __init__(self, user):
        self.cart_id = IDGenerator.generate_id("CRT")
        self.user = user
        self.items = []

    def add_product(self, product, quantity):
        self.items.append((product, quantity))

    def get_total(self):
        return sum(item[0].price * item[1] for item in self.items)

    def __str__(self):
        return f"Cart {self.cart_id} - {len(self.items)} items"
```

### SOLID-Compliant Cart Code (Production-Ready)
```python
from services.utilities import IDGenerator
from typing import List, Tuple

class Cart:
    def __init__(self, user):
        self.cart_id = IDGenerator.generate_id("CRT")
        self.user = user
        self.items: List[Tuple[object, int]] = []  # List of (Product, Quantity)

    def add_product(self, product, quantity):
        if quantity > 0:
            self.items.append((product, quantity))

    def remove_product(self, product):
        self.items = [item for item in self.items if item[0] != product]
    
    def get_total(self):
        return sum(item[0].get_final_price() * item[1] for item in self.items)
    
    def clear_cart(self):
        self.items.clear()
    
    def __str__(self):
        return f"Cart {self.cart_id} - {len(self.items)} items - Total: ${self.get_total()}"
```

### Final Benefits of This Approach
| Principle | Fix |
|-----------|-----|
| ✅ **SRP** (Single Responsibility) | Order handles only order-related tasks, Cart handles only cart-related tasks. |
| ✅ **OCP** (Open-Closed) | New features like order cancellation and cart clearing can be added easily. |
| ✅ **LSP** (Liskov Substitution) | Order and Cart follow expected behaviors, making them easy to extend. |
| ✅ **ISP** (Interface Segregation) | Order does not handle notifications directly; a dedicated NotificationService is used. |
| ✅ **DIP** (Dependency Inversion) | Notifications are handled via NotificationService, making it easier to modify the communication method. |




## Notification Service

### Unorganized Notification Code
```python
class NotificationService:
    def send_email(email, subject, message):
        print(f"Sending email to {email}: {subject} - {message}")
    
    def send_sms(phone, message):
        print(f"Sending SMS to {phone}: {message}")
```

### SOLID-Compliant Notification Code (Production-Ready)
```python
from abc import ABC, abstractmethod

class Notification(ABC):
    @abstractmethod
    def send(self, recipient, message):
        pass

class EmailNotification(Notification):
    def send(self, email, message):
        print(f"Sending Email to {email}: {message}")

class SMSNotification(Notification):
    def send(self, phone, message):
        print(f"Sending SMS to {phone}: {message}")

class NotificationService:
    def __init__(self):
        self.notifiers = []
    
    def register_notifier(self, notifier: Notification):
        self.notifiers.append(notifier)
    
    def notify_all(self, recipient, message):
        for notifier in self.notifiers:
            notifier.send(recipient, message)
```

### Final Benefits of This Approach
| Principle | Fix |
|-----------|-----|
| ✅ **SRP** (Single Responsibility) | Each notification method (Email, SMS) has its own dedicated class. |
| ✅ **OCP** (Open-Closed) | New notification methods (e.g., Push Notification) can be added without modifying existing code. |
| ✅ **LSP** (Liskov Substitution) | All notifiers follow the same interface, ensuring compatibility. |
| ✅ **ISP** (Interface Segregation) | Order and Cart do not directly handle notifications; they use a separate NotificationService. |
| ✅ **DIP** (Dependency Inversion) | NotificationService depends on an abstract Notification class rather than concrete implementations. |


