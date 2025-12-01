**Detailed Revision Notes on Java Primitive Wrapper Classes**

The primitive wrapper classes in Java serve as an object-oriented layer around the eight primitive types: `boolean`, `byte`, `char`, `short`, `int`, `long`, `float`, and `double`. The corresponding wrapper classes are **Boolean**, **Byte**, **Character**, **Short**, **Integer**, **Long**, **Float**, and **Double**.

**Key Functionalities:**

1.  **Objectification:** They convert primitive data types into objects, enabling their use where object references are required, such as in the **Collections Framework** (e.g., `ArrayList`, `HashMap`) and **Generics** (e.g., `List<Integer>`).
2.  **Nullability:** As reference types, wrapper objects can hold a `null` value, which primitive types cannot.
3.  **Utility Methods:** They provide numerous utility methods for type conversion, string parsing, comparison, and bit manipulation.
4.  **Immutability:** All primitive wrapper classes are **immutable** and `final`. Operations that appear to modify a value (like addition) actually result in the creation of a new wrapper object.
5.  **Autoboxing and Unboxing:** These are automatic conversions performed by the Java compiler.
    *   **Autoboxing:** Primitive $\rightarrow$ Wrapper Object (e.g., `int` to `Integer`). Internally, this often uses the static `valueOf()` method.
    *   **Unboxing:** Wrapper Object $\rightarrow$ Primitive Value (e.g., `Integer` to `int`). Internally, this uses the appropriate `xxxValue()` method (e.g., `intValue()`).

***

## I. Class Structure and Fields (Constants)

The numeric wrapper classes (`Byte`, `Short`, `Integer`, `Long`, `Float`, `Double`) are subclasses of the abstract class `Number`. `Boolean` and `Character` directly extend `Object`.

| Primitive Type | Wrapper Class |
| :---: | :---: |
| `boolean` | `Boolean` |
| `byte` | `Byte` |
| `char` | `Character` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |

### Common Fields (Constants)

All numeric wrapper classes, plus `Character` and `Boolean`, define static constants useful for type inspection and boundary checks:

| Field Name | Description & Example | Classes |
| :---: | :---: | :---: |
| **MIN\_VALUE** | The minimum value the primitive type can hold. | All numeric |
| **MAX\_VALUE** | The maximum value the primitive type can hold. | All numeric |
| **SIZE** | The number of bits used to represent the primitive value (e.g., Integer is 32). | All numeric, Character |
| **BYTES** | The number of bytes used to represent the primitive value (e.g., Integer is 4). | All numeric, Character |
| **TYPE** | The Class object representing the primitive type. | All classes |
| **TRUE/FALSE** | Static `Boolean` objects corresponding to `true` and `false`. | Boolean |
| **NaN** | Not-a-Number constant. | Float, Double |
| **POSITIVE\_INFINITY** | Positive infinity constant. | Float, Double |
| **NEGATIVE\_INFINITY** | Negative infinity constant. | Float, Double |

**Code Example (Constants):**
```java
// Integer Constants
System.out.println(Integer.MAX_VALUE); // Output: 2147483647
System.out.println(Integer.SIZE);     // Output: 32
System.out.println(Integer.BYTES);    // Output: 4

// Floating Point Constants
System.out.println(Double.POSITIVE_INFINITY); // Output: Infinity
System.out.println(Double.NaN); // Output: NaN

// Boolean Constants
Boolean bTrue = Boolean.TRUE;
```

***

## II. Creation and Parsing Methods

These methods handle the conversion of primitives and strings into wrapper objects or primitive values.

### 1. Object Creation / Boxing Methods

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `valueOf(primitive p)` | Returns a wrapper instance representing the specified primitive value. **Preferred over constructors due to caching (Flyweight Optimization)**, especially for values between -128 and 127 (for Integer, Long, Short, Byte). | All classes |
| `valueOf(String s)` | Returns a wrapper object holding the value represented by the string (assumes radix 10). | All except Character |
| `valueOf(String s, int radix)` | Returns a wrapper object holding the value extracted from the string using the specified radix. | Byte, Short, Integer, Long |

