
---

# Comprehensive Notes for Java Revision

## Category 1: Atomic Variables (Concurrency)

The atomic classes (`AtomicInteger`, `AtomicIntegerArray`, `AtomicReference`, `AtomicBoolean`) are found in the `java.util.concurrent.atomic` package. They provide non-blocking thread-safe operations on single variables or array elements, leveraging low-level hardware primitives like Compare-And-Swap (CAS) to avoid the overhead associated with explicit synchronization (e.g., using `synchronized` keyword or locks).

### The Compare-And-Swap (CAS) Mechanism

Compare-And-Swap (CAS) is an atomic instruction supported by modern processors that ensures thread-safe updates.

1.  **Operands:** CAS operates on three values: Memory location (**V**), Expected value (**E**), and New value (**N**).
2.  **Operation:** CAS atomically compares the current value at **V** with **E**. If they match, the memory location **V** is updated to the new value **N**.
3.  **Result:** The operation returns `true` if successful (updated) or `false` if unsuccessful (current value did not match expected value).
4.  **Retry (Spin-waiting):** Atomic operations like `incrementAndGet()` utilize CAS in a loop: if the CAS operation fails (due to contention), the thread repeats the read, recalculation, and CAS attempt until it succeeds. This non-blocking approach reduces context switching overhead but can lead to *spin-waiting* under high contention.

---

## 1. AtomicBoolean

### Description and Purpose
`AtomicBoolean` manages a `boolean` value that can be updated atomically. It is useful for implementing atomically updated flags, implementing simple lock mechanisms, or managing signals between threads. It provides atomic access to the variable, meaning operations occur in a single, indivisible step, ensuring thread safety without explicit synchronization. It cannot be used as a replacement for the `Boolean` wrapper class.

### Constructors
| Constructor | Description |
| :--- | :--- |
| `AtomicBoolean()` | Creates a new `AtomicBoolean` initialized to `false`. |
| `AtomicBoolean(boolean initialValue)` | Creates a new `AtomicBoolean` initialized to the specified value. |

### Key Functionalities/Methods

| Function/Method | Description | Returns |
| :--- | :--- | :--- |
| `get()` | Returns the current value (with volatile memory effects). | `boolean` (the current value). |
| `set(boolean newValue)` | Sets the value to `newValue` (with volatile memory effects). | `void`. |
| `getAndSet(boolean newValue)` | **Atomically** sets the value to `newValue` and returns the old value. | `boolean` (the previous value). |
| `compareAndSet(boolean expect, boolean update)` | **Atomically** sets the value to `update` if the current value equals `expect`. | `true` if successful (the update occurred). |

### Specialized Memory Order Methods (Since Java 9+)
| Function/Method | Description |
| :--- | :--- |
| `lazySet(boolean newValue)` | Sets the value with memory effects similar to `setRelease`. |
| `getPlain()` / `setPlain(boolean newValue)` | Accesses the value with memory semantics as if the variable was non-`volatile`. |
| `getAcquire()` / `setRelease(boolean newValue)` | Accesses the value using acquire/release memory effects. |
| `compareAndExchange(...)` | Atomically sets the value if it equals the expected value, returning the *witness value* (current value before attempt). Variants include `compareAndExchangeAcquire` and `compareAndExchangeRelease`. |
| `weakCompareAndSet(...)` | A potentially atomic set operation if the current value equals the expected value. The deprecated version implies volatile effects but has plain effects, so `weakCompareAndSetPlain(boolean, boolean)` is recommended instead. |

### Code Example: Implementing a Simple Lock

This demonstrates using `compareAndSet` to acquire a lock and `set` to release it atomically in a multi-threaded context.

```java
import java.util.concurrent.atomic.AtomicBoolean;

public class AtomicBooleanLockExample {
    // Initialized to false (unlocked)
    private static AtomicBoolean lock = new AtomicBoolean(false);

    public static void main(String[] args) {
        Runnable task = () -> {
            // Attempt to acquire lock: CAS(false, true)
            if (lock.compareAndSet(false, true)) {
                try {
                    System.out.println(Thread.currentThread().getName() + " acquired the lock.");
                    // Critical section of code...
                } finally {
                    lock.set(false); // Release the lock
                    System.out.println(Thread.currentThread().getName() + " released the lock.");
                }
            } else {
                System.out.println(Thread.currentThread().getName() + " could not acquire the lock.");
            }
        };

        // Two threads racing for the lock
        Thread t1 = new Thread(task, "Thread-1");
        Thread t2 = new Thread(task, "Thread-2");
        t1.start();
        t2.start();
    }
}
// Explanation: Only one thread will successfully transition the lock from 'false' to 'true' 
// using compareAndSet(false, true) and enter the critical section.
```

