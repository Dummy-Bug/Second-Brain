
## 🎯 Intent

Abstract Factory is a **creational design pattern** that lets you produce families of related objects **without specifying their concrete classes**.

It is an extension of the **Factory Method Pattern** but instead of creating a single product, it creates a set of related products.

---

## Problem

Let’s say you want to integrate with multiple HRMS platforms like **Keka** and **Darwinbox**, but not just for employee data. You also need to fetch **attendance** and **payroll** data.

Initially, you might write something like this:

```java
if (hrms.equals("keka")) {
    new KekaEmployeeService();
    new KekaAttendanceService();
    new KekaPayrollService();
} else {
    new DarwinboxEmployeeService();
    new DarwinboxAttendanceService();
    new DarwinboxPayrollService();
}
```

Now imagine adding a new HRMS. You’ll have to change every such conditional across the codebase. This leads to **tight coupling**, violates **Open/Closed Principle**, and is hard to maintain.

---

## ✅ Solution

The **Abstract Factory** suggests defining an **HRMSFactory** interface with methods like:

- `getEmployeeService()`
    
- `getAttendanceService()`
    
- `getPayrollService()`
    

Then for each HRMS, create a concrete factory:

- `KekaHRMSFactory`
    
- `DarwinboxHRMSFactory`
    

This way, the client interacts only with the abstract factory and is agnostic to the concrete implementations.

---

## 🌟 Applicability

### 🔹 When you need to create families of related objects

> You want to create objects that are designed to be used together.

In our example: `KekaEmployeeService`, `KekaAttendanceService`, and `KekaPayrollService` form one family.

### 🔹 When your code needs to be decoupled from the object families

> Avoid hard-coding the logic of which object to instantiate.

### 🔹 When you expect to add new families without breaking existing code

> Adding a new HRMS like `GreytHR` requires only a new factory and implementation.

### 🔹 When you want to ensure consistency across related components

> You don’t want to mix services from different HRMS systems.

---

## 🧰 Structure

|Component|Description|
|---|---|
|**AbstractFactory**|Declares methods to create abstract products (services)|
|**ConcreteFactory**|Implements creation methods to return specific product variants|
|**AbstractProducts**|Interfaces like `EmployeeService`, `AttendanceService`, etc.|
|**ConcreteProducts**|Implementations like `KekaEmployeeService`, `DarwinboxPayrollService`, etc.|
|**Client**|Uses only interfaces, relies on the factory to provide dependencies|

---

## 🛠 How to Implement

1. Define common interfaces for each product (e.g. `EmployeeService`, `AttendanceService`, `PayrollService`).
    
2. Create abstract factory interface `HRMSFactory` with methods for each product.
    
3. Implement concrete factories for each vendor (`KekaHRMSFactory`, `DarwinboxHRMSFactory`).
    
4. In client code, use the abstract factory to create services.
    
5. Decide the concrete factory at runtime based on configuration/input.
    

---

## 🗂 Code Example: HRMS

### 1. Abstract Products

```java
public interface EmployeeService {
    void fetchEmployee(String empId);
}

public interface AttendanceService {
    void fetchAttendance(String empId);
}

public interface PayrollService {
    void fetchSalary(String empId);
}
```

### 2. Concrete Products

```java
public class KekaEmployeeService implements EmployeeService {
    public void fetchEmployee(String empId) {
        System.out.println("Keka employee: " + empId);
    }
}

public class DarwinboxAttendanceService implements AttendanceService {
    public void fetchAttendance(String empId) {
        System.out.println("Darwinbox attendance: " + empId);
    }
}
public class DarwinboxPayrollService implements PayrollService {
    public void fetchPayroll(String empId) {
        System.out.println("Darwinbox payroll: " + empId);
    }
}
// ... and so on for each HRMS and each interface
```

### 3. Abstract Factory

```java
public interface HRMSFactory {
    EmployeeService getEmployeeService();
    AttendanceService getAttendanceService();
    PayrollService getPayrollService();
}
```

### 4. Concrete Factories

```java
public class KekaHRMSFactory implements HRMSFactory {
    public EmployeeService getEmployeeService() {
        return new KekaEmployeeService();
    }
    public AttendanceService getAttendanceService() {
        return new KekaAttendanceService();
    }
    public PayrollService getPayrollService() {
        return new KekaPayrollService();
    }
}

public class DarwinboxHRMSFactory implements HRMSFactory {
    public EmployeeService getEmployeeService() {
        return new DarwinboxEmployeeService();
    }
    public AttendanceService getAttendanceService() {
        return new DarwinboxAttendanceService();
    }
    public PayrollService getPayrollService() {
        return new DarwinboxPayrollService();
    }
}
```

### 5. Client Code

```java
public class HRClient {
    private final HRMSFactory factory;

    public HRClient(HRMSFactory factory) {
        this.factory = factory;
    }

    public void process(String empId) {
        factory.getEmployeeService().fetchEmployee(empId);
        factory.getAttendanceService().fetchAttendance(empId);
        factory.getPayrollService().fetchSalary(empId);
    }
}
```

### 6. Choosing Concrete Factory at Runtime

```java
public class MainApp {
    public static void main(String[] args) {
        String hrmsName = "keka"; // this can come from config or input
        HRMSFactory factory;

        if ("keka".equalsIgnoreCase(hrmsName)) {
            factory = new KekaHRMSFactory();
        } else {
            factory = new DarwinboxHRMSFactory();
        }

        HRClient client = new HRClient(factory);
        client.process("EMP123");
    }
}
```

In the above, you're choosing a **concrete HRMSFactory** at runtime (e.g., `KekaHRMSFactory`). This determines which specific set of related services are used.

The client (`HRClient`) is **agnostic** to which factory is passed—it simply relies on the abstract interface.

---

## 📅 Pros and Cons

### 👍 Pros

- Groups related object creation in one place
    
- Enforces consistency among related products
    
- Easily extendable: just add new factories for new families (e.g., GreytHR)
    
- Decouples client code from concrete implementations
    

### 🔞 Cons

- Can become complex with too many product families
    
- Adding new products (e.g., `LeaveService`) requires changing all factories
    
- More boilerplate than simple factories
    
