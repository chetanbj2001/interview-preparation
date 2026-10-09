# Stream API in Java 8

## 1. Overview & Why Use Stream API?
Introduced in Java 8, the **Stream API** (`java.util.stream`) processes sequences of elements declaratively. It allows functional-style operations (filtering, mapping, reducing) on collections without mutating the original source data.

### Key Benefits
* **Declarative Code**: Expresses *what* to do rather than *how* to loop through elements.
* **No Side-Effects**: Operates on input streams without modifying the underlying source collection.
* **Lazy Evaluation**: Intermediate operations execute only when a terminal operation is invoked.
* **Easy Parallelism**: Leverages multi-core hardware via `.parallelStream()` without manually handling threads.

---

## 2. Collection vs Stream

| Feature | Collection | Stream |
| :--- | :--- | :--- |
| **Primary Focus** | Storing and managing data in memory | Processing and computing data |
| **Modification** | Elements can be added, modified, or removed | Elements cannot be modified or added |
| **Iteration** | External iteration (explicit `for` / `foreach` loops) | Internal iteration (managed by the Stream framework) |
| **Reusability** | Can be traversed multiple times | Single-use; consumed after a terminal operation |
| **Evaluation** | Eagerly constructed (all elements are loaded) | Lazily evaluated (processed on demand) |

---

## 3. Stream Pipeline Architecture
A Stream pipeline consists of three core components:

1. **Source**: Produces data (e.g., `Collection`, array, or I/O channel).
2. **Intermediate Operations**: Transforms a stream into another stream (e.g., `filter`, `map`). These are lazily evaluated.
3. **Terminal Operation**: Produces a non-stream result (e.g., `List`, `long` count, `Optional`) and consumes the stream.

```
+----------+      +-------------------+      +------------------+      +------------------+
|  Source  | ---> | Intermediate Op 1 | ---> | Intermediate Op 2| ---> | Terminal Operation|
| (List)   |      |   (filter)        |      |      (map)       |      |    (collect)     |
+----------+      +-------------------+      +------------------+      +------------------+
```

---

## 4. Creating Streams

```java
import java.util.*;
import java.util.stream.*;

public class StreamCreation {
    public static void main(String[] args) {
        // From Collection
        List<String> list = Arrays.asList("Java", "Spring", "Docker");
        Stream<String> streamFromList = list.stream();

        // From Array
        String[] array = {"Java", "Spring", "Docker"};
        Stream<String> streamFromArray = Arrays.stream(array);

        // Direct Stream.of()
        Stream<Integer> streamOfValues = Stream.of(1, 2, 3, 4, 5);

        // Infinite Streams
        Stream<Integer> iteratedStream = Stream.iterate(0, n -> n + 2).limit(5); // 0, 2, 4, 6, 8
        Stream<Double> generatedStream = Stream.generate(Math::random).limit(3);
    }
}
```

---

## 5. Intermediate vs Terminal Operations

### Intermediate Operations
Returns a new `Stream` and executes lazily.

* `filter(Predicate<T>)`: Filters elements matching a condition.
* `map(Function<T, R>)`: Transforms each element to another object.
* `flatMap(Function<T, Stream<R>>)`: Flattens nested streams/collections into a single stream.
* `distinct()`: Removes duplicate elements using `equals()`.
* `sorted()` / `sorted(Comparator<T>)`: Sorts elements naturally or by a custom comparator.
* `limit(long maxSize)`: Truncates the stream to a specified size.
* `skip(long n)`: Discards the first `n` elements.

### Terminal Operations
Triggers the processing of the pipeline and produces a non-stream output.

* `forEach(Consumer<T>)`: Iterates over elements (used for side-effects).
* `collect(Collector)`: Accumulates elements into collections or structures.
* `count()`: Returns total element count as a `long`.
* `reduce()`: Combines elements into a single value using an associative accumulation function.
* `findFirst()` / `findAny()`: Returns an `Optional` of matching elements.
* `anyMatch()` / `allMatch()` / `noneMatch()`: Evaluates matching conditions returning a `boolean`.

---

## 6. Core Operations with Examples

### `filter()`, `map()`, and `flatMap()`

```java
import java.util.*;
import java.util.stream.*;

public class CoreOperations {
    public static void main(String[] args) {
        // filter & map
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");
        List<String> filtered = names.stream()
            .filter(name -> name.startsWith("A") || name.startsWith("C"))
            .map(String::toUpperCase)
            .collect(Collectors.toList());
        System.out.println(filtered); // [ALICE, CHARLIE]

        // flatMap
        List<List<String>> nestedList = Arrays.asList(
            Arrays.asList("Java", "Spring"),
            Arrays.asList("Docker", "Kubernetes")
        );

        List<String> flattened = nestedList.stream()
            .flatMap(Collection::stream)
            .collect(Collectors.toList());
        System.out.println(flattened); // [Java, Spring, Docker, Kubernetes]
    }
}
```

