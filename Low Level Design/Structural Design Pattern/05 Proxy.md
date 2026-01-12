# 🛡️ Proxy Design Pattern

The **Proxy** is a **structural design pattern** that provides a surrogate or placeholder for another object to control access to it.

## ❓ Problem

Direct access to an object may be:

- **Expensive** (e.g., remote service, large object)
- **Unsafe** (e.g., restricted permissions)
- **Unnecessary** (e.g., object used only in certain cases)

We need a way to **delay, control, or enhance access** to the original object.

## ✅ Solution

Create a **Proxy class** that implements the same interface as the real object and **controls access** to it (e.g., lazy loading, logging, access control, caching).

## 🎯 Applicability

Use the Proxy Pattern when:

🔹 You want to add access control (authorization)  
🔹 You want to delay object creation (lazy loading)  
🔹 You want to add logging, caching, or auditing  
🔹 You want to work with remote or resource-heavy objects  
🔹 You want to protect a sensitive resource

## 🧱 Structure

Component         | Description  
------------------|-------------  
**Subject**        | Interface that both RealSubject and Proxy implement  
**RealSubject**    | The actual object that performs operations  
**Proxy**          | Controls access to the RealSubject  
**Client**         | Interacts with Subject (not aware if it's real or proxy)

## 🛠 How to Implement

1. Define a `Subject` interface with the required methods  
2. Implement `RealSubject` with actual behavior  
3. Implement `Proxy` that wraps `RealSubject` and adds logic (e.g., access check)  
4. Client uses the `Subject` interface  

---

## 🧠 Real-World Analogy

> Think of a **bank ATM card** as a proxy to your bank account. It controls and mediates access, verifies your PIN, limits actions, and logs operations — without giving direct access to the vault.

---

## 🧪 Java Example: YouTube Access Control Proxy

```java
// 🎯 Subject Interface
interface YouTubeVideo {
    void play(String userRole);
}

// ✅ RealSubject
class RealYouTubeVideo implements YouTubeVideo {
    private final String title;

    public RealYouTubeVideo(String title) {
        this.title = title;
        loadFromNetwork(); // expensive operation
    }

    private void loadFromNetwork() {
        System.out.println("Loading video: " + title + " from YouTube...");
    }

    public void play(String userRole) {
        System.out.println("Playing video: " + title);
    }
}

// 🛡️ Proxy
class YouTubeProxy implements YouTubeVideo {
    private final String title;
    private RealYouTubeVideo realVideo;

    public YouTubeProxy(String title) {
        this.title = title;
    }

    public void play(String userRole) {
        if (!userRole.equals("Premium")) {
            System.out.println("Access Denied. Upgrade to Premium to watch: " + title);
            return;
        }

        if (realVideo == null) {
            realVideo = new RealYouTubeVideo(title); // lazy loading
        }

        realVideo.play(userRole);
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        YouTubeVideo video1 = new YouTubeProxy("Design Patterns in Java");

        video1.play("Free");     // Access denied
        video1.play("Premium");  // Loads and plays video
        video1.play("Premium");  // Reuses cached video
    }
}
