The JVM divides its memory into several distinct areas, each serving a different purpose. Here’s an overview of the key memory areas:
### JVM Memory Structure

**Method Area**:

- **Purpose**: Stores class structures such as the class's metadata, static variables, and constants.
- **Example**: When you define static variables or methods in a class, they reside in the Method Area.

**Heap Area**:

- **Purpose**: The runtime data area where objects are allocated. It is shared among all threads.
- **Example**: When you create an object using `new`, it is allocated in the Heap.However the reference to this object is stored in stack area.

**JVM Language Stacks**

- Each thread in a Java application has its own **JVM Language Stack**.
- The stack is a last-in, first-out (LIFO) structure used to manage method calls and local variables.
- When a method is called, a new stack frame is created on the top of the stack. This frame contains all the local variables and the execution context of the method.

**Key Components:**

- **Stack Frame**: Each method call creates a new stack frame that holds:
    - **Local Variables**: Variables defined inside the method.
    - **Return Address**: The point in the code to return to after the method execution is complete.
    - **Intermediate Results**: Temporary data that may be used during the method execution.
![[Screenshot 2024-10-15 at 1.44.46 PM.png]]**How It Works:

1. When `main()` is called, a frame is pushed onto the stack.
2. Inside `firstMethod()`, a new frame is created, and `a` is stored in that frame.
3. When `secondMethod(value)` is called, another frame is created with `value` and `b` as local variables.
4. After execution of `secondMethod()`, its frame is popped from the stack, returning control to `firstMethod()`.

### PC (Program Counter) Registers

#### Overview:

- Each thread has its own **Program Counter (PC) Register**.
- The PC register keeps track of the address of the current instruction being executed by the thread.
- When a method is invoked, the PC register stores the address of the instruction that will be executed next.
- The PC register is crucial for context switching between threads. When a thread is paused, the PC register helps to remember where it left off.

### Native Method Stacks

#### Overview:

- **Native Method Stacks** are used for managing native methods (methods implemented in languages like C or C++).
- Similar to JVM Language Stacks, each thread has its own native stack that maintains the execution of native methods.

#### Functionality:

- Native methods can be called through the Java Native Interface (JNI), allowing Java code to interact with code written in other languages.
- When a native method is invoked, a frame is added to the native method stack to manage local variables and the execution context.