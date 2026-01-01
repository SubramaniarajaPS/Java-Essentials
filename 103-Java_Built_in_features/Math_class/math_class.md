### Java Math Class: Comprehensive Interview Revision Notes

The **`java.lang.Math`** class provides a robust set of static methods for performing advanced numeric operations, including exponential, logarithmic, square root, and trigonometric calculations. Because it is part of the `java.lang` package, it is **automatically available** and does not require an import statement.

---

### 1. Math vs. StrictMath: Key Differences
A common interview question involves distinguishing between these two classes.

*   **`Math` Class:**
    *   **Performance-focused:** It is designed for speed. Code generators may use **platform-specific native libraries** or microprocessor instructions to provide high-performance implementations.
    *   **Reproducibility:** It is **not defined to return bit-for-bit the same results** across different implementations. This relaxation allows for better performance where strict reproducibility is not required.
    *   **Implementation:** By default, many `Math` methods simply call the equivalent `StrictMath` methods, but they are encouraged to use native optimizations where available.
*   **`StrictMath` Class:**
    *   **Portability-focused:** Ensures that Java programs produce **identical results** across all platforms.
    *   **Algorithms:** It uses published algorithms from the **"Freely Distributable Math Library" (fdlibm)**. These C-based algorithms are transliterated into Java to ensure consistent floating-point behavior.
    *   **Predictability:** Methods like `sin`, `cos`, and `pow` in `StrictMath` must follow these specific algorithms exactly.

---

### 2. Constant Fields
The `Math` class contains three primary constant fields used in mathematical formulas:

*   **`E`:** The `double` value closest to **Euler’s number** (*e*), the base of natural logarithms (~2.718).
*   **`PI`:** The `double` value closest to **pi** ($\pi$), the ratio of a circle's circumference to its diameter (~3.14159).
*   **`TAU`:** (Added in Java 19) The `double` value closest to **tau** ($\tau$), which is **double the value of pi**.

---

### 3. Basic Math Functions
These functions handle common tasks like finding magnitudes, ranges, or random values.

*   **`abs()`:** Returns the **absolute value** (positive magnitude) of a number. It is overloaded to accept `int`, `long`, `float`, or `double`.
*   **`max(a, b)` / `min(a, b)`:** Returns the **greater** or **smaller** of two values.
*   **`random()`:** Returns a pseudorandom `double` greater than or equal to **0.0 and less than 1.0**. It creates a single instance of `java.util.Random` upon the first call and is **synchronized** for thread safety.
*   **`clamp(value, min, max)`:** (Added in Java 21) Forces a value to stay **within a specified range**. If the value is outside the range, it returns the nearest bound.

#### Code Examples: Basic Functions
```java
// absolute value
Math.abs(-10); // returns 10

// max and min
Math.max(5, 10); // returns 10
Math.min(5, 10); // returns 5

// clamping (saturation)
Math.clamp(15, 1, 10); // returns 10 (clamped to max)

// random range 1-10
int randomNum = (int)(Math.random() * 10) + 1; //
```

---

### 4. Rounding & Floating-Point Control
Java provides several ways to round numbers, each with specific logic.

*   **`ceil(double)`:** Rounds **up** to the smallest `double` value that is $\geq$ the argument and equal to a mathematical integer.
*   **`floor(double)`:** Rounds **down** to the largest `double` value that is $\leq$ the argument and equal to a mathematical integer.
*   **`rint(double)`:** Returns the `double` value **closest to an integer**. If two integers are equally close (e.g., 1.5), it rounds to the **even integer**.
*   **`round()`:** Rounds to the **nearest whole number**. Ties (e.g., .5) always round toward **positive infinity** (overloaded: `float` returns `int`, `double` returns `long`).

#### Code Examples: Rounding
```java
Math.ceil(7.343);  // returns 8.0
Math.floor(7.343); // returns 7.0

Math.round(23.445); // returns 23
Math.round(23.5);   // returns 24

Math.rint(1.5); // returns 2.0 (even)
Math.rint(2.5); // returns 2.0 (even)
```

---

### 5. Exponential, Logarithmic, & Power Functions
*   **`pow(base, exp)`:** Returns the base raised to the power of the exponent.
*   **`sqrt(double)`:** Returns the **positive square root**. Returns `NaN` if the argument is $< 0$.
*   **`cbrt(double)`:** Returns the **cube root**.
*   **`exp(x)`:** Returns **Euler's number** $e$ raised to the power of $x$.
*   **`log(x)`:** Returns the **natural logarithm** (base $e$) of $x$.
*   **`log10(x)`:** Returns the **base 10 logarithm**.

