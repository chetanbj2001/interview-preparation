# Generics - Tricky Interview Questions

## Q1. Why is `List<Integer>` not a subtype of `List<Number>`?

Because Java Generics are **invariant**.

```java
List<Integer> integers = new ArrayList<>();
// List<Number> numbers = integers; // Compile-time error
```

If this were allowed:

```java
List<Number> numbers = integers;
numbers.add(10.5); // Double
```

Then `List<Integer>` would contain a `Double`, breaking type safety.

### Correct approach

Use a wildcard:

```java
List<? extends Number> numbers = integers;
```

Now you can safely read `Number` values, but you cannot add new values.

### Interview Point

> `List<Integer>` and `List<Number>` are completely different types. Generics are invariant.

---

# Q2. Why can we read from `? extends` but generally cannot add to it?

Consider:

```java
List<? extends Number> list = new ArrayList<Integer>();
```

The actual list could be:

```java
List<Integer>
List<Double>
List<Float>
```

We don't know the exact type.

Therefore this is unsafe:

```java
// list.add(10);      // Compile-time error
// list.add(10.5);    // Compile-time error
```

But reading is safe:

```java
Number number = list.get(0);
```

Whatever the actual subtype is, it is guaranteed to be a `Number`.

### Rule

```text
? extends T
    ↓
Producer
    ↓
Read safely
    ↓
Cannot generally add
```

---

# Q3. Why can we add to `? super` but reading returns `Object`?

Consider:

```java
List<? super Integer> list = new ArrayList<Number>();
```

The actual list could be:

```java
List<Integer>
List<Number>
List<Object>
```

We can safely add an `Integer`:

```java
list.add(10);
```

Because `Integer` can be stored in all three possible lists.

But when reading:

```java
Object value = list.get(0);
```

We cannot safely assume the exact type.

Therefore:

```java
Object value = list.get(0);
```

is safe.

But:

```java
// Integer value = list.get(0); // Compile-time error
```

### Rule

```text
? super T
    ↓
Consumer
    ↓
Can add T
    ↓
Reading gives Object
```

---

# Q4. Can we overload methods only by changing generic types?

No.

After type erasure, these methods have the same signature:

```java
void process(List<String> list) {
}

void process(List<Integer> list) {
}
```

This causes a compilation error:

```text
name clash: process(List<Integer>) and process(List<String>)
```

Why?

Both become approximately:

```java
void process(List list)
```

after type erasure.

### Interview Point

> You cannot overload methods only by changing generic type arguments.

---

# Q5. What happens to Generics at runtime?

Java uses **type erasure**.

For example:

```java
List<String> list = new ArrayList<>();
list.add("Java");

String value = list.get(0);
```

Conceptually, the compiler handles the generic type information and inserts the required cast when retrieving:

```java
String value = (String) list.get(0);
```

At runtime, the JVM generally works with the erased type such as `List`.

### Why does Java use type erasure?

Main reason:

> **Backward compatibility with older Java code written before Generics were introduced.**

Generics were introduced in **Java 5**.

---

# Q6. Why can't we use `new T()`?

Consider:

```java
class Box<T> {

    T create() {
        // return new T(); // Compile-time error
        return null;
    }
}
```

The problem is that the actual type of `T` is not known in a way that allows Java to determine which constructor should be called.

For example:

```java
Box<String>
Box<Integer>
Box<Employee>
```

The JVM cannot simply perform:

```java
new T();
```

### Common solution

Pass the type explicitly:

```java
class Box<T> {

    private Class<T> type;

    Box(Class<T> type) {
        this.type = type;
    }

    T create() throws Exception {
        return type.newInstance();
    }
}
```

Usage:

```java
Box<String> box = new Box<>(String.class);
```

> Note: `Class.newInstance()` is deprecated in newer Java versions, but this example is valid for understanding the Java 8 interview concept.

---

# Q7. Why can't we create generic arrays?

This is not allowed:

```java
// T[] array = new T[10];
```

The reason is that Java arrays are **reified** — they know their component type at runtime.

Generics use **type erasure**.

This mismatch can cause type-safety problems.

### Preferred approach

Use a collection:

```java
List<T> list = new ArrayList<>();
```

Or receive an array from the caller:

```java
class Box<T> {

    private T[] values;

    Box(T[] values) {
        this.values = values;
    }
}
```

---

# Q8. Can static methods use the class-level generic type `T`?

No.

Example:

```java
class Box<T> {

    // static T value; // Compile-time error

    // static void print(T value) { } // Compile-time error
}
```

Why?

Because `T` belongs to an **instance of the class**.

For example:

```java
Box<String>
Box<Integer>
```

But static members belong to the class itself, not to a particular object/type parameterization.

### However, static methods can have their own generic type

```java
class Utility {

    static <T> void print(T value) {
        System.out.println(value);
    }
}
```

Usage:

```java
Utility.print("Java");
Utility.print(100);
```

### Interview Point

> Static methods cannot directly use class-level type parameters, but they can declare their own generic parameters.

---

# Q9. Can a class have multiple generic parameters?

Yes.

Example:

```java
class Pair<K, V> {

    private K key;
    private V value;

    Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    K getKey() {
        return key;
    }

    V getValue() {
        return value;
    }
}
```

Usage:

```java
Pair<Integer, String> pair =
        new Pair<>(1, "Java");

System.out.println(pair.getKey());
System.out.println(pair.getValue());
```

Common naming conventions:

```text
T → Type
E → Element
K → Key
V → Value
N → Number
R → Return type
```

---

# Q10. What is the difference between `T` and `?`?

### `T`

`T` represents a **named type parameter**.

```java
class Box<T> {

    private T value;

    void set(T value) {
        this.value = value;
    }

    T get() {
        return value;
    }
}
```

The same `T` represents the same type within its scope.

### `?`

`?` represents an **unknown type**.

```java
void print(List<?> list) {
    for (Object value : list) {
        System.out.println(value);
    }
}
```

It means:

> "I don't care what the exact element type is."

### Simple difference

```text
T  → Named type
?  → Unknown type
```

---

# Q11. What happens when we use a raw type?

Example:

```java
List list = new ArrayList();

list.add("Java");
list.add(100);
```

A raw `List` removes generic type safety.

Now:

```java
String value = (String) list.get(1);
```

causes:

```text
ClassCastException
```

### Generic version

```java
List<String> list = new ArrayList<>();

list.add("Java");
// list.add(100); // Compile-time error
```

Generics move many errors from **runtime to compile time**.

### Interview Point

> Raw types exist mainly for backward compatibility and should generally be avoided in new code.

---

# Q12. Can we use `instanceof` with Generics?

You cannot check an exact parameterized type at runtime:

```java
List<String> list = new ArrayList<>();

// if (list instanceof List<String>) { } // Compile-time error
```

Because generic type arguments are erased.

You can check the raw type:

```java
if (list instanceof List) {
    System.out.println("It is a List");
}
```

You can also use an unbounded wildcard:

```java
if (list instanceof List<?>) {
    System.out.println("It is a List");
}
```

### Interview Point

> Runtime cannot distinguish `List<String>` from `List<Integer>` because of type erasure.

---

# Q13. Why can't we overload these two methods?

```java
void process(List<String> list) {
}

void process(List<Integer> list) {
}
```

Because after type erasure both become:

```java
void process(List list)
```

Therefore they have the same method signature.

But this is allowed:

```java
void process(List<String> list) {
}

void process(Set<String> set) {
}
```

because `List` and `Set` are different parameter types.

---

# Q14. What is the main reason Java Generics use type erasure?

The major reason is:

> **Backward compatibility.**

Before Java 5:

```java
List list = new ArrayList();
```

After Java 5:

```java
List<String> list = new ArrayList<>();
```

Existing Java bytecode and APIs needed to continue working.

Therefore Java implemented Generics primarily through compile-time checking and type erasure rather than introducing fully reified generics.

---

# Q15. What is PECS?

PECS means:

> **Producer Extends, Consumer Super**

### Producer

If you only want to **read**:

```java
List<? extends Number>
```

The list produces `Number` values.

### Consumer

If you want to **write/add**:

```java
List<? super Integer>
```

The list consumes `Integer` values.

Example:

```java
static double sum(List<? extends Number> numbers) {

    double total = 0;

    for (Number number : numbers) {
        total += number.doubleValue();
    }

    return total;
}
```