---

## 2. AtomicInteger

### Description and Purpose
`AtomicInteger` facilitates atomic operations on a single `int` value. It extends `Number` to allow access by numerically-based tools. It is commonly used for implementing atomically incremented counters.

### Constructors
| Constructor | Description |
| :--- | :--- |
| `AtomicInteger()` | Creates a new `AtomicInteger` initialized to `0`. |
| `AtomicInteger(int initialValue)` | Creates a new `AtomicInteger` initialized to the specified value. |

### Key Functionalities/Methods (Reads/Writes/CAS)
| Function/Method | Description | Returns |
| :--- | :--- | :--- |
| `get()` | Returns the current value (volatile memory effects). | `int`. |
| `set(int newValue)` | Sets the value to `newValue` (volatile memory effects). | `void`. |
| `getAndSet(int newValue)` | **Atomically** sets the value to `newValue` and returns the old value. | `int` (the previous value). |
| `compareAndSet(int expectedValue, int newValue)` | **Atomically** sets the value to `newValue` if the current value equals `expectedValue`. | `true` if successful. |

### Arithmetic and Update Methods
These methods are performed atomically:

| Function/Method | Description (Equivalent Operation) | Returns |
| :--- | :--- | :--- |
| `incrementAndGet()` | Atomically increments by one (`addAndGet(1)`). | The **updated** value. |
| `getAndIncrement()` | Atomically increments by one (`getAndAdd(1)`). | The **previous** value. |
| `decrementAndGet()` | Atomically decrements by one (`addAndGet(-1)`). | The **updated** value. |
| `getAndDecrement()` | Atomically decrements by one (`getAndAdd(-1)`). | The **previous** value. |
| `addAndGet(int delta)` | Atomically adds `delta` to the current value. | The **updated** value. |
| `getAndAdd(int delta)` | Atomically adds `delta` to the current value. | The **previous** value. |
| `updateAndGet(IntUnaryOperator)` | Atomically updates the value by applying the given function. | The **updated** value. |
| `getAndUpdate(IntUnaryOperator)` | Atomically updates the value by applying the given function. | The **previous** value. |
| `accumulateAndGet(int x, IntBinaryOperator)` | Atomically updates using a function of the current value and `x`. | The **updated** value. |
| `getAndAccumulate(int x, IntBinaryOperator)` | Atomically updates using a function of the current value and `x`. | The **previous** value. |

### Value Conversion Methods
`AtomicInteger` extends `Number`, providing standard numerical conversion methods that return the current value after conversion: `intValue()`, `longValue()`, `floatValue()`, and `doubleValue()`.

### Code Example: Concurrent Counter using `getAndAdd()`

The `getAndAdd()` method adds the specified value to the current value and returns the value *before* the addition.

```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicIntegerAddExample {
    public static void main(String[] args) {
        // Initially value as 18
        AtomicInteger val = new AtomicInteger(18);

        // Adds 8 and gets the previous value
        int previousValue = val.getAndAdd(8); 

        System.out.println("Previous value returned by getAndAdd(8): " + previousValue); // Output: 18
        System.out.println("Current value of AtomicInteger: " + val.get()); // Output: 26
    }
}
// Explanation: getAndAdd(8) ensures the addition happens atomically. It first returns the original value (18) 
// and then updates the internal value to 26.
```

---

## 3. AtomicIntegerArray

### Description and Purpose
`AtomicIntegerArray` is an `int` array where elements may be updated atomically. It provides methods to perform atomic operations on elements specified by an index.

### Constructors
| Constructor | Description |
| :--- | :--- |
| `AtomicIntegerArray(int length)` | Creates a new array of the specified length, with all elements initially zero. |
| `AtomicIntegerArray(int[] array)` | Creates a new array with the same length as, and all elements copied from, the given array. Throws `NullPointerException` if the input array is `null`. |

### Key Functionalities/Methods
Most methods require an index (`i`) as the first argument to specify the array element being operated on.

| Function/Method | Description | Returns |
| :--- | :--- | :--- |
| `length()` | Returns the length of the array. | `int`. |
| `get(int i)` | Returns the current value of the element at index `i`. | `int`. |
| `set(int i, int newValue)` | Sets the element at index `i` to `newValue`. | `void`. |
| `getAndSet(int i, int newValue)` | Atomically sets element at index `i` to `newValue` and returns the old value. | `int` (the previous value). |
| `compareAndSet(int i, int expected, int update)` | Atomically sets element `i` to `update` if current value equals `expected`. | `true` if successful. |

### Indexed Arithmetic and Update Methods
These methods operate on the element at the specified index `i`:

