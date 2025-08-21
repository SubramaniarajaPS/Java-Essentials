### Introduction to Java

Java is a popular, high-level, object-oriented programming language that was originally developed by Sun Microsystems and released in 1995. It is currently owned by Oracle, and billions of devices run Java. Java runs on various platforms, including Windows, Mac OS, and different versions of UNIX.

Key features and principles of Java include:

- Object-Oriented Programming (OOP): Java is fundamentally an object-oriented language, where nearly everything is considered an object. It supports core OOP principles such as Inheritance, Encapsulation, Polymorphism, and Abstraction.
    
- Platform Independent (Write Once, Run Anywhere - WORA): Java code is compiled into a platform-neutral bytecode (with a .class extension), not machine-specific code. This bytecode can then be executed by the Java Virtual Machine (JVM) on any underlying platform that supports Java without recompilation. This makes Java highly portable.
    
- Simple: Java is designed to be easy to learn, especially for developers familiar with C or C++. It removes complex features like explicit pointers, operator overloading, and multiple inheritance found in C++.
    
- Secure: Java is designed with security in mind, providing automatic strict type checking and runtime exception handling. It avoids direct interaction with underlying memory or the operating system, reducing vulnerabilities like buffer overflows and memory leaks.
    
- Robust: Java provides strong memory management and built-in exception handling mechanisms.
    
- Multithreading: Java has built-in support for multiprocessing and multithreading, allowing programs to perform multiple tasks concurrently, which helps in constructing interactive and smooth applications.
    
- High Performance: Despite being interpreted, Java achieves high performance with the help of the Just-In-Time (JIT) compiler.
    
- Distributed: Java is designed for distributed systems and is widely used for internet-based applications.
    
- Dynamic: Java can adapt to evolving environments and carries extensive runtime information for verifying and resolving object accesses.

Java is widely used for building desktop applications, web applications, Android apps, enterprise solutions, game development, big data applications, and more.


### Java Environment: JDK, JRE, and JVM

Understanding how Java works involves three core components:

- Java Development Kit (JDK): This is a software development environment used for developing Java applications and applets. It includes the JRE, an interpreter/loader (Java), a compiler (javac), an archiver (jar), a documentation generator (Javadoc), and other tools necessary for Java development. As a developer, you install the JDK, which automatically includes the JRE and JVM.
    
- Java Runtime Environment (JRE): The JRE provides the minimum requirements for executing a Java application. It consists of the JVM, Java core packages, classes, and supporting files. When you install the JRE, it deploys the code to create a JVM for your specific platform. For a client machine that only needs to run Java applications, only the JRE (and thus JVM) is required, not the full JDK.
    
- Java Virtual Machine (JVM): The JVM is a specification that provides a runtime environment in which Java bytecode (.class file) can be executed. It creates a platform to run Java bytecode and converts it into native machine language that the computer hardware can understand. JVM is the core of the Java ecosystem, enabling the "write once, run anywhere" philosophy.

JVM Architecture Components:

1. Class Loader: Loads .class files (bytecode) into main memory. The class containing the main() method is usually loaded first. It has three phases,
    - Loading : Class Loading is a three step process of: the JVM locating the binary representation of a class or interface (.class), deriving the class or interface from it, and loading that information into the JVM method area.
    - Linking: 
        - Verification: Process of ensuring the class or interface is structurally correct.
        - Preparation: Handles the initialization of static fields in a class to their default values.
        - Resolution: Ensures that the JVM can locate and access the necessary classes and their members when they are used.
    - Initialization: Final stage where the JVM executes the class's initialization method (the <clinit> method). This involves executing static initializers (static blocks) and assigning initial values to static variables. 
    
