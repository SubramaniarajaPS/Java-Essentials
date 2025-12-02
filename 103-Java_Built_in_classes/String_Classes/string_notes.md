## String Handling Classes: String, StringBuffer, and StringBuilder

This revision note covers the core concepts, functionalities, and methods of Java's primary character sequence handling classes: `String`, `StringBuffer`, and `StringBuilder`.

---

### I. Fundamentals and Characteristics

| Class | Immutability | Thread Safety/Synchronization | Primary Use Case |
| :--- | :--- | :--- | :--- |
| **String** | **Immutable** (Cannot be changed after creation) | **Thread-Safe** (due to immutability) | Storing constant character sequences. Used frequently for literal values, often benefiting from the String Pool for memory optimization. |
| **StringBuffer** | **Mutable** (Can be modified) | **Thread-Safe** (methods are synchronized) | When strings must be modified often, especially in a multi-threaded environment where concurrent access is a concern. |
| **StringBuilder** | **Mutable** (Can be modified) | **Not Thread-Safe** (No synchronization) | When strings must be modified often in a single-threaded environment. Offers better performance than `StringBuffer` because it avoids synchronization overhead. |

**Note on Object Creation:**
When a `String` is modified (e.g., concatenated), a **new** `String` object is created in memory, and the old one remains untouched (immutability). `StringBuffer` and `StringBuilder` modify the sequence **in place**, which is more efficient for heavy manipulation.

**Example of Immutability vs. Mutability:**

```java
// String (Immutable)
String name = "Kunal"; // Object 1: "Kunal" created
name = name + " Kushwaha"; // Object 2: "Kunal Kushwaha" created. Object 1 becomes eligible for garbage collection.

// StringBuilder (Mutable)
StringBuilder sbl = new StringBuilder("Java"); // Single object created
sbl.append(" Programming"); // Object modified in place.
```

#### Constructors

All three classes offer multiple constructors for initialization:

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| `ClassName()` | Creates an empty sequence (initial capacity of 16 for Buffer/Builder). | String, StringBuffer, StringBuilder |
| `ClassName(String str)` | Initializes with the characters of the specified string. (Capacity = length + 16 for Buffer/Builder). | String, StringBuffer, StringBuilder |
| `ClassName(CharSequence seq)` | Initializes with the characters of a specified `CharSequence`. (Since 1.5) | String, StringBuffer, StringBuilder |
| `ClassName(int capacity)` | Creates an empty sequence with the specified initial capacity. | StringBuffer, StringBuilder |
| `String(char[] value)` | Initializes a String by copying the content of a character array. | String |
| `String(StringBuffer buffer)` / `String(StringBuilder builder)` | Initializes a String by copying the content of a StringBuffer or StringBuilder. | String |

**Coding Examples (Constructors):**

```java
// String Constructors
String s1 = new String();         // Empty string
String s2 = new String("Example"); // From literal string
char[] chars = {'H', 'e', 'l', 'l', 'o'};
String s3 = new String(chars);   // From char array

// StringBuffer and StringBuilder Constructors
StringBuffer sb1 = new StringBuffer(); // Capacity 16
StringBuilder sbl1 = new StringBuilder(32); // Custom capacity 32
StringBuffer sb2 = new StringBuffer("Initial"); // Capacity 7 + 16 = 23
```

---

### II. Length, Capacity, and Sizing Methods

These methods deal with the size of the character sequence and the internal storage buffer.

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| **`length()`** | Returns the number of characters in the sequence. | String, StringBuffer, StringBuilder |
| **`capacity()`** | Returns the current storage capacity of the internal buffer. | StringBuffer, StringBuilder |
| **`isEmpty()`** | Returns `true` if the length is 0. (Since 1.6) | String |
| **`isBlank()`** | Returns `true` if the string is empty or contains only white space codepoints. (Since 11) | String |
| **`setLength(int newLength)`** | Sets the character sequence length. If increased, null characters (`\u0000`) are appended. If decreased, the string is truncated. | StringBuffer, StringBuilder |
| **`ensureCapacity(int minCapacity)`** | Guarantees that the internal buffer can store at least `minCapacity` characters. If current capacity is less, it grows to `minCapacity` or (2 * old capacity + 2), whichever is larger. | StringBuffer, StringBuilder |
| **`trimToSize()`** | Attempts to reduce storage space by resizing the buffer to match the current length. | StringBuffer, StringBuilder |