**Code Example (valueOf):**
```java
// Primitives to Wrapper
Integer intObj = Integer.valueOf(100); // Uses cache
Double doubleObj = Double.valueOf(3.14);
Character charObj = Character.valueOf('A'); 

// String to Wrapper (Radix 10 implied)
Boolean boolObj = Boolean.valueOf("True");
Byte byteObj = Byte.valueOf("12");

// String to Wrapper (Specified Radix)
Integer hexObj = Integer.valueOf("FF", 16); // 255
System.out.println(hexObj); // Output: 255
```

### 2. String Parsing Methods (String to Primitive)

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `parseXxx(String s)` | Parses the string argument as a signed decimal primitive value. | All numeric wrappers, Boolean |
| `parseXxx(String s, int radix)` | Parses the string argument as a signed primitive value in the specified radix. | Byte, Short, Integer, Long |
| `parseUnsignedXxx(String s)` | Parses the string as an unsigned decimal primitive value (returns `int` or `long`). | Integer, Long |
| `parseUnsignedXxx(String s, int radix)` | Parses the string as an unsigned primitive value in the specified radix. | Integer, Long |
| `parseXxx(CharSequence s, int beginIndex, int endIndex, int radix)` | Parses a specified subsequence of characters. | Integer, Long |
| `parseDouble(String s)` / `parseFloat(String s)` | Returns a new `double` or `float` initialized to the value represented by the string. | Double, Float |

**Code Example (Parsing):**
```java
// Basic Parsing
int val = Integer.parseInt("1234");
boolean isParsed = Boolean.parseBoolean("True");

// Radix Parsing
byte b = Byte.parseByte("101", 2); // 5
int octal = Integer.parseInt("10", 8); // 8

// Unsigned Parsing
long unsignedVal = Long.parseUnsignedLong("18446744073709551615"); // Max Long value unsigned
```

### 3. Decoding Methods (String to Wrapper Object)

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `decode(String nm)` | Decodes a String (accepting decimal, hexadecimal (prefixed `0x`, `0X`, or `#`), and octal (prefixed `0`)) into the corresponding wrapper object. | Byte, Short, Integer, Long |

**Code Example (Decoding):**
```java
Integer decInt = Integer.decode("10"); // 10
Integer hexInt = Integer.decode("#FF"); // 255
Integer octInt = Integer.decode("010"); // 8
```

***

## III. Value Retrieval and Type Conversion (Unboxing)

These methods retrieve the primitive value encapsulated within the wrapper object or convert it to another primitive type.

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `xxxValue()` (e.g., `intValue()`) | Returns the value of this object as its corresponding primitive type. (Unboxing mechanism) | All classes |
| `byteValue()` | Returns the numeric value as a `byte` (may involve narrowing primitive conversion). | All numeric wrappers |
| `shortValue()` | Returns the numeric value as a `short` (may involve narrowing primitive conversion). | All numeric wrappers |
| `intValue()` | Returns the numeric value as an `int`. | All numeric wrappers |
| `longValue()` | Returns the numeric value as a `long` (may involve widening primitive conversion). | All numeric wrappers |
| `floatValue()` | Returns the numeric value as a `float` (may involve conversion). | All numeric wrappers |
| `doubleValue()` | Returns the numeric value as a `double` (may involve widening primitive conversion). | All numeric wrappers |

**Code Example (Value Retrieval):**
```java
Double wrapperD = Double.valueOf(123.45);
float f = wrapperD.floatValue(); // Narrowing conversion
int i = wrapperD.intValue();     // Narrowing conversion (truncation)

Integer wrapperI = Integer.valueOf(100);
byte b = wrapperI.byteValue();
long l = wrapperI.longValue(); // Widening conversion (safe)

Character wrapperC = Character.valueOf('Z');
char c = wrapperC.charValue();
```

***

## IV. Comparison and Equality

