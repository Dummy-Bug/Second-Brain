# 🌉 Bridge Design Pattern

**Bridge** is a **structural design pattern** that decouples an abstraction from its implementation so the two can vary independently.

## ❓ Problem

You often face a **combinatorial explosion** when:

- You want to combine multiple abstractions with multiple implementations  
- You end up with too many subclasses (e.g., `BlueCircle`, `RedRectangle`, `GreenTriangle`)  

Tightly coupling abstraction and implementation limits flexibility and makes code hard to extend.

## ✅ Solution

Split the hierarchy into two:

- **Abstraction** — the high-level interface
- **Implementation** — the platform-specific details

Bridge allows you to mix and match them without creating new subclasses for every combination.

## 🎯 Applicability

Use the Bridge Pattern when:

🔹 You want to avoid a **class explosion** caused by many variants  
🔹 You want to **decouple abstraction and implementation**  
🔹 You want to change implementation at runtime  
🔹 You want to **combine different platforms and logic** independently

## 🧱 Structure

Component             | Description
----------------------|------------
**Abstraction**        | Defines the high-level interface and holds a reference to the Implementor
**RefinedAbstraction** | Extends the abstraction and delegates work to the implementor
**Implementor**        | Interface for low-level implementation
**ConcreteImplementor**| Provides the actual implementation

---

## 🧠 Real-World Analogy

> A **remote control (abstraction)** can work with **different brands of TVs (implementations)**. You don’t want a new remote class for each TV.

---
Bridge Pattern:
[Abstraction] --> [Implementor]
   ▲                  ▲
[RefinedA]        [ConcreteImpl]

Strategy Pattern:
[Context] --> [Strategy]
                  ▲
           [ConcreteStrategyA/B]

## 🧪 Java Example: Message Sender Bridge

```java
// 🧱 Implementor
interface MessageSender {
    void send(String message);
}

// ✅ ConcreteImplementors
class EmailSender implements MessageSender {
    public void send(String message) {
        System.out.println("Sending Email: " + message);
    }
}

class SMSSender implements MessageSender {
    public void send(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

// 🎯 Abstraction
abstract class Message {
    protected MessageSender sender;

    public Message(MessageSender sender) {
        this.sender = sender;
    }

    public abstract void sendMessage(String content);
}

// 🧩 RefinedAbstraction
class TextMessage extends Message {
    public TextMessage(MessageSender sender) {
        super(sender);
    }

    public void sendMessage(String content) {
        sender.send("Text: " + content);
    }
}

class UrgentMessage extends Message {
    public UrgentMessage(MessageSender sender) {
        super(sender);
    }

    public void sendMessage(String content) {
        sender.send("URGENT: " + content.toUpperCase());
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        MessageSender email = new EmailSender();
        MessageSender sms = new SMSSender();

        Message msg1 = new TextMessage(email);
        msg1.sendMessage("Hello via Email");

        Message msg2 = new UrgentMessage(sms);
        msg2.sendMessage("Server down");
    }
}
```
```
Sending Email: Text: Hello via Email  
Sending SMS: URGENT: SERVER DOWN  

## 🧠 Real-World Examples

✅ Java & Frameworks

- JDBC bridges database operations to different drivers
    
- UI frameworks decouple views from rendering APIs
    
- Device abstraction + different platforms
    

✅ Tools

- Drawing APIs decouple shapes (Circle, Rectangle) from rendering engines (OpenGL, DirectX)
    
- Messaging apps (WhatsApp, Slack) use bridges to support SMS, Email, Push notifications


