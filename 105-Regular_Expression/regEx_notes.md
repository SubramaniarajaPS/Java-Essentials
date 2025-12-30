# Regex API and Pattern matching

### **1. Core Classes in `java.util.regex`**
The Java Regex API consists primarily of three classes:
*   **`Pattern`**: A compiled representation of a regular expression. It is immutable and thread-safe.
*   **`Matcher`**: The engine that interprets the pattern and performs match operations against an input string (a `CharSequence`). It is not thread-safe.
*   **`PatternSyntaxException`**: An unchecked exception indicating a syntax error in a regex pattern.

---

### **2. Regular Expression Syntax**

#### **A. String Literals & Metacharacters**
*   **Literal Matching**: The simplest match where the regex and input are identical (e.g., regex `foo` matches input `foo`).
*   **Metacharacters**: Special characters that affect matching: `<([{\^-=$!|]})?*+.>`.
    *   **The Dot (`.`)**: Matches any single character (except line terminators by default).
*   **Quoting**: To treat a metacharacter as a literal, precede it with a backslash (`\`) or enclose it in `\Q` (start) and `\E` (end).

#### **B. Character Classes**
Character classes define a set of characters to match a single character in the input string.

| Construct | Description | Example Pattern |
| :--- | :--- | :--- |
| **Simple** | `[abc]` matches 'a', 'b', or 'c'. | `[bcr]at` matches "bat", "cat", "rat". |
| **Negation** | `[^abc]` matches any character *except* 'a', 'b', or 'c'. | `[^bcr]at` matches "hat". |
| **Ranges** | `[a-z]` matches a range of characters. | `` matches any digit. |
| **Union** | `[a-d[m-p]]` matches characters in either set. | `[1-3]` matches 1,2,3,7,8,9. |
| **Intersection** | `[a-z&&[def]]` matches only common characters. | `[1-6&&]` matches 3,4,5,6. |
| **Subtraction** | `[a-z&&[^bc]]` matches the first set excluding the second. | `[0-9&&[^345]]` matches 0,1,2,6,7,8,9. |

#### **C. Predefined Character Classes (Shorthands)**
Note: In Java string literals, the backslash must be escaped (e.g., `"\\d"`).
*   **`\d`**: A digit ``.
*   **`\D`**: A non-digit `[^0-9]`.
*   **`\s`**: A whitespace character `[ \t\n\x0B\f\r]`.
*   **`\S`**: A non-whitespace character `[^\s]`.
*   **`\w`**: A word character `[a-zA-Z_0-9]`.
*   **`\W`**: A non-word character `[^\w]`.

---

### **3. Quantifiers**
Quantifiers specify the number of occurrences to match.

#### **Quantifier Types**
*   **`X?`**: X, once or not at all.
*   **`X*`**: X, zero or more times.
*   **`X+`**: X, one or more times.
*   **`X{n}`**: X, exactly *n* times.
*   **`X{n,}`**: X, at least *n* times.
*   **`X{n,m}`**: X, at least *n* but no more than *m* times.

#### **Quantifier Flavors (Greedy vs. Reluctant vs. Possessive)**
| Flavor | Syntax | Behavior |
| :--- | :--- | :--- |
| **Greedy** | `.*` | Consumes the entire string and "backs off" character by character to find a match. |
| **Reluctant** | `.*?` | Starts with nothing and "eats" one character at a time to find the shortest match. |
| **Possessive** | `.*+` | Consumes the entire string and **never** backs off. |

**Code Example (Quantifier Comparison):**
```java
String input = "xfooxxxxxxfoo";
// Greedy: finds "xfooxxxxxxfoo"
Pattern.compile(".*foo").matcher(input).find(); 

// Reluctant: finds "xfoo" and "xxxxxxfoo"
Pattern.compile(".*?foo").matcher(input).find(); 

// Possessive: No match found because it consumes all and won't back off for "foo"
Pattern.compile(".*+foo").matcher(input).find(); 
```
[Source: 248]

---

### **4. Capturing Groups and Backreferences**
*   **Capturing Groups**: Created by parentheses `(...)`, they treat multiple characters as a single unit.
*   **Numbering**: Groups are numbered by counting opening parentheses from left to right. **Group 0** always represents the entire expression.
*   **Backreferences**: Recalls the text captured by a group within the same regex using `\n` (e.g., `\1` for group 1).
*   **Named Groups**: Use `(?<name>pattern)` and recall with `\k<name>`.

**Code Example (Backreference):**
```java
// Matches two digits followed by the exact same two digits
Pattern p = Pattern.compile("(\\d\\d)\\1");
Matcher m = p.matcher("1212");
boolean found = m.find(); // true
```
[Source: 33, 76]

---

### **5. Boundary Matchers**
These identify the location of the match rather than characters.
*   **`^`**: Beginning of a line.
*   **`$`**: End of a line.
*   **`\b`**: A word boundary.
*   **`\B`**: A non-word boundary.
*   **`\A`**: Beginning of the entire input.
*   **`\z`**: End of the entire input.
*   **`\G`**: End of the previous match.

---

### **6. Pattern Class API**

#### **Match Flags**
Flags modify the behavior of the matcher.
*   **`CASE_INSENSITIVE` (`(?i)`)**: Matches regardless of case.
*   **`MULTILINE` (`(?m)`)**: `^` and `$` match after/before line terminators.
*   **`DOTALL` (`(?s)`)**: The dot `.` matches line terminators.
*   **`COMMENTS` (`(?x)`)**: Ignores whitespace and allows `#` for comments.
*   **`LITERAL`**: Treats the regex as a literal string.
*   **`CANON_EQ`**: Enables canonical equivalence (Unicode).

#### **Key Methods**
*   **`static Pattern compile(String regex, [int flags])`**: Compiles the regex.
*   **`Matcher matcher(CharSequence input)`**: Returns a matcher for the input.
*   **`static boolean matches(String regex, CharSequence input)`**: Convenience method for single-use full matches.
*   **`String[] split(CharSequence input, [int limit])`**: Splits input around matches.
*   **`static String quote(String s)`**: Returns a literal string for the pattern.

---

### **7. Matcher Class API**

#### **Matching Operations**
*   **`boolean matches()`**: Matches the **entire** input against the pattern.
*   **`boolean lookingAt()`**: Matches the input **starting from the beginning** (doesn't require matching the whole string).
*   **`boolean find()`**: Scans for the **next** match.

#### **Retrieval & Index Methods**
*   **`int start()` / `int end()`**: Returns the start and end+1 index of the match.
*   **`String group()`**: Returns the text found in the previous match.
*   **`int groupCount()`**: Returns the number of capturing groups.

#### **Replacement Methods**
*   **`String replaceAll(String replacement)`**: Replaces every match.
*   **`String replaceFirst(String replacement)`**: Replaces only the first match.
*   **`Matcher appendReplacement(StringBuffer sb, String replacement)`**: Non-terminal step to replace while iterating.
*   **`StringBuffer appendTail(StringBuffer sb)`**: Terminal step to append the remainder of the input.

**Code Example (Advanced Replacement):**
```java
Pattern p = Pattern.compile("cat");
Matcher m = p.matcher("one cat two cats");
StringBuffer sb = new StringBuffer();
while (m.find()) {
    m.appendReplacement(sb, "dog"); // Replaces "cat" with "dog"
}
m.appendTail(sb); // Appends the rest: "one dog two dogs"
```
[Source: 136, 140, 328]

---

### **8. Unicode and Exception Handling**
*   **Unicode**: Java supports Unicode 6.0. Use `\uFFFF` for specific code points or `\p{prop}` for properties like `\p{IsLatin}` or `\p{Lu}` (uppercase).
*   **`PatternSyntaxException` Methods**: Use `getDescription()`, `getIndex()`, `getPattern()`, and `getMessage()` to debug malformed regex.

---

### **9. Interview Quick Tips: String Class Support**
The `String` class has several built-in regex methods that are wrappers for the `Pattern`/`Matcher` API:
*   `string.matches(regex)`
*   `string.split(regex)`
*   `string.replaceAll(regex, replacement)`
*   `string.replaceFirst(regex, replacement)`

**Performance Note**: `Pattern.compile()` is expensive. For repeated matches, compile the pattern once and reuse the `Pattern` object, often as a `static final` field.

---

> For more detailed references : 

[Regular Expression - Dev.java Blog](https://dev.java/learn/regex/)

[Regular Expression - GFG Blog](https://www.geeksforgeeks.org/java/regular-expressions-in-java/)

[Learn RegEx - Web Dev Simplified Youtube](https://www.youtube.com/watch?v=rhzKDrUiJVk)