*   **Increment/Decrement:** `getAndIncrement(int i)`, `incrementAndGet(int i)`, `getAndDecrement(int i)`, `decrementAndGet(int i)`.
*   **Addition:** `getAndAdd(int i, int delta)`, `addAndGet(int i, int delta)`.
*   **Function Updates:** `getAndUpdate(int i, IntUnaryOperator)`, `updateAndGet(int i, IntUnaryOperator)`, `getAndAccumulate(int i, int x, IntBinaryOperator)`, `accumulateAndGet(int i, int x, IntBinaryOperator)`.

### Utility and Representation
*   `toString()`: Returns the String representation of the current values of the array.

---

## 4. AtomicReference

### Description and Purpose
`AtomicReference<V>` is used for an object reference (of generic type `V`) that may be updated atomically. This class is useful for atomically managing references to immutable objects or shared data structures.

### Constructors
| Constructor | Description |
| :--- | :--- |
| `AtomicReference()` | Creates a new `AtomicReference` with a `null` initial value. |
| `AtomicReference(V initialValue)` | Creates a new `AtomicReference` with the given initial value. |

### Key Functionalities/Methods
Operations mirror those in `AtomicInteger` but operate on the generic type `V`.

| Function/Method | Description | Returns |
| :--- | :--- | :--- |
| `get()` | Returns the current reference value (volatile memory effects). | `V`. |
| `set(V newValue)` | Sets the reference value (volatile memory effects). | `void`. |
| `getAndSet(V newValue)` | **Atomically** sets the value to `newValue` and returns the old value. | `V` (the previous value). |
| `compareAndSet(V expectedValue, V newValue)` | **Atomically** sets the value to `newValue` if the current value references `expectedValue`. | `boolean` (`true` if successful). |
| `updateAndGet(UnaryOperator<V> updateFunction)` | Atomically updates the current value by applying the given function. The function should be side-effect-free. | `V` (the updated value). |
| `accumulateAndGet(V x, BinaryOperator<V> accumulatorFunction)` | Atomically updates the current value with the result of applying the accumulator function to the current value and `x`. | `V` (the updated value). |

### Specialized Memory Order Methods
Similar to other atomic classes, `AtomicReference` provides methods for different memory effects (volatile, plain, opaque, acquire, release), including `lazySet`, `getPlain`, `setPlain`, `getOpaque`, `setOpaque`, `getAcquire`, `setRelease`, `compareAndExchange`, and their variants.

---

## Category 2: Arbitrary-Precision Numbers

### 5. BigInteger

### Description and Purpose
`BigInteger` represents immutable, arbitrary-precision integers. It is essential for handling numbers that exceed the capacity of primitive types like `long` (e.g., $2^{63}-1$), high-precision calculations, cryptography, or algorithms requiring modular arithmetic and GCD. Operations follow the semantics of Java's primitive integer arithmetic. `BigInteger` values are supported in the range $-2^{\text{Integer.MAX\_VALUE}}$ (exclusive) to $+2^{\text{Integer.MAX\_VALUE}}$ (exclusive).

### Constants
The class provides predefined constants for common values:
*   `BigInteger.ZERO` (0)
*   `BigInteger.ONE` (1)
*   `BigInteger.TWO` (2)
*   `BigInteger.TEN` (10)

### Constructors and Factory Methods
| Constructor/Method | Description | Usage |
| :--- | :--- | :--- |
| `BigInteger.valueOf(long val)` | Preferred static factory method for converting a `long` value to a `BigInteger`, allowing for reuse/caching. | `BigInteger num = BigInteger.valueOf(123456789L);` |
| `BigInteger(String val)` | Translates the decimal String representation into a `BigInteger` (most common way to create large numbers). | `BigInteger num = new BigInteger("12345678901...");` |
| `BigInteger(String val, int radix)` | Translates a String representation in the specified radix (base). | `BigInteger a = new BigInteger("1010", 2);` (Binary representation) |
| `BigInteger(byte[] val)` | Translates a byte array containing the two's-complement representation. | `byte[] bytes = {0x12, 0x34}; BigInteger num = new BigInteger(bytes);` |
| `BigInteger(int numBits, Random rnd)` | Constructs a randomly generated non-negative `BigInteger`. | |
| `BigInteger.probablePrime(int bitLength, Random rnd)` | Returns a positive `BigInteger` that is probably prime (probability of being composite $\leq 2^{-100}$). | |

### Key Functionalities/Methods

#### A. Arithmetic Operations
All arithmetic operations return a new `BigInteger` because the class is immutable.