These methods define how wrapper objects and primitive values are compared.

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `compareTo(Wrapper other)` | Compares this object numerically to the argument object, implementing the `Comparable` interface. Returns 0 (equal), < 0 (less than), or > 0 (greater than). Float/Double include total order handling for NaN and +/-0.0. | All classes |
| `compare(primitive x, primitive y)` | Static method comparing two primitive values numerically. | All classes |
| `compareUnsigned(primitive x, primitive y)` | Static method comparing two primitive values treating them as unsigned numbers. | Byte, Short, Integer, Long |
| `equals(Object obj)` | Compares the object to the specified object for value equality. For Float/Double, this uses bit-for-bit comparison to ensure consistency with the `equals` contract, treating all NaNs as equal. **(Always use `equals()` for wrapper objects, not `==`)** | All classes |
| `hashCode()` | Returns a hash code for this object. For numeric types, often related to the underlying primitive value. | All classes |
| `hashCode(primitive value)` | Static method returning a hash code for the specified primitive value. | All classes |

**Code Example (Comparison):**
```java
// Instance comparison
Integer a = 10;
Integer b = 20;
System.out.println(a.compareTo(b)); // Output: -1

// Static primitive comparison
int result = Integer.compare(15, 15); // 0
System.out.println(result);

// Unsigned comparison (useful for large positive values that wrap around)
int uResult = Integer.compareUnsigned(-1, 1); // -1 is greater than 1 when unsigned
System.out.println(uResult > 0); // Output: true

// Equality
Float f1 = Float.valueOf(0.0f / 0.0f); // NaN
Float f2 = Float.valueOf(0.0f / 0.0f); // NaN
System.out.println(f1.equals(f2)); // Output: true (due to bit comparison logic)
```

***

## V. String Representation (Formatting)

These methods generate various string representations of the primitive value, often supporting different numerical bases (radices).

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `toString()` | Returns a decimal string representation of this object's value (base 10). | All classes |
| `toString(primitive p)` | Static method returning a decimal string representation of the primitive value. | All classes |
| `toString(int/long i, int radix)` | Returns a string representation of the argument in the specified radix (base). | Integer, Long |
| `toBinaryString(int/long i)` | Returns the base 2 (binary) string representation (unsigned). | Integer, Long |
| `toOctalString(int/long i)` | Returns the base 8 (octal) string representation (unsigned). | Integer, Long |
| `toHexString(int/long i)` | Returns the base 16 (hexadecimal) string representation (unsigned). | Integer, Long |
| `toUnsignedString(int/long i)` | Returns the unsigned decimal string representation. | Integer, Long |
| `toUnsignedString(int/long i, int radix)` | Returns the unsigned string representation in the specified radix. | Integer, Long |

**Code Example (String Conversion):**
```java
int num = 255;
System.out.println(Integer.toString(num)); // Output: 255
System.out.println(Integer.toHexString(num)); // Output: ff
System.out.println(Integer.toBinaryString(num)); // Output: 11111111
System.out.println(Long.toOctalString(100L)); // Output: 144
```

***

## VI. Arithmetic, Bitwise, and System Utility Methods

These static methods provide mathematical, bit manipulation, and system-property related functionality.

### 1. Simple Arithmetic

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `sum(primitive a, primitive b)` | Returns the sum of two primitive values. | All numeric wrappers |
| `max(primitive a, primitive b)` | Returns the greater of two primitive values. | All numeric wrappers |
| `min(primitive a, primitive b)` | Returns the smaller of two primitive values. | All numeric wrappers |
| `signum(primitive i)` | Returns the signum function of the value: -1 (negative), 0 (zero), or 1 (positive). | Integer, Long |
| `divideUnsigned(int/long dividend, int/long divisor)` | Returns the unsigned quotient. | Integer, Long |
| `remainderUnsigned(int/long dividend, int/long divisor)` | Returns the unsigned remainder. | Integer, Long |
| `toUnsignedInt(short/byte x)` / `toUnsignedLong(int/short/byte x)` | Converts the signed primitive to a larger primitive, treating it as unsigned. | Byte, Short, Integer, Long |

