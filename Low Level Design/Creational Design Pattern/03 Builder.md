
## 🎯 Intent

Builder is a **creational design pattern** that lets you construct **complex objects step-by-step**. It allows different representations of the object using the **same construction process**.

---

## ❓ Problem

Constructors with many parameters become confusing:

```java
new Student("Alice", 42, true, "Bangalore", "Science", null, true);
```

❌ Difficult to read and understand. What does `null` or last `true` mean?

Also, with optional parameters, you'd need multiple constructors or telescoping constructors, which is hard to maintain.

---

## ✅ Solution

Use a **Builder** class that:

- Provides readable setter methods (like `.setName()`)
    
- Lets you build objects step-by-step
    
- Ends with a `.build()` method to return the final object
    

---

## 📑 Structure

|Component|Description|
|---|---|
|**Product**|The complex object being built (e.g., `Student`)|
|**Builder**|Provides methods to set properties and a `build()` method|
|**Director (Optional)**|Defines specific steps and sequences to build variations|

---

## 🚀 Applicability

Use Builder Pattern when:

### 🔹 Object has many optional or configurable parameters

> You don't want to write many constructors.

### 🔹 You want code that's easy to read and write

> `new StudentBuilder().setName("Alice").setRollNo(42).build()` is clearer.

### 🔹 Object creation needs to be controlled step-by-step

> Like validating inputs or setting only specific fields in sequence.

---

## 📝 Example without Director (Fluent Style)

```java
class Student {
    private String name;
    private int rollNo;
    private boolean fullTime;

    // constructor private to enforce use of Builder
    private Student(StudentBuilder builder) {
        this.name = builder.name;
        this.rollNo = builder.rollNo;
        this.fullTime = builder.fullTime;
    }

    public static class StudentBuilder {
        private String name;
        private int rollNo;
        private boolean fullTime;

        public StudentBuilder setName(String name) {
            this.name = name;
            return this;
        }

        public StudentBuilder setRollNo(int rollNo) {
            this.rollNo = rollNo;
            return this;
        }

        public StudentBuilder setFullTime(boolean fullTime) {
            this.fullTime = fullTime;
            return this;
        }

        public Student build() {
            return new Student(this);
        }
    }
}

// Usage
Student s = new Student.StudentBuilder()
    .setName("Alice")
    .setRollNo(42)
    .setFullTime(true)
    .build();
```

---

## 👨‍🎓 Analogy

Imagine you're filling a university form:

- Fill in each field one by one
    
- Submit the form once you're done = `build()`
    

---

## 🌟 Pros

### ✅ Improves readability

> No more long constructors.

### ✅ More maintainable and scalable

> Add/remove fields easily without constructor chaos.

### ✅ Enforces immutability (if you avoid setters in `Product`)

---

## ❌ Cons

### ❌ More boilerplate code (Builder class needed)

### ❌ Some learning curve for newcomers

---

Let me know if you'd like the **Director-based version**, or want an example using real-world objects like `StringBuilder` or `HttpClient.Builder`!