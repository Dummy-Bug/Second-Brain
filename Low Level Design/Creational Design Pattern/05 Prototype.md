## 🎯 Intent

Prototype is a **creational design pattern** that allows you to **clone existing objects** instead of creating new ones from scratch, especially when object creation is expensive or complex.

---

## ❓ Problem

Creating a new object via constructor can be:

- Resource-intensive (e.g., loading data from DB, I/O, complex computations)
    
- Redundant when you need many similar objects with only small differences
    
- Rigid if you have to create many subclasses for slightly different configurations
    

---

## ✅ Solution

Use an existing object (the **prototype**) as a template. Call its `clone()` method to make **copies** of it, optionally modifying fields after cloning.

---

## 🎯 Applicability

Use the Prototype Pattern when:

### 🔹 Object creation is **expensive**

> e.g., creating a report or form with lots of pre-filled defaults

### 🔹 You need many **copies of a similar object**

> Document templates, UI components, onboarding forms, etc.

### 🔹 You want to **decouple instantiation from usage**

> The client doesn't need to know the concrete class or constructor

### 🔹 You want to **avoid subclassing**

> Customize a few fields on a cloned object instead

---

## 🧱 Structure

|Component|Description|
|---|---|
|**Prototype**|Interface that declares the `clone()` method|
|**ConcretePrototype**|Implements the `clone()` logic (deep or shallow)|
|**Client**|Clones objects using the `clone()` method instead of `new`|

---

## 🛠 How to Implement

1. Define a `Prototype` interface with a `clone()` method.
    
2. Implement this interface in classes you want to clone.
    
3. In the client, call `.clone()` instead of using `new`.
    

---

## 🧾 Code Example: Employee Form Clone

java

CopyEdit

`public interface FormPrototype {     FormPrototype clone(); }  public class EmployeeForm implements FormPrototype {     private String name;     private String department;      public EmployeeForm(String name, String department) {         this.name = name;         this.department = department;     }      public FormPrototype clone() {         return new EmployeeForm(name, department);     }      public void setDepartment(String dept) {         this.department = dept;     }      public String toString() {         return name + " from " + department;     } }`

### Usage:

java

CopyEdit

`public class Main {     public static void main(String[] args) {         EmployeeForm original = new EmployeeForm("John", "Engineering");          EmployeeForm copy = (EmployeeForm) original.clone();         copy.setDepartment("HR");          System.out.println(original);  // John from Engineering         System.out.println(copy);      // John from HR     } }`

---

## 🧠 Real-World Examples

### ✅ Java

- `Object.clone()` (from `java.lang.Object`)
    
- `Cloneable` interface (marker interface to allow cloning)
    

### ✅ Libraries

- Game object templates
    
- UI framework components (duplicating panels, buttons, etc.)
    
- Workflow engines (duplicating default process configurations)