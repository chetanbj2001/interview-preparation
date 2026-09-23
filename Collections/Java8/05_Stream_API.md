# Stream API (Java 8)

## 1. What is a Stream?

A **Stream** is a sequence of elements that supports **functional-style operations** (filter, map, reduce...) to process data **declaratively**.

```java
List<String> names = Arrays.asList("Chetan",

// Before Java 8 (imperative: HOW to do it)
List<String> result = new ArrayList<>();
for (String n : names) {
    if (n.startsWith("C")) {
        result.add(n.toUpperCase());
    }
}

// Java 8 Stream (declarative: WHAT to do)
List<String> result = names.stream()
        .filter(n -> n.startsWith("C"))
        .map(String::toUpperCase)
        .collect(Collectors.toList());   //
```

> **One-liner:** A Stream is a pipeline for processing data from a source. It does **not store** data.

---

## 2. Stream vs Collection

| Collection | Stream |
|---|---|
| Stores data | Does **not** store data. It processes data from a source. |
| Can be iterated many times | Can be consum
| Eager: all elements exist in memory | **Lazy**: computed on demand |
| External iteration (`for` loop) | Internal for you) |
| Can add or remove elements | Does **not modify** the source |

---

## 3. Stream Pipeline

```text
Source  →  Intermediate operations (0 or moractly 1)
list.stream()  .filter()  .map()  .sorted()        .collect()
```

### Creating Streams

```java
list.stream();                          // from a Collection
Arrays.stream(arr);                     // f
Stream.of("a", "b", "c");               // from values
IntStream.range(1, 5);                  // 1
IntStream.rangeClosed(1, 5);            // 1,2,3,4,5
Stream.iterate(1, x -> x * 2).limit(5); // 1imit)
Stream.generate(Math::random).limit(3); // infinite without limit
"hello".chars();                        // I
```

---

## 4. Intermediate vs Terminal Operations (most asked)

| | Intermediate | Terminal |
|---|---|---|
| Returns | A **new Stream** | A **result** (value, collection, `void`, or Optional) |
| Execution | **Lazy**: nothing runs until a | Triggers execution of the whole pipeline |
| How many | Zero or more | Exactly one. The stream is closed after it. |
| Examples | `filter`, `map`, `flatMap`, `diskip`, `peek` | `forEach`, `collect`,`reduce`, `count`, `min`, `max`, `anyMatch`, `allMatch`, `noneMatch`, `findFirst`, `findAny`, `toArray` |

### Laziness proof

```java
Stream.of("a", "b", "c")
      .filter(s -> {
          System.out.println("filter " + s);
          return true;
      });
// Prints NOTHING, because there is no terminal operation
```

### Elements flow one by one (vertical execu

```java
Stream.of("a", "b", "c")
      .map(s -> { System.out.println("map "  })
      .filter(s -> { System.out.println("filter " + s); return true; })
      .forEach(s -> System.out.println("forE
```
Output:
```text
map a → filter A → forEach A
map b → filter B → forEach B
map c → filter C → forEach C
```
Each element goes through the **whole pipeli stream does **not** map all elements firstand then filter them.

### Short-circuiting

Some operations stop early without processing every element:
- Intermediate: `limit()`
- Terminal: `findFirst()`, `findAny()`, `anyMatch()`, `allMatch()`, `noneMatch()`

```java
Stream.iterate(1, x -> x + 1)   // infinite
      .filter(x -> x % 5 == 0)
      .findFirst();              // Optionalircuiting
```

---

## 5. Common Intermediate Operations

### `filter(Predicate)`: keep the matching elements
```java
nums.stream().filter(n -> n % 2 == 0);    // even numbers
```

### `map(Function)`: transform each element
```java
names.stream().map(String::length);        /
employees.stream().map(Employee::getName); // Stream<String>
```

### `flatMap(Function<T, Stream<R>>)`: trans (one to many)
```java
List<List<Integer>> nested = Arrays.asList(
        Arrays.asList(1, 2), Arrays.asList(3, 4));

nested.stream()
      .flatMap(List::stream)            // S
      .collect(Collectors.toList());

// Split sentences into words
Stream.of("hello world", "java stream")
      .flatMap(s -> Arrays.stream(s.split(" ")))
      .collect(Collectors.toList());    // [
```

