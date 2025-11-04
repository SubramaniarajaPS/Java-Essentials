
# Java Streams (java.util.stream) — Quick Reference Notes with Examples

## Overview: What is a Stream?

- **Definition:** A stream is a pipeline to process elements from a source (like a collection, array, file) through a sequence of operations (filter, map, reduce), often in a functional style.
  - Streams do not store data; they convey elements through operations.
  - Operations do not modify the source; they produce new streams or results.
  - Many operations are lazy (deferred until a terminal operation runs).
  - Streams can be finite or infinite.
  - A stream is consumable: once a terminal operation runs, it can’t be reused.

- **Types of Streams:**  
  - **Object stream:** `Stream<T>`  
  - **Primitive streams:** `IntStream`, `LongStream`, `DoubleStream`

- **Common Sources:**
  - **From collections:** `collection.stream()`, `collection.parallelStream()`
  - **From arrays:** `Arrays.stream(arr)`
  - **Factories:** `Stream.of(...)`, `IntStream.range(a, b)`, `Stream.iterate(seed, op)`
  - **I/O and utilities:** `BufferedReader.lines()`, `Files.walk(...)`, `Random.ints()`, `BitSet.stream()`, `Pattern.splitAsStream(...)`, `JarFile.stream()`

- **Example: Filter–Map–Sum**
  ```java
  int sum = widgets.stream()
                   .filter(b -> b.getColor() == RED)
                   .mapToInt(Widget::getWeight)
                   .sum();
  ```

## Stream Pipelines: Intermediate vs Terminal Operations

- **Pipeline Structure:** Source → zero or more intermediate ops → terminal op.
  - Intermediate ops (e.g., `filter`, `map`) are lazy and return a stream.
  - Terminal ops (e.g., `forEach`, `reduce`, `sum`) traverse the stream and produce a value or side-effect.

- **Execution:**  
  - Work is fused where possible (e.g., filter-map-sum in one pass).
  - Traversal begins only at the terminal operation.

- **Escape Hatches:**  
  - `iterator()` and `spliterator()` are terminal operations that don’t trigger eager traversal; they allow manual iteration.

- **Stateless vs Stateful Intermediate Ops:**
  - **Stateless:** No memory of previous elements (e.g., `filter`, `map`).
  - **Stateful:** Depends on previously seen elements (e.g., `distinct`, `sorted`), may require buffering or multiple passes.

- **Short-Circuiting Ops:**
  - **Intermediate:** Can turn infinite input into a finite stream (e.g., `limit(n)`).
  - **Terminal:** Can finish without consuming all elements (e.g., `findFirst`, `anyMatch`).

- **Example: Short-Circuiting**
  ```java
  // Stops as soon as it finds the first string longer than 1000 chars
  Optional<String> first = strings.stream()
                                  .filter(s -> s.length() > 1000)
                                  .findFirst();
  ```

## Parallelism

- **Serial vs Parallel:**
  - Streams are serial by default.
  - Use `parallelStream()` or `.parallel()` to switch to parallel mode; `.sequential()` switches back.
  - Mode is applied to the whole pipeline when the terminal op runs.
  - `isParallel()` tells you the current mode.

- **Correctness:**  
  - Apart from explicitly nondeterministic ops (e.g., `findAny()`), serial vs parallel should not change the result.

- **Example: Parallel Sum**
  ```java
  int sumOfWeights = widgets.parallelStream()
                            .filter(b -> b.getColor() == RED)
                            .mapToInt(Widget::getWeight)
                            .sum();
  ```

## Behavioral Parameters (Lambdas and Method References)

- **Definition:** Functions you pass to stream ops that define behavior (e.g., the predicate for `filter`, mapper for `map`, comparator for `sorted`).
  - Must be non-interfering (must not modify the data source during execution).
  - Typically should be stateless (not depend on mutable state that can change during pipeline execution).

- **Examples:**
  - **Filter:** `filter(b -> b.getColor() == RED)`
  - **Map:** `mapToInt(Widget::getWeight)`
  - **Sort:** `sorted(Comparator.comparingInt(Widget::getWeight))`

## Non-Interference

- **Rule:** Don’t modify the underlying data source while the pipeline is executing (from terminal op start to finish), unless the source is a concurrent collection.
  - Concurrent sources have a spliterator with `CONCURRENT` characteristic.
  - Modifying the source beforehand is okay; those changes are visible when the terminal operation runs (for well-behaved sources like JDK collections).

