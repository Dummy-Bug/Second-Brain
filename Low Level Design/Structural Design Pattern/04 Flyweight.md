# 🪶 Flyweight Design Pattern

The **Flyweight** is a structural design pattern that enables efficient memory usage by **sharing common parts of state** between multiple objects instead of duplicating them.

## ❓ Problem

When working with a large number of similar objects:

- Memory usage grows unnecessarily  
- Performance degrades  
- Common data is duplicated  

## ✅ Solution

Separate object state into:

- **Intrinsic state** (shared, stored inside the flyweight)  
- **Extrinsic state** (contextual, passed by client when needed)

Use a **Flyweight Factory** to share instances with the same intrinsic state.

## 🎯 Applicability

Use Flyweight when:

- You have a large number of objects  
- Objects share common, immutable data  
- You want to reduce memory footprint  

## 🧱 Structure

Component         | Description  
------------------|-------------  
**Flyweight**      | Interface or abstract class for shared object  
**ConcreteFlyweight** | Stores intrinsic state  
**FlyweightFactory** | Manages and reuses shared flyweights  
**Client**         | Supplies extrinsic state and uses flyweights  

## 🛠 How to Implement

1. Identify intrinsic and extrinsic parts of the object  
2. Store intrinsic state in Flyweight object  
3. Pass extrinsic state from client code  
4. Use factory to cache and manage flyweights  

## 🧠 Real-World Analogy

> A document editor: Each character (like 'a', 'b') is stored once, but font, position, and formatting are passed from outside.

---

## 🧪 Java Example: Robot Flyweight

```java
import java.util.*;

// 🎯 Flyweight Interface
interface Robot {
    void display(String color, int x, int y); // extrinsic
}

// 🪶 ConcreteFlyweight
class RobotType implements Robot {
    private final String type;   // intrinsic
    private final String shape;  // intrinsic

    public RobotType(String type, String shape) {
        this.type = type;
        this.shape = shape;
    }

    @Override
    public void display(String color, int x, int y) {
        System.out.println("Displaying " + type + " robot with shape " + shape +
            " in color " + color + " at (" + x + ", " + y + ")");
    }
}

// 🏭 Flyweight Factory
class RobotFactory {
    private static final Map<String, Robot> robotMap = new HashMap<>();

    public static Robot getRobot(String type) {
        if (!robotMap.containsKey(type)) {
            if (type.equals("Humanoid"))
                robotMap.put(type, new RobotType("Humanoid", "RoundHead"));
            else if (type.equals("Dog"))
                robotMap.put(type, new RobotType("Dog", "Quadruped"));
            System.out.println("Created new robot of type: " + type);
        }
        return robotMap.get(type);
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        Robot humanoid = RobotFactory.getRobot("Humanoid");
        humanoid.display("Red", 10, 20);

        Robot humanoid2 = RobotFactory.getRobot("Humanoid");
        humanoid2.display("Blue", 15, 25);

        Robot dog = RobotFactory.getRobot("Dog");
        dog.display("Yellow", 5, 10);
    }
}
