This comprehensive guide details the key concepts, functionalities, and implementation of Java's Sealed Classes, Generics, and Enums, designed for efficient last-minute revision.

---

## I. Sealed Classes

Sealed classes and interfaces, introduced officially in Java SE 17 (preview in JDK 15 and refined in JDK 16), provide a mechanism to control inheritance by explicitly listing which classes or interfaces are permitted to extend or implement them.

### 1. Core Concepts and Motivation

| Feature | Description |
| :--- | :--- |
| **Definition** | A class or interface that restricts which other classes or interfaces may extend or implement it. They provide fine-grained control over the inheritance hierarchy. |
| **Purpose** | To define a **closed hierarchy**. This contrasts with standard Java classes (which allow unlimited subclasses) and `final` classes (which allow none). |
| **Benefits** | **Controlled Extensibility:** Only specific, known classes can extend the base class, improving code safety and security, especially in public APIs. **Domain Modeling:** Allows developers to represent specific possibilities that exist in a domain (e.g., only `Car` and `Truck` extend `Vehicle`). **Exhaustive Pattern Matching:** Enables better compile-time checks for switch/case patterns. |
| **Accessibility** | The main motivation is to allow a superclass to be widely accessible but not widely extensible. |

### 2. Sealed Class Syntax and Implementation

A sealed class or interface must use the `sealed` keyword and the `permits` clause. The `permits` clause must be the last modifier, following any `extends` or `implements` clauses.

#### A. Defining a Sealed Type

To seal a class or interface, you use the `sealed` modifier followed by the `permits` clause listing all allowed subtypes:

```java
// Sealed Interface Example
public sealed interface Service permits Car, Truck { 
    int getMaxServiceIntervalInMonths();
}

// Sealed Class Example
public abstract sealed class Vehicle permits Car, Truck { 
    protected final String registrationNumber; 
    // Constructor and other methods...
}
```
*   **Constraint:** You cannot use a wildcard (`*`) in the `permits` list; the permitted types must be explicitly named.
*   **Location Constraint:** All permitted subclasses must belong to the **same module** as the sealed class. If working outside of Java modules (unnamed module), they must be in the **same package**.

#### B. Permitted Subclass Modifiers

Every permitted subclass of a sealed class/interface **must** explicitly define one of three modifiers to declare how the hierarchy continues below it:

1.  **`final`**: The class cannot be extended any further. (Interfaces cannot be final).

    *Example:*
    ```java
    public final class Truck extends Vehicle implements Service {
        // Implementation... 
    } 
    ```

2.  **`sealed`**: The class itself is also sealed, meaning it can only be extended by a restricted set of descendants defined in its own `permits` clause.

    *Example:*
    ```java
    public sealed class Dog extends Animal permits Labrador {} 
    ```

3.  **`non-sealed`**: The inheritance hierarchy is opened up, and any class can extend this class freely.

    *Example:*
    ```java
    public non-sealed class Car extends Vehicle implements Service {
        // Implementation... 
    }
    ```

### 3. Functionalities and Checks

#### A. Compiler Checks and Pattern Matching

The primary benefit of knowing all possible subclasses at compile time is enabling exhaustive checking, often utilized with `switch` expressions.

When a `switch` expression uses a sealed class or interface, the compiler can verify that every permitted subclass is covered, potentially eliminating the need for a `default` clause.

*Example:*
```java
switch (shape) {
    case Circle c -> // logic for Circle
    case Rectangle r -> // logic for Rectangle
    // No default needed because the compiler knows all subtypes
}
```
If the hierarchy were not sealed, the compiler would require a `default` clause because it could not guarantee that all possible input values were covered.

#### B. Reflection API Support

Sealed classes are supported by the reflection API via two public methods added to `java.lang.Class`:
1.  `isSealed()`: Returns `true` if the given class or interface is sealed.
2.  `getPermittedSubclasses()`: Returns an array of objects representing all permitted subclasses.
---

## II. Generics

Generics, introduced in Java 5, enable types (classes and interfaces) to be parameters when defining classes, interfaces, and methods. This concept is often referred to as parameterized types.

### 1. Generics: Why and How

| Feature | Description |
| :--- | :--- |
| **Goal** | To provide a strongly type check at **compile time**. This reduces bugs and adds abstraction over types. |
| **Problem Solved** | Eliminates code duplication by allowing one class to be flexible for many different types. Solves the lack of type safety and the need for manual casting common in non-generic collections (like `ArrayList` holding `Object`). |
| **Benefits** | **Type Safety:** Prevents `ClassCastException` at runtime by shifting type checking to compile time. **Code Reusability:** Write generic algorithms once. **Elimination of Casts:** Reduces explicit type casting. |
| **Syntax** | Uses angle brackets (`<>`) to define type parameters. |

### 2. Generic Declarations and Syntax