This works with:

```java
List<Integer>
List<Double>
List<Float>
```

### Interview one-liner

> Use `extends` when the generic structure is a producer and `super` when it is a consumer.

---

# Q16. Why is `ArrayList<Integer>` allowed but `ArrayList<int>` not?

Generics work with **reference types**, not primitive types.

This is invalid:

```java
// List<int> list;
```

Use the wrapper:

```java
List<Integer> list = new ArrayList<>();
```

Java provides **autoboxing**:

```java
list.add(10);
```

Conceptually:

```java
list.add(Integer.valueOf(10));
```

And unboxing:

```java
int value = list.get(0);
```

Conceptually:

```java
int value = list.get(0).intValue();
```

---

# Q17. What is the difference between `List<?>` and raw `List`?

### Raw type

```java
List list = new ArrayList();
```

Generic type checking is mostly disabled.

You can add different types:

```java
list.add("Java");
list.add(100);
```

### Unbounded wildcard

```java
List<?> list = new ArrayList<String>();
```

The exact type is unknown, but type safety is maintained.

You cannot normally add values:

```java
// list.add("Java"); // Compile-time error
```

You can safely read as:

```java
Object value = list.get(0);
```

### Key difference

```text
Raw List
→ legacy
→ loses type safety

List<?>
→ unknown type
→ maintains type safety
```

---

# Q18. What are the advantages and disadvantages of Generics?

## Advantages

### 1. Compile-time type safety

```java
List<String> list = new ArrayList<>();
```

Wrong types are detected at compile time.

### 2. Less casting

Without Generics:

```java
String value = (String) list.get(0);
```

With Generics:

```java
String value = list.get(0);
```

### 3. Code reusability

```java
class Box<T> {
    T value;
}
```

Can be used as:

```java
Box<String>
Box<Integer>
Box<Employee>
```

### 4. Better API design

Collections and APIs can clearly define what type they accept and return.

---

## Disadvantages / Limitations

### 1. No primitive types

```java
List<int> // invalid
```

Use:

```java
List<Integer>
```

### 2. Type erasure

Generic type information is mostly erased at runtime.

### 3. Generic arrays are restricted

```java
// new T[10]
```

is not allowed.

### 4. Cannot directly instantiate `T`

```java
// new T()
```

is not allowed.

### 5. Generic type checking can be complex

Wildcards and bounds can make APIs harder to understand.

---

# Quick Tricky Revision

| Question                           | Key Answer                       |
| ---------------------------------- | -------------------------------- |
| `List<Integer>` → `List<Number>`?  | ❌ No, Generics are invariant     |
| `? extends`                        | Producer / read                  |
| `? super`                          | Consumer / write                 |
| PECS                               | Producer Extends, Consumer Super |
| `new T()`?                         | ❌ Not directly                   |
| `new T[]`?                         | ❌ Not directly                   |
| Primitive generic type?            | ❌ Use wrapper                    |
| Static method with class `T`?      | ❌ Directly no                    |
| Static generic method?             | ✅ Declare its own `<T>`          |
| Generic overload by type argument? | ❌ Due to erasure                 |
| Exact `instanceof List<String>`?   | ❌                                |
| Raw type?                          | Legacy, avoid                    |
| `T` vs `?`                         | Named type vs unknown type       |
| Main reason for erasure?           | Backward compatibility           |

---

# Interview Golden Rules

```text
Generics
    ↓
Compile-time type safety
    ↓
Invariant
    ↓
? extends → Producer
? super   → Consumer
    ↓
PECS
    ↓
Type Erasure
    ↓
Runtime generic type information mostly unavailable
```

### Most Important Questions to Master

1. Why is `List<Integer>` not a subtype of `List<Number>`?
2. Explain `? extends` vs `? super`.
3. Explain PECS with an example.
4. What is type erasure?
5. Why can't we use `new T()`?
6. Why can't we create generic arrays?
7. Why can't static members use class-level `T`?
8. Why can't we overload methods using only generic type arguments?
9. Raw type vs wildcard.
10. Why don't Generics support primitive types?

```

Next file after this is **`03_Generics_Quick_Revision.md`**, which will be a very short last-minute interview revision sheet.
```