**Coding Examples (Length and Capacity):**

```java
String s = "Interview";
StringBuffer sb = new StringBuffer("Prep");

// Length and Status
int len = s.length(); // 9
boolean empty = s.isEmpty(); // false
boolean blank = s.isBlank(); // false (contains non-whitespace)

// Capacity Management (Buffer/Builder)
int cap = sb.capacity(); // 4 (Prep) + 16 (default) = 20
sb.append(" Time");
sb.ensureCapacity(50); // Ensures capacity is at least 50
sb.setLength(2); // String becomes "Pr\u0000\u0000..." if length > current length, but here it truncates to "Pr"

// Output example (assuming we re-initialize sb after setLength):
StringBuffer sb_capacity = new StringBuffer("abc");
System.out.println(sb_capacity.capacity()); // Prints 19 (3 + 16)
sb_capacity.ensureCapacity(100);
System.out.println(sb_capacity.capacity()); // Prints >= 100
```

---

### III. Character Access and Inspection Methods

These methods are used for examining individual characters or Unicode code points within the sequence.

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| **`charAt(int index)`** | Returns the `char` value at the specified index (0 to length - 1). | String, StringBuffer, StringBuilder |
| **`codePointAt(int index)`** | Returns the character (Unicode code point) at the specified index. | String, StringBuffer, StringBuilder |
| **`codePointBefore(int index)`** | Returns the Unicode code point before the specified index (index 1 to length). | String, StringBuffer, StringBuilder |
| **`codePointCount(int begin, int end)`** | Returns the number of Unicode code points in the specified text range. | String, StringBuffer, StringBuilder |
| **`offsetByCodePoints(int index, int offset)`**| Returns the index offset from the given index by `codePointOffset` code points. | String, StringBuffer, StringBuilder |

**Coding Examples (Character Access):**

```java
String str = "Hello";
int index = 1;

char letter = str.charAt(index); // 'e' (index 1)

// Example for finding character index:
String data = "Naveen";
int charIndex = data.indexOf('a'); // 1
int lastCharIndex = data.lastIndexOf('e'); // 4

// Code Point Example (Unicode handling)
int cp = data.codePointAt(0); // Returns the Unicode code point of 'N'
```

---

### IV. String Manipulation Methods

This category includes methods that modify the content of the mutable sequences (Buffer/Builder) or create new modified strings (String).

#### IV A. Mutating Methods (StringBuffer & StringBuilder)

These methods change the object's content *in place*. They typically return a reference to the modified object (`this`).

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| **`append(...)`** | Appends the string representation of various types (Object, String, char[], int, boolean, etc.) to the end of the sequence. | StringBuffer, StringBuilder |
| **`insert(int offset, ...)`** | Inserts the string representation of various types at the specified index. | StringBuffer, StringBuilder |
| **`delete(int start, int end)`** | Removes characters in the substring from `start` (inclusive) to `end` (exclusive). | StringBuffer, StringBuilder |
| **`deleteCharAt(int index)`** | Removes the character at the specified position. | StringBuffer, StringBuilder |
| **`replace(int start, int end, String str)`**| Replaces the characters in the substring from `start` (inclusive) to `end` (exclusive) with the specified String `str`. | StringBuffer, StringBuilder |
| **`reverse()`** | Replaces the sequence with its reverse. (Treats surrogate pairs as single characters.) | StringBuffer, StringBuilder |
| **`setCharAt(int index, char ch)`** | Sets the character at the specified index to `ch`. | StringBuffer, StringBuilder |
| **`appendCodePoint(int codePoint)`**| Appends the string representation of the Unicode code point argument. | StringBuffer, StringBuilder |
| **`repeat(int codePoint, int count)`** / `repeat(CharSequence cs, int count)` | Repeats a code point or CharSequence `count` times and appends it to the sequence. (Since 21) | StringBuffer, StringBuilder |

**Coding Examples (Mutating Methods):**