| Concept | Syntax Example | Explanation |
| :--- | :--- | :--- |
| **Generic Class** | `public class Box<T> { ... }` | `T` (Type parameter) is a placeholder used within the class definition. Classes can have multiple parameters, e.g., `<K, V>`. |
| **Generic Interface** | `public interface Pair<K, V> { ... }` | Interfaces can also be parameterized over types. |
| **Generic Method** | `public static <T> void printArray(T[] array)` | The type parameter declaration (`<T>`) is placed *before* the return type in the method signature. Generic methods can exist even in non-generic classes. |
| **Invocation/Instantiation** | `Box<Integer> integerBox = new Box<Integer>();` | The concrete type (`Integer`—the type argument) replaces the parameter placeholder. |
| **Diamond Operator** | `Box<Integer> integerBox = new Box<>();` | Introduced in Java 7+, allows omitting the type arguments in the constructor if the compiler can infer them from the context. |

#### Type Parameter Naming Convention

Type parameters are conventionally represented by single, uppercase letters to distinguish them from regular class or interface names:
*   **T:** Type (most common)
*   **E:** Element (used heavily in Collections Framework)
*   **K, V:** Key, Value (used in Maps/Pairs)
*   **N:** Number

### 3. Advanced Generics: Bounded Types and Wildcards

Bounded types restrict the types that can be used, while wildcards represent unknown types, providing flexibility and control over how data is read or written.

#### A. Type Parameters with Bounds (T extends Bound)

This restricts `T` to be a subtype of a specific class or to implement a specific interface.

*   **Syntax:** `<T extends Bound>`.
    *   The keyword `extends` is used both for classes (subclassing) and interfaces (implementing).
    *   **Benefit:** Allows access to methods defined in the upper bound.

*Example: Restricting to number types*
```java
// T must be Number or any subclass of Number
public <T extends Number> List<T> fromArrayToList(T[] a) { ... }

// This allows calling methods like doubleValue() on T.
```

*   **Multiple Bounds:** A type can have multiple upper bounds using `&`.
    *   **Constraint:** If one of the bounds is a class, it must be listed **first**.

*Example: Multiple Bounds*
```java
<T extends Number & Comparable>
```

#### B. Wildcards (?)

The wildcard (`?`) represents an unknown type, used primarily when defining generic parameters or fields, rather than defining the generic type itself.

| Wildcard Type | Syntax | Usage & Write Safety |
| :--- | :--- | :--- |
| **Unbounded** | `List<?>` | Represents an unknown type. **Read-only operations are safe.** Cannot add items (except `null`) because the specific type is unknown, preventing type safety issues. |
| **Upper Bounded** | `List<? extends T>` | **`T` or any subtype (child).** Permits reading (consuming) elements out of the list, as they are guaranteed to be at least of type `T`. Cannot write to the list. |
| **Lower Bounded** | `List<? super T>` | **`T` or any supertype (parent).** Permits writing (producing) elements of type `T` into the list, as `T` is guaranteed to be compatible with the list's type. Reading elements requires casting to `Object`. |
| **Wildcard vs. Type Parameter** | `?` vs. `T` | `T` names the type (used when returning or enforcing relationships). `?` represents an unknown type (used when flexibility or polymorphism is needed). Only Wildcards support the `super` keyword. |

### 4. Constraints and Type Erasure

#### A. Generics Constraints

1.  **No Primitive Types:** Generics only work with **non-primitive reference types**. Wrapper classes (like `Integer`, `Double`) must be used for primitive types.
2.  **Instantiation:** You cannot create instances of type parameters directly (e.g., `new T()`) because the exact type is erased at runtime.
3.  **Static Members:** Static fields and methods cannot reference the generic type parameter of the class, as static members are shared across all instances regardless of the type parameter.
4.  **Exceptions:** Java does not support generic exceptions because the generic type information is not available to the JVM at runtime, making them impossible to catch specifically.

#### B. Type Erasure

Type erasure is a process applied during compilation where all type parameters are removed and replaced with their bounds or with `Object` if unbounded. This mechanism maintains backward compatibility with older Java versions.

| Scenario | Code Before Compilation (Conceptual) | Code After Type Erasure (Internal) |
| :--- | :--- | :--- |
| **Unbounded Type (`T`)** | `public class Node<T> { T data; }` | `public class Node { Object data; }` |
| **Bounded Type (`T extends Bound`)** | `public <T extends Number> void method(T t)` | `public void method(Number t)` |

After erasure, the compiler inserts necessary type casts to maintain type safety when retrieving objects, ensuring the code works correctly at runtime despite the loss of generic information.

---

## III. Enums

Enums (enumerations) are a special data type/class introduced in Java 5 that represents a fixed set of named constants.

### 1. Fundamentals and Structure

| Feature | Description |
| :--- | :--- |
| **Definition** | A predefined, fixed set of named constants. Ideal for things like days of the week, directions, or status codes. |
| **Nature** | An enum declaration defines a **class**. The named constants are actually **objects** (instances) of that enum class. |
| **Inheritance** | Enums implicitly extend the abstract class `java.lang.Enum`. They cannot extend any other class. |
| **Type Safety** | Enums are type-safe; variables can only be assigned one of the predefined constants. |
| **Syntax** | Declared using the `enum` keyword. Constants are listed in a comma-separated list, conventionally in **uppercase**. A semicolon is required if methods, fields, or constructors are added after the constants. |