| Function/Method | Pseudo-code Value | Notes |
| :--- | :--- | :--- |
| `add(BigInteger val)` | `(this + val)` | |
| `subtract(BigInteger val)` | `(this - val)` | |
| `multiply(BigInteger val)` | `(this * val)` | Also `parallelMultiply(BigInteger val)` for better algorithmic performance with very large inputs. |
| `divide(BigInteger val)` | `(this / val)` | Throws `ArithmeticException` if `val` is zero. |
| `remainder(BigInteger val)` | `(this % val)` | |
| `divideAndRemainder(BigInteger val)` | Returns array: `[quotient, remainder]`. | |
| `pow(int exponent)` | `(this^exponent)` | `exponent` must be non-negative. |
| `abs()` | Absolute value | |
| `negate()` | `(-this)` | |
| `sqrt()` | Returns the integer square root (`floor(sqrt(n))`). | Throws `ArithmeticException` if `this` is negative. |

#### B. Modular Arithmetic, Primality, and GCD
Modular arithmetic methods return a non-negative result between 0 and `modulus - 1`.

| Function/Method | Pseudo-code Value | Use Case |
| :--- | :--- | :--- |
| `mod(BigInteger m)` | `(this mod m)` | Always returns a non-negative result. Throws `ArithmeticException` if $m \leq 0$. |
| `modPow(BigInteger exponent, BigInteger m)` | `(this^exponent mod m)` | Critical for cryptography (e.g., RSA encryption). Permits negative exponents. |
| `modInverse(BigInteger m)` | `(this^{-1} mod m)` | Used to compute private exponents in RSA encryption. |
| `gcd(BigInteger val)` | Greatest Common Divisor of `abs(this)` and `abs(val)`. | |
| `nextProbablePrime()` | Returns the first integer greater than `this` that is probably prime. | |

#### C. Bitwise and Shift Operations
| Function/Method | Pseudo-code Value | Notes |
| :--- | :--- | :--- |
| `and(BigInteger val)` | `(this & val)` | |
| `or(BigInteger val)` | `(this | val)` | |
| `xor(BigInteger val)` | `(this ^ val)` | |
| `not()` | `(~this)` | |
| `shiftLeft(int n)` | `(this << n)` | Shift distance `n` can be negative (performing a right shift). |
| `shiftRight(int n)` | `(this >> n)` | Shift distance `n` can be negative (performing a left shift). |
| `testBit(int n)` | `true` if the bit at index `n` is set. | |
| `setBit(int n)` | Returns value with designated bit set. | |
| `bitLength()` | Number of bits in the minimal two's-complement representation (excluding sign bit). | |

#### D. Comparison and Conversion
| Function/Method | Description | Returns |
| :--- | :--- | :--- |
| `compareTo(BigInteger val)` | Compares numerically. | -1 (less than), 0 (equal), or 1 (greater than). |
| `equals(Object x)` | Compares for exact equality. | `boolean`. |
| `min(BigInteger val)` / `max(BigInteger val)` | Returns the lesser/greater of `this` and `val`. | `BigInteger`. |
| `signum()` | Returns -1 (negative), 0 (zero), or 1 (positive). | `int`. |
| `toString()` / `toString(int radix)` | Returns the decimal string representation, or string representation in a specified radix. | `String`. |
| `intValue()` / `longValue()` | Converts to primitive type (can lose magnitude and sign information). | `int`/`long`. |
| `intValueExact()` / `longValueExact()` | Converts to primitive type, throwing `ArithmeticException` if the value will not fit exactly. | `int`/`long`. |

### Code Example: Factorial Calculation
`BigInteger` is necessary for calculating large factorials, as the results quickly exceed the capacity of `long`.

```java
import java.math.BigInteger;

public class BigIntegerFactorial {

    public static void main(String[] args) {
        int n = 100; // Calculate 100!
        BigInteger factorial = BigInteger.ONE;

        for (int i = 1; i <= n; i++) {
            // Convert primitive 'i' to BigInteger before multiplication
            factorial = factorial.multiply(BigInteger.valueOf(i)); 
        }

        System.out.println(n + "! = " + factorial); 
        // Example output shows that BigInteger successfully calculates a value that 
        // would overflow standard integer types.
    }
}
```

---

### 6. BigDecimal

### Description and Purpose
`BigDecimal` provides immutable, arbitrary-precision signed decimal numbers. It is composed of an arbitrary precision integer *unscaled value* and a 32-bit integer *scale*. The value represented is $(\text{unscaledValue} \times 10^{-\text{scale}})$. It is used for operations requiring exact results (e.g., financial or scientific calculations), avoiding the minor floating-point inaccuracies found in `float` and `double`.

