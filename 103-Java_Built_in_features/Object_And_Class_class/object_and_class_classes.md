This document provides comprehensive notes on the Java `Object` class and the `Class` class, designed for quick revision, focusing on core concepts, contracts, and methods supported by code examples.

---

## Ⅰ. The `java.lang.Object` Class

The `Object` class is the **root of the class hierarchy** in Java.

### Core Concepts

*   **Inheritance:** Every class in Java, whether predefined or user-defined, is a direct or indirect descendant (superclass) of the `Object` class. If a class does not explicitly extend another class, it directly inherits from `Object`.
*   **Purpose:** Java developers created the `Object` class to centralize **common modules, methods, or functionalities** that every object might need. This prevents developers from having to create these methods repeatedly in every class.
*   **Location:** The `Object` class is located in the `java.lang` package.
*   **Inherent Methods:** All Java classes inherit the methods of the `Object` class. If the generic logic of these methods does not meet a subclass's specific needs, the subclass should override them.
*   **Total Methods:** The `Object` class implements 11 common properties through 11 methods.

### Essential Methods and Examples

The `Object` class declares nine primary methods (excluding its constructor, which is `public Object()`).

#### 1. `public String toString()`

*   **Purpose:** Returns a concise but informative **string representation** of the object, which is highly recommended to be overridden for human readability and debugging.
*   **Default Behavior:** The default implementation returns a string equal to the value of: `getClass().getName() + '@' + Integer.toHexString(hashCode())`. This prints the class name, the `@` symbol, and the hexadecimal representation of the object's hash code.
*   **Invocation:** When you try to print an object reference (e.g., `System.out.println(myObject)`), it internally calls the `.toString()` method.

##### **Java Example: Overriding `toString()`**

```java
// Default behavior returns uninformative output (e.g., classname@hashcode)

class JavaDev {
    String name;
    public JavaDev(String name) { this.name = name; }
    
    @Override
    public String toString() { 
        // Overridden to return meaningful content (e.g., object attributes)
        return "Developer Name: {'" + name + "'}";
    }
}

public class RevisionDemo {
    public static void main(String[] args) {
        JavaDev developer = new JavaDev("Shriansh");
        System.out.println(developer.toString()); 
        // Output: Developer Name: {'Shriansh'}
    }
}
```

#### 2. `public int hashCode()`

*   **Purpose:** Returns an integer hash code value for the object, primarily used to support **hash tables** like `HashMap` and `HashSet`.
*   **Mechanism:** Internally, the JVM generates a unique integer, which is a representation of the object's memory address, although it is not the actual address. This is a **Native method** (written in languages like C/C++).
*   **Contract (Crucial for interviews):**
    1.  Multiple invocations on the same object must consistently return the same integer, provided the information used in `equals` comparisons is unmodified.
    2.  If two objects are equal according to `equals(Object)`, then their `hashCode()` methods **must produce the same integer result**.
    3.  It is generally necessary to **override `hashCode()` whenever `equals()` is overridden** to maintain this contract.

##### **Java Example: Overriding `hashCode()`**

```java
class Vehicle {
    String car_no;
    public Vehicle(String car_no) {
        this.car_no = car_no;
    }
    
    @Override
    public int hashCode() {
        // Custom hash code based on a meaningful property 
        // (Ensuring objects that are 'equal' by state have equal hash codes)
        return Integer.parseInt(car_no); 
    }
}
```

#### 3. `public boolean equals(Object obj)`

*   **Purpose:** Indicates whether some other object is "equal to" this one.
*   **Default Behavior:** The default implementation in `Object` performs the **most discriminating equivalence relation**; it returns `true` if and only if both reference variables refer to the **exact same object** in memory (`x == y` is true). It compares object identity, not content.
*   **Override Necessity:** To test if two objects are equal based on their contents or state (functional equality/equivalency), you **must override** this method.
*   **Contract:** Must be reflexive, symmetric, transitive, and consistent.

##### **Java Example: `equals()` vs `==` (Identity vs Content)**

```java
public class RevisionDemo {
    public static void main(String[] args) {
        // Using String, which overrides equals() for content comparison
        String s1 = new String("Java");
        String s2 = new String("Java"); 
        String s3 = s1; 

        // 1. == compares references (memory location)
        System.out.println("s1 == s2: " + (s1 == s2));   // false (Two different objects created via 'new')
        System.out.println("s1 == s3: " + (s1 == s3));   // true (Same reference)

        // 2. equals() compares values/content
        System.out.println("s1.equals(s2): " + s1.equals(s2)); // true (Content "Java" is the same)
    }
}
```

#### 4. `public final Class<?> getClass()`

*   **Purpose:** Returns the **runtime `Class` object** of this instance. It is used to get the class metadata (name, fields, methods, etc.).
*   **Key Property:** It is declared as `public final` and **cannot be overridden**.
*   **Lock:** The returned `Class` object is the one that is locked by static synchronized methods of the represented class.

##### **Java Example: `getClass()`**

