
**Iterator** is a behavioral design pattern that provides a way to **access elements** of a collection **sequentially** without exposing its underlying structure.

---

## ❓ Problem

You want to:

- Traverse a complex data structure (like a custom collection)
    
- Avoid exposing internal details of that data structure
    
- Provide a uniform way to iterate over different types of collections
    

---

## ✅ Solution

Define an **Iterator interface** that provides methods like `hasNext()` and `next()`.  
Let collections return **their own iterator** that implements this interface.

---

## 🎯 Applicability

Use the Iterator Pattern when:

- You want to iterate over a collection without exposing its internals
    
- You need multiple or custom traversal strategies (forward, backward, filtering, etc.)
    
- You want a uniform interface to iterate over different collection types
    

---

## 🧱 Structure

+---------------+
|    Client     |
+---------------+
       |
       v
+----------------+       uses        +----------------------+
|   Aggregate    | <---------------- |      Iterator        |
| (StudentGroup) |                  | (StudentIterator)     |
+----------------+                  +----------------------+
       ▲                                      ▲
       |                                      |
+--------------------+            +---------------------------+
| ConcreteAggregate  |            |    ConcreteIterator       |
|  (MyStudentGroup)  |            |  (MyStudentGroupIterator) |
+--------------------+            +---------------------------+


---

## 🧱 Component Table

|Component|Description|
|---|---|
|**Iterator**|Defines traversal operations (`StudentIterator`)|
|**ConcreteIterator**|Implements iteration logic (`MyStudentGroupIterator`)|
|**Aggregate**|Defines a method to get an iterator (`StudentGroup`)|
|**ConcreteAggregate**|Implements the aggregate and returns its iterator (`MyStudentGroup`)|
|**Client**|Uses iterator to traverse collection (`Main`)|

---

## 🛠 How to Implement

- Create an `Iterator` interface with `hasNext()` and `next()`
    
- Create a `Collection` (aggregate) interface with `createIterator()`
    
- Implement both `ConcreteIterator` and `ConcreteCollection`
    
- Client uses iterator to access elements
    

---

## 🧠 Real-World Examples

✅ Java

- `java.util.Iterator` and `Iterable`
    
- `for-each` loop behind the scenes uses `Iterator`
    

✅ Libraries

- Cursor-style iteration in JDBC
    
- Custom iterable models in web frameworks
    

---

## 🧠 Real-World Analogy

> Think of a TV remote. You don’t need to know how the TV works internally—you just press "next" to change the channel. The remote is the iterator, and the TV is the collection.

---

## 🧪 Testing Strategy

- Unit test iterator logic for correct traversal
    
- Validate edge cases (empty collections, end of iteration)
    
- Test client usage with iterator interface only

```Java
import java.util.*;

// 🎯 Iterator Interface
interface StudentIterator {
    boolean hasNext();
    String next();
}

// 🎯 Aggregate Interface
interface StudentGroup {
    StudentIterator createIterator();
}

// ✅ Concrete Aggregate
class MyStudentGroup implements StudentGroup {
    private List<String> students = new ArrayList<>();

    public void addStudent(String name) {
        students.add(name);
    }

    public StudentIterator createIterator() {
        return new MyStudentGroupIterator(students);
    }
}

// ✅ Concrete Iterator
class MyStudentGroupIterator implements StudentIterator {
    private List<String> students;
    private int position = 0;

    public MyStudentGroupIterator(List<String> students) {
        this.students = students;
    }

    public boolean hasNext() {
        return position < students.size();
    }

    public String next() {
        return students.get(position++);
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        MyStudentGroup group = new MyStudentGroup();
        group.addStudent("Alice");
        group.addStudent("Bob");
        group.addStudent("Charlie");

        StudentIterator iterator = group.createIterator();

        while (iterator.hasNext()) {
            System.out.println("Student: " + iterator.next());
        }
    }
}

```