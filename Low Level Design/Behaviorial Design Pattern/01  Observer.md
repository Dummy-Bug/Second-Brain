**Observer** is a behavioral design pattern that lets an object (subject) notify other objects (observers) about changes in its state, without knowing who or how many observers there are.

---

## ❓ Problem

You need to maintain consistency between related objects without tightly coupling them:

- One change should trigger updates in many dependent objects
    
- You want dynamic relationships between objects
    
- Manual updates across objects would be error-prone
    

---

## ✅ Solution

Introduce a **Subject** interface with methods to attach/detach observers and notify them. Observers implement a common interface and register themselves with the subject. When the subject changes, it **notifies all** observers.

---

## 🎯 Applicability

Use the Observer Pattern when:

- Many objects depend on another object’s state
    
- You want to decouple the Subject from its Observers
    
- You need runtime subscription/unsubscription
    
- You want a push model (subject tells observers of change)
    

---

## 🧱 Structure

|Component|Description|
|---|---|
|**Subject**|Maintains a list of observers and notifies them (`WeatherStation`)|
|**Observer**|Interface for receiving updates (`DisplayDevice`)|
|**ConcreteSubject**|Implements Subject and holds the state (`WeatherStation`)|
|**ConcreteObserver**|Implements Observer and reacts to updates (`PhoneDisplay`, `LEDPanel`)|
|**Client**|Registers observers and updates subject (`Main`)|

---

### 🧩 Class Diagram

+------------------+     notifies      +---------------------+
|    Subject       |-----------------> |      Observer        |
| (WeatherStation) |                   |   (DisplayDevice)    |
+------------------+                   +---------------------+
        ▲                                        ▲
        |                                        |
        |                                +----------------------+
        |                                |     PhoneDisplay     |
        |                                +----------------------+
        |                                |     LEDPanel         |
        |                                +----------------------+
        |
+------------------+
| ConcreteSubject  |
| (WeatherStation) |
+------------------+

- `WeatherStation` **has-a** list of `DisplayDevice`
    
- `PhoneDisplay` and `LEDPanel` **is-a** `DisplayDevice`    

---

## 🛠 How to Implement

- Create an `Observer` interface with `update()` method
    
- Create a `Subject` interface with methods to register, remove, and notify observers
    
- Implement the Subject and maintain the observer list
    
- Implement concrete observers and register them
    

---

## 🧠 Real-World Examples

✅ Java

- `java.util.Observer` and `Observable` (deprecated)
    
- Swing's Event Listener Model
    

✅ Libraries

- RxJava (Reactive Streams)
    
- EventBus (Guava, Spring)
    

---

## 🧠 Real-World Analogy

> Think of a **YouTube Channel** (Subject). Subscribers (Observers) are notified whenever a new video is posted. The channel doesn’t know _who_ the subscribers are — it just notifies all of them.

---

## 🧪 Testing Strategy

- Use mocks to test observer updates
    
- Verify subject calls all observer methods
    
- Add/remove observers dynamically and test behavior

```Java
import java.util.*;

// 🧩 Observer Interface
interface DisplayDevice {
    void update(float temperature);
}

// ✅ Concrete Observer A
class PhoneDisplay implements DisplayDevice {
    public void update(float temperature) {
        System.out.println("📱 Phone Display: Temp updated to " + temperature + "°C");
    }
}

// ✅ Concrete Observer B
class LEDPanel implements DisplayDevice {
    public void update(float temperature) {
        System.out.println("💡 LED Panel: Temp changed to " + temperature + "°C");
    }
}

// 🧩 Subject Interface
interface Subject {
    void addObserver(DisplayDevice observer);
    void removeObserver(DisplayDevice observer);
    void notifyObservers();
}

// ✅ Concrete Subject
class WeatherStation implements Subject {
    private List<DisplayDevice> observers = new ArrayList<>();
    private float temperature;

    public void addObserver(DisplayDevice observer) {
        observers.add(observer);
    }

    public void removeObserver(DisplayDevice observer) {
        observers.remove(observer);
    }

    public void setTemperature(float temperature) {
        this.temperature = temperature;
        notifyObservers(); // 🚨 Notify all observers
    }

    public void notifyObservers() {
        for (DisplayDevice observer : observers) {
            observer.update(temperature);
        }
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        WeatherStation station = new WeatherStation();

        DisplayDevice phone = new PhoneDisplay();
        DisplayDevice panel = new LEDPanel();

        station.addObserver(phone);
        station.addObserver(panel);

        station.setTemperature(26.5f);
        station.setTemperature(30.0f);
    }
}
```

**Output** 
📱 Phone Display: Temp updated to 26.5°C  
💡 LED Panel: Temp changed to 26.5°C  
📱 Phone Display: Temp updated to 30.0°C  
💡 LED Panel: Temp changed to 30.0°C