### `distinct()`: remove duplicates (uses `equals()` and `hashCode()`)
```java
Stream.of(1, 2, 2, 3).distinct();       // 1,2,3
```

### `sorted()` / `sorted(Comparator)`
```java
names.stream().sorted();                    r
names.stream().sorted(Comparator.reverseOrder());         // reverse
employees.stream().sorted(Comparator.compari
                                    .reversed()
                                    .thenCom
```

### `limit(n)` / `skip(n)`
```java
nums.stream().skip(2).limit(3);   // elements at index 2, 3, 4
```

### `peek(Consumer)`: for **debugging** only
```java
nums.stream().peek(System.out::println).map(s.toList());
```

> `distinct()` and `sorted()` are **stateful**: they need to see other elements before they can produce output.
`filter` and `map` are **stateless**.

---

## 6. `map()` vs `flatMap()`

| | `map()` | `flatMap()` |
|---|---|---|
| Mapping | One to one | One to many, then f
| Function returns | `R` | `Stream<R>` |
| `List<List<T>>` input | Gives `Stream<List

> The same idea as Optional's `map` and `flalevel of wrapping.

---

## 7. Common Terminal Operations

```java
List<Integer> nums = Arrays.asList(5, 3, 8, 1, 9);

nums.stream().forEach(System.out::println);
nums.stream().count();
nums.stream().min(Integer::compare);                      // Optional[1]
nums.stream().max(Comparator.naturalOrder())
nums.stream().anyMatch(n -> n > 8);                       // true
nums.stream().allMatch(n -> n > 0);
nums.stream().noneMatch(n -> n < 0);                      // true
nums.stream().findFirst();
nums.stream().collect(Collectors.toList());
nums.stream().toArray(Integer[]::new);
```