```java
StringBuilder mutable = new StringBuilder("ABC");

// 1. Append
mutable.append(" DEF"); // ABC DEF

// 2. Insert (at offset 3, after 'C')
mutable.insert(3, " X"); // ABC X DEF

// 3. Delete
mutable.delete(0, 4); // X DEF (Removes ABC and space)

// 4. Delete Char At
mutable.deleteCharAt(0); // DEF (Removes 'X')

// 5. Replace (Replaces index 1 to 2 ('E') with "G")
mutable.replace(1, 2, "G"); // DGF

// 6. Reverse
mutable.reverse(); // FGD

// 7. Set Char At
mutable.setCharAt(1, 'L'); // FLD

// 8. Append complex types
StringBuilder sbl_append = new StringBuilder();
sbl_append.append(true); // "true"
sbl_append.append(123); // "true123"
sbl_append.append('k'); // "true123k"

// 9. Repeat (since Java 21)
StringBuilder rep_sb = new StringBuilder();
rep_sb.repeat("hi", 3); // "hihihi"
```

#### IV B. Non-Mutating Methods (String)

These methods return a **new** `String` object representing the modification.

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| **`concat(String str)`** | Concatenates the specified string to the end of this string (equivalent to using the `+` operator). | String |
| **`replace(char oldChar, char newChar)`**| Returns a new string resulting from replacing all occurrences of `oldChar` with `newChar`. | String |
| **`replace(CharSequence target, CharSequence replacement)`** | Replaces every substring matching the literal `target` sequence with the `replacement` sequence. | String |
| **`replaceFirst(String regex, String replacement)`**| Replaces the *first* substring that matches the given regular expression. | String |
| **`replaceAll(String regex, String replacement)`**| Replaces *all* substrings that match the given regular expression. | String |
| **`toLowerCase()` / `toUpperCase()`** | Converts all characters to lower/upper case (using the default locale). Locale-specific versions also available. | String |
| **`trim()`** | Returns a string with all leading and trailing space (<= 'U+0020') removed. | String |
| **`strip()`** / `stripLeading()` / `stripTrailing()` | Returns a string with all leading/trailing Unicode white space removed. (Since 11). | String |
| **`indent(int n)`** / `stripIndent()` | Adjusts or removes indentation from lines. (Since 12/15). | String |
| **`repeat(int count)`** | Returns a new string whose value is the concatenation of this string repeated `count` times. (Since 11). | String |
| **`translateEscapes()`** | Returns a string with escape sequences (like `\n`, `\t`) translated. (Since 15). | String |
| **`transform(Function<? super String, ? extends R> f)`** | Applies a function to the string. (Since 12). | String |

**Coding Examples (Non-Mutating Methods):**

```java
String original = "  Hello Java  ";

// 1. Concatenation
String s_concat = original.concat("!"); // "  Hello Java  !"

// 2. Replacement
String s_replace = original.replace('a', 'o'); // "  Hello Jovo  "
String s_rep_seq = original.replace("Java", "World"); // "  Hello World  "

// 3. Trimming
String s_trim = original.trim(); // "Hello Java" (Removes ASCII space)
String s_strip = original.strip(); // "Hello Java" (Removes Unicode whitespace)

// 4. Case conversion
String s_lower = original.toLowerCase(); // "  hello java  "

// 5. Regular Expression replacement
String regex_s = "123x456";
String first_rep = regex_s.replaceFirst("\\d", "Y"); // "Y23x456" (Replaces first digit)
String all_rep = regex_s.replaceAll("\\d", "Z"); // "ZZZxZZZ" (Replaces all digits)

// 6. Repetition
String s_repeat = "Java".repeat(3); // "JavaJavaJava"
```

---

### V. Search, Comparison, and Locating Subsequences