### `distinct()`, `sorted()`, `limit()`, and `skip()`

```java
import java.util.*;
import java.util.stream.*;

public class SlicingAndSorting {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 2, 8, 2, 9, 1, 5, 4);

        List<Integer> processed = numbers.stream()
            .distinct()                   // [5, 2, 8, 9, 1, 4]
            .sorted(Comparator.reverseOrder()) // [9, 8, 5, 4, 2, 1]
            .skip(1)                      // [8, 5, 4, 2, 1]
            .limit(3)                     // [8, 5, 4]
            .collect(Collectors.toList());

        System.out.println(processed); // [8, 5, 4]
    }
}
```

### `forEach()`, `collect()`, `count()`, and `reduce()`

```java
import java.util.*;
import java.util.stream.*;

public class Aggregation {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // count
        long count = numbers.stream().filter(n -> n > 2).count(); // 3

        // reduce (summing elements)
        int sum = numbers.stream()
            .reduce(0, (a, b) -> a + b); // 15

        // reduce (finding max)
        Optional<Integer> max = numbers.stream()
            .reduce(Integer::max); // Optional[5]
    }
}
```

### Finding and Matching (`findFirst`, `findAny`, `anyMatch`, `allMatch`, `noneMatch`)

```java
import java.util.*;

public class MatchingAndFinding {
    public static void main(String[] args) {
        List<String> techStack = Arrays.asList("Java", "Spring", "Hibernate", "Microservices");

        // Short-circuiting terminal operations
        boolean hasSpring = techStack.stream().anyMatch(s -> s.contains("Spring")); // true
        boolean allLongWords = techStack.stream().allMatch(s -> s.length() > 2);    // true
        boolean noneEmpty = techStack.stream().noneMatch(String::isEmpty);          // true

        Optional<String> first = techStack.stream()
            .filter(s -> s.startsWith("M"))
            .findFirst(); // Optional[Microservices]

        Optional<String> any = techStack.parallelStream()
            .filter(s -> s.length() > 4)
            .findAny(); // Returns any matching element non-deterministically
    }
}
```

---

## 7. Lazy Evaluation & Single-Use Rules

### Lazy Evaluation
Intermediate operations do not run until a terminal operation is called.

```java
List<String> names = Arrays.asList("John", "Jane", "Adam");

Stream<String> stream = names.stream().filter(name -> {
    System.out.println("Filtering: " + name); // Will NOT print here!
    return name.startsWith("J");
});

System.out.println("Terminal operation executed:");
stream.collect(Collectors.toList()); // Filtering log executes NOW during collect()
```

### Single-Use Constraint
A stream cannot be reused after a terminal operation executes. Attempting to reuse it throws an `IllegalStateException`.

```java
Stream<String> stream = Stream.of("A", "B", "C");
stream.forEach(System.out::println);

// Throws IllegalStateException: stream has already been operated upon or closed
stream.forEach(System.out::println); 
```

---

## 8. Tricky Interview Questions & Answers

### Q1. What is the difference between `map()` and `flatMap()`?
* **`map()`**: Used for **1-to-1** transformation. Takes a function that maps an object `T` to `R`. Returns `Stream<R>`.
* **`flatMap()`**: Used for **1-to-Many** transformation and flattening. Takes a function mapping `T` to `Stream<R>` and flattens nested streams into a single `Stream<R>`.

### Q2. What happens if you do not call a terminal operation on a Stream?
Nothing happens. Intermediate operations are lazily evaluated. Without a terminal operation, intermediate transformations are not executed.

### Q3. Is `findAny()` guaranteed to return the first element in a sequential stream?
In sequential streams, `findAny()` typically returns the first element due to order preservation, but it is explicitly non-deterministic in parallel streams for performance optimization. Use `findFirst()` when order must be guaranteed.

### Q4. Can a Stream source be modified while executing a Stream pipeline?
No. Streams do not support structural modification of their underlying source during pipeline execution. Modifying the source leads to a `ConcurrentModificationException`.

---

## 9. Quick Revision

* **Streams do not store data**: They pull data from sources like `Collections` or arrays.
* **Intermediate ops return `Stream` and are lazy**: `filter()`, `map()`, `flatMap()`, `sorted()`, `distinct()`, `limit()`, `skip()`.
* **Terminal ops return results/side-effects and trigger processing**: `collect()`, `forEach()`, `reduce()`, `count()`, `findFirst()`, `anyMatch()`.
* **Single-use only**: Streams cannot be reused after triggering a terminal operation.
* **Java 8 Compatibility**: Collect streams using `Collectors.toList()` (avoid `Stream.toList()` introduced in Java 16).
