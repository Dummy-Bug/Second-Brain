# 🌲 Composite Design Pattern

The **Composite** pattern is a **structural design pattern** that allows you to treat individual objects and compositions of objects uniformly, particularly useful in scenarios involving tree-like structures.

## ❓ Problem

When working with hierarchical structures, you often need to treat both **individual objects** and **groups of objects** the same way. Specifically, this becomes challenging when you're dealing with leaf nodes (files) and composite nodes (directories).

### Example Problems:

- Managing a **file system** where directories contain files and other directories.
- Performing uniform operations (like listing or deleting) on both files (leaf nodes) and directories (composite nodes).

## ✅ Solution

With the **Composite pattern**, we:

- Define a **Component** interface for common operations.
- Implement **Leaf** classes for individual objects (like files).
- Implement **Composite** classes for groups of objects (like directories).
- Treat both leaf and composite objects uniformly using the same interface.

## 🎯 Applicability

Use the Composite Pattern when:

🔹 You need to work with **tree structures** of objects (files and directories).  
🔹 You want to **treat files and directories the same way** (e.g., perform actions like delete, list).  
🔹 You want to avoid complex conditional checks for leaf vs composite.

## 🧱 Structure

Component            | Description  
---------------------|-------------  
**Component**         | Interface or abstract class defining common operations for leaf and composite nodes (e.g., `getSize`, `delete`)  
**Leaf**              | Represents individual objects in the structure (e.g., files)  
**Composite**         | Represents groups of objects (e.g., directories that can contain files or other directories)  
**Client**            | Interacts with the component interface and doesn't need to know whether it's dealing with a leaf or composite  

---

## 🧠 Real-World Analogy

> Think of a **file system**:  
> - **Files** are **leaf** nodes because they cannot contain other files or directories.  
> - **Directories** are **composite** nodes because they can contain files or other directories.  
> You can **perform operations like delete, move, or list files** on both files and directories uniformly.

---

## 🧪 Java Example: File System

```java
import java.util.ArrayList;
import java.util.List;

// 🎯 Component Interface
interface FileSystemComponent {
    void display(String indent);
    int getSize(); // For calculating the size of files/directories
}

// ✅ Leaf Class: File
class File implements FileSystemComponent {
    private String name;
    private int size;

    public File(String name, int size) {
        this.name = name;
        this.size = size;
    }

    public void display(String indent) {
        System.out.println(indent + "File: " + name);
    }

    public int getSize() {
        return size;
    }
}

// ✅ Composite Class: Directory
class Directory implements FileSystemComponent {
    private String name;
    private List<FileSystemComponent> components = new ArrayList<>();

    public Directory(String name) {
        this.name = name;
    }

    public void add(FileSystemComponent component) {
        components.add(component);
    }

    public void remove(FileSystemComponent component) {
        components.remove(component);
    }

    public void display(String indent) {
        System.out.println(indent + "Directory: " + name);
        for (FileSystemComponent component : components) {
            component.display(indent + "  ");
        }
    }

    public int getSize() {
        int size = 0;
        for (FileSystemComponent component : components) {
            size += component.getSize();
        }
        return size;
    }
}

// 🚀 Client
public class Main {
    public static void main(String[] args) {
        // Create individual files
        File file1 = new File("File1.txt", 100);
        File file2 = new File("File2.txt", 200);

        // Create a directory and add files to it
        Directory dir1 = new Directory("Documents");
        dir1.add(file1);
        dir1.add(file2);

        // Create another directory and add a directory inside
        Directory dir2 = new Directory("Root");
        dir2.add(dir1);
        dir2.add(new File("File3.txt", 300));

        // Display the file system structure
        dir2.display("");

        // Calculate total size of directory
        System.out.println("Total size: " + dir2.getSize() + " KB");
    }
}
OUTPUT :- 
Directory: Root
  Directory: Documents
    File: File1.txt
    File: File2.txt
  File: File3.txt
Total size: 600 KB

```
## 🧠 Real-World Examples

✅ File System

- **Files** (leaf nodes) and **Directories** (composite nodes) are treated uniformly for operations like listing contents, calculating total size, and deleting.
    

✅ GUI Frameworks

- **Buttons, Labels** (leaf nodes) and **Panels, Windows** (composite nodes) can be treated uniformly in UI layouts.
