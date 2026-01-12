### Scenario Setup

- **Function A**: This is a function that performs some computation or task.
- **Function B**: This function calls `A` once.
- **Function C**: This function calls `A` multiple times (making it a potential hot spot).
![[Screenshot 2024-10-15 at 12.28.38 PM.png]]
### Traditional Interpretation

1. **Execution Flow**:
    
    - When you run the program, the **interpreter** executes the bytecode line by line.
    - For every call to `A()` made by `B()` and `C()`, the interpreter translates the bytecode for `A()` into machine code each time it encounters that call.
2. **Performance**:
    
    - Each time `B` calls `A`, the interpreter translates the bytecode for `A` into machine code.
    - Each time `C` calls `A`, the interpreter does the same translation, resulting in redundant work.
    - If `A` is computationally expensive or contains significant logic, this repeated interpretation can lead to slower overall performance, especially since `C` calls `A` multiple times.

### JIT Compilation

1. **Execution Flow**:
    
    - When you run the program, the **JVM** starts interpreting the bytecode.
    - The JVM monitors the execution and identifies that `A()` is called by `B()` and, especially, multiple times by `C()`. The calls in `C()` make `A()` a **hot spot**.
2. **Just-In-Time Compilation**:
    
    - After observing that `C()` calls `A()` repeatedly, the JIT compiler kicks in.
    - It compiles the bytecode for `A()` into native machine code **just in time** before the first execution of `A()` within `C()`.
    - The JIT compiler caches this machine code, so subsequent calls to `A()` from `C()` (or even from `B()`) can directly execute the precompiled machine code.
3. **Performance**:
    
    - The first time `C()` calls `A()`, the JIT compiler compiles `A()` into machine code, which might take a bit longer than simply interpreting it.
    - However, subsequent calls to `A()` from `C()` (and also from `B()`) run much faster because they execute the cached machine code directly, significantly improving performance.

### Why JVM is not compiling whole bytcode to machine code ?

* If the JVM compiled the entire bytecode at once, it might waste resources compiling methods that are rarely called, leading to unnecessary overhead.
* JIT compilation compiles code as needed, which helps manage memory usage more effectively. The JVM can also discard compiled code that is no longer needed, freeing up resources.
* Compiling all bytecode at startup would lead to longer startup times for Java applications. By using JIT compilation, the JVM can start executing the application quickly, compiling code in the background as it runs.

A **compiler** is a program which converts a program from one level of language to another. Example conversion of C++ program into machine code. The java compiler converts high-level java code into bytecode (which is also a type of machine code).

An **interpreter** is a program which converts a program at one level to another programming language at the **same level.** Example conversion of Java program into C++

In Java, the Just In Time Code generator converts the bytecode into the native machine code which are at the same programming levels.

Hence, Java is both compiled as well as interpreted language.