2. Runtime Memory/Data Area: The JVM defines various runtime data areas used during program execution. These include:

    - Heap Area: A shared runtime data area where objects and arrays are stored. It is created when the JVM starts and can be fixed or dynamic in size. When the new keyword is used, the object is allocated in the heap, and its reference is stored in the stack. There is only one heap per running JVM process. Instance variables are stored in the Heap.
    - Method Area: Logically a part of the heap, it holds static variables and information about native methods.
    - JVM Stacks: A stack is created when a thread is created. It is used to store method execution data, including local variables, method arguments, and return addresses. Each thread has its own stack, ensuring thread safety. Once a method completes execution, its associated stack frame is automatically removed.
    - Native Method Area: Responsible for holding information about Java methods implemented in other languages like C or C++.
    - Program Counter (PC) Registers: (Mentioned, but not detailed in sources provided beyond listing).

3. Execution Engine: The final stage of the JVM, responsible for executing the class file. It has three main components:

    - Interpreter: Reads bytecode line by line and converts it into machine instructions. It is quick to load and fast for execution.
    - JIT (Just-In-Time) Compiler: Improves performance by compiling bytecode into native machine code during runtime.
    - Garbage Collector: An automatic memory management process that removes unused objects from the heap, freeing up memory. This eliminates the need for manual memory deallocation by developers.


### Basic Java Syntax and Program Structure

A basic Java program requires a class declaration and a main method as its entry point.

Example: Hello World Program

```Java
public class MyFirstJavaProgram {

/* This is my first java program.
* This will print 'Hello, World!' as the output
*/
    public static void main(String []args) {
        System.out.println("Hello, World!"); // prints Hello, World!
    }
}
```

- public class MyFirstJavaProgram: Defines a public class named MyFirstJavaProgram. In Java, every source file (.java) can have one top-level public class, and that class must match the name of the source file. A class is a collection of related code.

- public static void main(String []args): This is the main method, which serves as the entry point for your program. The JVM specifically looks for a static method named main that is public, takes an array of strings (String args[]) as a parameter, and does not return a value (void).
- public: An access modifier that makes the class or method accessible from anywhere.
- static: A keyword that means the method belongs to the class itself, rather than any specific object of that class.
- void: Indicates that the method does not return any value.
- String[] args: An array of String objects to hold command-line arguments passed to the program upon execution.

- System.out.println("Hello, World!");: This statement prints the string "Hello, World!" to the console. System is a class, out is a static member of the System class, and println() is a method to print output followed by a new line.

- Semicolons (;): Semicolons are mandatory at the end of most statements in Java. If omitted, the program will fail to compile.

- Comments: Used for notes within the code, ignored by the compiler.
- Single-line comments: Begin with //.
- Multi-line comments: Begin with /* and end with */. Can span multiple lines.
- Javadoc comments: Begin with /** and end with */. Used for generating HTML documentation. They can contain HTML tags and special "@doc tags" like @see, @version, @author, @since, @param, @return, @throws.

Coding Standards/Naming Conventions:

- Classes and Interfaces: Start with a capital letter (e.g., MyFirstJavaProgram, Runnable). If the name has multiple words, each subsequent word starts with a capital letter (Camel Casing).
- Variables and Methods: Start with a small letter (e.g., salary, show). Subsequent words start with a capital letter (camel casing).
- Constants: Use all capital letters, with words separated by underscores (e.g., PI, BRAND_NAME).


### Variables and Data Types 

Variables are containers that store data in memory during the processing cycle. They have a name and hold values. Java is a strongly typed language, meaning the type of data stored in a variable must be specified.

Categories of Variables:

 1. Primitive Data Types: In-built, simple data types that store the actual values directly in memory, typically on the stack.

    - int: Whole numbers (integers).
    - double: Numbers with decimal points (double-precision 64-bit floating point numbers).
    - char: Single characters, can store any Unicode character.
    - boolean: Can hold only two values: true or false.
    - byte: Small numbers (range -128 to 127).
    - short: Larger range than byte.
    - long: For very large integer values; append 'L' or 'l' at the end of the number.
    - float: Single-precision 32-bit floating point numbers; append 'F' or 'f' at the end of the number.

