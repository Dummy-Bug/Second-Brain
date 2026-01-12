
## 🎯 Intent

Factory Method is a **creational design pattern** that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created.

---

## ❓ Problem

Imagine that you are building an **HR platform integration module** that can interact with various HRMS providers like **Keka** and **Darwinbox**. Initially, the app is built to support only Keka, so the business logic is tightly coupled with `KekaEmployeeService`.

However, over time, you need to support Darwinbox too. You now find your code filled with `if-else` or `switch` statements, checking which HRMS is selected and creating objects accordingly.

This leads to a violation of the **Open/Closed Principle** and makes the code rigid and hard to maintain.

---

## ✅ Solution

The **Factory Method Pattern** suggests that you replace direct object construction (`new`) with a **factory method**. You delegate object creation to a single place that decides what to instantiate.

By doing this:

- Your core logic doesn't care _which_ object it is using
    
- Adding new HRMS integrations becomes easier without modifying existing code
    

---

## 🎯 Applicability

Here are scenarios when you should use the **Factory Method Pattern**:

### 🔹 When you don’t know beforehand the exact types and dependencies

You might not know at compile-time which class you need. The factory pattern defers the decision to runtime, allowing selection based on inputs (like `hrmsName`).

> In our example: you don’t know whether the request is for Keka or Darwinbox until runtime.

### 🔹 When you want to separate product creation from usage

Separating creation logic allows the main business logic to be clean and unaware of object instantiation details.

> In our example: the main service or controller doesn't care _how_ the `EmployeeService` is built.

### 🔹 When you want to allow extending frameworks without modifying code

If users of your codebase or framework want to plug in new behavior (new HRMS vendor), they can simply extend the factory or register a new service without changing client code.

> You can just add a `GreytHRService` class and update the factory.

### 🔹 When object creation is expensive and you want to reuse/cache objects

You can use the factory to manage object caching or pooling (e.g., DB connections, HRMS clients).

> Imagine creating heavy network client objects—these can be managed and reused by the factory instead of being instantiated every time.

---

## 🧱 Structure

|Component|Description|
|---|---|
|**Product Interface**|Common interface implemented by all HRMS services (`EmployeeService`)|
|**Concrete Products**|Actual classes like `KekaEmployeeService`, `DarwinboxEmployeeService`|
|**Factory Class**|`EmployeeServiceFactory` to instantiate appropriate implementation|
|**Client**|Uses factory to get an instance and call logic, without knowing the implementation|

---

## 🛠 How to Implement

1. Define a common product interface.
    
2. Create an empty factory method in the creator class.
    
3. Replace object construction in the creator’s code with calls to the factory method.
    
4. Move construction logic from the main code to the factory method.
    
5. Create concrete subclasses that override the factory method.
    
6. (Optional) Pass arguments to the factory method to choose product types dynamically.
    

---

## 🧾 Code Example: Keka/Darwinbox

### 1. Product Interface

```java
public interface EmployeeService {
    void fetchEmployee(String empId);
}
```

### 2. Concrete Products

```java
public class KekaEmployeeService implements EmployeeService {
    public void fetchEmployee(String empId) {
        System.out.println("Fetching employee from Keka: " + empId);
    }
}

public class DarwinboxEmployeeService implements EmployeeService {
    public void fetchEmployee(String empId) {
        System.out.println("Fetching employee from Darwinbox: " + empId);
    }
}
```

### 3. Factory Class

```java
public class EmployeeServiceFactory {
    public static EmployeeService getService(String hrms) {
        switch (hrms.toLowerCase()) {
            case "keka": return new KekaEmployeeService();
            case "darwinbox": return new DarwinboxEmployeeService();
            default: throw new IllegalArgumentException("Unknown HRMS: " + hrms);
        }
    }
}
```

### 4. Client Code

```java
public class Main {
    public static void main(String[] args) {
        EmployeeService service = EmployeeServiceFactory.getService("keka");
        service.fetchEmployee("E123");
    }
}
```

---

## 👍 Pros (with examples)

### ✅ Avoids tight coupling between client and concrete products

The client code only interacts with the `EmployeeService` interface and doesn't care which concrete implementation it uses.

> Example: The controller/service layer just asks the factory for a service and calls `fetchEmployee()`. Whether it talks to **Keka** or **Darwinbox** is hidden from it.

---

### ✅ Single Responsibility: centralizes object creation

All logic for choosing and instantiating `EmployeeService` implementations is placed in one factory class.

> Example: You don’t have HRMS-specific logic spread across your entire codebase. It’s all in `EmployeeServiceFactory`.

---

### ✅ Open/Closed Principle: easy to extend

You can introduce new HRMS integrations like `GreytHREmployeeService` by just adding a new class and a `case` in the factory. No need to modify the client code.

> Example: Add "greythr" case in the factory without changing any logic in service/controller layers.

---

## 👎 Cons (with examples)

### ❌ Can lead to a lot of subclasses

For each type of product (in this case, HRMS integration), you need a new class. This can make your codebase grow rapidly.

> Example: `KekaEmployeeService`, `DarwinboxEmployeeService`, `GreytHRService`, etc. Each HRMS requires a separate concrete class.

---

### ❌ More complex than using `new` directly

If you only have one or two HRMS types, this may feel over-engineered compared to just using `new KekaEmployeeService()` directly.

> Example: If the system will **only ever** use Keka, the factory might be unnecessary abstraction.

---

Let me know if you'd like to add **Abstract Factory** using the same HRMS example!