*Example:*
```java
public enum Day { 
    SUNDAY, MONDAY, TUESDAY;
}
```

### 2. Functionalities and Usage

#### A. Instantiation and Comparison

*   **Accessing Constants:** Instances cannot be created using the `new` keyword. You access constants directly via the enum type name:

    ```java
    Day today = Day.MONDAY;
    ```

*   **Comparison:** Because the JVM guarantees that only one instance of each enum constant exists, you can safely use the `==` operator for comparison. This avoids potential runtime `NullPointerException` if comparing a null enum variable using the `equals()` method.

#### B. Fields, Constructors, and Custom Methods

Enums are powerful because they can define custom members, treating each constant like a specific object.

*   **Fields:** Can have private instance variables to store additional information associated with each constant.

*   **Constructors:** Used to initialize fields. They are **implicitly private** (or package-private) and cannot be invoked manually. They are called automatically for every constant when the enum is loaded.

*Example: Enum with Fields and Constructor*
```java
public enum Season { 
    WINTER("Cold"), SPRING("Warm"); 

    private final String description; // Field

    // Private Constructor
    Season(String description) { 
        this.description = description; 
    } 

    public String getDescription() { 
        return description; 
    } 
}
```

#### C. Abstract Methods and Strategy Pattern

Enums can define abstract methods, which forces every constant to provide a unique implementation for that method. This is useful for implementing the Strategy Pattern.

*Example: Operation Enum*
```java
public enum Operation {
    ADD {
        @Override
        public int apply(int x, int y) { return x + y; }
    },
    // ... other operations 
    MULTIPLY {
        @Override
        public int apply(int x, int y) { return x * y; } 
    };
    
    public abstract int apply(int x, int y); // Abstract method
}
```

### 3. Inbuilt Functions and Methods

All enums inherit useful methods from `java.lang.Enum`:

| Method | Type | Description |
| :--- | :--- | :--- |
| **`values()`** | Static | Returns an **array** containing all enum constants in their declaration order. Used commonly with enhanced `for` loops for iteration. |
| **`valueOf(String name)`** | Static | Returns the enum constant matching the specified string name. Throws `IllegalArgumentException` if no match is found. |
| **`ordinal()`** | Instance | Returns the zero-based position (index) of the constant in its declaration order (starts at 0). |
| **`name()`** | Instance | Returns the exact name of the constant as a `String`. |
| **`toString()`** | Instance | By default, returns the same value as `name()`. Can be overridden for customization. |

*Example using inbuilt methods:*
```java
// Iterating all constants
for (Status s : Status.values()) {
    System.out.println(s + " has ordinal: " + s.ordinal()); 
    // Example output: RUNNING has ordinal: 0
} 
```

### 4. Advanced Data Structures and Patterns

*   **`EnumSet`**: A specialized, highly efficient `Set` implementation tailored for use with enum types. It is more compact and efficient than `HashSet`.
    *Example: `EnumSet.of(Day.SATURDAY, Day.SUNDAY)`

*   **`EnumMap`**: A specialized, efficient `Map` implementation that must use enum constants as keys. Internally represented as an array, offering performance gains over `HashMap`.

*   **Switch Statements:** Enums work well with both traditional and modern (Java 12+) `switch` statements and expressions, improving readability and allowing the compiler to potentially check for exhaustive coverage.

*   **Singleton Pattern:** Enums provide a simple, thread-safe, and serialization-safe way to implement the Singleton pattern by defining an enum with only a single constant (`INSTANCE`).


---

> For more detailed references : 

[Sealed Class - Telusko Yotube](https://www.youtube.com/watch?v=glvYULuaf-k)

[Sealed Classes & Interface - Will Tollefson Yotube](https://www.youtube.com/watch?v=ZSm1ibc7Yxs)

[Guide to ENUM - Medium Blog](https://medium.com/@pratik.941/comprehensive-guide-to-enums-in-java-58dab564580a)

[ENUMS - dev.java Blog](https://dev.java/learn/classes-objects/enums/)

[ENUM - Will Tollefson Youtube](https://youtu.be/J6Zpt9rb23s?list=TLGGzZTCmzvj_eMwOTEyMjAyNQ)

[ENUM - Telusko Youtube](https://youtu.be/3iyKhywOZeg?list=TLGGQFFZW7qQZwAwOTEyMjAyNQ)

[ENUM - Engineering Digest Youtube](https://youtu.be/rAlplrI8Y3Q?list=TLGGzdNSyeOTI6YwOTEyMjAyNQ)

[Generics - Bro Code Youtube](https://www.youtube.com/watch?v=H9vc4gTtGGA)

[Generics - Coding with John Youtube](https://www.youtube.com/watch?v=K1iu1kXkVoA)