### Introduction to Java

Java is a popular, high-level, object-oriented programming language that was originally developed by Sun Microsystems and released in 1995. It is currently owned by Oracle, and billions of devices run Java. Java runs on various platforms, including Windows, Mac OS, and different versions of UNIX.

Key features and principles of Java include:

- **Object-Oriented Programming (OOP):** Java is fundamentally an object-oriented language, where nearly everything is considered an object. It supports core OOP principles such as _Inheritance, Encapsulation, Polymorphism, and Abstraction._
    
- **Platform Independent (Write Once, Run Anywhere - WORA):** Java code is compiled into a _platform-neutral bytecode_ (with a .class extension), not machine-specific code. This bytecode can then be executed by the Java Virtual Machine (JVM) on any underlying platform that supports Java without recompilation. This makes Java highly portable.
    
- **Simple:** Java is designed to be easy to learn, especially for developers familiar with C or C++. It removes complex features like explicit pointers, operator overloading, and multiple inheritance found in C++.
    
- **Secure:** Java is designed with security in mind, providing automatic strict type checking and runtime exception handling. It avoids direct interaction with underlying memory or the operating system, reducing vulnerabilities like buffer overflows and memory leaks.
    
- **Robust:** Java provides strong memory management and built-in exception handling mechanisms.
    
- **Multithreading:** Java has built-in support for_ multiprocessing and multithreading,_ allowing programs to perform multiple tasks concurrently, which helps in constructing interactive and smooth applications.
    
- **High Performance:** Despite being interpreted, Java achieves high performance with the help of the _Just-In-Time (JIT) compiler._
    
- **Distributed:** Java is designed for distributed systems and is widely used for internet-based applications.
    
- **Dynamic:** Java can adapt to evolving environments and carries extensive runtime information for verifying and resolving object accesses.

Java is widely used for building desktop applications, web applications, Android apps, enterprise solutions, game development, big data applications, and more.


### Java Environment: JDK, JRE, and JVM

Understanding how Java works involves three core components:

- **Java Development Kit (JDK):** This is a software development environment used for developing Java applications and applets. _It includes the JRE, an interpreter/loader (Java), a compiler (javac), an archiver (jar), a documentation generator (Javadoc), and other tools necessary for Java development._ As a developer, you install the JDK, which automatically includes the JRE and JVM.
    
- **Java Runtime Environment (JRE):** The JRE provides the minimum requirements for executing a Java application. _It consists of the JVM, Java core packages, classes, and supporting files. When you install the JRE, it deploys the code to create a JVM for your specific platform._ For a client machine that only needs to run Java applications, only the JRE (and thus JVM) is required, not the full JDK.
    
- **Java Virtual Machine (JVM):** The JVM is a specification that provides a runtime environment in which Java bytecode (.class file) can be executed. It creates a platform to run Java bytecode and converts it into native machine language that the computer hardware can understand. JVM is the core of the Java ecosystem, enabling the "write once, run anywhere" philosophy.

**JVM Architecture Components:**

1. **Class Loader:** Loads .class files (bytecode) into main memory. The class containing the main() method is usually loaded first. It has three phases,
    - **Loading:** Class Loading is a three step process of: _the JVM locating the binary representation of a class or interface (.class), deriving the class or interface from it, and loading that information into the JVM method area._
    - **Linking:** 
        - **Verification:** Process of ensuring the class or interface is _structurally correct._
        - **Preparation:** Handles the initialization of static fields in a class to their _default values._
        - **Resolution:** Ensures that the JVM can locate and access the necessary classes and their members when they are used.
    - **Initialization:** Final stage where the JVM executes the class's initialization method (_the clinit method_). This involves executing static initializers (static blocks) and assigning initial values to static variables. 
    