```java
// Declaration: public final Class<?> getClass()
import java.util.ArrayList;

public class RevisionDemo {
    public static void main(String[] args) {
        Integer i = Integer.valueOf(10);
        ArrayList<String> list = new ArrayList<>();

        // Retrieve the runtime class of the object
        System.out.println(i.getClass());     
        // Output: class java.lang.Integer
        
        System.out.println(list.getClass());
        // Output: class java.util.ArrayList
    }
}
```

#### 5. `protected Object clone() throws CloneNotSupportedException`

*   **Purpose:** Creates and returns a copy of the current object.
*   **Mechanism (Shallow Copy):** The default implementation performs a **shallow copy**, creating a new instance and copying the content of all fields via assignment. If the fields contain references to other mutable objects, those objects are *not* copied, meaning the original and clone share references to internal objects.
*   **Requirement:** The class must implement the **`Cloneable` interface**; otherwise, calling `clone()` results in a `CloneNotSupportedException`.

##### **Java Example: `clone()`**

```java
// Must implement Cloneable interface
class Employee implements Cloneable { 
    int rollNumber = 100;
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        // Calls Object's shallow copy implementation
        return super.clone(); 
    }
}

public class RevisionDemo {
    public static void main(String[] args) {
        Employee e1 = new Employee();
        try {
            Employee e2 = (Employee) e1.clone(); 
            
            // Check state copy
            System.out.println("e1 roll number: " + e1.rollNumber); // 100
            System.out.println("e2 roll number: " + e2.rollNumber); // 100

            // Check if they are distinct objects (Reference comparison)
            System.out.println("e1 != e2: " + (e1 != e2)); // true (A new object was created)

        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```

#### 6. `protected void finalize() throws Throwable`

*   **Purpose:** Provides an opportunity for an object to perform **cleanup tasks** and release system resources (e.g., I/O connections) before it is destroyed by the Garbage Collector (GC).
*   **Invocation:** It is called by the GC when it determines there are **no more references** to the object. It is guaranteed to be called at most once for any given object.
*   **Reliability Warning:** Developers should not rely on this method for critical resource cleanup because Java does not guarantee when, or even if, `finalize()` will be called.

##### **Java Example: `finalize()`**

```java
public class FinalizeDemo {
    @Override
    protected void finalize() throws Throwable { 
        System.out.println("Finalize method called just before GC collects object.");
        super.finalize(); 
    }

    public static void main(String[] args) {
        FinalizeDemo e = new FinalizeDemo(); 
        e = null; // Object is now unreferenced

        // Explicitly calling the garbage collector (GC might run immediately or later)
        System.gc(); 
        
        // Expected output (if GC runs): Finalize method called just before GC collects object.
    }
}
```

#### 7. Concurrency Methods (`wait`, `notify`, `notifyAll`)

These methods are used in a multi-threading environment for **thread synchronization and communication** related to an object's monitor (lock).

*   **Monitor Requirement:** The calling thread **must own the object's monitor** (lock), typically achieved using a `synchronized` block or method. Failure to own the monitor throws an `IllegalMonitorStateException`.
*   **`public final void wait()` (and variants `wait(long timeout)`, `wait(long timeout, int nanos)`):** Causes the current thread to **release the monitor lock** and enter the waiting state. The thread remains dormant until it is notified, interrupted, or (for timed variants) the specified time elapses. **Waits should always occur in loops** to guard against spurious wakeups.
*   **`public final void notify()`:** Wakes up **a single arbitrary thread** that is waiting on this object's monitor. The awakened thread must re-obtain the lock before proceeding.
*   **`public final void notifyAll()`:** Wakes up **all threads** that are waiting on this object's monitor. All awakened threads compete in the usual manner to obtain the lock.

##### **Java Example: `wait()` and `notify()` Pattern**

```java
// Assumes 'msg' is the shared Message object used as the lock/monitor
class Waiter implements Runnable {
    private Message msg; 
    public Waiter(Message m) { this.msg = m; }
    
    @Override 
    public void run() {
        String name = Thread.currentThread().getName();
        synchronized (msg) { // Must be in synchronized block
            try {
                System.out.println(name + " waiting to get notified...");
                // Thread releases lock and waits
                msg.wait(); 
            } catch(InterruptedException e){ 
                e.printStackTrace(); 
            }
            System.out.println(name + " notified and resuming processing: " + msg.getMsg());
        }
    }
}
class Notifier implements Runnable {
    private Message msg;
    public Notifier(Message msg) { this.msg = msg; }
    
    @Override
    public void run() {
        // Simulate work done
        try { Thread.sleep(1000); } catch (InterruptedException e) { /* handle */ }

        synchronized (msg) { // Must be in synchronized block
            msg.setMsg(Thread.currentThread().getName() + " work done");
            // Wake up waiting thread(s)
            msg.notify(); // or msg.notifyAll();
        }
    }
}
```

---

## Ⅱ. The `java.lang.Class` Class

The `Class` class represents the metadata or blueprint of classes, interfaces, enums, arrays, primitive types, and the keyword `void` within a running Java application. It is the **entry point for Java Reflection**.

