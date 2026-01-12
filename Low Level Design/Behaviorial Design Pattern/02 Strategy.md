**Strategy** is a behavioral design pattern that enables selecting an algorithm's behavior at runtime by encapsulating algorithms into separate classes and making them interchangeable.

---

## ❓ Problem

You have multiple algorithms for a specific task, and you want to:

- Avoid hardcoding them inside one class
    
- Switch between them dynamically at runtime
    
- Follow the Open/Closed principle (open to extension, closed to modification)
    

---

## ✅ Solution

Encapsulate each algorithm in a separate **Strategy** class that implements a common interface. The **Context** class uses this interface and delegates the algorithm execution to the current strategy object.

---

## 🎯 Applicability

Use Strategy Pattern when:

- You need different variants of an algorithm (e.g., sorting, payment, compression)
    
- You want to replace inheritance with composition for algorithm selection
    
- You want to isolate algorithm logic from the context
    
- You want to dynamically switch behavior at runtime
    

---

## 🧱 Structure

+-------------+
|   Context   |
+-------------+
       |
       v uses
+-----------------+       is-a         +---------------------+
|   Strategy      | <----------------- | ConcreteStrategyA   |
| (PaymentMethod) |                   | (CreditCardPayment) |
+-----------------+                   +---------------------+
                                      | ConcreteStrategyB   |
                                      | (PayPalPayment)     |
                                      +---------------------+


---

## 🔩 Component Mapping

|Component|Description|
|---|---|
|**Strategy**|Interface for algorithms (`PaymentMethod`)|
|**ConcreteStrategy**|Concrete implementation (`CreditCardPayment`, `PayPalPayment`)|
|**Context**|Uses a Strategy (`PaymentService`)|
|**Client**|Configures and uses context (`Main`)|

---

## 🛠 How to Implement

- Define a common interface for all algorithms
    
- Implement multiple strategy classes
    
- Context class uses a strategy object to perform the task
    
- Client sets or changes the strategy dynamically
    

---

## 🧠 Real-World Examples

✅ Java

- `java.util.Comparator` for sorting
    
- `javax.servlet.Filter` chain
    

✅ Libraries

- Spring Security: different authentication strategies
    
- Logging frameworks like SLF4J
    

---

## 🧠 Real-World Analogy

> You go to a travel website and choose between flights, trains, or buses. The booking system stays the same, but the route and method vary by strategy.

---

## 🧪 Testing Strategy

- Unit test each strategy implementation separately
    
- Mock or inject strategies in context to test behavior
    
- Integration test with different combinations

```Java
// 🎯 Strategy Interface
interface PaymentMethod {
    void pay(int amount);
}

// ✅ Concrete Strategy A
class CreditCardPayment implements PaymentMethod {
    public void pay(int amount) {
        System.out.println("Paid ₹" + amount + " using Credit Card");
    }
}

// ✅ Concrete Strategy B
class PayPalPayment implements PaymentMethod {
    public void pay(int amount) {
        System.out.println("Paid ₹" + amount + " using PayPal");
    }
}

// 🔁 Context
class PaymentService {
    private PaymentMethod method;

    public PaymentService(PaymentMethod method) {
        this.method = method;
    }

    public void setPaymentMethod(PaymentMethod method) {
        this.method = method;
    }

    public void makePayment(int amount) {
        method.pay(amount);
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        PaymentService service = new PaymentService(new CreditCardPayment());
        service.makePayment(1000); // Paid using Credit Card

        service.setPaymentMethod(new PayPalPayment());
        service.makePayment(500); // Paid using PayPal
    }
}

```
