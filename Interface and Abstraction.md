# Interface and Abstraction

- 🔹 Why Use an Interface Here?
- 🔄 Scalability: You can add new payment methods (like CryptoPayment) without modifying existing code.
- 🔧 Flexibility: The PaymentProcessor class works with any payment method that follows the PaymentMethod interface.
- ✅ Code Reusability: The same logic applies to multiple payment types without code duplication.
- 📏 Enforcing Contract: All payment methods must implement process_payment and refund_payment, ensuring consistency.

### *Example*
```python
from abc import ABC, abstractmethod

# Interface: Defines a common contract for all payment methods
class PaymentMethod(ABC):
    @abstractmethod
    def process_payment(self, amount: float) -> bool:
        """Process the payment and return True if successful, False otherwise."""
        pass

    @abstractmethod
    def refund_payment(self, amount: float) -> bool:
        """Process a refund and return True if successful, False otherwise."""
        pass

# Concrete Class 1: Implements UPI Payment
class UPIPayment(PaymentMethod):
    def __init__(self, upi_id: str):
        self.upi_id = upi_id

    def process_payment(self, amount: float) -> bool:
        print(f"Processing UPI payment of ₹{amount} via {self.upi_id}")
        return True  # Simulating a successful payment

    def refund_payment(self, amount: float) -> bool:
        print(f"Refunding ₹{amount} to {self.upi_id}")
        return True  # Simulating a successful refund

# Concrete Class 2: Implements Credit Card Payment
class CreditCardPayment(PaymentMethod):
    def __init__(self, card_number: str, card_holder: str):
        self.card_number = card_number
        self.card_holder = card_holder

    def process_payment(self, amount: float) -> bool:
        print(f"Charging ₹{amount} to credit card {self.card_number} (Holder: {self.card_holder})")
        return True

    def refund_payment(self, amount: float) -> bool:
        print(f"Refunding ₹{amount} to credit card {self.card_number}")
        return True

# Concrete Class 3: Implements PayPal Payment
class PayPalPayment(PaymentMethod):
    def __init__(self, email: str):
        self.email = email

    def process_payment(self, amount: float) -> bool:
        print(f"Processing PayPal payment of ₹{amount} for {self.email}")
        return True

    def refund_payment(self, amount: float) -> bool:
        print(f"Refunding ₹{amount} to PayPal account {self.email}")
        return True

# Payment Processor: Uses the Interface to Accept Any Payment Type
class PaymentProcessor:
    def __init__(self, payment_method: PaymentMethod):
        self.payment_method = payment_method

    def make_payment(self, amount: float):
        if self.payment_method.process_payment(amount):
            print("Payment successful!")
        else:
            print("Payment failed!")

    def make_refund(self, amount: float):
        if self.payment_method.refund_payment(amount):
            print("Refund successful!")
        else:
            print("Refund failed!")

# -----------------
# ✅ Demonstrating the Usage
# -----------------

# Creating payment methods
upi_payment = UPIPayment("user@upi")
card_payment = CreditCardPayment("1234-5678-9876-5432", "John Doe")
paypal_payment = PayPalPayment("john@example.com")

# Processing payments via different methods
processor = PaymentProcessor(upi_payment)
processor.make_payment(1000.0)
processor.make_refund(500.0)

processor = PaymentProcessor(card_payment)
processor.make_payment(2500.0)
processor.make_refund(1000.0)

processor = PaymentProcessor(paypal_payment)
processor.make_payment(1500.0)
processor.make_refund(750.0)

```
