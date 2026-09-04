# Functional Interfaces

## 1. Concepts

### 1.1 What is a Functional Interface?
An interface with exactly **one abstract method** (SAM – Single Abstract Method). Can have any number of default/static methods.

```java
@FunctionalInterface
interface Calculator {
    int operate(int a, int b);
}
```

### 1.2 Why `@FunctionalInterface`?
Not mandatory, but tells the compiler to enforce single-abstract-method rule → compile error if a second abstract method is added. Best practice to always use it.

### 1.3 Core Built-in Functional Interfaces (java.util.function)

| Interface | Method | Purpose |
|---|---|---|
| `Function<T,R>` | `R apply(T t)` | Takes input, returns output |
| `Predicate<T>` | `boolean test(T t)` | Boolean condition |
| `Consumer<T>` | `void accept(T t)` | Takes input, returns nothing |
| `Supplier<T>` | `T get()` | Takes nothing, returns output |
| `UnaryOperator<T>` | `T apply(T t)` | Function where input/output same type |
| `BinaryOperator<T>` | `T apply(T t1, T t2)` | Two same-type inputs, same-type output |
| `BiFunction<T,U,R>` | `R apply(T t, U u)` | Two inputs, one output |
| `BiPredicate<T,U>` | `boolean test(T t, U u)` | Boolean condition on two inputs |
| `BiConsumer<T,U>` | `void accept(T t, U u)` | Two inputs, no return |
| `BooleanSupplier` | `boolean getAsBoolean()` | Supplies a boolean |

```java
Function<Integer, Integer> square = x -> x * x;
Predicate<Integer> isEven = x -> x % 2 == 0;
Consumer<String> printer = System.out::println;
Supplier<Double> random = Math::random;
BinaryOperator<Integer> add = (a, b) -> a + b;
BiPredicate<String, Integer> lengthCheck = (s, len) -> s.length() == len;
BiConsumer<String, Integer> print = (s, i) -> System.out.println(s + i);
```

### 1.4 Primitive-Specialized Functional Interfaces
Java provides `int`, `long`, `double` specialized versions to **avoid autoboxing overhead**. Very commonly asked in interviews for performance-conscious code.

| Category | Examples |
|---|---|
| Function | `IntFunction<R>`, `ToIntFunction<T>`, `IntToDoubleFunction`, `IntUnaryOperator`, `IntBinaryOperator` |
| Predicate | `IntPredicate`, `LongPredicate`, `DoublePredicate` |
| Consumer | `IntConsumer`, `LongConsumer`, `DoubleConsumer` |
| Supplier | `IntSupplier`, `LongSupplier`, `DoubleSupplier` |

```java
IntPredicate isPositive = x -> x > 0;      // no boxing to Integer
IntBinaryOperator sum = (a, b) -> a + b;
ToIntFunction<String> len = String::length;
```

**Why they matter:** `Predicate<Integer>` boxes every `int` into `Integer` — costly in tight loops or Stream pipelines (`IntStream` uses these specialized interfaces internally).

### 1.5 Default Methods on Functional Interfaces
`Predicate`, `Function`, `Consumer` provide default methods for composition:

```java
Predicate<Integer> isPositive = x -> x > 0;
Predicate<Integer> isEven = x -> x % 2 == 0;

Predicate<Integer> combined = isPositive.and(isEven);
Predicate<Integer> either = isPositive.or(isEven);
Predicate<Integer> negated = isPositive.negate();
```

```java
Function<Integer, Integer> addTwo = x -> x + 2;
Function<Integer, Integer> multiplyThree = x -> x * 3;

Function<Integer, Integer> combined = addTwo.andThen(multiplyThree); // (x+2)*3
Function<Integer, Integer> composed = addTwo.compose(multiplyThree); // (x*3)+2
```

### 1.6 Static Helper Methods
```java
Function<String, String> identity = Function.identity(); // returns input unchanged
Predicate<String> isEqualToHello = Predicate.isEqual("hello");
```
`Function.identity()` is heavily used in `Collectors.toMap()`:
```java
Map<String, Integer> map = list.stream()
    .collect(Collectors.toMap(Function.identity(), String::length));
```

### 1.7 `Comparator` as a Functional Interface
`Comparator<T>` is functional (`int compare(T o1, T o2)`) and is one of the most interview-tested examples.

```java
List<String> names = Arrays.asList("Bob", "Alice", "Charlie");

names.sort(Comparator.comparing(String::length));
names.sort(Comparator.comparing(String::length).reversed());
names.sort(Comparator.comparing(String::length).thenComparing(Comparator.naturalOrder()));
```

Key default/static methods: `comparing()`, `thenComparing()`, `reversed()`, `naturalOrder()`, `reverseOrder()`, `nullsFirst()`, `nullsLast()`.

### 1.8 Custom Functional Interface with Generics
```java
@FunctionalInterface
interface Transformer<T, R> {
    R transform(T input);
}

Transformer<String, Integer> lengthFinder = String::length;
```