- **Example: Safe Pre-Modification**
  ```java
  List<String> l = new ArrayList<>(List.of("one", "two"));
  Stream<String> sl = l.stream();
  l.add("three");               // Modified before terminal op starts
  String s = sl.collect(Collectors.joining(" ")); // "one two three"
  ```

## Stateless Behaviors

- **Avoid stateful lambdas:** Their output depends on mutable state that can change, leading to nondeterminism, especially in parallel streams.

- **Bad Example (Stateful):**
  ```java
  Set<Integer> seen = Collections.synchronizedSet(new HashSet<>());
  stream.parallel()
        .map(e -> { if (seen.add(e)) return 0; else return e; })
        .forEach(...); // Results may vary across runs
  ```

- **Guidance:** Restructure pipelines to avoid stateful behavioral parameters.

## Side-Effects

- **Discouraged:** Side-effects (mutating external state, I/O) inside behavioral parameters can break statelessness, harm thread-safety, and may be elided by optimizations.
  - No guarantees about visibility to other threads, execution order, thread assignment, or even whether they run (except for `forEach`/`forEachOrdered`).

- **Bad Example (Side-Effects):**
  ```java
  List<String> results = new ArrayList<>();
  stream.filter(s -> pattern.matcher(s).matches())
        .forEach(s -> results.add(s)); // Not thread-safe, unnecessary
  ```

- **Good Example (No Side-Effects):**
  ```java
  List<String> results = stream.filter(s -> pattern.matcher(s).matches())
                               .toList();
  ```

- **Note:** `forEach()` and `peek()` rely on side-effects; use with care. `println` for debugging is typically okay.

## Ordering

- **Encounter Order:**  
  - Some sources are ordered (e.g., `List`, arrays); others aren’t (e.g., `HashSet`).
  - Intermediate ops can impose or remove order (e.g., `sorted()` imposes; `unordered()` removes).
  - Some terminal ops ignore order (e.g., `forEach()`).

- **Determinism:**  
  - In ordered streams, results respect encounter order.
  - In unordered streams, any permutation that is semantically correct is acceptable.

- **Parallel Performance:**  
  - Relaxing order requirements (via `unordered()`) can improve performance for some stateful or terminal operations (`distinct()`, `groupingBy()`).
  - Ordering-sensitive ops like `limit()` may buffer and reduce parallel gains.

- **Example: De-Ordering for Performance**
  ```java
  long countDistinct = stream.unordered().parallel().distinct().count();
  ```

## Reduction Operations

- **Purpose:** Combine elements into a single result (sum, max, count, or custom aggregation).

- **Built-ins:** `sum()`, `max()`, `count()`, `reduce(...)`, `collect(...)`

- **Why Reductions:**  
  - More abstract and parallel-friendly than manual mutation loops.
  - Require associative and stateless functions for correctness in parallel.

- **Basic Reduce Examples:**
  ```java
  int sum = numbers.stream().reduce(0, Integer::sum);
  int sum2 = numbers.parallelStream().reduce(0, Integer::sum);
  ```

- **Find Max with Primitive Stream:**
  ```java
  OptionalInt heaviest = widgets.parallelStream()
                                .mapToInt(Widget::getWeight)
                                .max();
  ```

- **General Three-Arg Reduce:**
  - Signature:
    - `<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner)`
  - Requirements:
    - `identity` is an identity for `combiner`.
    - `combiner` is associative and compatible with `accumulator`.

- **Example (Combined Map + Reduce):**
  ```java
  int sumOfWeights = widgets.stream()
                            .reduce(0,
                                    (sum, b) -> sum + b.getWeight(),
                                    Integer::sum);
  ```

## Mutable Reduction (collect)

- **Definition:** Accumulate into a mutable container (`Collection`, `StringBuilder`, etc.) using `collect`.

- **Why:** More efficient for certain tasks (e.g., string concatenation) than ordinary `reduce`.

- **Three-Function Form:**
  - `<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator, BiConsumer<R, R> combiner)`

- **Examples:**
  - **String concatenation (bad vs good):**
    ```java
    // Bad: O(n^2) copies
    String concatenated = strings.reduce("", String::concat);

    // Better: collect into StringBuilder (conceptual)
    StringBuilder sb = strings.stream().collect(StringBuilder::new,
                                                StringBuilder::append,
                                                StringBuilder::append);
    String result = sb.toString();
    ```
  - **Collect to List:**
    ```java
    List<String> list = stream.map(Object::toString)
                              .collect(ArrayList::new, ArrayList::add, ArrayList::addAll);
    ```
  - **Using Collector factories:**
    ```java
    List<String> list = stream.map(Object::toString)
                              .collect(Collectors.toList());
    ```