**Code Example (Arithmetic):**
```java
int x = 10, y = 5;
System.out.println(Integer.sum(x, y)); // Output: 15
System.out.println(Integer.max(x, y)); // Output: 10
System.out.println(Integer.signum(-50)); // Output: -1
```

### 2. Bitwise Manipulation (Integer and Long)

These methods operate on the two's complement binary representation.

| Method | Description |
| :---: | :---: |
| `bitCount(int/long i)` | Returns the number of one-bits (population count). |
| `highestOneBit(int/long i)` | Returns an int/long value with at most a single one-bit, positioned at the highest-order one-bit found in $i$. |
| `lowestOneBit(int/long i)` | Returns an int/long value with at most a single one-bit, positioned at the lowest-order one-bit found in $i$. |
| `numberOfLeadingZeros(int/long i)` | Returns the count of zero bits preceding the highest-order one-bit. (Returns 32 for int/64 for long if $i=0$). |
| `numberOfTrailingZeros(int/long i)` | Returns the count of zero bits following the lowest-order one-bit. (Returns 32 for int/64 for long if $i=0$). |
| `rotateLeft(int/long i, int distance)` | Returns the value obtained by rotating the bits left by `distance`. |
| `rotateRight(int/long i, int distance)` | Returns the value obtained by rotating the bits right by `distance`. |
| `reverse(int/long i)` | Returns the value obtained by reversing the order of all bits. |
| `reverseBytes(primitive i)` | Returns the value obtained by reversing the order of the bytes. |
| `compress(int/long i, int/long mask)` | Compresses the bits of $i$ according to the specified mask (removes bits not set in the mask). |
| `expand(int/long i, int/long mask)` | Expands the contiguous bits of $i$ into positions defined by the mask (fills non-mask bits with zeros). |

**Code Example (Bitwise):**
```java
int num = 0b00101010; // 42
System.out.println(Integer.bitCount(num)); // Output: 3
System.out.println(Integer.highestOneBit(num)); // Output: 32 (00100000)
System.out.println(Integer.numberOfLeadingZeros(num)); // Output: 26 (32 - 6)

int reversedBytes = Integer.reverseBytes(num);
System.out.println(Integer.toHexString(reversedBytes)); // Bytes reversed

int rotated = Integer.rotateLeft(num, 3); // Rotate bits left by 3
```

### 3. System Property Methods

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `getInteger(String nm)` | Returns the integer value of the named system property, or `null` if not found or invalid format. | Integer, Long |
| `getInteger(String nm, int val)` | Returns the system property value, or the default `int` value `val`. | Integer, Long |
| `getInteger(String nm, Integer val)` | Returns the system property value, or the default `Integer` object `val`. | Integer, Long |
| `getBoolean(String name)` | Returns `true` if the system property named by the argument exists and is equal (ignoring case) to "true". | Boolean |

**Code Example (System Properties):**
```java
// Assuming system property "my.count" = "50"
// Integer value is retrieved from the system properties
Integer prop = Integer.getInteger("my.count", 0);
```

***

## VII. Specialized Floating-Point Methods (Float and Double)

These methods deal with the specific behaviors of IEEE 754 floating-point numbers (NaN, Infinity).

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `isNaN(primitive v)` / `isNaN()` | Returns `true` if the value is Not-a-Number (NaN). | Float, Double |
| `isInfinite(primitive v)` / `isInfinite()` | Returns `true` if the value is positive or negative infinity. | Float, Double |
| `isFinite(primitive d)` | Returns `true` if the argument is a finite floating-point value (not NaN or infinity). | Float, Double |
| `doubleToLongBits(double value)` / `floatToIntBits(float value)` | Returns the IEEE 754 bit layout representation (normalized NaN bit pattern). | Double, Float |
| `doubleToRawLongBits(double value)` / `floatToRawIntBits(float value)` | Returns the IEEE 754 bit layout representation, preserving original NaN bit patterns. | Double, Float |
| `longBitsToDouble(long bits)` / `intBitsToFloat(int bits)` | Returns the primitive float/double value corresponding to the given bit representation. | Double, Float |
| `toHexString(float/double d)` | Returns a hexadecimal string representation of the floating-point argument. | Float, Double |
| `float16ToFloat(short floatBinary16)` | Returns the `float` value closest to the numerical value of a binary16 value encoded in a `short`. | Float |
| `floatToFloat16(float f)` | Returns the binary16 value, encoded in a `short`, closest in value to the argument. | Float |