#### Code Examples: Powers and Roots
```java
Math.pow(5, 2); // returns 25.0
Math.sqrt(25);  // returns 5.0
Math.cbrt(125); // returns 5.0

Math.log(Math.E); // returns 1.0
```

---

### 6. Trigonometric & Hyperbolic Functions
All trigonometric methods in the `Math` class use **radians**, not degrees.

*   **Basic:** `sin(a)`, `cos(a)`, `tan(a)`.
*   **Inverse:** `asin(a)`, `acos(a)`, `atan(a)`.
*   **Coordinates:** `atan2(y, x)` converts rectangular coordinates $(x, y)$ to polar coordinates (angle $\theta$).
*   **Conversion:** `toDegrees(rad)` and `toRadians(deg)` facilitate switching between units.
*   **Hyperbolic:** `sinh(x)`, `cosh(x)`, `tanh(x)`.

#### Code Examples: Trigonometry
```java
double radians = Math.toRadians(180); // returns PI
Math.sin(Math.PI / 2); // returns 1.0
```

---

### 7. Java 8+ Overflow & "Exact" Arithmetic
A critical interview topic is how Java handles **integer overflow**. Standard operators (+, -, *, /) simply wrap around when they overflow. The `Exact` methods were introduced to detect these errors.

*   **Overflow Detection:** `addExact`, `subtractExact`, `multiplyExact`, `incrementExact`, `decrementExact`, and `negateExact` perform the calculation and **throw an `ArithmeticException`** if the result exceeds the range of the primitive type.
*   **`toIntExact(long)`:** Safely casts a `long` to an `int`, throwing an exception if the value is too large for an `int`.
*   **Divisional Logic:**
    *   **`floorDiv(x, y)`:** Returns the largest integer $\leq$ the algebraic quotient (rounds toward negative infinity). Standard `/` truncates toward zero.
    *   **`floorMod(x, y)`:** Returns the modulus based on `floorDiv`. For negative numbers, this differs from the `%` operator.

#### Code Examples: Overflow and Division
```java
// Overflow
Math.addExact(Integer.MAX_VALUE, 1); // throws ArithmeticException

// negation overflow (MIN_VALUE has no positive equivalent in same range)
Math.negateExact(Integer.MIN_VALUE); // throws ArithmeticException

// floorDiv vs /
Math.floorDiv(-7, 2); // returns -4
int result = -7 / 2;  // returns -3
```

---

### 8. Precision & Advanced Functions
*   **`ulp(double/float)`:** Returns the **Unit in the Last Place**, which is the distance between a floating-point value and the next larger value in magnitude. It is a measure of precision.
*   **`hypot(x, y)`:** Calculates $\sqrt{x^2 + y^2}$ **without intermediate overflow or underflow**, making it more accurate for large numbers than manual calculation.
*   **`signum(x)`:** Returns **1.0** if positive, **-1.0** if negative, and **0.0** if zero.
*   **`copySign(mag, sign)`:** Returns the first argument with the sign of the second.
*   **`fma(a, b, c)`:** (Fused Multiply-Add) Computes $(a \times b + c)$ with only **one rounding step**, improving accuracy and performance compared to `(a * b) + c`.
*   **`nextUp()` / `nextDown()` / `nextAfter()`:** Returns the adjacent floating-point number in a specific direction.
*   **`getExponent()`:** Returns the unbiased exponent used in the representation of the number.
*   **`scalb(d, scaleFactor)`:** Returns $d \times 2^{scaleFactor}$ calculated as a single correctly rounded multiply.

---

### Summary Analogy for Interview
Think of the **`Math` class** as a modern, high-speed calculator that uses every trick in its hardware to give you an answer instantly. Think of **`StrictMath`** as a mathematician following a specific, 500-page rulebook (the `fdlibm` library); no matter who the mathematician is or where they are, they will always arrive at the exact same bit-for-bit result, even if it takes a little longer.

---
> For more detailed references : 

[Java Math class - GFG Blog](https://www.geeksforgeeks.org/java/java-math-class/)

[Java Math class - Baledung Blog](https://www.baeldung.com/java-lang-math)

[Java Math - Bro Code Youtube](https://www.youtube.com/watch?v=nle8CQXYhl4)