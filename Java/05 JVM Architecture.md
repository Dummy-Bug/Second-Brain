![[Pasted image 20241015132136.png]]

### Class Loader 
The Class Loader is a subsystem of the Java Virtual Machine (JVM) that is responsible for loading class files into memory during the execution of a Java program. When a Java program is executed, the Class Loader reads the `.class` files (which contain the compiled bytecode) and loads the corresponding classes into the JVM.
![[Screenshot 2024-10-15 at 1.27.54 PM.png]]

### How the Class Loader Works

1. **Loading**:
    
    - When you run `java HelloWorld`, the Class Loader first looks for `HelloWorld.class`.
    - If found, it loads this class into the JVM.
2. **Linking**:
    
    - After loading `HelloWorld`, the Class Loader checks for any references to other classes, such as `DynamicClass`.
    - It loads `DynamicClass` only when it is referenced (lazy loading).
3. **Initialization**:
    
    - The `main` method in `HelloWorld` is executed, which prints "Hello, World!".
    - When `dynamicClass.display()` is called, `DynamicClass` is loaded and initialized, which results in printing "This is a dynamically loaded class!".