### 1.9 Functional Interface vs Lambda vs Method Reference
- **Functional interface** = the contract (type).
- **Lambda / method reference** = the implementation of that contract.

### 1.10 How Many Interfaces in `java.util.function`?
43 interfaces total (as of Java 8) — includes all base + primitive-specialized + Bi-variants.

---

## 2. Interview Questions

### Q1. What is a functional interface? Give examples.
An interface with exactly one abstract method. Examples: `Runnable`, `Comparator`, `Function`, `Predicate`.

### Q2. Can a functional interface have multiple default methods?
Yes. Only the abstract method count is restricted to one.

### Q3. Is `@FunctionalInterface` mandatory?
No, it's optional but recommended — enables compile-time safety.

### Q4. Difference between `Predicate` and `Function<T, Boolean>`?
Both can represent boolean logic, but `Predicate` is primitive-friendly (`test`), has built-in `and/or/negate`, and is the idiomatic choice for conditions/filtering.

### Q5. Difference between `Supplier` and `Callable`?
`Supplier.get()` takes no args and doesn't throw checked exceptions; `Callable.call()` can throw checked exceptions. `Callable` is used with `ExecutorService`.

### Q6. Can `Runnable` be treated as a functional interface?
Yes — single abstract method `run()`.

### Q7. Why do primitive-specialized functional interfaces exist (`IntPredicate`, `IntFunction`, etc.)?
To avoid autoboxing/unboxing overhead when working with primitives, especially in Stream pipelines (`IntStream`, `LongStream`, `DoubleStream`).

### Q8. What is `Function.identity()` used for?
Returns a function that returns its input unchanged — commonly used as the key-mapper in `Collectors.toMap()`.

### Q9. Is `Comparator` a functional interface? How is it used with lambdas?
Yes — `compare(T o1, T o2)` is its single abstract method. Used heavily with `Comparator.comparing()`, `.thenComparing()`, `.reversed()`.

### Q10. Difference between `BiFunction` and chaining two `Function`s?
`BiFunction<T,U,R>` takes two different-typed inputs in one call; chaining `Function`s via `andThen`/`compose` builds a pipeline for a single input.

---

## 3. Tricky Interview Questions

### Q1. Can a functional interface extend another interface?
Yes, if the parent interface has no abstract methods, or if it's another functional interface being narrowed (still must remain single-abstract-method overall).

### Q2. Why can't a functional interface have 2 abstract methods, but can extend `Object`'s methods like `equals()`, `toString()`?
Because `Object` class methods (`equals`, `hashCode`, `toString`) are not counted — every class inherits them from `Object`, so declaring them explicitly in the interface doesn't violate SAM rule.

```java
@FunctionalInterface
interface MyInterface {
    void doWork();
    boolean equals(Object obj); // allowed, doesn't count
}
```

### Q3. What happens if two default methods from different interfaces conflict?
Diamond problem — compiler forces you to override and explicitly resolve using `InterfaceName.super.methodName()`.

### Q4. Can you assign a lambda to `Object` type?
No. Lambdas can only be assigned to a functional interface type, not `Object` (even though everything is an Object) — target type must expose a single abstract method.

### Q5. `Predicate<Integer>` vs `IntPredicate` — what's the real difference at runtime?
`Predicate<Integer>` causes boxing of every `int` to `Integer` on each call — extra object creation and GC pressure. `IntPredicate` works directly with primitive `int`, no boxing. Matters in hot loops / large Streams.

### Q6. Why does `Comparator.comparing(String::length).reversed()` work, but you can't just negate it like a `Predicate`?
`Comparator` doesn't have `negate()` — it has `reversed()` because reversing comparison order ≠ boolean negation; the return value is an `int`, not `boolean`.

### Q7. Can two lambdas implementing the same functional interface be `==` equal?
No. Each lambda expression, even with identical logic, compiles to a distinct instance (unless the JVM's lambda metafactory caches it in specific stateless cases) — never rely on lambda reference equality.

---

## 4. Quick Revision

- Functional Interface = 1 abstract method (SAM), any number of default/static methods.
- `@FunctionalInterface` → compile-time check, optional but recommended.
- Core interfaces: `Function`, `Predicate`, `Consumer`, `Supplier`, `UnaryOperator`, `BinaryOperator`, `BiFunction`, `BiPredicate`, `BiConsumer`, `BooleanSupplier`.
- Primitive versions (`IntPredicate`, `IntFunction`, etc.) avoid autoboxing — used internally by `IntStream`/`LongStream`/`DoubleStream`.
- `Function.identity()` — common in `Collectors.toMap()`.
- `Comparator` is functional too — `comparing()`, `thenComparing()`, `reversed()`.
- `Object` class methods don't count toward the SAM rule.
- Composition: `andThen`, `compose` (Function), `and`, `or`, `negate` (Predicate).
- Lambdas need a target functional interface type — can't assign to `Object`.
- 43 interfaces total in `java.util.function`.
