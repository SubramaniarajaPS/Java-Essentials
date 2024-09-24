# Memory Management

Memory management is the process of controlling and coordinating the way a software application access computer memory.

![image](https://github.com/user-attachments/assets/6ee10312-5911-4cb2-a2e1-7def80fc7ff9)

## Stack 
- *Static memory allocation.*
- *Last-In-First-Out.*
- The process of storing and retrieving data from the stack is very fast as there is no lookup required.
- Data that is stored on the stack has to be *finite and static* i.e. size of the data is known at *compile time.*
- Call Stack has many function stack frames within it.
- When a function call is made a *Stack Frames* is created within the *call stack*.
- Stack frames stores
  - Return address of the caller
  - Input params
  - Local Variables
- Multi-threaded applications can have a stack per thread.
- Memory management of the stack is simple and straightforward.
- Data that are stored on stack are *local variables(value types or primitives, primitive constants), pointers and function frames.*
- There is a limit on the size of value that can be stored on the Stack.
- JVM will throw *java.lang.StackOverFlowError* for stack.

## Heap
- *Dynamic memory allocation.*
- The program needs to look up the data in heap using pointers.
- It is slower than stack
- Data with dynamic size can be stored here.
- Heap is shared among threads of an application.
- Due to its dynamic nature heap is trickier to manage.
- Data that are stored on the heap are global variables, reference types like objects, strings, maps, and other complex data structures.
- JVM will throw *java.lang.OutOfMemoryError: Java Heap Space.*
- There is *no limit on the size* of the value that can be stored on the heap.
- Heap memory is used for *runtime operations.*

<img width="984" alt="image" src="https://github.com/user-attachments/assets/8e0477ad-e8c6-4f3c-8b9e-e563c4b7189c">

## Garbage Collection
- Garbage collection in Java is the process by which Java programs perform automatic memory management.
- When Java programs run on the JVM, objects are created on the heap, which is a portion of memory dedicated to the program.
- Eventually, some objects will no longer be needed. The garbage collector finds these unused objects and deletes them to free up memory.
- The main objective of Garbage Collector is to free heap memory by destroying *unreachable objects.*

![image](https://github.com/user-attachments/assets/bd348038-288a-42e6-a94b-5fa991d2482c)

```java
int a = 100;
// if a is in local varible, it will be stored in stack as a part of stack frame,
// else it will be stored in heap with its reference in stack.
Human h1 = new Human();
// h1 will be stored in stack as a reference varible pointing towards an object that is created in Heap.  
```

## Java String Pool
- A Java String Pool is a place in heap memory where all the strings defined in the program are stored.
- On standard assignment of a value to a string variable, the variable is allocated stack, while the value is stored in the heap in the string constant pool.

```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = "Class";
```
![image](https://github.com/user-attachments/assets/e4807464-03ce-4070-8bf2-8c314e94d269)


*To know more about memory management* Refer https://www.javatpoint.com/memory-management-in-java