**Empty stream edge cases:**
- `allMatch()` on an empty stream returns `t condition.
- `anyMatch()` returns `false`.
- `noneMatch()` returns `true`.

### `findFirst()` vs `findAny()`
- `findFirst()` returns the first element in encounter order.
- `findAny()` returns any element. It is fasIn a sequential stream it usually returns the first one, but that is not guaranteed.

---

## 8. `reduce()`

Combines all elements into **one result**.

```java
List<Integer> nums = Arrays.asList(1, 2, 3,

// 1. With identity: returns T
int sum = nums.stream().reduce(0, (a, b) -> a + b);       // 10
int sum2 = nums.stream().reduce(0, Integer::
int product = nums.stream().reduce(1, (a, b) -> a * b);   // 24

// 2. Without identity: returns Optional<T> (the stream may be empty)
Optional<Integer> max = nums.stream().reduce

// 3. With a combiner (used for parallel strpe)
int totalLength = Stream.of("a", "bb", "ccc")
        .reduce(0, (acc, s) -> acc + s.lengt
```

- **Identity** is the starting value. It must be neutral: `0` for sum, `1` for product.
- `reduce` without an identity returns an `Oream has no result.

---

## 9. Primitive Streams: `IntStream`, `LongS

They avoid boxing and unboxing, and provide

```java
int sum = nums.stream().mapToInt(Integer::intValue).sum();     // no Optional
OptionalDouble avg = nums.stream().mapToInt(
IntSummaryStatistics stats = nums.stream().mapToInt(i -> i).summaryStatistics();
// stats.getMin(), getMax(), getAverage(), g

IntStream.range(1, 4).boxed().collect(Collec List<Integer>
```

| Conversion | Method |
|---|---|
| `Stream<T>` to `IntStream` | `mapToInt()` |
| `IntStream` to `Stream<Integer>` | `boxed(

> `Stream<Integer>` has no `sum()`. You needduce(0, Integer::sum)`.

---

## 10. Common Mistakes

1. **Reusing a stream** throws `IllegalState been operated upon or closed`.
   ```java
   Stream<String> s = names.stream();
   s.forEach(System.out::println);
   s.count();    // IllegalStateException
   ```
2. **No terminal operation**: nothing execut
3. **Modifying the source inside the pipeline** can throw `ConcurrentModificationException`. Streams should be
**non-interfering**.
4. **Side effects in `map` or `filter`**, like adding to an external list. Use `collect()` instead.
5. **Using `peek()` for business logic**: itit may not run at all when the pipeline canskip it (for example, `count()` on a sized stream in Java 9+).
6. **Streams for everything**: a simple loop when you need an index, `break`, or checkedexceptions.
7. **Infinite stream without `limit()`** run
8. **`sorted()` or `distinct()` on huge streams**: they are stateful and hold elements in memory.

---

## 11. Important Interview Questions

**Q1. What is a Stream? How is it different from a Collection?**
A pipeline for processing data from a source lazy, it can be consumed only once, and itdoesn't modify the source. A Collection stores data.

**Q2. Intermediate vs terminal operations?**
Intermediate operations return a Stream and n produces a result and triggers thepipeline. A pipeline has exactly one terminal operation.

**Q3. What does "lazy evaluation" mean in streams?**
Intermediate operations don't run until a teThis enables optimizations likeshort-circuiting and processing elements one at a time.

**Q4. Can a stream be reused?**
No. After a terminal operation it is closed,alStateException`. Create a new stream fromthe source, or use a `Supplier<Stream<T>>`.

**Q5. `map()` vs `flatMap()`?**
`map` is one to one. `flatMap` maps each ele the result, for example `List<List<T>>` to`List<T>`.

**Q6. Does a stream modify the original collection?**
No. It produces a new result, and the source

**Q7. What is `reduce()`?**
A terminal operation that combines elements into one value using an identity and an accumulator. Without an identity
it returns an `Optional`.

**Q8. Stateless vs stateful operations?**
Stateless operations (`filter`, `map`) process each element independently. Stateful operations (`sorted`, `distinct`)
need to know about other elements.

---

## 12. Tricky Follow-up Questions

**Q1. What is the output?**
```java
Stream.of(1, 2, 3).peek(System.out::println)
```
**Answer:** Nothing. There is no terminal op

**Q2. How many times is `map` called?**
```java
Stream.of(1, 2, 3, 4, 5)
      .map(x -> { System.out.println("map " + x); return x * 2; })
      .filter(x -> x > 4)
      .findFirst();
```
**Answer:** 3 times (for 1, 2 and 3). `findFirst` short-circuits once `6` passes the filter.

**Q3. What does `Stream.of(1,2,3).count()` return?**
`3L`. The type is `long`, not `int`.

**Q4. What is `IntStream.range(1, 5).sum()`?
`10` (1+2+3+4). The end is **exclusive**. `rangeClosed(1, 5)` gives 15.

**Q5. What is `Stream.empty().allMatch(x -> false)`?**
`true`. The condition holds vacuously becaus

**Q6. `Arrays.asList(arr).stream()` where `alement type?**
`int[]`. You get `Stream<int[]>` with **one** element. Use `Arrays.stream(arr)` to get an `IntStream`.

**Q7. `collect(Collectors.toList())` or `toList()`?**
`Stream.toList()` is **Java 16** and returns Java 8, use `collect(Collectors.toList())`.

**Q8. Is the order preserved with `forEach`
No. Use `forEachOrdered()` to keep encounter order.

---

## 13. Quick Revision

- A Stream is a pipeline: **source, then intermediate operations, then one terminal operation**.
- It doesn't store data, doesn't modify the  only once**.
- Intermediate operations are **lazy** and return a Stream. The terminal operation triggers execution.
- Elements flow **one at a time** through th
- Short-circuiting operations: `limit`, `findFirst`, `findAny`, `anyMatch`, `allMatch`, `noneMatch`.
- `map` is one to one. `flatMap` is one to m
- `distinct` and `sorted` are stateful. `filter` and `map` are stateless.
- `reduce(identity, op)` returns `T`. `reduc
- `count()` returns `long`. `min`, `max`, `findFirst` return an Optional.
- Primitive streams avoid boxing: `mapToInt(ryStatistics()`, `boxed()`.
- `range` excludes the end. `rangeClosed` includes it.
- A reused stream throws `IllegalStateExcept
- `peek` is for debugging only. `Stream.toList()` is Java 16.

---