2. Reference Data Types (Non-Primitives):

    - More complex data types that store a memory address (reference) that points to a location in the heap where the actual object data is 
    stored. Strings and Arrays are examples of reference types.
    - When you declare a variable (e.g., int age;), you declare its existence.
    - When you assign a value (e.g., age = 27;), you define it.
    - Variables can be declared and initialized in one go (e.g., int age = 27;).
    - You cannot use an uninitialized variable.

Scope of Variables: Refers to the part of the program where a variable is accessible.

- Local Scope: Variables declared inside a method (or within a set of curly braces) have a local scope and are only recognized within that method. They are stored on the stack.
    
- Class Scope: Variables declared inside a class but not within any methods (often at the top of the class) have a class scope and are recognized throughout the entire class. These are also known as instance variables (if not static) or static variables (if static). Instance variables are stored in the Heap.

Type Casting: Converting one data type to another.

- Widening (Implicit): Automatically done by Java when converting from a smaller type to a larger type (e.g., int to double).
- Narrowing (Explicit): Requires explicit casting by the programmer when converting from a larger type to a smaller type, as data might be lost (e.g., double to int requires (int)).
- Upcasting and Downcasting (with Objects): Relates to casting objects in an inheritance hierarchy.

null reference: When a reference variable is uninitialized, Java initializes it to null, indicating the absence of a value. A NullPointerException occurs when a program attempts to use an object reference that has the null value.


### Operators

Operators are special symbols that perform operations on variables or values. They are essential for manipulating data efficiently.

Types of Operators:

