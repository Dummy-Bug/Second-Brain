**Chain of Responsibility** is a behavioral design pattern that allows a request to pass through a **chain of handlers**. Each handler decides either to process the request or to pass it to the next handler in the chain.

---

## ❓ Problem

You want to:

- Avoid coupling the sender of a request to its receiver
    
- Allow multiple objects the opportunity to handle a request
    
- Process requests in a dynamic and flexible order
    

---

## ✅ Solution

Chain handlers together. Each handler decides whether to **handle the request** or **pass it along**. The client sends the request to the first handler.

---

## 🎯 Applicability

Use the Chain of Responsibility pattern when:

- You have multiple objects that can handle a request
    
- You want to **decouple request senders from receivers**
    
- You want to add/remove handlers dynamically at runtime
    
- You need to process requests in **stages** or **tiers**
    

---

## 🧱 Structure (General)

pgsql


+---------+
| Client  |
+---------+
     |
     v
+-----------------+     setNext()     +-----------------+
|  Handler        |----------------->|  Handler        |
| (Abstract)      |                  | (ConcreteLogger)|
+-----------------+                  +-----------------+
     ^                                      ^
     |                                      |
+---------------+                +------------------+
| ConcreteLogger|                | ConcreteLogger   |
| (DebugLogger) |                | (ErrorLogger)    |
+---------------+                +------------------+


---

## 🧱 Component Table

|Component|Description|
|---|---|
|**Handler**|Defines method to set next handler and handle request|
|**ConcreteHandler**|Processes request or delegates to next (`ErrorLogger`, etc.)|
|**Client**|Sends request to the first handler in the chain|

---

## 🛠 How to Implement

- Define an abstract `Logger` class with a `setNext()` and `log()` method
    
- Implement concrete loggers for different log levels
    
- Set up a chain of loggers in the desired order
    
- Client logs a message through the first logger
    

---

## 🧠 Real-World Examples

✅ Java

- `javax.servlet.Filter` chain
    
- Exception handling with `try-catch` blocks
    

✅ Libraries

- Logging frameworks like SLF4J, Logback
    
- Event pipelines and middlewares
    

---

## 🧠 Real-World Analogy

> Think of customer support: the call starts at Level 1 support. If they can’t solve it, they escalate to Level 2, and so on. Each level decides whether to handle or escalate the issue.

---

## 🧪 Testing Strategy

- Test each handler independently
    
- Test the full chain by sending various requests
    
- Mock or stub next handlers in unit tests