### Scale and Precision
*   **Scale:** If zero or positive, the scale is the number of digits to the right of the decimal point. If negative, the unscaled value is multiplied by $10$ to the power of the negation of the scale (e.g., a scale of -3 means multiplication by 1000).
*   **Precision:** The number of digits in the unscaled value. The precision of a zero value is 1.

### Rounding and Context Control
`BigDecimal` provides full control over rounding behavior.
*   **MathContext (`mc`):** Used to specify precision and rounding mode for operations. If the precision setting is 0 (`MathContext.UNLIMITED`), operations are exact.
*   **Rounding Modes:** Eight rounding modes are available (e.g., `ROUND_HALF_UP`, `ROUND_DOWN`). The integer fields for rounding modes (like `ROUND_HALF_UP`) are deprecated; the `RoundingMode` enum should be used instead. If an exact result cannot be represented and no rounding mode is specified, an `ArithmeticException` is thrown.

### Constants
*   `BigDecimal.ZERO` (0, scale 0)
*   `BigDecimal.ONE` (1, scale 0)
*   `BigDecimal.TEN` (10, scale 0)

### Constructors and Factory Methods
| Constructor/Method | Description | Notes |
| :--- | :--- | :--- |
| `BigDecimal(String val)` | Translates a string representation. **This is the generally preferred method** because it guarantees an exact value corresponding to the string (e.g., `"0.1"` is exactly 0.1). |
| `BigDecimal.valueOf(long val)` | Translates a `long` to a `BigDecimal` with a scale of zero. |
| `BigDecimal.valueOf(double val)` | Translates a `double` using its canonical string representation (`Double.toString(double)`). **This is the preferred way to convert a `double`**. |
| `BigDecimal(double val)` | Translates a `double` into its exact decimal representation of the binary floating-point value. **Use is generally not recommended** as the results can be unpredictable (e.g., `new BigDecimal(0.1)` does not equal 0.1). |
| `BigDecimal(BigInteger unscaledVal, int scale)` | Creates a `BigDecimal` from an unscaled value and scale. |

### Key Functionalities/Methods

#### A. Arithmetic Operations
Arithmetic methods determine a preferred scale based on the operands. Most have `MathContext` overloads for controlled rounding.

| Function/Method | Preferred Scale of Result | Notes |
| :--- | :--- | :--- |
| `add(BigDecimal augend)` | `max(this.scale(), augend.scale())`. | |
| `subtract(BigDecimal subtrahend)` | `max(this.scale(), subtrahend.scale())`. | |
| `multiply(BigDecimal multiplicand)` | `this.scale() + multiplicand.scale()`. | |
| `divide(BigDecimal divisor)` | `this.scale() - divisor.scale()`. | Requires an exact quotient; throws `ArithmeticException` if the decimal expansion is non-terminating (e.g., $1/3$). |
| `divide(BigDecimal divisor, int scale, RoundingMode roundingMode)` | Explicitly set to `scale`. | |
| `divideToIntegralValue(BigDecimal divisor)` | Returns the integer part of the quotient (rounded down). |
| `remainder(BigDecimal divisor)` | Returns the remainder (`this % divisor`). | This is *not* the modulo operation (the result can be negative). |
| `divideAndRemainder(BigDecimal divisor)` | Returns the quotient and remainder in a two-element array. | Faster than calling `divideToIntegralValue` and `remainder` separately. |
| `pow(int n)` | Computes $this^n$ exactly, to unlimited precision. |
| `sqrt(MathContext mc)` | Returns an approximation to the square root, rounded by `mc`. | Throws `ArithmeticException` if `this` is less than zero. |

#### B. Scale and Precision Manipulation
| Function/Method | Description | Notes |
| :--- | :--- | :--- |
| `unscaledValue()` | Returns the *unscaled value* as a `BigInteger`. | |
| `scale()` / `precision()` | Returns the scale or precision, respectively. | |
| `setScale(int newScale, RoundingMode roundingMode)` | Returns a `BigDecimal` with the specified `newScale`. Rounding is applied if the scale is reduced. | `BigDecimal` objects are immutable; `setScale` returns a new object. |
| `setScale(int newScale)` | Used to increase scale, or reduce it only if no rounding is required (i.e., if trailing zeros exist). Throws `ArithmeticException` otherwise. |
| `movePointLeft(int n)` / `movePointRight(int n)` | Returns a `BigDecimal` equivalent to moving the decimal point $n$ places left (adding $n$ to scale) or right (subtracting $n$ from scale). |
| `scaleByPowerOfTen(int n)` | Returns a `BigDecimal` equal to $(\text{this} \times 10^n)$. |
| `stripTrailingZeros()` | Returns a numerically equal `BigDecimal` with any trailing zeros removed from the representation. |