These methods examine string content to find substrings, compare values, or check starting/ending patterns.

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| **`indexOf(...)`** | Returns the index of the first occurrence of a specified character (int) or substring (String), optionally starting from a given index (`fromIndex`). | String, StringBuffer, StringBuilder |
| **`lastIndexOf(...)`**| Returns the index of the last occurrence of a specified character (int) or substring (String), searching backward from a given index. | String, StringBuffer, StringBuilder |
| **`contains(CharSequence s)`** | Returns `true` if this string contains the specified sequence of characters. | String |
| **`startsWith(String prefix)`** / `startsWith(String prefix, int offset)` | Tests if the string begins with the specified prefix (optionally at a starting offset). | String |
| **`endsWith(String suffix)`** | Tests if the string ends with the specified suffix. | String |
| **`equals(Object anObject)`** | Compares this string to the specified object. Returns true only if the object is a String and represents the same sequence of characters. | String |
| **`equalsIgnoreCase(String anotherString)`** | Compares two strings, ignoring case differences. | String |
| **`contentEquals(CharSequence cs)` / `contentEquals(StringBuffer sb)`** | Compares the string to a specified CharSequence or StringBuffer. | String |
| **`compareTo(String anotherString)`** / `compareToIgnoreCase(String str)` | Compares two strings lexicographically. Returns 0 if equal, negative if this string precedes the argument, positive if it follows. | String |
| **`compareTo(StringBuffer/StringBuilder another)`** | Compares two mutable sequences lexicographically. (Since 11) | StringBuffer, StringBuilder |
| **`regionMatches(int toffset, String other, int ooffset, int len)`** / `regionMatches(boolean ignoreCase, ...)` | Tests if a specified region of this string matches a region of another string. | String |
| **`matches(String regex)`** | Tells whether or not this string matches the given regular expression. | String |

**Coding Examples (Search and Comparison):**

```java
String text = "Coding Interview Prep";
String other = "coding interview prep";

// 1. Search (indexOf/lastIndexOf)
int firstD = text.indexOf('I'); // 7
int secondI = text.indexOf('i', 8); // 12
int lastP = text.lastIndexOf('P'); // 19

// 2. Contains and Matches
boolean hasCode = text.contains("Code"); // true
boolean matchesPattern = text.matches(".*Prep"); // true (Checks if it ends with Prep)

// 3. Prefix/Suffix
boolean starts = text.startsWith("Coding"); // true
boolean startsAtOffset = text.startsWith("ing", 3); // true
boolean ends = text.endsWith("Prep"); // true

// 4. Comparison
boolean eq = text.equals(other); // false (Case sensitive)
boolean eq_ignore = text.equalsIgnoreCase(other); // true

// 5. ContentEquals (compares sequence content)
StringBuffer sb_text = new StringBuffer("Coding Interview Prep");
boolean content_eq = text.contentEquals(sb_text); // true

// 6. Comparison (Lexicographical)
int comparison = text.compareTo(other); // A negative value (because 'C' comes before 'c' in Unicode, but since 'c' is ASCII 99 and 'C' is 67, 'C' is lexicographically smaller than 'c', so 'text' precedes 'other' when comparing using character values)

// 7. Region Matches (checking regions)
// toffset=7 ('I'), ooffset=8 ('i'), len=9 ("Interview")
boolean region = text.regionMatches(7, other, 8, 9); // false (Case sensitive mismatch 'I' vs 'i')
boolean region_ignore = text.regionMatches(true, 7, other, 8, 9); // true
```

---

### VI. Substring and Subsequence Extraction Methods

These methods extract a portion of the original character sequence.

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| **`substring(int beginIndex)`** | Returns a new String containing the characters from `beginIndex` (inclusive) to the end of the sequence. | String, StringBuffer, StringBuilder |
| **`substring(int beginIndex, int endIndex)`** | Returns a new String containing characters from `beginIndex` (inclusive) to `endIndex` (exclusive). | String, StringBuffer, StringBuilder |
| **`subSequence(int beginIndex, int endIndex)`**| Returns a `CharSequence` that is a subsequence of this sequence. Behaves identically to `substring(beginIndex, endIndex)`. (Required for CharSequence interface implementation). | String, StringBuffer, StringBuilder |

**Coding Examples (Substring/Subsequence):**

```java
String example = "ABCDEFGHI";

// 1. Substring from index 3 (D) to the end
String sub1 = example.substring(3); // "DEFGHI"

// 2. Substring from index 1 (B) up to index 5 (exclusive, so up to E)
String sub2 = example.substring(1, 5); // "BCDE"

// 3. SubSequence (identical behavior, returns CharSequence type)
CharSequence seq = example.subSequence(0, 3); // "ABC"

// Applicable to mutable types too (though they return String results)
StringBuilder sbl = new StringBuilder("Mutable String");
String sub_mut = sbl.substring(9); // "String"
```