**Code Example (Floating Point):**
```java
double d = 1.0 / 0.0; // Positive Infinity
System.out.println(Double.isInfinite(d)); // Output: true
System.out.println(Double.isFinite(d)); // Output: false

double nan = Math.sqrt(-1);
long bits = Double.doubleToLongBits(nan); // Get IEEE 754 bit representation

System.out.println(Double.toHexString(1.0)); // Output: 0x1.0p0
```

***

## VIII. Specialized Boolean Methods

| Method | Description | Applicable Class |
| :---: | :---: | :---: |
| `logicalAnd(boolean a, boolean b)` | Returns the result of applying the logical AND operator (`&&`). | Boolean |
| `logicalOr(boolean a, boolean b)` | Returns the result of applying the logical OR operator (`||`). | Boolean |
| `logicalXor(boolean a, boolean b)` | Returns the result of applying the logical XOR operator (`^`). | Boolean |
| `getBoolean(String name)` | Returns `true` if and only if the system property named by the argument exists and is equal (ignoring case) to "true". | Boolean |

**Code Example (Boolean):**
```java
boolean x = true;
boolean y = false;
System.out.println(Boolean.logicalAnd(x, y)); // Output: false
System.out.println(Boolean.logicalXor(x, y)); // Output: true
```

***

## IX. Specialized Character Methods (Unicode Handling and Classification)

The `Character` class provides extensive functionality for working with individual characters, including Unicode classification and manipulation. Many methods have overloaded versions accepting either a `char` (16-bit code unit, cannot handle supplementary characters) or an `int` (32-bit code point, handles all Unicode characters).

### 1. Case and Type Classification

| Method | Description | Example |
| :---: | :---: | :---: |
| `isUpperCase(char/int)` | Determines if the character is an uppercase letter. | `Character.isUpperCase('A')` $\rightarrow$ `true` |
| `isLowerCase(char/int)` | Determines if the character is a lowercase letter. | `Character.isLowerCase('a')` $\rightarrow$ `true` |
| `isTitleCase(char/int)` | Determines if the character is a titlecase character (rarely used). | |
| `isDigit(char/int)` | Determines if the character is a Unicode decimal digit. | `Character.isDigit('7')` $\rightarrow$ `true` |
| `isLetter(char/int)` | Determines if the character is a letter. | `Character.isLetter('Z')` $\rightarrow$ `true` |
| `isLetterOrDigit(char/int)` | Determines if the character is a letter or a digit. | |
| `isWhitespace(char/int)` | Determines if the character is Java whitespace. | `Character.isWhitespace('\n')` $\rightarrow$ `true` |
| `isSpaceChar(char/int)` | Determines if the character is a Unicode space character (category `SPACE_SEPARATOR`, `LINE_SEPARATOR`, or `PARAGRAPH_SEPARATOR`). | |
| `isISOControl(char/int)` | Determines if the character is an ISO control character (`\u0000` to `\u001F` or `\u007F` to `\u009F`). | |
| `isDefined(char/int)` | Determines if the character has a defined meaning in Unicode. | |
| `getType(char/int)` | Returns an `int` value indicating the character's general Unicode category (e.g., `LOWERCASE_LETTER`). | |
| `isMirrored(char/int)` | Determines whether the character is mirrored (changes glyph when displayed right-to-left). | |
| `isAlphabetic(int)` | Determines if the code point is alphabetic. | |
| `isIdeographic(int)` | Determines if the code point is a CJKV ideograph. | |
| `isEmoji(int)`, `isEmojiPresentation(int)`, etc. | Checks various Unicode Emoji properties. | |

