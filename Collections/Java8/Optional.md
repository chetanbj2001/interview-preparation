# Optional in Java 8

## 1. What is Optional?

`Optional<T>` is a container object introduced in Java 8 that may or may not contain a value.

It is mainly used to handle the possibility of `null` and reduce `NullPointerException`.

```java
Optional<String> name = Optional.of("Chetan");
```

Think of it as:

```text
Optional
   ↓
Value present     → contains value
Value absent      → Optional.empty()
```

---

# 2. Why Do We Need Optional?

Without Optional:

```java
String name = getName();

if (name != null) {
    System.out.println(name.toUpperCase());
}
```

With Optional:

```java
Optional<String> name = getName();

name.ifPresent(value -> System.out.println(value.toUpperCase()));
```

### Interview Point

> Optional is mainly used to represent the absence of a value explicitly instead of returning `null`.

---

# 3. Creating Optional

There are three important ways.

## `Optional.of()`

Used when the value is definitely not null.

```java
Optional<String> name = Optional.of("Chetan");
```

If we pass `null`:

```java
Optional<String> name = Optional.of(null);
```

It throws:

```text
NullPointerException
```

---

## `Optional.ofNullable()`

Used when the value may be null.

```java
String name = null;

Optional<String> optional =
        Optional.ofNullable(name);
```

Result:

```text
Optional.empty
```

This is the most commonly used method when dealing with potentially null values.

---

## `Optional.empty()`

Creates an empty Optional.

```java
Optional<String> optional =
        Optional.empty();
```

---

# 4. `of()` vs `ofNullable()`

| Method | Null Allowed? | Result |
|---|---|---|
| `of()` | No | Throws NPE |
| `ofNullable()` | Yes | `Optional.empty()` |
| `empty()` | N/A | Empty Optional |

### Interview Question

**Which one should you use when the value may be null?**

Answer:

```java
Optional.ofNullable(value);
```

---

# 5. Checking Value

## `isPresent()`

Checks whether a value exists.

```java
Optional<String> name =
        Optional.of("Chetan");

if (name.isPresent()) {
    System.out.println(name.get());
}
```

Returns:

```text
true
```

For:

```java
Optional.empty()
```

it returns:

```text
false
```

---

# 6. `get()`

`get()` returns the value inside Optional.

```java
Optional<String> name =
        Optional.of("Chetan");

System.out.println(name.get());
```

Output:

```text
Chetan
```

### Important

Calling `get()` on an empty Optional:

```java
Optional<String> name =
        Optional.empty();

name.get();
```

throws:

```text
NoSuchElementException
```

### Interview Point

Avoid blindly using:

```java
optional.get();
```

Prefer methods such as:

```java
orElse()
orElseGet()
orElseThrow()
ifPresent()
```

---

# 7. `ifPresent()`

Executes code only when a value is present.

```java
Optional<String> name =
        Optional.of("Chetan");

name.ifPresent(value ->
        System.out.println(value));
```

Equivalent traditional code:

```java
if (name != null) {
    System.out.println(name);
}
```

---

# 8. `orElse()`

Provides a default value when Optional is empty.

```java
Optional<String> name =
        Optional.empty();

String result =
        name.orElse("Unknown");

System.out.println(result);
```

Output:

```text
Unknown
```

If the value exists:

```java
Optional<String> name =
        Optional.of("Chetan");

String result =
        name.orElse("Unknown");
```

Result:

```text
Chetan
```

---

# 9. `orElseGet()`

`orElseGet()` accepts a Supplier and creates the default value only when needed.

```java
String result =
        optional.orElseGet(() -> "Unknown");
```

---

# 10. `orElse()` vs `orElseGet()`

This is a **very important interview question**.

### `orElse()`

```java
String result =
        optional.orElse(getDefaultValue());
```

`getDefaultValue()` is evaluated even if Optional already contains a value.

### `orElseGet()`

```java
String result =
        optional.orElseGet(() -> getDefaultValue());
```

`getDefaultValue()` is executed only when Optional is empty.

