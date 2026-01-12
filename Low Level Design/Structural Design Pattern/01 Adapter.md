# 🧩 Adapter Design Pattern

Adapter is a structural design pattern that allows objects with incompatible interfaces to work together by converting one interface into another the client expects.

## ❓ Problem
Sometimes you want to reuse an existing class, but its interface doesn’t match the one you need:

- Legacy or third-party APIs with incompatible interfaces  
- Rewriting existing code isn’t feasible  
- Want to reuse code without modifying it  

## ✅ Solution
Create an Adapter class that implements the interface expected by the client and wraps the original (incompatible) class. The Adapter translates method calls from the client into the appropriate calls to the wrapped class.

## 🎯 Applicability
Use the Adapter Pattern when:

🔹 You want to reuse an existing class with an incompatible interface  
🔹 You want to integrate legacy or third-party components  
🔹 You need to decouple the client from the implementation  
🔹 You want different APIs to work under a common interface  

## 🧱 Structure

  +----------------+       adapts to      +----------------+
  |    Target      | <------------------- |    Adapter     |
  | (interface)    |                      | (implements    |
  +----------------+                      |  Target, wraps |
         ▲                                |  Adaptee)      |
         |                                +----------------+
  +----------------+                               ▲
  |  ClientClass   |                               |
  +----------------+                       +----------------+
                                           |    Adaptee      |
                                           | (incompatible   |
                                           |  interface)     |
                                           +----------------+


| Component   | Description                                                   |     |
| ----------- | ------------------------------------------------------------- | --- |
| **Target**  | Interface expected by the client (`Student`)                  |     |
| **Adaptee** | Existing class with a different interface (`SchoolStudent`)   |     |
| **Adapter** | Implements Target and adapts Adaptee (`SchoolStudentAdapter`) |     |
| **Client**  | Uses Target without knowing about the Adaptee (`Main`)        |     |
✅ **Concrete Example: Student Adapter**

  +----------------+       adapts to      +-------------------------+
  |   Student      | <------------------- | SchoolStudentAdapter    |
  | (Target)       |                      | (implements Student,    |
  +----------------+                      |  wraps SchoolStudent)   |
         ▲                                           ▲
         |                                           |
  +----------------+                         +---------------------+
  | CollegeStudent |                         |   SchoolStudent     |
  | (compatible)   |                         | (Adaptee)           |
  +----------------+                         +---------------------+

## 🛠 How to Implement
- Define the Target interface that the client uses.  
- Create an Adapter class that implements the Target interface.  
- Inside the Adapter, hold a reference to an Adaptee object.  
- In the Adapter's methods, call the Adaptee's methods and adapt the inputs/outputs as necessary.

## 🧠 Real-World Examples

✅ Java  
- `java.util.Arrays.asList()` adapts arrays to List  
- `InputStreamReader` adapts byte streams to character streams  

✅ Libraries  
- SLF4J bridges to Log4j or Logback  
- GUI frameworks adapting low-level events to high-level handlers  

## 🧠 Real-World Analogy

> A power plug adapter allows a device with a US plug to be used in a European socket.  
> It doesn’t change the electricity, only the interface.

## 🧪 Testing Strategy

- Test via the Target interface to ensure expected behavior  
- Validate that Adapter correctly translates calls to Adaptee  
- Use integration tests to verify end-to-end compatibility  

## 🧾 Java Example

```java
import java.util.*;

// 🎯 Target Interface
interface Student {
    String getName();
}

// ✅ Compatible Class
class CollegeStudent implements Student {
    private String name;
    public CollegeStudent(String name) { this.name = name; }
    public String getName() { return name; }
}

// ❌ Incompatible Class
class SchoolStudent {
    private String fullName;
    public SchoolStudent(String fullName) { this.fullName = fullName; }
    public String getFullName() { return fullName; }
}

// 🔌 Adapter
class SchoolStudentAdapter implements Student {
    private SchoolStudent schoolStudent;
    public SchoolStudentAdapter(SchoolStudent schoolStudent) {
        this.schoolStudent = schoolStudent;
    }
    public String getName() {
        return schoolStudent.getFullName();
    }
}

// 🔁 Service Layer
class StudentService {
    public List<Student> getAllStudents() {
        List<Student> result = new ArrayList<>();

        // College students
        result.add(new CollegeStudent("Alice"));
        result.add(new CollegeStudent("Charlie"));

        // School students (adapted)
        List<SchoolStudent> schoolStudents = List.of(
            new SchoolStudent("Bob"),
            new SchoolStudent("Daisy")
        );
        for (SchoolStudent s : schoolStudents) {
            result.add(new SchoolStudentAdapter(s));
        }

        return result;
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        StudentService service = new StudentService();
        List<Student> students = service.getAllStudents();

        for (Student student : students) {
            System.out.println("Student Name: " + student.getName());
        }
    }
}
