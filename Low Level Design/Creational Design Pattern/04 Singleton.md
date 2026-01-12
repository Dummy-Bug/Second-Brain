# 🧩 Singleton Design Pattern - Java

## 🎯 Intent

Singleton is a **creational design pattern** that ensures a **class has only one instance** and provides a **global point of access** to it.

---

## ❓ Problem

You need exactly **one instance** of a class — like a **configuration manager**, **logger**, or **database connection pool**.

Using this:
```java
Logger log1 = new Logger();
Logger log2 = new Logger();
```

❌ Multiple instances get created, which can lead to inconsistent state, performance issues, or wasted resources.

---

## ✅ Solution

Use a class that:
- Has a **private constructor**
- Stores its only instance in a **static field**
- Provides a **public static method** to access the instance

---

## 📑 Structure

| Component | Description |
|----------|-------------|
| **Singleton** | The class that manages its only instance |
| **Private Constructor** | Prevents external instantiation |
| **Static Instance** | Holds the single instance |
| **Public Accessor** | Returns the instance on demand |

---

## 🚀 Applicability

Use Singleton Pattern when:

### 🔹 You need **exactly one instance** of a class  
> e.g., a logging system, app configuration manager

### 🔹 You want **global access** to that instance  
> Without exposing class internals or lifecycle

### 🔹 You want to **control resource usage**  
> e.g., only one database connection pool in memory

---

## 👨‍🎓 Analogy

Imagine a **printer spooler** in an office:

- Only one should exist to avoid conflicting jobs
- Everyone accesses the **same** spooler instance
- Creating a new one for each user would be chaos

---

## 🌟 Pros

### ✅ Controlled access to the sole instance  
> No unexpected duplicates

### ✅ Reduces memory overhead  
> Especially for shared resources like configs, loggers, etc.

### ✅ Can be extended to support lazy loading or configuration

---

## ❌ Cons

### ❌ Difficult to test  
> Hard to mock or inject in unit tests

### ❌ Hidden dependencies  
> May encourage global state and tight coupling

### ❌ Not suitable for every use case  
> Overuse leads to **anti-patterns**

---

## 📝 Code Examples

### 1. 🔧 Basic Singleton (Not Thread-Safe)
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

---

### 2. 🧵 Thread-Safe (Synchronized Method)
```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

---

### 3. ⚡ Double-Checked Locking (Efficient & Thread-Safe)
```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

---

### 4. 🧊 Eager Initialization
```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

---

### 5. 📦 Static Block Initialization
```java
public class Singleton {
    private static final Singleton instance;

    static {
        instance = new Singleton();
    }

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

---

### 6. 🦺 Enum Singleton (Best Practice)
```java
public enum Singleton {
    INSTANCE;

    public void doSomething() {
        System.out.println("Doing work!");
    }
}
```

---

## 📝 Best Practice

> Use **Enum Singleton** for most cases — it's simple, safe, and covers edge cases like reflection and serialization.