### Remember

```text
orElse()
    → default value is evaluated

orElseGet()
    → Supplier executes only when needed
```

For expensive operations, `orElseGet()` can avoid unnecessary work.

---

# 11. `orElseThrow()`

Used when the value must exist.

```java
String name =
        optional.orElseThrow(
                () -> new RuntimeException("Name not found")
        );
```

If the value exists → returns value.

If empty → throws the specified exception.

### Java 8

The above Supplier-based form is the Java 8 style.

---

# 12. `map()`

`map()` transforms the value inside Optional.

```java
Optional<String> name =
        Optional.of("chetan");

Optional<String> result =
        name.map(String::toUpperCase);

System.out.println(result.get());
```

Output:

```text
CHETAN
```

Without Optional:

```java
String result =
        name != null
        ? name.toUpperCase()
        : null;
```

---

# 13. `flatMap()`

`flatMap()` is mainly used when the mapping function already returns an Optional.

Example:

```java
Optional<String> name =
        Optional.of("Chetan");

Optional<String> result =
        name.flatMap(value ->
                Optional.of(value.toUpperCase()));
```

### Difference

```text
map()
    → function returns normal value

flatMap()
    → function returns Optional
```

---

# 14. `map()` vs `flatMap()`

Suppose:

```java
Function<String, String> function
```

Use:

```java
optional.map(function);
```

But if:

```java
Function<String, Optional<String>> function
```

Use:

```java
optional.flatMap(function);
```

### Interview Point

> `flatMap()` prevents nested Optional such as `Optional<Optional<T>>`.

---

# 15. Optional with Objects

Suppose:

```java
class Employee {

    private String name;

    public String getName() {
        return name;
    }
}
```

Instead of:

```java
Employee employee = getEmployee();

if (employee != null) {
    System.out.println(employee.getName());
}
```

We can use:

```java
Optional<Employee> employee =
        Optional.ofNullable(getEmployee());

employee.map(Employee::getName)
        .ifPresent(System.out::println);
```

This is a common practical use of Optional.

---

# 16. Optional Chaining

Optional can be chained.

```java
Optional<Employee> employee =
        Optional.ofNullable(getEmployee());

String name = employee
        .map(Employee::getName)
        .orElse("Unknown");
```

Flow:

```text
Employee
   ↓
map(Employee::getName)
   ↓
String
   ↓
orElse("Unknown")
```

---

# 17. Optional Should Not Usually Be Used for Fields

Avoid unnecessarily doing:

```java
class Employee {

    private Optional<String> name;
}
```

Optional was primarily designed for representing optional return values and handling absence explicitly.

For normal fields, use:

```java
private String name;
```

and handle null appropriately.

---

# 18. Optional Should Not Be Used Everywhere

Optional is not a replacement for every `null`.

Bad:

```java
Optional<String> name =
        Optional.of("Chetan");

if (name.isPresent()) {
    System.out.println(name.get());
}
```

This often defeats the purpose of Optional.

Prefer:

```java
name.ifPresent(System.out::println);
```

---

# 19. Optional with Stream

Optional is commonly used with Streams.

Example:

```java
Optional<String> result =
        names.stream()
                .filter(name -> name.startsWith("C"))
                .findFirst();
```

`findFirst()` returns:

```java
Optional<String>
```

because there may be no matching element.

---

# 20. Common Interview Questions

## Q1. What is Optional?

**Answer:**

`Optional<T>` is a container introduced in Java 8 that may contain a value or be empty. It is mainly used to represent the absence of a value explicitly and reduce null-related problems.

---

## Q2. Difference between `of()` and `ofNullable()`?

**Answer:**

`of()` does not accept null and throws `NullPointerException`.

```java
Optional.of(null); // NPE
```

`ofNullable()` accepts null and creates an empty Optional.

```java
Optional.ofNullable(null); // Optional.empty()
```

---

## Q3. What happens when `get()` is called on empty Optional?

**Answer:**

It throws:

```text
NoSuchElementException
```

---

## Q4. Difference between `orElse()` and `orElseGet()`?

**Answer:**

`orElse()` evaluates the default value even when Optional contains a value.

`orElseGet()` uses a Supplier and evaluates the default only when Optional is empty.

---

## Q5. Difference between `map()` and `flatMap()`?

**Answer:**

`map()` is used when the mapping function returns a normal value.

`flatMap()` is used when the mapping function already returns an Optional.

---

## Q6. Can Optional contain null?

**Answer:**

No.

An Optional either contains a non-null value or is empty.

```java
Optional.ofNullable(null);
```

produces:

```java
Optional.empty()
```

---

## Q7. Is Optional a replacement for null?

**Answer:**

No.

Optional is a tool for explicitly representing an optional value, especially in return values. It should not be treated as a universal replacement for null.

---

## Q8. What is the purpose of `ifPresent()`?

**Answer:**

It executes the given Consumer only when a value is present.

```java
optional.ifPresent(System.out::println);
```

---

## Q9. What happens if `Optional.of(null)` is used?

**Answer:**

It throws:

```text
NullPointerException
```

Use:

```java
Optional.ofNullable(null);
```

instead.

---

## Q10. Why is `Optional.get()` considered risky?

**Answer:**

Because calling `get()` on an empty Optional throws `NoSuchElementException`.

---

# 21. Tricky Interview Questions

### Q1. What is the output?

```java
Optional<String> optional =
        Optional.of("Java");

System.out.println(
        optional.orElse("Default")
);
```

Output:

```text
Java
```

---

### Q2. What happens here?

```java
Optional<String> optional =
        Optional.empty();

System.out.println(optional.get());
```

Answer:

```text
NoSuchElementException
```

---

### Q3. What happens here?

```java
Optional<String> optional =
        Optional.ofNullable(null);

System.out.println(optional);
```

Output:

```text
Optional.empty
```

---

### Q4. What is the difference?

```java
optional.orElse(createObject());
```

and:

```java
optional.orElseGet(() -> createObject());
```

Answer:

`createObject()` can execute even when the Optional contains a value with `orElse()`.

With `orElseGet()`, it executes only when Optional is empty.

---

# 22. Practical Interview Example

Suppose we have:

```java
public Employee findEmployee(int id) {
    return repository.findById(id);
}
```

A better API can return:

```java
public Optional<Employee> findEmployee(int id) {
    return repository.findById(id);
}
```

Then the caller can explicitly handle absence:

```java
Employee employee =
        service.findEmployee(101)
               .orElseThrow(
                   () -> new RuntimeException("Employee not found")
               );
```

This is one reason Optional is commonly seen in Spring Data JPA:

```java
Optional<Employee> findById(Long id);
```

---

# 23. Quick Revision

```text
Optional
    ↓
Represents value or absence of value

of()
    → value must not be null

ofNullable()
    → null allowed

empty()
    → empty Optional

isPresent()
    → checks value

ifPresent()
    → executes if value exists

get()
    → returns value
    → risky if empty

orElse()
    → default value

orElseGet()
    → default Supplier

orElseThrow()
    → throws exception if empty

map()
    → transforms value

flatMap()
    → transforms to Optional without nesting
```

---

# 24. Most Important Interview Points

For interviews, remember these first:

1. `Optional` was introduced in Java 8.
2. It represents a value that may or may not be present.
3. `of()` → never use with a possibly-null value.
4. `ofNullable()` → use when value may be null.
5. `get()` on empty Optional throws `NoSuchElementException`.
6. Know `orElse()` vs `orElseGet()` very well.
7. Know `map()` vs `flatMap()`.
8. Optional is not a universal replacement for `null`.
9. Avoid unnecessary `isPresent()` + `get()`.
10. Optional is commonly used with repository/service return values and Stream operations.

---

# 25. One-Line Interview Answer

> **Optional is a Java 8 container used to represent a value that may or may not be present, helping developers handle absence explicitly instead of relying directly on null.**