### 2. Case Conversion and Numeric Value

| Method | Description | Example |
| :---: | :---: | :---: |
| `toLowerCase(char/int)` | Converts the character to lowercase. | `Character.toLowerCase('C')` $\rightarrow$ `'c'` |
| `toUpperCase(char/int)` | Converts the character to uppercase. | `Character.toUpperCase('c')` $\rightarrow$ `'C'` |
| `toTitleCase(char/int)` | Converts the character to titlecase. | `Character.toTitleCase('c')` $\rightarrow$ `'C'` |
| `digit(char/int, int radix)` | Returns the numeric value of the character in the specified radix (returns -1 if invalid). | `Character.digit('f', 16)` $\rightarrow$ `15` |
| `forDigit(int digit, int radix)` | Determines the character representation for a specific digit in the specified radix. | `Character.forDigit(15, 16)` $\rightarrow$ `'f'` |
| `getNumericValue(char/int)` | Returns the `int` value represented by the Unicode character (e.g., Roman numerals). | |

### 3. Unicode Identifier Checks

| Method | Description |
| :---: | :---: |
| `isJavaIdentifierStart(char/int)` | Determines if the character is permissible as the first character in a Java identifier. |
| `isJavaIdentifierPart(char/int)` | Determines if the character may be part of a Java identifier (other than the first character). |
| `isUnicodeIdentifierStart(char/int)` | Determines if the character is permissible as the first character in a Unicode identifier. |
| `isUnicodeIdentifierPart(char/int)` | Determines if the character may be part of a Unicode identifier (other than the first character). |
| `isIdentifierIgnorable(char/int)` | Determines if the character is regarded as an ignorable control character in a Java/Unicode identifier. |

### 4. Unicode and Surrogate Pair Methods (Code Points)

These methods handle characters outside the Basic Multilingual Plane (BMP) that require two `char` values (a surrogate pair) for representation.

| Method | Description |
| :---: | :---: |
| `isValidCodePoint(int codePoint)` | Checks if the integer is a valid Unicode code point value (U+0000 to U+10FFFF). |
| `isBmpCodePoint(int codePoint)` | Checks if the code point is in the Basic Multilingual Plane (representable by a single `char`). |
| `isSupplementaryCodePoint(int codePoint)` | Checks if the code point is in the supplementary character range. |
| `isHighSurrogate(char ch)` / `isLowSurrogate(char ch)` | Determines if the `char` is a leading (high) or trailing (low) surrogate code unit. |
| `isSurrogate(char ch)` | Determines if the `char` is any surrogate code unit. |
| `isSurrogatePair(char high, char low)` | Checks if the pair of `char` values is a valid Unicode surrogate pair. |
| `charCount(int codePoint)` | Determines the number of `char` values (1 or 2) needed to represent the code point. |
| `toCodePoint(char high, char low)` | Converts a surrogate pair to its supplementary code point value. |
| `highSurrogate(int codePoint)` / `lowSurrogate(int codePoint)` | Returns the leading/trailing surrogate for a supplementary character. |
| `toChars(int codePoint)` / `toChars(int codePoint, char[] dst, int dstIndex)` | Converts a code point to its UTF-16 `char` representation (array). |
| `codePointAt(CharSequence seq, int index)` | Returns the code point at the given index in a sequence. |
| `codePointBefore(CharSequence seq, int index)` | Returns the code point preceding the given index. |
| `codePointCount(CharSequence seq, int beginIndex, int endIndex)` | Returns the number of Unicode code points in the specified text range. |
| `offsetByCodePoints(CharSequence seq, int index, int codePointOffset)` | Returns the index offset from a given index by a specified number of code points. |
| `reverseBytes(char ch)` | Returns the value obtained by reversing the order of the bytes in the specified `char`. |
| `getName(int codePoint)` / `codePointOf(String name)` | Gets the official Unicode name of a code point, or vice versa. |