- **Composability with Collectors:**
  - Build complex collectors via `Collectors` combinators.
  - Example: sum of salaries by department:
    ```java
    Collector<Employee, ?, Integer> summingSalaries =
        Collectors.summingInt(Employee::getSalary);

    Map<Department, Integer> salariesByDept =
        employees.stream().collect(Collectors.groupingBy(Employee::getDepartment,
                                                         summingSalaries));
    ```

- **Parallelizability Conditions:**  
  - Combining with an empty container must leave results unchanged.
  - Splitting and combining must be equivalent to sequential accumulation (associativity-like requirements).

## Reduction, Concurrency, and Ordering

- **Potential Parallel Cost:**  
  - Some reductions (like merging `Map`s) can be expensive in parallel due to combiner work.

- **Concurrent Reduction:**  
  - If the result container is concurrent (e.g., `ConcurrentHashMap`), accumulators can deposit results concurrently, reducing combiner overhead.

- **Collector Characteristics:**  
  - Use collectors with `Collector.Characteristics.CONCURRENT` for concurrent reduction.
  - Concurrent reduction is only used when:
    - The stream is parallel, and
    - Collector is `CONCURRENT`, and
    - Stream is unordered or collector is `UNORDERED`.

- **Example: Concurrent Grouping**
  ```java
  Map<Buyer, List<Transaction>> salesByBuyer =
      txns.parallelStream()
          .unordered()
          .collect(Collectors.groupingByConcurrent(Transaction::getBuyer));
  ```

- **Caveat:**  
  - If per-key encounter order matters, don’t use concurrent reduction; prefer sequential or merge-based parallel reduction.

## Associativity

- **Definition:** Operation `op` is associative if `(a op b) op c == a op (b op c)`.
  - Enables safe parallel partitioning and merging.
  - Examples: numeric addition, min, max, string concatenation.

- **Parallelization Insight:**
  - Allows computing `(a op b)` and `(c op d)` in parallel, then combining.

## Low-Level Stream Construction

- **Spliterators:**  
  - Underlying driver of all streams, providing traversal and splitting for parallelism.
  - Quality matters: good spliterators provide size, balanced splits, and characteristics for optimization.

- **Simple but Limited Approach:**  
  - `Spliterators.spliteratorUnknownSize(iterator, characteristics)` works but has poor parallel performance (unknown size, basic splitting).

- **Better Spliterators:**  
  - Provide accurate sizing and meaningful characteristics to improve performance.

- **Mutable Sources and Timing:**  
  - Prefer sources that are `IMMUTABLE` or `CONCURRENT`.
  - Otherwise use late-binding spliterators or Supplier-based factories so binding to data happens when the terminal op starts.

- **Supplier-Based Construction:**  
  - Useful when the source can change between creation and execution; obtain the spliterator only when needed.

- **Result:**  
  - Reduces interference risk and ensures modifications before terminal execution are visible (given non-interfering, stateless behavioral parameters).

---

## Mini Cheat Sheet of Key Rules

- **Don’t mutate the source during pipeline execution** unless it’s a concurrent source.
- **Prefer stateless, non-interfering lambdas** for correctness and performance.
- **Use `parallel()` carefully;** ensure operations are associative and stateless.
- **Avoid side-effects;** prefer reductions (`reduce`, `collect`) over external mutations.
- **Leverage `unordered()`** when you don’t care about encounter order to improve parallel performance.
- **Use `Collectors`** for readable, composable, and parallel-friendly mutable reductions.
- **Ensure associativity** for functions used in parallel reductions.


FOR MORE DETAILED REFERENCES : 

[Java Stream Package](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/stream/Stream.html)

[Java Primitive Streams](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/stream/IntStream.html)

[Stream Collector ](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/stream/Collectors.html)

[Stream Gatherer Docs](https://www.baeldung.com/java-stream-gatherers)

[Easy Tutorial - Javabrahman](https://www.javabrahman.com/java-8/java-8-streams-api-tutorial-with-examples/)

[dev.java - Stream tutorial](https://dev.java/learn/api/streams/)