### Core Concepts

*   **Representation:** An instance of `Class<T>` models the definition of a type `T`.
*   **Instantiation:** `Class` has **no public constructor**. `Class` objects are constructed automatically by the Java Virtual Machine (JVM) as classes are loaded.

### Ways to Obtain a `Class` Object

1.  **Using `.class` Syntax (Class Literal):**
    *   The simplest way, used when the type name is known but no instance exists.
    *   Example: `Class stringClass = String.class;`.
    *   This is the preferred way to get the `Class` object for **primitive types** (e.g., `boolean.class`).

2.  **Using `Object.getClass()`:**
    *   Used when an object instance is available.
    *   Example: `String greeting = "Hello"; Class stringClass = greeting.getClass();`.

3.  **Using `Class.forName(String className)`:**
    *   A static method that loads a class using its fully qualified name (including the package) provided as a string.
    *   Useful for dynamic loading (e.g., plugins).
    *   Throws `ClassNotFoundException`.
    *   Example: `Class t = Class.forName("java.lang.Thread");`.

4.  **Using `TYPE` Field (for Primitive Wrappers):**
    *   Wrapper classes (like `Integer`, `Double`) contain a static field `TYPE` which returns the `Class` object for the corresponding primitive type.
    *   Example: `Class c = Double.TYPE;` (equivalent to `double.class`).

### Frequently Used `Class` Methods

| Method | Description |
| :--- | :--- |
| **`getName()`** | Returns the **fully qualified name** (e.g., `java.lang.String`). |
| **`getSimpleName()`** | Returns just the **unqualified name** as found in the source code (e.g., `String`). |
| **`getSuperclass()`** | Returns the `Class` object representing the **direct parent class**. Returns `null` for `Object`, interfaces, primitives, or `void`. |
| **`getInterfaces()`** | Returns an array of `Class` objects representing the interfaces directly implemented by this class. |
| **`getMethods()`** | Returns an array of `Method` objects reflecting **all public methods**, including those inherited from superclasses and superinterfaces. |
| **`getDeclaredMethods()`** | Returns an array of `Method` objects reflecting **all methods declared directly** by this class (including private, protected, default), **excluding inherited methods**. |
| **`isPrimitive()`** | Returns `true` if this object represents one of the eight primitive types (`boolean`, `byte`, `char`, `short`, `int`, `long`, `float`, `double`) or `void`. |
| **`isArray()`** | Returns `true` if this object represents an array class. |
| **`isInterface()`** | Returns `true` if this object represents an interface type. |
| **`isEnum()`** | Returns `true` if this class was declared as an enum. |
| **`isInstance(Object obj)`** | The dynamic equivalent of the `instanceof` operator. Checks if `obj` is assignment-compatible with the class represented by this `Class` object. |
| **`isAssignableFrom(Class<?> cls)`** | Checks if the class represented by **this** object is a superclass or superinterface of the specified class `cls`. |
| **`newInstance()`** | Creates a new instance of the class using the nullary constructor (Deprecated in modern Java; prefer using `Constructor.newInstance()`). |

##### **Java Example: Checking Class Properties using Reflection**

```java
import java.util.ArrayList;

class Animal {}
class Dog extends Animal {}

public class ClassReflectionDemo {
    public static void main(String[] args) {
        
        // 1. Basic Information 
        Class<ArrayList> listClass = ArrayList.class;
        System.out.println("Full Name: " + listClass.getName()); // Output: java.util.ArrayList
        System.out.println("Simple Name: " + listClass.getSimpleName()); // Output: ArrayList
        
        // 2. Inheritance and Type Checks
        Class<Dog> dogClass = Dog.class;
        
        // Check Superclass
        System.out.println("Dog's Superclass: " + dogClass.getSuperclass().getSimpleName()); // Output: Animal
        
        // Check Interface
        System.out.println("Implements java.io.Serializable? " + 
                           java.io.Serializable.class.isAssignableFrom(listClass)); // Output: true

        // 3. Dynamic Instantiation (Modern Approach)
        try {
            Class<String> stringClass = String.class;
            // Get constructor accepting a single String argument
            java.lang.reflect.Constructor<String> constructor = 
                stringClass.getConstructor(String.class);
            
            // Create a new instance dynamically
            String dynamicObject = constructor.newInstance("Runtime String");
            System.out.println("Created dynamically: " + dynamicObject);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Analogy

If the **`Object` class** is the **genetic DNA** that every organism starts with (providing basic survival functions like breathing, eating, and communicating), then the **`Class` class** is the **entire genome project** dedicated to studying that DNA—allowing you to inspect the structure, lineage, traits, and functions of the organism itself at any time during its lifetime.



> For more detailed references : 

[Oracle Docs - Object class](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)

[Oracle Docs - Class class](https://docs.oracle.com/javase/8/docs/api/java/lang/Class.html)

[Object class - Scalar Blog](https://www.scaler.com/topics/object-class-in-java/)

[Class class - Scalar Blog](https://www.scaler.com/topics/class-class-in-java/)