**Code Example (Unicode/Utility):**
```java
// Check character type
System.out.println(Character.isLetterOrDigit('5')); // Output: true
System.out.println(Character.getType('A')); // Output: 1 (UPPERCASE_LETTER constant)

// Surrogate example
int supplementaryCP = 0x2F81A;
char highS = Character.highSurrogate(supplementaryCP);
char lowS = Character.lowSurrogate(supplementaryCP);
int convertedCP = Character.toCodePoint(highS, lowS);
System.out.println(Character.charCount(supplementaryCP)); // Output: 2
```

***

## X. Constable and System Descriptor Methods

These methods relate to the advanced Java feature set concerning nominal descriptors and constants.

| Method | Description | Applicable Classes |
| :---: | :---: | :---: |
| `describeConstable()` | Returns an `Optional` containing the nominal descriptor for this instance. | All classes |
| `resolveConstantDesc(MethodHandles.Lookup lookup)` | Resolves this instance as a `ConstantDesc`. | Double, Float, Integer, Long |

**Code Example:**
```java
Integer i = 10;
// Optional describes how this Integer instance can be reconstructed
java.util.Optional<Integer> descriptor = i.describeConstable(); 
```

## XI. Constable and System Descriptor Methods

The wrapper concept, which relies on composition, forms the basis for numerous formalized **structural design patterns**. These patterns define the architectural intent of wrapping one or more objects.

Four structural design patterns that use the wrapper concept are:

1.  **Decorator**: This pattern is also known by the alternative nickname "Wrapper". Its primary intent is the **dynamic extension of functionality** by placing a wrapped object inside a special wrapper object that contains new behaviors. The Decorator implements the **same interface** as the component it wraps.
2.  **Adapter**: The Adapter pattern's intent is to bridge incompatible systems by converting the interface of an existing class (the adaptee) into a **different interface** expected by the client (the target). The Adapter class is considered a wrapper class for the adaptee, and the pattern itself is sometimes known as the Wrapper pattern.
3.  **Proxy**: The Proxy provides **controlled access** to another object, acting as a placeholder or surrogate. It is built on the composition principle and acts as a wrapper for the actual subject class. Like the Decorator, the Proxy implements the **same interface** as the actual object.
4.  **Facade**: The Facade pattern is a wrapper archetype whose intent is to **simplify client interaction** with a complex subsystem. It provides a single, high-level, **simplified interface** that wraps and manages a complex set of subsystem classes. The main difference from other wrappers is that the Facade typically wraps **multiple classes**.

## XII.  Conversions between different Wrappers and primitive types

## 1. Boxing and Autoboxing

**Boxing** generally refers to the procedure of converting a primitive value into an object of its corresponding wrapper class.

**Autoboxing** is the automatic conversion that the Java compiler makes between the primitive types and their corresponding object wrapper classes. It was introduced in Java 5 to simplify interactions with APIs, such as the Collections Framework, which only accept objects.

### When Autoboxing Occurs:

The Java compiler applies autoboxing when a primitive value is automatically converted to an object of the corresponding wrapper class:
1.  **Assigned to a Wrapper Variable:** When a primitive value is assigned to a variable of the corresponding wrapper class.
2.  **Passed as a Method Parameter:** When a primitive value is passed as a parameter to a method that expects an object of the corresponding wrapper class.
3.  **Used in Collections:** When adding a primitive type to a generic collection (like an `ArrayList<Integer>`) which requires object types.

### Internal Mechanism (Implicit Conversion Primitive $\rightarrow$ Wrapper)

When the compiler performs autoboxing, it internally replaces the simple conversion with a call to the static factory method, typically `valueOf()`.

| Primitive Type | Wrapper Class | Autoboxing Example | Internal Compiler Action |
| :---: | :---: | :---: | :---: |
| `char` | `Character` | `Character ch = 'a';` | `Character ch = Character.valueOf('a');` |
| `int` | `Integer` | `Integer val = 2;` | `Integer val = Integer.valueOf(2);` |

The use of `valueOf()` is strategic because, for numerical wrappers like `Integer`, it returns cached instances for frequently requested values (typically -128 to 127), which yields significantly better space and time performance compared to using the deprecated constructors.

