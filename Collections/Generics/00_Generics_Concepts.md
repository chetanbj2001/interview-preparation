# Generics in Java

## 1. What are Generics?

Generics allow us to write classes, interfaces, and methods that work with different types while providing **compile-time type safety**.

Without Generics:

```java
List list = new ArrayList();

list.add("Java");
list.add(100);

String value = (String) list.get(0);
```

Problems:

* Type casting is required.
* Wrong types can be inserted.
* Errors may occur at runtime.

With Generics:

```java
List<String> list = new ArrayList<>();

list.add("Java");
// list.add(100); // Compile-time error

String value = list.get(0);
```

The compiler knows that the list contains `String`.

---

# 2. Why do we need Generics?

Generics mainly provide:

### 1. Type Safety

```java
List<String> names = new ArrayList<>();

names.add("Chetan");
// names.add(100); // Compile-time error
```

### 2. No unnecessary casting

Without Generics:

```java
String name = (String) list.get(0);
```

With Generics:

```java
String name = names.get(0);
```

### 3. Reusable code

The same generic class/method can work with different types.

```java
Box<String>
Box<Integer>
Box<Double>
```

---

# 3. Generic Class

A class can define a type parameter.

```java
class Box<T> {

    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}
```

Usage:

```java
Box<String> stringBox = new Box<>();
stringBox.set("Java");

String value = stringBox.get();
```

Another type:

```java
Box<Integer> intBox = new Box<>();
intBox.set(100);

Integer number = intBox.get();
```

Here:

```text
T → Type parameter
```

`T` is replaced by the actual type when the object is created.

---

# 4. Generic Method

A method can have its own type parameter.

```java
public static <T> void print(T value) {
    System.out.println(value);
}
```

Usage:

```java
print("Java");
print(100);
print(10.5);
```

The important syntax is:

```java
<T> void
```

The `<T>` before the return type declares the method's type parameter.

---

# 5. Generic Method vs Generic Class

### Generic class

```java
class Box<T> {
    T value;
}
```

`T` belongs to the class.

### Generic method

```java
static <T> void print(T value) {
}
```

`T` belongs only to the method.

A non-generic class can have a generic method:

```java
class Utility {

    static <T> void print(T value) {
        System.out.println(value);
    }
}
```

---

# 6. Generic Interface

Interfaces can also have type parameters.

```java
interface Repository<T> {

    void save(T value);

    T find();
}
```

Implementation:

```java
class UserRepository implements Repository<String> {

    @Override
    public void save(String value) {
        System.out.println(value);
    }

    @Override
    public String find() {
        return "Chetan";
    }
}
```

---

# 7. Multiple Generic Parameters

A class can have multiple type parameters.

```java
class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}
```

Usage:

```java
Pair<Integer, String> pair =
        new Pair<>(1, "Java");
```

Common naming conventions:

```text
T → Type
E → Element
K → Key
V → Value
N → Number
```

These are conventions, not Java keywords.

---

# 8. Bounded Type Parameters

Sometimes we don't want to allow every type.

We can restrict the type using `extends`.

```java
class Calculator<T extends Number> {

    private T value;

    public Calculator(T value) {
        this.value = value;
    }
}
```

Allowed:

```java
Calculator<Integer> c1 = new Calculator<>(10);
Calculator<Double> c2 = new Calculator<>(10.5);
```

Not allowed:

```java
// Calculator<String> c = new Calculator<>("Java");
```

Because `String` does not extend `Number`.

---

# 9. Multiple Bounds

A type parameter can have multiple bounds.

```java
<T extends Number & Comparable<T>>
```

Example:

```java
public static <T extends Number & Comparable<T>>
boolean compare(T a, T b) {
    return a.compareTo(b) > 0;
}
```

### Important rule

If there is a class bound, it must come first.

```java
<T extends Number & Comparable<T>>
```

Valid.

```java
// <T extends Comparable<T> & Number>
```

Invalid.

---

# 10. Wildcards

A wildcard is represented by:

```java
?
```

It means:

> Unknown type.

Example:

```java
List<?> list;
```

This means:

> A List of some unknown type.

It could be:

```java
List<String>
List<Integer>
List<Double>
```

---

# 11. Upper-Bounded Wildcard

Syntax:

```java
<? extends T>
```

Example:

```java
List<? extends Number> numbers;
```

It can refer to:

```java
List<Integer>
List<Double>
List<Float>
```

because all of them extend `Number`.

### Main idea

`extends` is useful when the collection is a **producer** of values.

```java
Number number = numbers.get(0);
```

Reading is safe because whatever the actual type is, it is at least a `Number`.

---

# 12. Lower-Bounded Wildcard

Syntax:

```java
<? super T>
```

Example:

```java
List<? super Integer> numbers;
```

It can refer to:

```java
List<Integer>
List<Number>
List<Object>
```

We can safely add an `Integer`:

```java
numbers.add(10);
```

But reading a value gives only `Object` safely:

```java
Object value = numbers.get(0);
```

---

# 13. `extends` vs `super`

| `<? extends T>`       | `<? super T>`          |
| --------------------- | ---------------------- |
| Upper bound           | Lower bound            |
| Producer              | Consumer               |
| Good for reading      | Good for writing       |
| Can read as `T`       | Can add `T`            |
| Cannot safely add `T` | Reading gives `Object` |

Example:

```java
List<? extends Number> producer;
```

Read:

```java
Number n = producer.get(0);
```

But:

```java
// producer.add(10); // Not allowed
```

Consumer:

```java
List<? super Integer> consumer;

consumer.add(10);
```

---

# 14. PECS

PECS means:

> **Producer Extends, Consumer Super**

### Producer

If a structure produces values for you:

```java
<? extends T>
```

### Consumer

If you put values into it:

```java
<? super T>
```

Example:

```java
public static <T> void copy(
        List<? extends T> source,
        List<? super T> destination) {

    for (T value : source) {
        destination.add(value);
    }
}
```

Here:

```text
source      → Producer → extends
destination → Consumer → super
```

This is one of the most important Generics interview concepts.

---

# 15. Type Erasure

Java Generics mainly provide type checking at compile time.

The compiler performs type checking and then erases most generic type information from the runtime representation.

Example:

```java
List<String> names = new ArrayList<>();
```

Conceptually, runtime does not retain the generic type in the same way the compiler uses it.

This is called:

> **Type Erasure**

It allows Java Generics to maintain compatibility with older Java code.

---

# 16. Why can't we use primitive types?

Generics work with reference types, not primitives.

Invalid:

```java
// List<int> numbers;
```

Use wrapper classes:

```java
List<Integer> numbers;
List<Double> values;
List<Long> ids;
```

Java automatically handles boxing/unboxing:

```java
numbers.add(10);
```

The `int` is boxed into `Integer`.

---

# 17. Generics with Collections

Generics are heavily used by the Java Collections Framework.

```java
List<String> names = new ArrayList<>();

Set<Integer> numbers = new HashSet<>();

Map<String, Integer> marks = new HashMap<>();
```

Without Generics:

```java
List list = new ArrayList();
```

This is a **raw type** and loses compile-time type safety.

Prefer:

```java
List<String> list = new ArrayList<>();
```

---

# 18. Limitations of Generics

### Cannot use primitive types

```java
// List<int> ❌
```

Use:

```java
List<Integer> ✅
```

### Cannot directly create `new T()`

```java
// T obj = new T(); ❌
```

Because the actual type is not known in that form at runtime.

### Cannot create generic arrays directly

```java
// T[] array = new T[10]; ❌
```

### Cannot use class-level T directly in a static context

```java
class Box<T> {

    // static T value; ❌
}
```

Because `T` belongs to an instance of `Box<T>`, while `static` belongs to the class itself.

A static method can declare its own type parameter:

```java
static <T> void print(T value) {
    System.out.println(value);
}
```

---

# 19. Raw Types

A raw type removes the generic type information.

```java
List list = new ArrayList();
```

This is allowed for backward compatibility but should generally be avoided.

Better:

```java
List<String> list = new ArrayList<>();
```

Raw types can cause:

* Runtime `ClassCastException`
* Loss of type safety
* Compiler warnings

---

# 20. `T` vs `?`

### `T`

Represents a specific type.

```java
<T> void process(T value)
```

The same `T` can connect multiple parts of the method.

Example:

```java
static <T> T identity(T value) {
    return value;
}
```

### `?`

Represents an unknown type.

```java
List<?> list;
```

You don't know the exact type.

### Simple difference

```text
T → I care about the specific type.

? → I don't need to know the specific type.
```

---

# ⭐ Interview Summary

```text
Generics
   ↓
Compile-time type safety
   ↓
Reusable type-safe code
```

Remember:

```text
T → Type
E → Element
K → Key
V → Value

extends → Producer
super    → Consumer

PECS → Producer Extends, Consumer Super

<?>            → Unknown type
<? extends T>  → Upper bound
<? super T>    → Lower bound

Type Erasure → Generic type information is mostly removed
               from the runtime representation
```

### Most Important Interview Topics

1. Why Generics?
2. Generic class vs generic method
3. Bounded types
4. `? extends`
5. `? super`
6. PECS
7. Type erasure
8. `T` vs `?`
9. Raw types
10. Limitations of Generics
