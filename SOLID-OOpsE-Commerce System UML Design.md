## **Class Definitions and Relationships**

### **1. User** *(Base Class)*
- `id: str`
- `name: str`
- `email: str`
- `password: str`
- `is_authenticated: bool`
- **Methods:**
  - `verify_password(password) -> bool`
  - `update_details(name, email) -> None`

### **2. AuthService**
- **Methods:**
  - `hash_password(password: str) -> str`
  - `verify_password(hashed_password: str, input_password: str) -> bool`
  - `register(username: str, password: str, email: str) -> str`
  - `login(username: str, password: str) -> str`

### **3. Product** *(Base Class)*
- `id: str`
- `name: str`
- `price: float`
- `stock: int`
- `category: Optional[str]`
- `description: Optional[str]`
- `discount: float`
- **Methods:**
  - `is_available() -> bool`
  - `apply_discount(discount_percentage: float) -> None`
  - `get_final_price() -> float`

### **4. PhysicalProduct** *(Inherits from Product)*
- `weight: float`
- `dimensions: str`

### **5. DigitalProduct** *(Inherits from Product)*
- `file_size: float`
- `download_link: str`

### **6. ProductService**
- **Methods:**
  - `add_product(product: Product) -> bool`
  - `get_product_by_id(product_id: str) -> Optional[Product]`

### **7. Cart** *(Base Class)*
- `id: str`
- `user: User`
- `items: List[Tuple[Product, int]]`
- **Methods:**
  - `add_product(product: Product, quantity: int) -> None`
  - `remove_product(product: Product) -> None`
  - `get_total() -> float`

### **8. CartService**
- **Methods:**
  - `checkout(cart: Cart) -> Order`

### **9. Order** *(Base Class)*
- `id: str`
- `user: User`
- `cart: Cart`
- `total_price: float`
- `status: str`
- **Methods:**
  - `process_order() -> None`
  - `update_status(new_status: str) -> None`

### **10. OrderService**
- **Methods:**
  - `place_order(cart: Cart) -> Order`
  - `cancel_order(order: Order) -> None`

### **11. Notification** *(Abstract Class & Interface Implemented by Email & SMS)*
- **Methods:**
  - `send(recipient: str, message: str) -> None`

### **12. EmailNotification** *(Inherits Notification)*
- **Methods:**
  - `send(email: str, message: str) -> None`

### **13. SMSNotification** *(Inherits Notification)*
- **Methods:**
  - `send(phone: str, message: str) -> None`

### **14. Payment** *(Abstract Class & Interface Implemented by UPI and Card Payment)*
- **Methods:**
  - `process() -> bool`

### **15. UPIPayment** *(Inherits Payment)*
- **Methods:**
  - `process() -> bool`

### **16. CardPayment** *(Inherits Payment)*
- **Methods:**
  - `process() -> bool`

---

## **Key Inheritance & Implementation Relationships**
| **Base Class** | **Inherited By** |
|--------------|----------------|
| `Product` | `PhysicalProduct`, `DigitalProduct` |
| `Notification` | `EmailNotification`, `SMSNotification` |
| `Payment` | `UPIPayment`, `CardPayment` |
| `Order` | `CartService` Uses Order |
| `Cart` | `CartService` Uses Cart |

---

## **Final Benefits of This Approach**
| Principle | Benefit |
|-----------|---------|
| ✅ **SRP** | Each class has a single responsibility. |
| ✅ **OCP** | New product types, notifications, and payments can be added without modifying existing classes. |
| ✅ **LSP** | All subclasses replace the base class without breaking functionality. |
| ✅ **ISP** | Interfaces ensure classes only implement required methods. |
| ✅ **DIP** | High-level modules depend on abstractions, not concrete implementations. |

---
