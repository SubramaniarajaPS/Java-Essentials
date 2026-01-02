### **Java Arrays Utility Class Comprehensive Revision Notes**

The **`java.util.Arrays`** class is a member of the **Java Collections Framework** and provides a suite of static methods for manipulating arrays (searching, sorting, comparing, filling, and transforming). It was introduced in **JDK 1.2**.

---

### **1. Core Characteristics and Syntax**
*   **Static Nature:** All methods in this class are **static**, meaning you call them using `Arrays.methodName()` without creating an instance.
*   **Null Handling:** Methods generally throw a **`NullPointerException`** if the specified array reference is null, unless otherwise noted.
*   **Package:** The fully qualified name is **`java.util.Arrays`**.

---

### **2. Creating and Copying Arrays**
These methods allow for duplicating arrays, changing their size, or selecting specific subranges.
*   **`copyOf(original, newLength)`:** Copies the specified array. If the `newLength` is greater than the original, the new elements are padded with **zeros** (numeric), **nulls** (objects), or **false** (boolean). If shorter, the array is truncated.
*   **`copyOfRange(original, from, to)`:** Copies a specific range from index `from` (inclusive) to `to` (exclusive) into a new array.

#### **Code Examples: Copying**
```java
String[] intro = {"once", "upon", "a", "time"};

// Copy a range: [once, upon, a]
String[] abridgement = Arrays.copyOfRange(intro, 0, 3); 

// Copy and resize: [once, upon, a, time, null]
String[] expanded = Arrays.copyOf(intro, 5); 

// Truncate: [once, upon]
String[] truncated = Arrays.copyOf(intro, 2);
```
**Explanation:** `copyOfRange` takes the source array, the start index (inclusive), and the end index (exclusive). `copyOf` creates a new array of exactly the specified length, padding with default values if necessary.

---

### **3. Initializing and Filling Arrays**
*   **`fill(array, value)`:** Assigns a single value to **every element** of the array.
*   **`fill(array, from, to, value)`:** Assigns the value only to a specific subrange.
*   **`setAll(array, generator)`:** Uses a functional interface (generator) to compute each element based on its index.
*   **`parallelSetAll(array, generator)`:** Similar to `setAll`, but performs the operation **in parallel**.

#### **Code Examples: Filling and Setting**
```java
int[] data = new int;

// Fill with 10:
Arrays.fill(data, 10); 

// Range fill: (index 2 to 4 exclusive)
Arrays.fill(data, 2, 4, 7); 

// Using setAll with a lambda:
Arrays.setAll(data, i -> i * 2);
```
**Explanation:** `fill` is used for constant values. `setAll` provides a more dynamic way to initialize an array where the value depends on the index, though it may leave the array in an indeterminate state if the generator throws an exception.

---

### **4. Sorting and Searching**
*   **`sort(array)`:** Sorts the array into ascending numerical/natural order.
    *   **Implementation Note:** Primitives use **Dual-Pivot Quicksort** (O(n log(n))). Objects use **TimSort** (a stable, adaptive iterative mergesort).
*   **`parallelSort(array)`:** Introduced in **Java 8**, it uses a parallel sort-merge algorithm for larger arrays to improve performance.
*   **`binarySearch(array, key)`:** Searches for a value using the binary search algorithm.
    *   **Constraint:** The array **must be sorted** beforehand, or the results are undefined.
    *   **Return Value:** Returns the index if found; otherwise, returns **`(-(insertion point) - 1)`**.

#### **Code Examples: Sorting and Searching**
```java
int[] list = {50, 10, 30, 20, 40};
Arrays.sort(list); //

// Search for existing element
int index = Arrays.binarySearch(list, 30); // returns 2

// Search for missing element (7 would be at index 0)
int missing = Arrays.binarySearch(list, 7); // returns -0 - 1 = -1
```
**Explanation:** `sort` mutates the original array. `binarySearch` is highly efficient (O(log n)) but fails if the array is unsorted.

---

### **5. Comparing and Mismatch Detection**
*   **`equals(array1, array2)`:** Returns `true` if both arrays have the same length and contain the same elements in the same order.
*   **`deepEquals(array1, array2)`:** Used for **nested or multidimensional arrays**. It recursively checks sub-arrays for equality.
*   **`compare(array1, array2)`:** Lexicographical comparison. Returns `0` if equal, `<0` if array1 is "less," and `>0` if "greater".
*   **`compareUnsigned(array1, array2)`:** Treats elements as **unsigned** during lexicographical comparison.
*   **`mismatch(array1, array2)`:** Finds the index of the **first mismatch**; returns `-1` if the arrays are identical.

#### **Code Examples: Comparison**
```java
int[] a = {1, 2, 3};
int[] b = {1, 2, 3};
int[] c = {1, 2, 4};

Arrays.equals(a, b);   // true
Arrays.compare(a, c);  // -1 (3 is less than 4)
Arrays.mismatch(a, c); // 2 (index where they differ)

// Multidimensional example
int[][] multi1 = {{1, 2}, {3, 4}};
int[][] multi2 = {{1, 2}, {3, 4}};
Arrays.equals(multi1, multi2);     // false (compares references)
Arrays.deepEquals(multi1, multi2); // true (compares contents)
```
**Explanation:** `equals` is sufficient for 1D arrays, while `deepEquals` is mandatory for nested arrays to compare content rather than sub-array references. `mismatch` is useful for pinpointing exactly where two datasets diverge.

---

### **6. Transformation and Stream Integration**
*   **`asList(T... a)`:** Returns a **fixed-size list** backed by the specified array. Changes to the list write through to the array and vice versa.
*   **`toString(array)`:** Returns a string representation (e.g., ``).
*   **`deepToString(array)`:** Recursively converts nested arrays into strings, designed for multidimensional arrays.
*   **`stream(array)`:** Converts an array into a sequential **Stream**.
*   **`parallelPrefix(array, operator)`:** Cumulates each element in place using an associative function (e.g., prefix sums).

#### **Code Examples: Transformation**
```java
String[] cities = {"London", "Paris", "Tokyo"};

// Array to List
List<String> cityList = Arrays.asList(cities); 
// cityList.add("NYC"); // Throws UnsupportedOperationException

// Array to String
System.out.println(Arrays.toString(cities)); // [London, Paris, Tokyo]

// Parallel Prefix (Summing)
int[] nums = {1, 2, 3, 4};
Arrays.parallelPrefix(nums, (x, y) -> x + y); //
```
**Explanation:** `asList` creates a "view" of the array; it does not support operations that change the size (like `add` or `remove`). `parallelPrefix` is highly efficient for large-scale cumulative operations on multi-core systems.

---

### **7. Content-Based Hash Codes**
*   **`hashCode(array)`:** Returns an integer hash code based on the array's contents.
*   **`deepHashCode(array)`:** Computes hash codes for nested arrays recursively.

#### **Key Interview Concepts**
*   **Stability:** `Arrays.sort(Object[])` is guaranteed to be **stable** (equal elements are not reordered).
*   **O(n log(n)):** This is the average performance for sorting algorithms used in this class.
*   **Fixed-Size List:** Remember that `Arrays.asList()` returns a list that cannot be structurally modified.
*   **Floating Point Sort:** For `float` and `double`, `-0.0` is treated as less than `0.0`, and `NaN` is considered equal to itself and greater than any other value.

---

> For more detailed references : 

[Java Arrays Class - Scientecheasy Blog](https://www.scientecheasy.com/2021/09/arrays-class-in-java.html/)

[Java Arrays Class - Baledung Blog](https://www.baeldung.com/java-util-arrays)