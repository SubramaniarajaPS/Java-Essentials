
---

# Java Records and Pattern Matching Revision Notes

## I. Java Records: Immutable Data Carriers

Java Records are a concise syntax for creating classes whose primary purpose is to hold a fixed set of immutable data. They drastically reduce boilerplate code required for typical data classes (like DTOs or value objects).

### A. Core Functionality and Generated Members

A record is a **transparent carrier for shallowly immutable data**. When you define a record, the Java compiler automatically generates several crucial members.

| Functionality | Detail | Example Code |
| :--- | :--- | :--- |
| **Declaration Syntax** | Use the `record` keyword followed by the components list (the record descriptor). | ``` public record Person(String name, int age) {} ``` |
| **Component Fields** | Private and `final` fields are automatically generated for each component in the header, ensuring immutability. You cannot define non-static instance fields outside the header. | *(Implicitly generated)* |
| **Canonical Constructor** | A public constructor is generated that accepts all components and initializes the corresponding fields. This is the only constructor guaranteed to exist. | ``` Person p = new Person("Alice", 30); ``` |
| **Accessors (Getters)** | Public accessor methods are generated, named identically to the component fields (e.g., `name()` instead of `getName()`). | ``` String personName = p.name(); int personAge = p.age(); ``` |
| **Standard Methods** | Implements robust versions of `equals()`, `hashCode()`, and `toString()`, derived from all component fields. | ```  Prints: Person[name=Alice, age=30] System.out.println(p); ``` |
| **Immutability** | Records are immutable by default and do not generate setter methods. | *(No setters generated)* |

### B. Customization and Constraints

Records allow for some customization but impose strict constraints related to their nature as data carriers.

#### 1. Customizing Constructors

You can customize constructors, primarily for validation or to provide alternative initialization paths.

| Customization Type | Detail | Example Code |
| :--- | :--- | :--- |
| **Compact Canonical Constructor** | Used for argument validation or normalization. It omits the parameter list and explicit field assignments (`this.name = name`). The assignments are automatically performed at the end of the constructor block. | ``` public record PriceRange(double high, double low) { public PriceRange { if (high < low) { throw new IllegalArgumentException("High cannot be less than low"); } } } ``` |
| **Custom (Non-Canonical) Constructors** | Any constructor with a different signature must explicitly call the canonical constructor using `this()` as the first statement. | ``` public record Person(String name, String address) { public Person(String name) { this(name, "Unknown"); // Must call canonical constructor } } ``` |

#### 2. Methods and Static Members

Records can include additional methods and static members.

| Feature | Detail | Example Code |
| :--- | :--- | :--- |
| **Instance Methods** | You can define custom instance methods for utility or calculated values. | ``` public record Point(int x, int y) { public double getMagnitude() { return Math.sqrt(x*x + y*y); } } ``` |
| **Static Variables and Methods** | Records support static variables (often final constants) and static methods. | ``` public record Person(String name, String address) { public static String UNKNOWN_ADDRESS = "Unknown"; public static Person createUnnamed(String address) { return new Person("Unnamed", address); } } ``` |

#### 3. Inheritance and Constraints

Records are highly constrained regarding inheritance:
*   Records **cannot** extend any other class. They implicitly extend `java.lang.Record`.
*   Records are implicitly `final`, meaning they **cannot** be extended by other classes.
*   Records **can** implement interfaces.

### C. Common Use Cases

Records are perfectly suited for modeling data in modern applications:
*   **Data Transfer Objects (DTOs)** for REST APIs.
*   **Configuration Properties**, taking advantage of immutability.
*   **Response Objects** in microservices architectures.
*   Any situation where simple, immutable aggregates of data are needed.

---

## II. Pattern Matching: Data-Driven Dispatch

Pattern Matching allows you to test an object against a pattern and conditionally extract data if the test succeeds. It provides a flexible, data-driven alternative to traditional object-oriented polymorphism (Visitor pattern).

### A. Usage with `instanceof` (Type Patterns)

The `instanceof` operator was enhanced to include **Type Patterns**, combining the type check and casting into a single step.

| Usage Type | Detail | Example Code |
| :--- | :--- | :--- |
| **Type Pattern in `if`** | Checks the type and, if matched, assigns the object to a binding variable of that specific type, eliminating the need for explicit casting. | ``` Object obj = "Hello"; if (obj instanceof String s) { System.out.println(s.length()); // s is automatically String type } ``` |
| **Scope of Pattern Variables** | The pattern variable (`s` in the example above) is only in scope where the pattern is guaranteed to have matched (i.e., inside the `if` block, or after a conditional check like `&&`). | ``` if (!(obj instanceof String s)) { return; } // s is in scope here ``` |