---

### VII. Conversion, Formatting, and Utility Methods

These methods convert the character sequence to different formats or provide specialized utilities.

| Method | Description | Applicable Classes |
| :--- | :--- | :--- |
| **`toString()`** | Returns a String representation of the sequence. For String, it returns itself. For Buffer/Builder, it creates a new String containing the current sequence. | String, StringBuffer, StringBuilder |
| **`toCharArray()`** | Converts the string into a newly allocated character array. | String |
| **`getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)`** | Copies a range of characters from the sequence into a destination char array. | String, StringBuffer, StringBuilder |
| **`getBytes()`** / `getBytes(String charsetName)` / `getBytes(Charset charset)` | Encodes the String into a sequence of bytes using the specified (or default) charset. | String |
| **`split(String regex)`** / `split(String regex, int limit)` / `splitWithDelimiters(...)` | Splits this string around matches of the given regular expression, returning a String array. | String |
| **`join(CharSequence delimiter, CharSequence... elements)`** | Static method: Returns a new String composed of copies of the elements joined together by the delimiter. | String (static) |
| **`valueOf(...)`** | Static method: Returns the string representation of various primitive or object arguments (boolean, char, int, long, float, double, Object, char[]). | String (static) |
| **`format(String format, Object... args)`** | Static method: Returns a formatted string using the specified format string and arguments (similar to `printf`). Locale-specific version also available. | String (static) |
| **`formatted(Object... args)`** | Formats using the current string as the format string. (Since 15). | String |
| **`intern()`** | Returns a canonical representation for the string object, ensuring it resides in the String Pool. | String |
| **`chars()`** / `codePoints()` | Returns an `IntStream` of character values or Unicode code points from the sequence. (Since 9). | String, StringBuffer, StringBuilder |

**Coding Examples (Conversion and Utility):**

```java
String data = "apple,banana,cherry";
StringBuilder sbl = new StringBuilder("temp");

// 1. To String (converting mutable to immutable)
String converted = sbl.toString(); // "temp"

// 2. To Character Array
char[] charArray = data.toCharArray(); // {'a', 'p', 'p', 'l', 'e', ...}

// 3. Splitting
String[] fruitArray = data.split(","); // {"apple", "banana", "cherry"}

// 4. Joining (static method)
String joined = String.join(":", fruitArray); // "apple:banana:cherry"

// 5. ValueOf (converting primitives/objects to String)
String boolStr = String.valueOf(true); // "true"
String intStr = String.valueOf(100); // "100"

// 6. Formatting
String format_result = String.format("The value is %.2f", 3.14159); // "The value is 3.14"

// 7. Interning
String literal = "pool";
String new_s = new String("pool");
String interned_s = new_s.intern();
boolean check = (literal == interned_s); // true (Both refer to the String Pool object)
```

The difference between `String`, `StringBuffer`, and `StringBuilder` essentially comes down to where the performance cost lies: **String** takes a performance hit during repeated *modification* due to creating numerous new objects, similar to building a tower one fixed block at a time, requiring a new location for every height change. **StringBuffer** and **StringBuilder** minimize this by allowing modification in place, acting more like building with flexible clay. `StringBuffer` adds the overhead of synchronization for thread safety, which `StringBuilder` omits for increased single-threaded speed.

---
### VIII. Why Strings are immutable ?

String objects in Java are designed to be **immutable** (constant). This means that once a `String` object is created in memory, its content cannot be altered. If you perform an operation that seems to change a string, such as concatenation, Java actually creates a **brand new `String` object** in memory with the modified content, and the original object remains untouched.

There are several critical reasons why Java mandates String immutability:

### 1. Memory Optimization via the String Pool

Immutability enables Java to **save a tremendous amount of memory space**.