- Arithmetic Operators: Perform mathematical operations (e.g., + for addition, - for subtraction, * for multiplication, / for division, % for modulus).
- Unary Operators: Operate on a single operand (e.g., + (unary plus), - (unary minus), ++ (increment), -- (decrement)). The placement of ++ or -- (pre- or post-increment/decrement) affects when the value is updated relative to its use in an expression.
- Assignment Operator: Assigns a value to a variable (e.g., =).
- Relational Operators: Compare two values, resulting in a boolean true or false (e.g., == (equal to), != (not equal to), ### < (less than), > (greater than), ### <= (less than or equal to), >= (greater than or equal to)). For strings, equals() method or equalsIgnoreCase() should be used for content comparison, not ==.
- Logical Operators: Combine two or more conditions (e.g., && (logical AND), || (logical OR), ! (logical NOT)).
- instanceof Operator: Used for type checking, to test if an object is an instance of a class, a subclass, or an interface.


### Control Flow Statements

Control flow statements allow programs to make decisions and execute blocks of code repeatedly based on conditions.

- Decision Making Statements:

- if-then and if-then-else: Executes a block of code if a condition is true (if) and an alternative block if the condition is false (else). Nested if statements are possible, allowing for checking additional conditions after an initial one is met.
- switch Statement: Allows execution of a block of code based on matching a variable's value against different case values.

- Loop Control Statements: Repeat a set of instructions multiple times based on a condition.

- for Loop: Used when the number of iterations is known or definite. It has four parts: initialization, condition, loop body, and update.
- while Loop: Executes a block of statements repeatedly as long as a given condition remains true. Useful when the number of iterations is not known beforehand, like reading a file until the end.
- do-while Loop: Similar to while loop, but the block of code is executed at least once before the condition is checked. It's an exit-controlled loop.
- Enhanced for Loop (for-each loop): Introduced in Java 5 to simplify iteration over arrays and collections, making code cleaner and more readable, especially when the exact index is not required.
- Nested Loops: A loop inside another loop, often used when working with matrices or multi-dimensional data structures like 2D arrays.

- Branching Statements:

- break: Used inside loops and switch statements to terminate the current loop or switch case immediately and transfer control to the statement directly after the loop/switch.
- continue: Used inside loops to skip the current iteration and move directly to the next iteration of the loop.
- return: Used to exit from a method, either with or without a value (for void methods).


### Object-Oriented Programming (OOP) Concepts

OOP is a fundamental concept in Java that allows developers to structure code using classes and objects, making it more modular, reusable, and scalable. The core idea is to bind data and the functions that operate on it.

**Classes and Objects**

- Class: 
        
    - A blueprint or prototype from which objects are created. It represents a group of objects having similar properties and behavior. A class is not a real-world entity and does not occupy memory. It can contain data members (variables), methods, constructors, nested classes, and interfaces.
    - Class Declaration Syntax: [access_modifier] class ### <class_name> { data members; methods; constructors; ... }.
    
- Object: 

    - An instance of a class. Objects represent real-world entities and can hold data (attributes/properties) and perform actions (methods/behaviors). Objects are stored in the Heap memory.
    - Creating Objects: The new operator instantiates a class by allocating memory for a new object in the heap and returning a reference to that memory. It also invokes the class constructor. Example: Dog tuffy = new Dog("tuffy", "papillon", 5, "white");.
    - Reference Variables: In Java, a variable that holds an object (non-primitive type) doesn't hold the object itself, but rather a reference (an address in memory) to the object. Multiple reference variables can point to the same object. Changes made via one reference variable to the object will be visible to all other reference variables pointing to the same object.
    - Instance Variables: Variables declared within a class but outside any method, constructor, or block. Each object (instance) of the class will have its own copy of these variables.
    - Static Variables: Variables declared with the static keyword inside a class. There is only one instance of a static field per class, shared by all objects of that class. Static variables are stored in the Method Area.

**Methods**

- Method: 

    - A block of reusable code that performs a specific task or function (i.e., behavior). Methods define the actions an object can perform.
    - Can take inputs via arguments/parameters and return values.
    - Can have no input arguments and no return values (void type method).
    - All methods in Java must belong to a class.
    
- Method Overloading: 

    - Occurs when a class has multiple methods with the same name but different parameters (different types or number of arguments). The method's name plus its parameters form a unique method signature.
    
- Method Overriding: 

    - Happens when a subclass provides its own specific implementation for a method that is already defined in its superclass (parent class). It allows for specific behavior for derived classes while maintaining code reusability. It's good practice to use the @Override annotation when overriding methods.

**Constructors**

- Constructor:

    - A special method within a class that is automatically called when an object is created (instantiated). Its main job is to initialize the object, setting up its internal state or assigning default values to its attributes.
    - Have the same name as the class and do not have a return type.
    - Default Constructor: If no constructor is explicitly defined, Java provides a default no-argument constructor.
    - Parameterized Constructor: Takes arguments to initialize object attributes with specific values.
    - Overloaded Constructors: A class can have multiple constructors with different parameter lists, similar to overloaded methods, allowing objects to be initialized in various ways.

**Inheritance**

- Inheritance: 

    - A mechanism where one class (subclass/child class) inherits the attributes and methods from another class (superclass/parent class). This promotes code reusability.
    - extends keyword: Used to establish an inheritance relationship (Dog extends Animal).
    - Every class in Java, if it doesn't explicitly extend another class, implicitly extends the Object class, which is the root class in the Java hierarchy.
    - super keyword: Used within a subclass to refer to the immediate superclass's members (fields, methods, or constructors). For example, super() calls the superclass's constructor.
    - Multi-level Inheritance: A class can inherit from a parent, which in turn inherits from a grandparent (e.g., Class C extends Class B, and Class B extends Class A).
    - No Multiple Inheritance (of classes): Java does not support multiple inheritance directly for classes (a class cannot extend from two parent classes). This avoids complex issues like the "diamond problem". However, it can implement multiple interfaces (see Abstraction).

 **Polymorphism**

- Polymorphism (Greek: "poly" means many, "morph" means shape): 

    - The concept of "many shapes" or forms. In Java, it means that objects can identify as other objects, allowing objects of diverse types of classes to be regarded as the object of the superclass.
    - Achieved through method overriding (runtime polymorphism) where a single method call can behave differently based on the actual type of the object at runtime.

**Abstraction**

- Abstraction:

    - The process of hiding implementation details and showing only the essential features or functionality to the user.
    
- Abstract Classes:

    - Declared with the abstract keyword.
    - Cannot be instantiated directly (you cannot create objects of an abstract class).
    - Can contain both abstract methods (methods declared without an implementation, requiring subclasses to implement them) and concrete methods (regular methods with implementation).
    - If a class contains any abstract methods, it must be declared as an abstract class. However, an abstract class doesn't necessarily need to have abstract methods.
    - Subclasses that implement all abstract methods from an abstract superclass are called concrete classes, and their objects can be created.
    
- Interfaces:

    - A blueprint for a class; a collection of abstract methods. They specify a set of behaviors that any implementing classes must define.
    - All methods in an interface are implicitly public and abstract (before Java 8).
    - Fields in an interface are implicitly public, static, and final.
    - A class uses the implements keyword to implement an interface (e.g., Class B implements Interface A).
    - A class can implement multiple interfaces, providing a way to achieve "multiple inheritance-like behavior" for behaviors, as opposed to direct class inheritance.
    - An interface can extend another interface.
    - Functional Interface: An interface that has only one abstract method. It can be marked with the @FunctionalInterface annotation to ensure it adheres to this rule. These are crucial for Lambda Expressions in Java 8.
    - Marker Interface: An interface with no methods. They are used to "mark" a class to give special meaning or properties to the compiler or runtime (e.g., Serializable).

**Encapsulation**

- Encapsulation: 

    - The process of gathering (or binding) data (attributes) and the methods that operate on that data together within a single unit, which is a class. It often involves information hiding by restricting direct access to some of an object's components, which is achieved using access modifiers.
    - Access Modifiers: Define how the members of a class (variables, methods, constructors, nested classes, interfaces) and the class itself can be accessed from other parts of the program.
            ▪ public: Accessible from anywhere.
            ▪ private: Accessible only within the declaring class.
            ▪ protected: Accessible within the declaring class, its subclasses, and classes in the same package.
            ▪ default (no keyword): Accessible only within the same package.

**Association, Aggregation, and Composition**

  These are types of relationships between objects.

   - Association: A general "uses-a" relationship between two classes.
        
   - Aggregation: A "has-a" relationship where one object (the whole) contains another object (the part), but the part can exist independently of the whole. Example: A Library has-a collection of Books. If the library is destroyed, the books can still exist.
        
   - Composition: A stronger "part-of" relationship where one object (the part) cannot exist independently of the containing object (the whole). Example: An engine is-part-of a car. If the car is destroyed, the engine typically doesn't exist independently in a meaningful way.

**this keyword**

- this: A reference variable that refers to the current object. It can be used to distinguish instance variables from local variables if they have the same name (e.g., this.name = name;). Can also be used to call other constructors within the same class (this(...)).

**static keyword**

- A keyword that indicates a member belongs to the class itself rather than to any specific object (instance) of that class.
- Static fields (class variables): Shared by all objects of the class; only one copy exists.
- Static methods: Can be called directly on the class name without creating an object. They can only access static members directly.
- Static blocks: Used to initialize static variables. Executed once when the class is loaded.

**final keyword**

- A keyword that can be used for immutability and restrictions.
- final variable: Its value cannot be changed after initialization (becomes a constant).
- final method: Cannot be overridden by subclasses.
- final class: Cannot be subclassed (inherited from).

**Class**

- Object: The root class for all classes in Java. Every class implicitly extends Object.

- It provides a set of common methods inherited by all objects, including:

    - equals(): Used to compare two objects for equality. Overriding this method is crucial for custom object comparison.
    - hashCode(): Returns a hash code value for the object. Often overridden along with equals() to ensure proper behavior in hash-based collections like HashMap and HashSet.
    - toString(): Returns a string representation of the object. It's commonly overridden to provide a meaningful representation of an object's state.


### Collections Framework

The Collections Framework is a set of interfaces and classes in java.util package that provides a unified architecture for representing and manipulating collections of objects. It addresses limitations of raw arrays, such as fixed size.

- Collection (interface): The root interface of the Java Collection Hierarchy. It represents a group of individual objects as a single unit.

- Collections (class): A utility class in the java.util package that provides static methods to operate on or return collections (e.g., Collections.sort(), Collections.max(), Collections.min()).

- Collection API (concept): Refers to the overall framework.

- Data Structures within Collections Framework:

- List (interface): 
        Represents an ordered collection (sequence) of elements, allowing duplicate values. Elements are accessed by their integer index.
        ▪ ArrayList: A resizable array implementation of the List interface.
        ▪ LinkedList: A doubly-linked list implementation of the List interface.
    
- Set (interface): 
        Represents a collection that does not allow duplicate elements. Sets do not maintain any specific order inherently, though some implementations do.
        ▪ HashSet: Implements Set using a hash table for storage. Offers constant-time performance for basic operations (add, remove, contains, size).
        ▪ LinkedHashSet: Maintains insertion order.
        ▪ TreeSet: Stores elements in a sorted order.
    
- Map (interface): 
        Stores key-value pairs, where keys must be unique, but values can be duplicated. Maps do not maintain any order naturally.
        ▪ HashMap: Implements Map using a hash table. Provides fast lookups using hash codes.
        ▪ LinkedHashMap: Maintains insertion order.
        ▪ TreeMap: Stores key-value pairs in a sorted order based on the natural ordering of keys or a provided comparator.
    
- Iterator: An interface used to traverse elements in a Collection sequentially. Provides methods like hasNext(), next(), and remove().
    
- Comparator: An interface used to define custom comparison logic for sorting objects, typically used when the class's natural ordering (if it implements Comparable) is not sufficient or when sorting objects of user-defined classes.
    
- Arrays class: Provides static utility methods for arrays, such as equals(), fill(), sort(), binarySearch(), and toString().


### Exception Handling

Exception handling in Java allows developers to manage runtime errors effectively.

- Exception: An event that occurs during the execution of a program (runtime) that disrupts the normal flow of instructions.

- When an exception occurs, the runtime system creates an exception object. This object contains information such as the exception type, a message explaining why it failed, and the stack trace (a list of method calls from where the exception originated).

- Exception Hierarchy: All exceptions and errors in Java are subclasses of the Throwable class.

- Error: Represents irrecoverable conditions, such as the Java Virtual Machine running out of memory (OutOfMemoryError) or StackOverflowError. Errors are usually beyond the control of the programmer and generally should not be handled.
    
- Exception: The basic type of Throwable that can be thrown from standard Java library methods and user-defined methods, representing conditions that a program might want to catch and handle.

  - Checked Exceptions: Exceptions that are checked for at compile time. Java forces you to deal with them at compile time (either by handling them using try-catch or by declaring that your method throws them). Examples include FileNotFoundException, IOException, SQLException.
  - Unchecked Exceptions (Runtime Exceptions): Subclasses of RuntimeException. These are not checked at compile time, and the compiler does not force you to handle them. They typically indicate programming errors. Examples include ArithmeticException, NullPointerException, ArrayIndexOutOfBoundsException.
    - Common JVM Errors and RuntimeExceptions include ClassNotFoundException, NoClassDefFoundError, OutOfMemoryError, and StackOverflowError.

- Exception Handling Mechanisms:

- try-catch Block: A mechanism to handle exceptions. The code that might throw an exception is placed inside the try block. If an exception occurs, it is caught by the catch block, preventing abrupt program termination and allowing the application to continue running.
- finally Block: An optional block that always executes, regardless of whether an exception occurred or was handled. It's often used for resource cleanup, such as closing files or database connections.
- throw keyword: Used to explicitly throw an exception from within a method or block of code.
- throws keyword: Used in a method signature to declare that a method might throw a certain type of checked exception. This informs calling methods that they need to handle the declared exception. It's not for throwing multiple exceptions.
- Custom Exceptions: You can create your own exception classes by extending Exception or RuntimeException. This allows you to define application-specific errors.
- Chained Exceptions: Allows associating one exception with another, where one exception describes the cause of another.


### Memory Management

Java memory management is a fundamental concept involving the automatic allocation and deallocation of objects, managed by the Java Virtual Machine (JVM).

- The JVM uses a garbage collector to automatically remove unused objects, freeing up memory in the background. This eliminates the need for developers to manually handle memory management, which is prone to errors like memory leaks.

- A memory leak occurs when a program continues to use memory but fails to release it when it's no longer needed, leading to slow performance and potential crashes.

- JVM Memory Areas (as discussed in Section 2, but re-emphasized for memory management):
- Heap: Where objects and arrays are stored.
- JVM Stacks: Stores method execution data, local variables, method arguments, and return addresses for each thread.
- Method Area: Stores static variables and class information.


### Input/Output (I/O)

Java's I/O system is designed to handle various data sources and sinks (files, console, network connections) in diverse ways (sequential, random-access, buffered, binary, character, by lines/words).

- Streams: The Java I/O library often uses the abstraction of a stream, which represents any data source or sink as an object capable of producing or receiving data. The stream hides the details of the actual I/O device.
- InputStream / Reader: Classes for reading a single byte or an array of bytes/characters.
- OutputStream / Writer: Classes for writing a single byte or an array of bytes/characters.
- Byte Streams: Handle raw binary data.
- Character Streams: Handle character data, supporting Unicode.
- Buffered Streams: Provide improved performance by buffering data.

- File Handling: Working with files involves classes in the java.io package.
- File Class: Used to create an object representing a file or directory.
- FileReader: Used for reading character files. A FileNotFoundException can occur if the specified file does not exist.
- FileWriter: Used for writing small to medium-sized text files. Requires a try-catch block.
- BufferedReader and BufferedWriter: Provide better performance for reading/writing large amounts of text, typically combined with FileReader/FileWriter.

- User Input: The Scanner class (from java.util.Scanner) is commonly used to accept user input from the console. It's good practice to close the scanner when done to prevent unexpected behavior.

- Command Line I/O: System.in for standard input, System.out for standard output.


### Other Important Concepts

- Generics: 
    
- Allow classes and methods to operate on objects of various types while providing compile-time type safety. They use type parameters when defining classes/methods and type arguments when instantiating them (e.g., ArrayList### <String>). While powerful, Java's generics have certain limitations compared to more "pure" implementations in other languages.

- Annotations (Metadata): 
    
- Provide a formalized way to add information to code that can be used later by compilers, runtime, or other tools. Examples include @Override (already discussed), @FunctionalInterface, and @Deprecated. They combine metadata with source code, making it neater and easier to maintain.

- Inner Classes: 
    
- A class definition placed within another class definition. They can access members of the enclosing object, including private ones.
- Anonymous Inner Classes: Classes that don't have a name and cannot be reused. They are often used for one-time uses when custom behavior is needed without creating a new class.

- pass-by-value vs. pass-by-reference: Java is always pass by value.

- For primitive variables, the actual value is copied and passed to the method.
- For non-primitive variables (objects), the value of the memory address (the reference) is copied and passed. This means the method receives a copy of the reference, and both the original reference and the copied reference point to the same object in the heap.
- If the method modifies the contents of the object that the passed-in reference points to, those changes will affect the original object.
- However, if the method reassigns the passed-in reference to a new object, the original object outside the method remains unchanged because the copied reference now points to a different object.

- Java Database Connectivity (JDBC): A fundamental technology that allows Java applications to interact with databases. Developers can perform tasks like data insertion, retrieval, and modification.

- Java Swing: A Graphical User Interface (GUI) toolkit in Java used to develop desktop applications with a graphical interface. It provides components like buttons, text fields, and labels.