### B. Usage with `switch` (Type, Record, and Guarded Patterns)

Pattern Matching for `switch` allows the selector expression to be of any reference type and uses patterns in the `case` labels.

#### 1. Type and Record Patterns in Switch

| Pattern Type | Detail | Example Code |
| :--- | :--- | :--- |
| **Type Pattern in `switch`** | Matches the type of the switch selector expression. | ``` static double convertToDouble(Object o) { return switch (o) { case Integer i -> i.doubleValue(); case String s -> Double.parseDouble(s); default -> 0d; }; } ``` |
| **Record Pattern (Destructuring)** | Matches a record and automatically deconstructs it, extracting components into new binding variables. Matching is done by position and compatible type. | ``` public record Point(int x, int y) {} public void printPoint(Object obj) { switch (obj) { case Point(int x, int y) -> System.out.println("x:" + x + ", y:" + y); default -> {} } } ``` |
| **Nested Record Patterns** | Allows deconstructing records that contain other records, enabling deep extraction of data. | ``` public record Line(Point start, Point end) {} public void checkLine(Object shape) { if (shape instanceof Line(Point(int x1, int y1), Point(int x2, int y2))) { System.out.println("Start at " + x1 + "," + y1); } } ``` |
| **Using `var`** | Type inference (`var`) can be used for binding variables in patterns. | ``` if (obj instanceof Point(var x, var y)) { // x and y are inferred as int } ``` |

#### 2. Advanced Switch Concepts

| Concept | Detail | Example Code |
| :--- | :--- | :--- |
| **Guarded Patterns** | Uses the `when` clause to apply an additional boolean condition after the initial pattern match. | ``` case String s when s.length() > 5 -> // Only execute if string length > 5 ``` |
| **Null Handling** | `switch` statements/expressions can explicitly handle null values using a dedicated `case null` label, preventing `NullPointerException`. | ``` case null -> System.out.println("Input is null"); ``` |
| **Exhaustiveness** | `switch` expressions must be **exhaustive**, meaning all possible input values must be covered. Failure to cover all possibilities results in a compile-time error. | *(Requires either a `default` case or full coverage using Sealed Types.)* |
| **Sealed Types & Exhaustiveness** | If the selector type is a **Sealed Interface** (which permits a known, finite set of subtypes), the compiler ensures all permitted subtypes are covered in the `case` labels, eliminating the need for a `default` case. | ``` sealed interface Shape permits Circle, Square {} // switch (shape) needs only case Circle c and case Square s ``` |
| **Dominance** | Patterns are matched sequentially. A broader pattern (e.g., matching a supertype like `CharSequence`) must not precede a narrower pattern (e.g., matching a subtype like `String`), or the narrower pattern will be dominated and cause a compilation error. | *(Incorrect Order: `case CharSequence c -> ...; case String s -> ...;`)* |
| **Unnamed Patterns/Variables** | The single underscore (`_`) marks a variable or pattern component that the developer does not care about, increasing readability when extraction is unnecessary. | *(If `Point` has components `(x, y)` but we only need `x`):* ``` case Point(int x, _) -> System.out.println("Only need x: " + x); ``` |

### C. Other Pattern Matching Contexts

| Context | Detail | Example Code |
| :--- | :--- | :--- |
| **Enhanced `for` loop** | Pattern matching can be used in enhanced `for` loops (preview feature) to deconstruct records during iteration. | ``` for (Point(int x, int y) p : listOfPoints) { System.out.println(x + y); } ``` |

---

## Analogy for Records and Pattern Matching

If traditional Java classes are like complex, custom-built safes (requiring boilerplate code for locking/unlocking, comparison, and display, which you might forget or mess up), **Records** are like transparent, standardized, tamper-proof boxes. They hold immutable contents (data) clearly, and Java automatically handles the standard mechanisms (`equals`, `hashCode`, accessors) for you efficiently and correctly.

**Pattern Matching** is the standardized tool (like a smart scanner) used to interact with these boxes. Instead of manually inspecting the type and then grabbing the contents (old `instanceof` with manual cast), Pattern Matching lets you test the box's structure (type or components) and automatically extract the contents you need *in one secure motion*.

---
> For more detailed references : 

[Oracle Docs - Record](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Record.html)

[Baeldung - Record](https://www.baeldung.com/java-record-keyword)

[Medium - Record](https://medium.com/@ksaquib/understanding-java-records-a-comprehensive-guide-448442d8cda9)

[Youtube - Record](https://www.youtube.com/watch?v=gJ9DYC-jswo)

[Youtube - Pattern Matching in Switch](https://www.youtube.com/watch?v=g9tCSRyL3Qw)

[Youtube - Pattern Matching in Record](https://www.youtube.com/watch?v=M_pJGkjCZfU)