*   **String Pool Mechanism:** Java uses a feature called the **String Pool** (a separate memory structure inside the Heap). When a string is created using a literal value (e.g., `String s = "data"`), Java checks the String Pool to see if that value already exists.
*   **Object Sharing:** If the exact same string literal value is used multiple times (e.g., `String name1 = "John"` and `String name2 = "John"`), Java will only create the object once in the String Pool. Both reference variables (`name1` and `name2`) will point to that **single, shared string object** in memory.
*   **Necessity of Immutability:** This sharing mechanism would be impossible if strings were mutable. If one variable (`name1`) could change the shared object's value in memory (e.g., changing "John" to "Carl"), it would unexpectedly change the value for the other variable (`name2`) as well. Because strings are immutable, we know the object's value will never be changed, allowing numerous variables to safely point to the same shared object.

### 2. Security

Strings are heavily used throughout the Java system, including for critical elements like class loaders, network connections, and security parameters (such as file paths and user names).

*   **Preventing Malicious Changes:** Immutability is enforced for **security reasons**. If a string object (for example, one holding a user's account name) is passed to a validation method, that method relies on the string *not* changing while the validation and subsequent operation (like adding money to an account) are underway.
*   **Consistent State:** If strings were mutable, a malicious thread could hold a reference to the same string object and change its value after the security checks but before the core operation completes (e.g., changing the validated account holder name from "John" to "Carl"). Since strings are immutable, the method knows the string object it is pointing to will never change, and the security risk is eliminated.

### 3. Thread Safety

The immutability of String objects makes them inherently **thread safe**.

*   **Concurrent Reading:** In a multi-threaded environment, dozens or thousands of threads might concurrently reference and read the exact same string object in memory.
*   **No Synchronization Needed:** Since strings cannot be changed, there is no risk of data corruption or race conditions, as there is no possibility of conflicting writes. Threads can read the shared value safely without needing synchronization mechanisms (like those used by `StringBuffer`). This allows threads to safely share String objects.

In essence, String immutability trades the performance cost of **repeated modification** (which necessitates creating a new object every time, leading to potential $O(n^2)$ complexity for sequential concatenation) for the significant benefits of reliable **memory sharing, security assurance, and thread safety**.

---

### IX. How does the String pool save memory ?

The String Pool saves memory by facilitating the sharing of identical string objects among multiple reference variables. This mechanism is highly efficient because String objects are guaranteed to be immutable (cannot be changed after creation).

Here is a detailed explanation of how the String Pool optimizes memory usage:

### 1. Centralized Storage within the Heap
The **String Pool** is a specific, separate memory structure located inside the Heap memory. When a `String` is created using a literal value (e.g., `String s = "data"`), the Java Virtual Machine (JVM) checks this pool.

### 2. Eliminating Duplication through Sharing
When multiple string variables are initialized with the same literal value, the String Pool ensures that only one physical object is created for that value.

*   **First Creation:** When a string literal, such as `"John"`, is created for the first time, Java places this new string object into the String Pool.
*   **Subsequent Creations:** If another string variable is later created with the exact same literal value (e.g., `String name2 = "John"`), Java checks the String Pool and finds the existing object. Instead of creating a whole new object, Java takes the new variable and points it to the **exact same string object** that was already created in the pool.

**Memory Savings in Action:**
If you have ten reference variables all set to `"Kunal"`, the JVM does **not** create ten separate `"Kunal"` objects; it creates only **one** object in the String Pool, and all ten variables point to that single object. This drastically reduces the total memory consumed compared to creating a new object every time the value is used.

### 3. The Role of Immutability (Why Sharing is Safe)
The String Pool mechanism relies entirely on the fact that String objects are **immutable**.

*   **Safety Guarantee:** Since a string object in memory can never be changed, it is safe for multiple reference variables to point to the same object.
*   **Preventing Disruption:** If strings were mutable, and one variable pointing to the shared `"John"` object changed it to `"Carl"`, the value would unexpectedly change for all other variables pointing to that same object. Because strings are immutable, this risk is eliminated, allowing for the memory-saving sharing scheme to be possible.

### 4. Avoiding the Pool (New Keyword)
It is important to note that this memory optimization happens automatically only when using string literals. If you explicitly create a `String` object using the `new` keyword (e.g., `String s = new String("John")`), Java will create a brand new object **outside** of the String Pool, even if an identical value already exists in the pool. In this case, memory is not saved, and two separate objects exist in the heap memory.

**In summary, the String Pool maximizes memory efficiency by turning identical string literals into references to a single, shared, immutable object in memory**.

---

### IX. Why is repeated string concatenation inefficient ?

The repeated use of the `String` concatenation operator (`+`) is **inefficient** because `String` objects are **immutable** in Java.

When you perform concatenation on `String` objects, the action of joining strings does not modify the original string object in memory; instead, **a brand new `String` object is created** for the result of the concatenation.

Here is a breakdown of why this repeated creation of new objects is inefficient, particularly within a loop:

### 1. Memory and Performance Overhead

Every time the `+` operator is used to append to an existing `String`, Java must execute several steps:

*   **New Object Allocation:** A completely new `String` object is allocated in the Heap memory.
*   **Copying the Old Content:** The content of the original string must be copied over to the new, larger string object.
*   **Appending the New Content:** The new character or string is appended to this new object.
*   **Garbage Collection:** The old, intermediate string object (which is no longer referenced) is left in memory, becoming eligible for garbage collection (GC).

This process results in significant memory wastage because many intermediate string objects are created and discarded.

### 2. Time Complexity: Quadratic Performance ($O(N^2)$)

When concatenating a string repeatedly within a loop (e.g., adding $N$ characters one by one), the performance degrades quadratically, meaning the complexity is $O(N^2)$.

For example, if you start with an empty string and add $N$ characters (where $N$ is the final length):

1.  To add the first character, you copy 0 characters (0 operations).
2.  To add the second character, you copy 1 character (1 operation).
3.  To add the third character, you copy 2 characters (2 operations).
4.  ...
5.  To add the $N^{th}$ character, you copy $N-1$ characters ($N-1$ operations).

The total number of character copying operations is the sum of characters copied across all steps, resulting in a complexity of approximately $N \times (N+1) / 2$, which simplifies to **Big O of $N$ squared ($O(N^2)$)**, where $N$ is the total number of characters added. This is extremely poor performance for long sequences.

### The Solution: Mutable Alternatives

To avoid the $O(N^2)$ performance cost associated with repeated string modification, the sources recommend using mutable data types:

*   **`StringBuilder`:** This class is specifically designed to overcome the efficiency issues of `String` concatenation. `StringBuilder` objects are **mutable**, allowing changes to be made to the object **in place** without creating new objects. This dramatically improves performance, making repeated appends operate in approximately $O(N)$ time, as only one single object is modified.
*   **`StringBuffer`:** This class offers mutable strings and is thread-safe (synchronized), but since Java 5, `StringBuilder` is generally preferred in single-threaded environments because it is faster due to the absence of synchronization overhead.

In essence, repeated concatenation on `String` objects is like trying to build a wall where every time you add a brick, you have to build a whole new wall slightly larger than the previous one, wasting time and material, rather than simply appending the new brick to the existing wall.

---

> For more detailed references : 

[Oracle Docs - String](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/String.html)

[Oracle Docs - StringBuilder](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/StringBuilder.html)

[Oracle Docs - StringBuffer](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/StringBuffer.html)

[Youtube - Bro Code](https://www.youtube.com/watch?v=Ntl3DxhyrQQ)

[Youtube - Bro Code](https://www.youtube.com/watch?v=44iQGNUcik0)

[Youtube - Microsoft Developer](https://www.youtube.com/watch?v=pEW21fWqHQM)

[Youtube - Microsoft Developer](https://www.youtube.com/watch?v=tetONyHZjXQ)

[Youtube - Telsuko](https://youtu.be/cV-sOpOgof8?list=TLGGvFv_4PRvUogwMjEyMjAyNQ)

[Youtube - Telsuko](https://youtu.be/WQ4aA4-MESE?list=TLGG7xIHFOhUUbAwMjEyMjAyNQ)

[Youtube - Coding With John](https://youtu.be/Bj9Mx_Lx3q4?list=TLGGo5haD34XD8QwMjEyMjAyNQ)

[Youtube - Kunal Kushwaha](https://youtu.be/zL1DPZ0Ovlo?list=TLGGYvN9ShXgInowMjEyMjAyNQ)