#### C. Comparison and Representation
| Function/Method | Description | Important Distinction |
| :--- | :--- | :--- |
| `compareTo(BigDecimal val)` | Compares **numerically**. | Considers values equal if they are numerically the same, regardless of scale (e.g., 2.0 and 2.00 are equal). |
| `equals(Object x)` | Compares for equality of **value AND scale**. | Two objects are equal *only* if they have the same numerical value and the same representation/scale (e.g., 2.0 $\ne$ 2.00). |
| `toString()` | Returns the string representation, using scientific notation if an exponent is needed. | |
| `toPlainString()` | Returns the string representation *without* an exponent field. | |
| `toEngineeringString()` | Returns the string representation, using engineering notation (power of ten is a multiple of three) if an exponent is needed. | |
| `toBigIntegerExact()` | Converts to `BigInteger`, throwing `ArithmeticException` if it has a nonzero fractional part. | |

### Code Example: Avoiding Floating-Point Error
`BigDecimal` provides the exact arithmetic required for floating-point calculations where precision is paramount, unlike `double`.

```java
import java.math.BigDecimal;
import java.math.MathContext;

public class BigDecimalPrecisionExample {
    public static void main(String[] args) {
        
        // Using BigDecimal (String constructor guarantees exact value)
        BigDecimal X = new BigDecimal("0.03"); 
        BigDecimal Y = new BigDecimal("0.04");

        // Subtraction
        BigDecimal answer = Y.subtract(X); 
        System.out.println("BigDecimal Answer: " + answer); 
        // Output: 0.01 (Exact)

        // --- Comparison to Double (Primitive) ---
        double x_prim = 0.03;
        double y_prim = 0.04;
        double answer_prim = y_prim - x_prim;
        System.out.println("Double Answer: " + answer_prim); 
        // Output might be 0.010000000000000002 (Inexact due to binary representation)
    }
}
// Explanation: BigDecimal gives the exact answer (0.01) because it internally manages the scale 
// and unscaled integer value, treating decimal numbers as fixed-point rather than floating-point.
```
---

# Classifications of similar functionalities

The topics are comprehensively classified into two primary categories based on their underlying purpose: **Atomic Variables** for concurrent, thread-safe memory manipulation, and **Arbitrary-Precision Numbers** for handling large or high-precision mathematical data.

## Category 1: Atomic Variables (Concurrency Management)

These classes (`AtomicBoolean`, `AtomicInteger`, `AtomicIntegerArray`, `AtomicReference`) provide mechanisms for performing single-variable operations atomically, ensuring thread safety without relying on explicit synchronization like locks.

### A. Core Atomic Access and Volatile Operations

These methods facilitate fundamental atomic reads, writes, and non-blocking updates across all atomic classes, often utilizing memory effects described by the VarHandle specification.

| Functionality | AtomicBoolean | AtomicInteger | AtomicIntegerArray | AtomicReference | Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Get Value** | `get()` | `get()` | `get(int i)` | `get()` | Returns the current value (with volatile memory effects). |
| **Set Value** | `set(boolean)` | `set(int)` | `set(int i, int)` | `set(V)` | Sets the value (with volatile memory effects). |
| **Lazy Set** | `lazySet(boolean)` | `lazySet(int)` | `lazySet(int i, int)` | `lazySet(V)` | Sets the value with release memory effects (since 1.6). |
| **Get and Set** | `getAndSet(boolean)` | `getAndSet(int)` | `getAndSet(int i, int)` | `getAndSet(V)` | Atomically sets the new value and returns the **previous** value. |

### B. Compare-and-Swap (CAS) Primitives

These are the primary mechanisms for performing conditional atomic updates, based on the Compare-And-Swap (CAS) paradigm.

| Functionality | AtomicBoolean | AtomicInteger | AtomicIntegerArray | AtomicReference | Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Volatile CAS** | `compareAndSet(E, U)` | `compareAndSet(E, U)` | `compareAndSet(i, E, U)` | `compareAndSet(E, U)` | Atomically sets the value to $U$ if current value equals $E$ (strong atomic guarantee). |
| **Witness CAS** | `compareAndExchange(E, U)` | `compareAndExchange(E, U)` | `compareAndExchange(i, E, U)` | `compareAndExchange(E, U)` | Atomically sets the value to $U$ if current value equals $E$, returning the current value (the *witness value*). |
| **Weak CAS (Plain)**| `weakCompareAndSetPlain(E, U)` | `weakCompareAndSetPlain(E, U)` | `weakCompareAndSetPlain(i, E, U)` | `weakCompareAndSetPlain(E, U)` | Possibly atomically sets the value if current value equals $E$, using non-volatile memory semantics. (Replaces deprecated versions like `weakCompareAndSet`). |

