# 🧱 Facade Design Pattern

**Facade** is a structural design pattern that provides a **simplified interface** to a complex subsystem. It hides the subsystem's complexity and exposes only the necessary parts to the client.

## ❓ Problem

Subsystems often involve multiple classes or APIs that clients must interact with directly. This can lead to:

- Tight coupling between client and subsystem  
- Complex, repetitive, and error-prone client code  
- Lack of a clear entry point for using the system  

## ✅ Solution

Introduce a **Facade** class that wraps the subsystem and provides a **simple interface** for the most common tasks. The client interacts with the Facade, not the subsystem directly.

## 🎯 Applicability

Use the Facade Pattern when:

- You want to **simplify usage** of a complex system  
- You want to **decouple clients** from subsystem details  
- You want to **organize code** with a clear entry point  
- You need to **layer your system** (e.g., UI → Service → Subsystem)  

## 🧱 Structure

           +-------------+
           |   Client    |
           +-------------+
                 |
                 v
           +-------------+
           |   Facade    |  <----- Simplified interface
           +-------------+
             /    |    \
            /     |     \
           v      v      v
+-----------+ +-----------+ +------------+
| Subsystem | | Subsystem | | Subsystem  |
|   Class A | |   Class B | |   Class C  |
+-----------+ +-----------+ +------------+


Component        | Description  
------------------|-------------  
**Facade**        | Simplified interface for clients  
**Subsystem(s)**  | Complex components hidden from the client  
**Client**        | Uses the Facade instead of dealing with subsystems directly  

## 🛠 How to Implement

- Identify frequently used subsystem operations  
- Design a Facade class that delegates to the subsystem classes  
- Ensure the client only talks to the Facade, not the subsystems  
           +-------------+
           |   Client    |
           +-------------+
                 |
                 v
           +-------------+
           | GPayFacade  |  <----- One-call simplified payment
           +-------------+
             /    |    \
            /     |     \
           v      v      v
+-------------+ +-------------+ +----------------+
| BankService | | UPIService  | |   SMSService   |
+-------------+ +-------------+ +----------------+

## 🧠 Real-World Examples

✅ Java

- `javax.faces.context.FacesContext` (simplifies JSF operations)  
- `java.util.logging.Logger` hides logging implementation complexity  

✅ Libraries

- Spring’s `JdbcTemplate` wraps JDBC logic  
- Hibernate’s `Session` hides database operations  

## 🧠 Real-World Analogy

> You interact with a bank teller (Facade) instead of directly talking to the database, cash vault, or security. The teller handles all internal operations for you.

## 🧪 Testing Strategy

- Unit test the Facade for business logic  
- Mock subsystems when testing the Facade  
- Integration test Facade with real subsystems if needed  

```java
// 🧩 Subsystem classes
class BankService {
    public void debit(String account, double amount) {
        System.out.println("Debiting ₹" + amount + " from " + account);
    }
}

class UPIService {
    public void validateUPI(String upiId) {
        System.out.println("Validating UPI ID: " + upiId);
    }
}

class SMSService {
    public void sendSMS(String number, String message) {
        System.out.println("Sending SMS to " + number + ": " + message);
    }
}

// 🎯 Facade class
class GPayFacade {
    private BankService bank = new BankService();
    private UPIService upi = new UPIService();
    private SMSService sms = new SMSService();

    public void sendMoney(String upiId, String phoneNumber, double amount) {
        upi.validateUPI(upiId);
        bank.debit("user_account", amount);
        sms.sendSMS(phoneNumber, "₹" + amount + " sent to " + upiId);
        System.out.println("✅ Payment successful via GPay");
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        GPayFacade gpay = new GPayFacade();
        gpay.sendMoney("bob@upi", "9876543210", 500.0);
    }
}
