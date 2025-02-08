# **Software Design Principles and Patterns**

## **1. DRY (Don't Repeat Yourself) Principle**
### **Definition:**
DRY principle states that **every piece of knowledge must have a single, unambiguous representation** in the system. It helps in avoiding code duplication and makes maintenance easier.

### **Wrong Example (Code Duplication)**
```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def send_email(self, message):
        print(f"Sending email to {self.email}: {message}")

class Admin:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def send_email(self, message):
        print(f"Sending email to {self.email}: {message}")
```
*Issue:* The `send_email` method is repeated in both classes.

### **Correct Example (Using Inheritance for Reusability)**
```python
class Person:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def send_email(self, message):
        print(f"Sending email to {self.email}: {message}")

class User(Person):
    pass

class Admin(Person):
    pass
```
✅ **Benefit:** Avoids duplication and enhances maintainability.

---

## **2. KISS (Keep It Simple, Stupid) Principle**
### **Definition:**
KISS principle states that the system should be **as simple as possible**. Unnecessary complexity should be avoided.

### **Wrong Example (Overly Complex Code)**
```python
class Calculator:
    def compute(self, a, b, operation):
        if operation == "add":
            return a + b
        elif operation == "subtract":
            return a - b
        elif operation == "multiply":
            return a * b
        elif operation == "divide":
            return a / b
        else:
            return "Invalid Operation"
```
*Issue:* Too many conditionals make it hard to read and maintain.

### **Correct Example (Using a Dictionary for Simplicity)**
```python
class Calculator:
    def __init__(self):
        self.operations = {
            "add": lambda a, b: a + b,
            "subtract": lambda a, b: a - b,
            "multiply": lambda a, b: a * b,
            "divide": lambda a, b: a / b,
        }

    def compute(self, a, b, operation):
        return self.operations.get(operation, lambda a, b: "Invalid Operation")(a, b)
```
✅ **Benefit:** Reduces complexity and enhances readability.

---

# **Software Design Patterns**

## **1. Creational Patterns**
### **Singleton Pattern** (Ensures only one instance of a class is created)

**Wrong Example (Allows multiple instances):**
```python
class Database:
    def __init__(self):
        print("Database connection established")
```

```python
db1 = Database()
db2 = Database()  # Creates a new instance
```
*Issue:* Multiple database instances can be created, leading to inefficiency.

**Correct Example (Using Singleton):**
```python
class Database:
    _instance = None
    
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super(Database, cls).__new__(cls)
            print("Database connection established")
        return cls._instance
```
```python
db1 = Database()
db2 = Database()
print(db1 is db2)  # True
```
✅ **Benefit:** Ensures only one instance is used throughout the system.

---

## **2. Structural Patterns**
### **Adapter Pattern** (Allows incompatible interfaces to work together)

**Wrong Example (Tightly Coupled Code):**
```python
class PayPal:
    def make_payment(self, amount):
        print(f"Paid {amount} using PayPal")

paypal = PayPal()
paypal.make_payment(100)
```
*Issue:* If we want to switch to another payment provider, we need to modify the class.

**Correct Example (Using Adapter):**
```python
class PayPal:
    def make_payment(self, amount):
        print(f"Paid {amount} using PayPal")

class Stripe:
    def pay(self, amount):
        print(f"Paid {amount} using Stripe")

class PaymentAdapter:
    def __init__(self, payment_instance, method):
        self.payment_instance = payment_instance
        self.method = method

    def process_payment(self, amount):
        getattr(self.payment_instance, self.method)(amount)
```
```python
paypal_adapter = PaymentAdapter(PayPal(), "make_payment")
paypal_adapter.process_payment(100)

stripe_adapter = PaymentAdapter(Stripe(), "pay")
stripe_adapter.process_payment(200)
```
✅ **Benefit:** Easily switch between different payment gateways.

---

## **3. Behavioral Patterns**
### **Observer Pattern** (Allows objects to notify other objects when a change occurs)

**Wrong Example (Direct Dependencies, Hard to Scale):**
```python
class Order:
    def __init__(self):
        self.email_notifier = EmailNotifier()
        self.sms_notifier = SMSNotifier()

    def notify(self, message):
        self.email_notifier.send(message)
        self.sms_notifier.send(message)
```
*Issue:* Order is tightly coupled to notification classes.

**Correct Example (Using Observer Pattern):**
```python
class Order:
    def __init__(self):
        self.subscribers = []
    
    def subscribe(self, observer):
        self.subscribers.append(observer)

    def notify(self, message):
        for subscriber in self.subscribers:
            subscriber.update(message)
```
```python
class EmailNotifier:
    def update(self, message):
        print(f"Sending Email: {message}")

class SMSNotifier:
    def update(self, message):
        print(f"Sending SMS: {message}")
```
```python
order = Order()
order.subscribe(EmailNotifier())
order.subscribe(SMSNotifier())
order.notify("Order shipped!")
```
✅ **Benefit:** Easily extend notifications without modifying the `Order` class.

---

## **Conclusion**
| **Principle/Pattern** | **Benefit** |
|----------------|-----------------|
| **DRY** | Avoids code duplication, improving maintainability. |
| **KISS** | Reduces complexity, making the code easier to read and modify. |
| **Singleton** | Ensures only one instance of a class exists. |
| **Adapter** | Allows different classes to work together seamlessly. |
| **Observer** | Enables scalable event-driven architecture. |

This structured approach ensures a **scalable, modular, and maintainable system** using best design principles and patterns. 🚀