*Note: All atomic classes also include volatile, plain, opaque, acquire, and release variants of get/set/compare-and-exchange (e.g., `getPlain()`, `setRelease()`, `compareAndExchangeAcquire()`).*

### C. Numeric and Functional Updates (Arithmetic, Unary, and Binary)

These methods provide atomic operations for numerical manipulation (`AtomicInteger`, `AtomicIntegerArray`) or updates based on applied functions (`AtomicInteger`, `AtomicReference`, `AtomicIntegerArray`).

| Functionality | AtomicInteger | AtomicIntegerArray | AtomicReference | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Get and Add** | `getAndAdd(int delta)` | `getAndAdd(int i, int delta)` | N/A | Atomically adds `delta` and returns the **previous** value. |
| **Add and Get** | `addAndGet(int delta)` | `addAndGet(int i, int delta)` | N/A | Atomically adds `delta` and returns the **updated** value. |
| **Increment/Decrement**| `getAndIncrement()` / `incrementAndGet()` | `getAndIncrement(int i)` / `incrementAndGet(int i)` | N/A | Atomic increment/decrement (pre/post update). |
| **Functional Update (Post)** | `updateAndGet(IntUnaryOperator)` | `updateAndGet(i, IntUnaryOperator)` | `updateAndGet(UnaryOperator<V>)` | Atomically updates using a function and returns the **updated** value. |
| **Accumulate (Post)** | `accumulateAndGet(int x, IntBinaryOperator)` | `accumulateAndGet(i, x, IntBinaryOperator)` | `accumulateAndGet(V x, BinaryOperator<V>)` | Atomically updates using a binary function and returns the **updated** value. |

## Category 2: Arbitrary-Precision Numbers

These classes (`BigInteger`, `BigDecimal`) are used when primitive types (`int`, `long`, `float`, `double`) are insufficient, providing arbitrary magnitude (`BigInteger`) or exact decimal representation (`BigDecimal`).

### A. Construction and Constants

These methods manage the initialization of immutable number objects, including preferred static factory methods and built-in constants.

| Functionality | BigInteger | BigDecimal | Description |
| :--- | :--- | :--- | :--- |
| **Factory Method (long)**| `valueOf(long val)` | `valueOf(long val)` | Returns a `BigInteger` or `BigDecimal` corresponding to the `long` value (preferred over constructors for small numbers). |
| **String Constructor** | `BigInteger(String val)` | `BigDecimal(String val)` | Translates a string representation into the arbitrary-precision number. (This is the preferred way for `BigDecimal` to ensure exact value). |
| **From Byte Array**| `BigInteger(byte[] val)` | N/A | Translates a byte array containing the two's-complement binary representation. |
| **Constants** | `ZERO`, `ONE`, `TWO`, `TEN` | `ZERO`, `ONE`, `TWO`, `TEN` | Predefined constants for common numerical values. |

### B. Standard Arithmetic and Negation

All operations return new, immutable objects, and handle basic mathematical functions.

| Functionality | BigInteger | BigDecimal | Description |
| :--- | :--- | :--- | :--- |
| **Addition** | `add(BigInteger val)` | `add(BigDecimal augend)` | Returns the sum of two values. (BigDecimal includes `MathContext` overloads). |
| **Subtraction** | `subtract(BigInteger val)` | `subtract(BigDecimal subtrahend)` | Returns the difference of two values (BigDecimal includes `MathContext` overloads). |
| **Multiplication** | `multiply(BigInteger val)` | `multiply(BigDecimal multiplicand)` | Returns the product. (BigInteger also offers `parallelMultiply(val)`. BigDecimal includes `MathContext` overloads). |
| **Division** | `divide(BigInteger val)` | `divide(BigDecimal divisor)` | Returns the quotient. (For BigDecimal, if the exact quotient has a non-terminating decimal expansion, an `ArithmeticException` is thrown unless rounding is specified). |
| **Remainder** | `remainder(BigInteger val)` | `remainder(BigDecimal divisor)` | Returns the remainder ($this \% val$). |
| **Quotient and Remainder** | `divideAndRemainder(BigInteger val)` | `divideAndRemainder(BigDecimal divisor)` | Returns an array containing $[quotient, remainder]$. |
| **Power** | `pow(int exponent)` | `pow(int n)` | Returns the value raised to the power of the integer exponent. |
| **Absolute Value** | `abs()` | `abs()` | Returns the absolute value. |
| **Negate** | `negate()` | `negate()` | Returns the negative value. |

### C. Comparison, Extremes, and Representation