***

## 2. Unboxing

**Unboxing** is the automatic conversion of an object of a wrapper type (like `Integer`) back to its corresponding primitive value (like `int`).

### When Unboxing Occurs:

The Java compiler applies unboxing when a wrapper object is automatically converted to a primitive value:
1.  **Assigned to a Primitive Variable:** When a wrapper object is assigned to a variable of the corresponding primitive type.
2.  **Passed as a Method Parameter:** When a wrapper object is passed as a parameter to a method that expects a value of the corresponding primitive type.
3.  **Used in Arithmetic Operations:** When wrapper objects are used with arithmetic operators (like `+`, `%`, `+=`, etc.), which only apply to primitives.

### Internal Mechanism (Implicit Conversion Wrapper $\rightarrow$ Primitive)

When the compiler performs unboxing, it internally inserts a call to the corresponding primitive value method (`xxxValue()`) on the wrapper object.

| Wrapper Class | Primitive Type | Unboxing Example | Internal Compiler Action |
| :---: | :---: | :---: | :---: |
| `Integer` | `int` | `int val = object;` | `int val = object.intValue();` |

**Important Note on Unboxing:**
Since a wrapper object can legally be `null`, attempting to implicitly unbox a `null` wrapper (e.g., performing arithmetic on a `null Integer` object) will result in a runtime `NullPointerException` (NPE).

***

## 3. Implicit Type Conversion (Widening Primitive Conversion)

Implicit type conversion occurs when the conversion between two primitive types is performed automatically by the Java compiler because the conversion is guaranteed to be safe and loss-less. This is formally known as **Widening Primitive Conversion** (JLS 5.1.2).

This type of conversion often happens when retrieving a value from a wrapper object into a larger primitive type (e.g., when converting an `int` to a `long`, `float`, or `double`).

| Wrapper Class Method | Conversion Type | Example of Widening Conversion | Source Reference |
| :---: | :---: | :---: | :---: |
| `longValue()` | `int` $\rightarrow$ `long` | Returns the value of an `Integer` object as a `long` after a widening primitive conversion. | |
| `doubleValue()` | `int` $\rightarrow$ `double` | Returns the value of an `Integer` object as a `double` after a widening primitive conversion. | |
| `floatValue()` | `long` $\rightarrow$ `float` | Returns the value of a `Long` object as a `float` after a widening primitive conversion. | |

***

## 4. Explicit Type Conversion (Narrowing Primitive Conversion)

Explicit type conversion involves conversion between primitive types where potential loss of magnitude or precision exists. This conversion requires an explicit cast operator in code and is formally known as **Narrowing Primitive Conversion** (JLS 5.1.3).

If a conversion is a narrowing primitive conversion, it must be performed explicitly, often leading to truncation or loss of data.

| Wrapper Class Method | Conversion Type | Requirement for Narrowing | Source Reference |
| :---: | :---: | :---: | :---: |
| `byteValue()` | Larger numeric $\rightarrow$ `byte` | Returns the value of a `Short` or `Integer` object as a `byte` after a narrowing primitive conversion. | |
| `intValue()` | `double` $\rightarrow$ `int` | Returns the value of a `Double` object as an `int` after a narrowing primitive conversion. | |
| `shortValue()` | `float` $\rightarrow$ `short` | Returns the value of a `Float` object as a `short` after a narrowing primitive conversion. | |

A primitive cast, which is a form of explicit conversion, illustrates this:
```java
double d = 135.0; 
int integerValue = (int) d; // Explicit conversion (casting) is required.
```
***
> For more detailed references :

[Oracle Docs - Boolean](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Boolean.html)

[Oracle Docs - Byte](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Byte.html)

[Oracle Docs - Short](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Short.html)

[Oracle Docs - Long](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Long.html)

[Oracle Docs - Integer](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Integer.html)

[Oracle Docs - Float](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Float.html)

[Oracle Docs - Double](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Double.html)

[Oracle Docs - Character](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Character.html)