# 🎨 Decorator Design Pattern

**Decorator** is a structural design pattern that lets you attach new behaviors to objects dynamically by placing them inside special wrapper objects (decorators) that implement the same interface.

## ❓ Problem

You want to add behavior to individual objects without:

- Modifying their code  
- Using inheritance (which can lead to class explosion)  
- Affecting other objects of the same class  

## ✅ Solution

Wrap the original object in a **decorator** that implements the same interface and adds new behavior **before or after** delegating calls to the original object.

## 🎯 Applicability

Use the Decorator Pattern when:

- You need to add responsibilities to individual objects dynamically  
- Inheritance isn’t flexible enough  
- You want to keep code open for extension but closed for modification  

## 🧱 Structure

        +-------------+
        |  Client     |
        +-------------+
              |
              v
     +-------------------+
     |   Component       | <----------------------------------+
     +-------------------+                                   |
              ▲                                               |
              |                                               |
  +-------------------+                          +------------------------+
  | ConcreteComponent |                          |     Decorator          |
  +-------------------+                          +------------------------+
                                                  ▲
                                                  |
                                 +----------------+------------------+
                                 |                                   |
                +------------------------+           +------------------------+
                | ConcreteDecoratorA     |           | ConcreteDecoratorB     |
                +------------------------+           +------------------------+


Component        | Description  
------------------|-------------  
**Component**      | Interface defining the core behavior  
**ConcreteComponent** | Basic implementation of the component  
**Decorator**      | Implements Component and wraps a component  
**ConcreteDecorator** | Adds behavior before/after delegating to wrapped object  

## 🛠 How to Implement

1. Define a common interface for components.  
2. Create a concrete implementation of the component.  
3. Create an abstract decorator that implements the interface and holds a reference to a component.  
4. Implement concrete decorators to add behavior.
        +-------------------------+
        |         Client          |
        +-------------------------+
                   |
                   v
        +-------------------------+
        |     Notification        | <----------------------------------+
        +-------------------------+                                   |
                   ▲                                                     |
                   |                                                     |
       +-------------------------+                         +-------------------------+
       |   BasicNotification     |                         |     NotificationDecorator|
       +-------------------------+                         +-------------------------+
                                                                   ▲
                                                                   |
                                      +----------------------------+---------------------------+
                                      |                                                    |
                    +----------------------------+                    +----------------------------+
                    |     SMSNotification         |                    |     EmailNotification       |
                    +----------------------------+                    +----------------------------+

## 🧠 Real-World Examples

✅ Java

- `java.io.BufferedReader`, `java.io.InputStreamReader`  
- `java.util.Collections#synchronizedList()`  

✅ Libraries

- UI frameworks (e.g., adding scrollbars, borders)  
- Logging and instrumentation  

## 🧠 Real-World Analogy

> Wrapping a gift box: You can add multiple layers (wrapping paper, bow, card) without changing the gift itself. Each layer adds something new.

## 🧪 Testing Strategy

- Use mocks to verify decorator calls  
- Test decorator behavior independently  
- Test integration of multiple decorators together  

```java
// 🎯 Component Interface
interface Notification {
    void send(String message);
}

// ✅ Concrete Component
class BasicNotification implements Notification {
    public void send(String message) {
        System.out.println("Sending message: " + message);
    }
}

// 🔁 Base Decorator
abstract class NotificationDecorator implements Notification {
    protected Notification wrappee;

    public NotificationDecorator(Notification wrappee) {
        this.wrappee = wrappee;
    }

    public void send(String message) {
        wrappee.send(message);
    }
}

// 🎀 Concrete Decorator: SMS
class SMSNotification extends NotificationDecorator {
    public SMSNotification(Notification wrappee) {
        super(wrappee);
    }

    public void send(String message) {
        super.send(message);
        System.out.println("Also sending SMS: " + message);
    }
}

// 🎀 Concrete Decorator: Email
class EmailNotification extends NotificationDecorator {
    public EmailNotification(Notification wrappee) {
        super(wrappee);
    }

    public void send(String message) {
        super.send(message);
        System.out.println("Also sending Email: " + message);
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        Notification notification = new EmailNotification(
                                        new SMSNotification(
                                            new BasicNotification()
                                        ));

        notification.send("Payment received");
    }
}
```

## ✅ Output

Sending message: Payment received   
Also sending SMS: Payment received   
Also sending Email: Payment received