These methods handle comparisons, finding extremes, and converting the arbitrary-precision number to other types or strings.

| Functionality | BigInteger | BigDecimal | Description |
| :--- | :--- | :--- | :--- |
| **Numerical Compare**| `compareTo(BigInteger val)` | `compareTo(BigDecimal val)` | Compares numerically, returning -1, 0, or 1. (Note: For `BigDecimal`, this method considers 2.0 and 2.00 equal). |
| **Equality Check** | `equals(Object x)` | `equals(Object x)` | Compares for equality. (For `BigDecimal`, this checks **value AND scale**; 2.0 $\ne$ 2.00). |
| **Min/Max** | `min(BigInteger val)` / `max(BigInteger val)` | `min(BigDecimal val)` / `max(BigDecimal val)` | Returns the lesser or greater of the two values. |
| **Signum** | `signum()` | `signum()` | Returns -1 (negative), 0 (zero), or 1 (positive). |
| **To String** | `toString()` / `toString(int radix)` | `toString()` / `toPlainString()` / `toEngineeringString()` | Returns the String representation, optionally specifying radix or format (e.g., scientific/plain notation). |
| **To Primitive/Exact** | `intValue()` / `longValue()` / `floatValue()` / `doubleValue()` | `intValue()` / `longValue()` / `floatValue()` / `doubleValue()` | Converts to primitive types (may lose magnitude/precision). Both classes offer `Exact` variants (`intValueExact()`, `longValueExact()`, etc.) that check for lost information and throw an `ArithmeticException` if the value does not fit. |

### D. BigInteger Specific: Modular, Prime, and Bitwise Operations

These functionalities are unique to `BigInteger` for advanced number theory, cryptographic, and low-level bit manipulation.

| Functionality | Function/Method | Description |
| :--- | :--- | :--- |
| **Modular Arithmetic** | `mod(BigInteger m)` | Returns $this \pmod m$ (always non-negative). |
| **Modular Exponentiation** | `modPow(BigInteger exponent, BigInteger m)` | Returns $(this^{exponent} \pmod m)$ (permits negative exponents). |
| **Modular Inverse** | `modInverse(BigInteger m)` | Returns $this^{-1} \pmod m$. |
| **Primality/Generation** | `probablePrime(int bitLength, Random rnd)` | Returns a positive `BigInteger` that is probably prime. |
| | `nextProbablePrime()` | Returns the first integer greater than $this$ that is probably prime. |
| | `isProbablePrime(int certainty)` | Tests if the number is probably prime. |
| **Shifts** | `shiftLeft(int n)`, `shiftRight(int n)` | Returns $(this \times 2^n)$ or $(this / 2^n)$; supports negative shift distance. |
| **Bitwise Logic** | `and(BigInteger val)`, `or(BigInteger val)`, `xor(BigInteger val)`, `not()` | Analogues to Java's primitive bitwise operators. |
| **Single Bit Operations** | `testBit(int n)`, `setBit(int n)`, `clearBit(int n)`, `flipBit(int n)` | Manages individual bits in the two's-complement representation. |
| **Bit Counting** | `bitLength()`, `bitCount()`, `getLowestSetBit()` | Provides information about the number's bit structure. |
| **Square Root** | `sqrt()`, `sqrtAndRemainder()` | Returns the integer square root ($\lfloor\sqrt{n}\rfloor$) and/or the remainder. |

### E. BigDecimal Specific: Scale and Precision Control

These methods manage the internal representation (`unscaled value` and `scale`) of the decimal number and control rounding behavior.

| Functionality | Function/Method | Description |
| :--- | :--- | :--- |
| **Scale/Precision Info** | `scale()`, `precision()`, `unscaledValue()` | Returns the number of digits right of the decimal point (scale), total digits (precision), or the unscaled integer value. |
| **Set Scale/Rounding** | `setScale(int newScale, RoundingMode roundingMode)` | Returns a new `BigDecimal` with the specified scale, applying rounding if necessary. (The use of integer rounding constants like `ROUND_UP` is deprecated in favor of `RoundingMode` enum). |
| **Scale Change (Exact)** | `setScale(int newScale)` | Changes the scale only if no rounding is required; throws `ArithmeticException` otherwise. |
| **Decimal Point Movement** | `movePointLeft(int n)`, `movePointRight(int n)` | Creates an equivalent value by shifting the decimal point $n$ places (by modifying the scale). |
| **Trailing Zero Control** | `stripTrailingZeros()` | Returns a numerically equal value with any trailing zeros removed from the representation. |
| **Rounding/Plus** | `round(MathContext mc)`, `plus(MathContext mc)` | Returns the `BigDecimal` rounded according to the `MathContext` settings. |