2. **Runtime Memory/Data Area:** The JVM defines various runtime data areas used during program execution. These include:

    - **Heap Area:** A shared runtime data area where _objects and arrays_ are stored. It is created when the JVM starts and can be _fixed or dynamic in size_. When the new keyword is used, _the object is allocated in the heap, and its reference is stored in the stack._ There is only one heap per running JVM process. _Instance variables are stored in the Heap._
    - **Method Area:** Logically a part of the heap, it holds _static variables and information about native methods._
    - **JVM Stacks:** A stack is created when a thread is created. It is used to _store method execution data, including local variables, method arguments, and return addresses._ Each thread has its own stack, ensuring thread safety. Once a method completes execution, its associated stack frame is automatically removed.
    - **Native Method Area:** Responsible for holding information about Java methods _implemented in other languages_ like C or C++.
    - **Program Counter (PC) Registers:** Each JVM thread has its PC (program counter) register. For a non-native method, _the PC register stores the address of the current instruction being executed_. For a native method, the PC register has an _undefined value._
      

3. **Execution Engine:** The final stage of the JVM, responsible for executing the class file. It has three main components:

    - **Interpreter:** Reads bytecode line by line and converts it into machine instructions. It is quick to load and fast for execution.
    - **JIT (Just-In-Time) Compiler:** Improves performance by compiling bytecode into native machine code during _runtime_.
    - **[Garbage Collector](https://developers.redhat.com/articles/2021/08/20/stages-and-levels-java-garbage-collection#memory_management_and_avoiding_memory_leaks):** An automatic memory management process that removes unused objects from the _heap, freeing up memory._ This eliminates the need for manual memory deallocation by developers.
        - There are three basic steps in garbage collection:
            - **Mark:** The garbage collector scans the heap memory segment and marks all the live objects i.e., objects to which the application holds references. All the objects that have_ no references to them are eligible for removal._
            - **Sweep:** The garbage collector r_ecycles all the unreferenced objects from the heap._
            - **Compact:** The sweep step tends to _leave many empty regions in heap memory, causing memory fragmentation._ Therefore, the compact phase helps arrange the objects into the _contiguous blocks at the start of the heap._ This in turn helps with the allocation of new objects in sequence.
             


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

- **public static void main(String []args):** This is the main method, which serves as the entry point for your program. The JVM specifically looks for a static method named main that is public, takes an array of strings (String args[]) as a parameter, and does not return a value (void).
    - **public:** An _access modifier_ that makes the class or method accessible from anywhere.
    - **static:** A keyword that means the _method belongs to the class itself_, rather than any specific object of that class.
    - **void:** Indicates that the method _does not return any value._
    - **String[] args:** An array of String objects to hold _command-line arguments _passed to the program upon execution.
    - **System.out.println("Hello, World!");:** This statement prints the string "Hello, World!" to the console. _System is a class, out is a static member of the System class, and println() is a method to print output followed by a new line._
    - **Semicolons (;):** Semicolons are mandatory at the end of most statements in Java. If omitted, the program will fail to compile.
    - **Comments:** Used for notes within the code, _ignored by the compiler._
        - **Single-line comments:** Begin with //.
        - **Multi-line comments:** Begin with /* and end with */. Can span multiple lines.
        - **Javadoc comments:** Begin with /** and end with */. Used for generating HTML documentation. They can contain HTML tags and special "@doc tags" like @see, @version, @author, @since, @param, @return, @throws.

**Coding Standards/Naming Conventions:**

- **Classes and Interfaces:** Start with a capital letter (e.g., MyFirstJavaProgram, Runnable). If the name has multiple words, each subsequent word starts with a capital letter (Camel Casing).
- **Variables and Methods:** Start with a small letter (e.g., salary, show). Subsequent words start with a capital letter (camel casing).
- **Constants:** Use all capital letters, with words separated by underscores (e.g., PI, BRAND_NAME).


### Variables and Data Types 

Variables are containers that_ store data in memory during the processing cycle._ They have a name and hold values. Java is a_ strongly typed language,_ meaning the type of data stored in a variable must be specified.

**Categories of Variables:**

 1. **Primitive Data Types:** In-built, simple data types that store the actual values directly in memory, _typically on the stack._

    - **int:** Whole numbers (integers). 
    - **double:** Numbers with decimal points (double-precision 64-bit floating point numbers).
    - **char:** Single characters, can store any Unicode character.
    - **boolean:** Can hold only two values: true or false.
    - **byte:** Small numbers _(range -128 to 127)._
    - **short:** Larger range than byte.
    - **long:** For very large integer values; append 'L' or 'l' at the end of the number.
    - **float:** Single-precision 32-bit floating point numbers; append 'F' or 'f' at the end of the number.

2. **Reference Data Types (Non-Primitives):**
    - More complex data types that store a _memory address _(reference) that points to a location in the heap where the actual object data is 
    stored. _Strings and Arrays_ are examples of reference types.
    - When you declare a variable (e.g., int age;), you _declare its existence._
    - When you assign a value (e.g., age = 27;), _you define it._
    - Variables can be declared and initialized in one go (e.g., int age = 27;).
    - You _cannot use an uninitialized variable._

**Scope of Variables:** Refers to the part of the program where a variable is _accessible._

- **Local Scope:** Variables declared inside a method (or within a set of curly braces) have a local scope and are _only recognized within that method._ They are _stored on the stack._
    
- **Class Scope:** Variables declared inside a class but not within any methods (often at the top of the class) have a class scope and are _recognized throughout the entire class._ These are also known as _instance variables_ (if not static) or _static variables_ (if static). Instance variables are _stored in the Heap._

**Type Casting:** Converting one data type to another.

- **Widening (Implicit):** Automatically done by Java when converting from a _smaller type to a larger type_ (e.g., int to double).
- **Narrowing (Explicit):** Requires _explicit casting_ by the programmer when converting from a _larger type to a smaller type_, as data might be lost (e.g., double to int requires (int)).
- **Upcasting and Downcasting (with Objects):** Relates to casting objects in an _inheritance hierarchy._

**null reference:** When a _reference variable is uninitialized, Java initializes it to null,_ indicating the absence of a value. A NullPointerException occurs when a program attempts to use an object reference that has the null value.


### Operators

Operators are special symbols that perform operations on variables or values. They are essential for manipulating data efficiently.

Types of Operators:

- **Arithmetic Operators:** Perform _mathematical operations_ (e.g., + for addition, - for subtraction, * for multiplication, / for division, % for modulus).
- **Unary Operators:** Operate on a _single operand_ (e.g., + (unary plus), - (unary minus), ++ (increment), -- (decrement)). The placement of ++ or -- (pre- or post-increment/decrement) affects when the value is updated relative to its use in an expression.
- **Assignment Operator:** Assigns a value to a variable (e.g., =).
- **Relational Operators:** Compare two values, _resulting in a boolean_ true or false (e.g., == (equal to), != (not equal to), ### < (less than), > (greater than), ### <= (less than or equal to), >= (greater than or equal to)). For strings, _equals() method or equalsIgnoreCase()_ should be used for content comparison, _not ==._
- **Logical Operators:** Combine two or more conditions (e.g., && (logical AND), || (logical OR), ! (logical NOT)).
- **instanceof Operator:** Used for _type checking,_ to test if an object is an instance of a class, a subclass, or an interface.


### Control Flow Statements

Control flow statements allow programs to _make decisions and execute blocks of code repeatedly based on conditions._

- **Decision Making Statements:**

    - **if and if-else:** Executes a block of code if a condition is true (if) and an alternative block if the condition is false (else). Nested if statements are possible, allowing for checking additional conditions after an initial one is met.
    - **switch Statement:** Allows execution of a block of code based on matching a _variable's value against different case values._

- **Loop Control Statements:** Repeat a set of instructions multiple times based on a condition.

    - **for Loop:** Used when the _number of iterations is known or definite._ It has four parts: initialization, condition, loop body, and update.
    - **while Loop:** Executes a block of statements repeatedly as long as a _given condition remains true._ Useful when the _number of iterations is not known beforehand,_ like reading a file until the end.
    - **do-while Loop:** Similar to while loop, but the _block of code is executed at least once_ before the condition is checked. It's an _exit-controlled loop._
    - **Enhanced for Loop (for-each loop):** Introduced in Java 5 to simplify iteration over arrays and collections, making code cleaner and more readable, especially when the _exact index is not required._
    - **Nested Loops:** A loop inside another loop, often used when working with _matrices or multi-dimensional data structures like 2D arrays._

- **Branching Statements:**

    - **break:** Used inside loops and switch statements to _terminate the current loop or switch case immediately_ and transfer control to the statement directly after the loop/switch.
    - **continue:** Used inside loops to skip the _current iteration and move directly to the next iteration_ of the loop.
    - **return:** Used to exit from a method, either with or without a value (for void methods).


### Object-Oriented Programming (OOP) Concepts

OOP is a fundamental concept in Java that allows developers to structure code using classes and objects, making it more _modular, reusable, and scalable._ The core idea is to bind data and the functions that operate on it.

**Classes and Objects**

- **Class:**
        
    - A _blueprint or prototype_ from which objects are created. It represents a _group of objects having similar properties and behavior._ A class is not a real-world entity and does not occupy memory. It can contain data members (variables), methods, constructors, nested classes, and interfaces.
    - **Class Declaration Syntax:** [access_modifier] class <class_name> { data members; methods; constructors; ... }.
    
- **Object:** 

    - An _instance of a class._ Objects represent real-world entities and can _hold data (attributes/properties) and perform actions (methods/behaviors)._ Objects are stored in the _Heap memory._
    - **Creating Objects:** The new operator instantiates a class by allocating memory for a new object in the heap and returning a reference to that memory. It also _invokes the class constructor._ Example: Dog tuffy = new Dog("tuffy", "papillon", 5, "white");.
    - **Reference Variables:** In Java, a variable that holds an object (non-primitive type) doesn't hold the object itself, but rather a reference (an address in memory) to the object. Multiple reference variables can point to the same object. Changes made via one reference variable to the object will be visible to all other reference variables pointing to the same object.
    - **Instance Variables:** Variables _declared within a class but outside any method, constructor, or block._ Each object (instance) of the class will have its own copy of these variables.
    - **Static Variables:** Variables declared with the _static keyword inside a class._ There is only _one instance of a static field per class_, shared by all objects of that class. Static variables are stored in the_ Method Area._

**Methods**

- A block of reusable code that performs a specific task or function (i.e., behavior). Methods define the actions an object can perform.
- Can take inputs via arguments/parameters and return values.
- Can have no input arguments and no return values (void type method).
- All methods in Java must belong to a class.
    
- **Method Overloading:** 

    - Occurs when a class has multiple methods with the _same name but different parameters (different types or number of arguments)._ The method's name plus its parameters form a unique method signature.
    
- **Method Overriding:** 

    - Happens when a _subclass provides its own specific implementation for a method that is already defined in its superclass (parent class)._ It allows for specific behavior for derived classes while maintaining code reusability. It's good practice to use the _@Override_ annotation when overriding methods.

**Constructors**

- A special method within a class that is _automatically called when an object is created (instantiated)._ Its main job is to initialize the object, setting up its internal state or assigning default values to its attributes.
- Have the same name as the class and do not have a return type.
- **Default Constructor:** If no constructor is explicitly defined, Java provides a default no-argument constructor.
- **Parameterized Constructor:** Takes arguments to initialize object attributes with specific values.
- **Overloaded Constructors:** A class can have multiple constructors with different parameter lists, similar to overloaded methods, allowing objects to be initialized in various ways.



**Inheritance**

- A mechanism where one class (subclass/child class) _inherits the attributes and methods from another class_ (superclass/parent class). This promotes code reusability.
- **extends keyword:** Used to establish an inheritance relationship (Dog extends Animal).
- Every class in Java, if it doesn't explicitly extend another class, _implicitly extends the Object class,_ which is the root class in the Java hierarchy.
- **super keyword:** Used within a _subclass to refer to the immediate superclass's members_ (fields, methods, or constructors). For example, super() calls the superclass's constructor.
- **Multi-level Inheritance:** A class can inherit from a parent, which in turn inherits from a grandparent (e.g., Class C extends Class B, and Class B extends Class A).
- **No Multiple Inheritance (of classes):** Java _does not support multiple inheritance directly for classes_ (a class cannot extend from two parent classes). This avoids complex issues like the "diamond problem". However, it can implement _multiple interfaces (see Abstraction)._

 **Polymorphism** (Greek: "poly" means many, "morph" means shape): 

- The concept of "many shapes" or forms. In Java, it means that objects can identify as other objects, allowing objects of diverse types of classes to be regarded as the object of the superclass.
- Achieved through method overriding (runtime polymorphism) where a single method call can behave differently based on the actual type of the object at _runtime._

**Abstraction**

- The process of _hiding implementation details and showing only the essential features_ or functionality to the user.
    
- **Abstract Classes:**

    - Declared with the abstract keyword.
    - Cannot be instantiated directly (you cannot create objects of an abstract class).
    - Can contain both abstract methods (methods declared without an implementation, requiring subclasses to implement them) and concrete methods (regular methods with implementation).
    - If a class contains any abstract methods, it must be declared as an abstract class. However, an abstract class doesn't necessarily need to have abstract methods.
    - Subclasses that implement all abstract methods from an abstract superclass are called _concrete classes_, and their objects can be created.
    
- **Interfaces:**

    - A blueprint for a class; a _collection of abstract methods._ They specify a set of behaviors that any implementing classes must define.
    - All methods in an interface are _implicitly public and abstract (before Java 8)._
    - Fields in an interface are _implicitly public, static, and final._
    - A class uses the _implements_ keyword to implement an interface (e.g., Class B implements Interface A).
    - A class can implement _multiple interfaces, providing a way to achieve "multiple inheritance-like behavior"_ for behaviors, as opposed to direct class inheritance.
    - An interface can _extend_ another interface.
    - **Functional Interface:** An interface that has only _one abstract method_. It can be marked with the _@FunctionalInterface_ annotation to ensure it adheres to this rule. These are crucial for Lambda Expressions in Java 8.
    - **Marker Interface:** An interface with _no methods._ They are used to "mark" a class to give special meaning or properties to the compiler or runtime (e.g., Serializable).

**Encapsulation**

- The process of gathering (or binding) data (attributes) and the methods that operate on that data together within a single unit, which is a class. It often involves information hiding by restricting direct access to some of an object's components, which is achieved using access modifiers.
- **Access Modifiers:** Define how the members of a class (variables, methods, constructors, nested classes, interfaces) and the class itself can be accessed from other parts of the program.
    ▪ public: Accessible from anywhere.
    ▪ private: Accessible only within the declaring class.
    ▪ protected: Accessible within the declaring class, its subclasses, and classes in the same package.
    ▪ default (no keyword): Accessible only within the same package.

    
**Association, Aggregation, and Composition**

  These are types of relationships between objects.

   - **Association:** A general "uses-a" relationship between two classes.
        
   - **Aggregation:** A "has-a" relationship where one object (the whole) contains another object (the part), but the part can exist independently of the whole. Example: A Library has-a collection of Books. If the library is destroyed, the books can still exist.
        
   - **Composition:** A stronger "part-of" relationship where one object (the part) cannot exist independently of the containing object (the whole). Example: An engine is-part-of a car. If the car is destroyed, the engine typically doesn't exist independently in a meaningful way.

**this keyword**

- **this:** A reference variable that refers to the current object. It can be used to distinguish instance variables from local variables if they have the same name (e.g., this.name = name;). Can also be used to call other constructors within the same class (this(...)).

**static keyword**

- A keyword that indicates a member belongs to the _class itself_ rather than to any specific object (instance) of that class.
- **Static fields (class variables):** Shared by all objects of the class; only one copy exists.
- **Static methods:** Can be called directly on the _class name without creating an object._ They can only access static members directly.
- **Static blocks:** Used to initialize static variables. Executed once when the class is loaded.

**final keyword**

- A keyword that can be used for _immutability and restrictions._
- **final variable:** Its value cannot be changed after initialization (becomes a constant).
- **final method:** Cannot be _overridden_ by subclasses.
- **final class:** Cannot be _subclassed_ (inherited from).

**Class**

- **Object:** The root class for all classes in Java. Every class implicitly extends Object.

- It provides a set of common methods inherited by all objects, including:

    - **equals():** Used to compare two objects for equality. Overriding this method is crucial for custom object comparison.
    - **hashCode():** Returns a _hash code value for the object._ Often overridden along with equals() to ensure proper behavior in hash-based collections like HashMap and HashSet.
    - **toString():** Returns a _string representation of the object._ It's commonly overridden to provide a meaningful representation of an object's state.


### Collections Framework

The Collections Framework is a set of interfaces and classes in java.util package that provides a unified architecture for representing and manipulating collections of objects. It addresses limitations of raw arrays, such as fixed size.

- **Collection (interface):** The root interface of the Java Collection Hierarchy. It represents a group of individual objects as a single unit.

- **Collections (class):** A utility class in the java.util package that provides static methods to operate on or return collections (e.g., Collections.sort(), Collections.max(), Collections.min()).

- **Collection API (concept):** Refers to the overall framework.

- **Data Structures within Collections Framework:**

- **List (interface):**

    - Represents an _ordered collection (sequence) of elements, allowing duplicate values._ Elements are accessed by their _integer index._
        - **ArrayList:** A resizable array implementation of the List interface.
        - **LinkedList:** A doubly-linked list implementation of the List interface.
    
- **Set (interface):**

    - Represents a collection that _does not allow duplicate elements._ Sets _do not maintain any specific order inherently_, though some implementations do.
        - **HashSet:** Implements Set using a _hash table for storage._ Offers _constant-time performance for basic operations_ (add, remove, contains, size).
        - **LinkedHashSet:** _Maintains insertion order_.
        - **TreeSet:** _Stores elements in a sorted order_.
        
- **Map (interface):**
  
     - Stores _key-value pairs, where keys must be unique, but values can be duplicated._ Maps _do not maintain any order naturally_.
         - **HashMap:** Implements Map using a _hash table._ Provides fast lookups using hash codes.
         - **LinkedHashMap:** Maintains insertion order.
         - **TreeMap:** Stores key-value pairs in a _sorted order based on the natural ordering of keys_ or a provided comparator.       
        
- **Iterator:** An interface used to traverse elements in a Collection _sequentially_. Provides methods like _hasNext(), next(), and remove()._
    
- **Comparator:** An interface used to define _custom comparison logic for sorting objects,_ typically used when the class's natural ordering (if it implements Comparable) is not sufficient or when sorting objects of user-defined classes.
    
- **Arrays class:** Provides static utility methods for arrays, such as _equals(), fill(), sort(), binarySearch(), and toString()._


### Exception Handling

Exception handling in Java allows developers to manage _runtime errors effectively._

- **Exception:** An event that occurs _during the execution of a program (runtime) _that disrupts the normal flow of instructions.

- When an exception occurs, the runtime system creates an exception object. This object contains information such as the exception type, a message explaining why it failed, and the stack trace (a list of method calls from where the exception originated).

- **Exception Hierarchy:** All exceptions and errors in Java are subclasses of the _Throwable class._

- **Error:** Represents _irrecoverable conditions_, such as the Java Virtual Machine running out of memory (OutOfMemoryError) or StackOverflowError. Errors are usually beyond the control of the programmer and generally should not be handled.
    
- **Exception:** The basic type of Throwable that can be thrown from standard Java library methods and user-defined methods, representing conditions that a program might want to catch and handle.

  - **Checked Exceptions:** Exceptions that are checked for at _compile time._ Java forces you to deal with them at compile time (either by handling them using try-catch or by declaring that your method throws them). Examples include FileNotFoundException, IOException, SQLException.
  - **Unchecked Exceptions (Runtime Exceptions):** Subclasses of _RuntimeException_. These are _not checked at compile time,_ and the compiler does not force you to handle them. They typically indicate programming errors. Examples include ArithmeticException, NullPointerException, ArrayIndexOutOfBoundsException.
    - Common JVM Errors and RuntimeExceptions include _ClassNotFoundException, NoClassDefFoundError, OutOfMemoryError, and StackOverflowError._

- **Exception Handling Mechanisms:**

    - **try-catch Block:** A mechanism to handle exceptions. The code that might throw an exception is placed inside the try block. If an exception occurs, it is caught by the catch block, preventing abrupt program termination and allowing the application to continue running.
    - **finally Block:** An optional block that always executes, regardless of whether an exception occurred or was handled. It's often used for resource cleanup, such as closing files or database connections.
    - **throw keyword:** Used to explicitly throw an exception from within a method or block of code.
    - **throws keyword:** Used in a _method signature_ to declare that a method might throw a certain type of _checked exception_. This informs calling methods that they need to handle the declared exception. It's not for throwing multiple exceptions.
    - **Custom Exceptions:** You can create your own exception classes by extending Exception or RuntimeException. This allows you to define application-specific errors.
    - **Chained Exceptions:** Allows associating one exception with another, where one exception describes the cause of another.


### Input/Output (I/O)

Java's I/O system is designed to handle various data sources and sinks (files, console, network connections) in diverse ways (sequential, random-access, buffered, binary, character, by lines/words).

- **Streams:** The Java I/O library often uses the abstraction of a stream, which represents any data source or sink as an object capable of producing or receiving data. The stream hides the details of the actual I/O device.
    - **InputStream / Reader:** Classes for reading a single byte or an array of bytes/characters.
    - **OutputStream / Writer:** Classes for writing a single byte or an array of bytes/characters.
    - **Byte Streams:** Handle raw binary data.
    - **Character Streams:** Handle character data, supporting Unicode.
    - **Buffered Streams:** Provide improved performance by buffering data.

- **File Handling:** Working with files involves classes in the java.io package.
    - **File Class:** Used to create an object representing a file or directory.
    - **FileReader:** Used for reading character files. A FileNotFoundException can occur if the specified file does not exist.
    - **FileWriter:** Used for writing small to medium-sized text files. Requires a try-catch block.
    - **BufferedReader and BufferedWriter:** Provide better performance for reading/writing large amounts of text, typically combined with FileReader/FileWriter.
    
- **User Input:** The Scanner class (from java.util.Scanner) is commonly used to accept user input from the console. It's good practice to close the scanner when done to prevent unexpected behavior.
- **Command Line I/O:** System.in for standard input, System.out for standard output.


### Other Important Concepts

- **Generics:** 
    
    - Allow classes and methods to operate on objects of various types while providing _compile-time type safety._ They use type parameters when defining classes/methods and type arguments when instantiating them (e.g., ArrayList### <String>). While powerful, Java's generics have certain limitations compared to more "pure" implementations in other languages.

- **Annotations (Metadata):** 
    
    - Provide a _formalized way to add information to code that can be used later by compilers, runtime, or other tools._ Examples include @Override (already discussed), @FunctionalInterface, and @Deprecated. They combine metadata with source code, making it neater and easier to maintain.

- **Inner Classes:**
    
    - A class definition placed within another class definition. They can access members of the enclosing object, including private ones.
    - **Anonymous Inner Classes:** Classes that don't have a name and cannot be reused. They are often used for one-time uses when custom behavior is needed without creating a new class.

- **pass-by-value vs. pass-by-reference:** Java is always _pass by value._

    - For _primitive variables_, the _actual value is copied and passed to the method._
    - For _non-primitive variables (objects),_ the _value of the memory address (the reference) is copied and passed_. This means the method receives a copy of the reference, and both the original reference and the copied reference point to the same object in the heap.
    - If the method modifies the contents of the object that the passed-in reference points to, those changes will affect the original object.
    - However, if the method reassigns the passed-in reference to a new object, the original object outside the method remains unchanged because the copied reference now points to a different object.

- **Java Database Connectivity (JDBC):** A fundamental technology that allows Java applications to interact with databases. Developers can perform tasks like data insertion, retrieval, and modification.

- **Java Swing:** A Graphical User Interface (GUI) toolkit in Java used to develop desktop applications with a graphical interface. It provides components like buttons, text fields, and labels.

