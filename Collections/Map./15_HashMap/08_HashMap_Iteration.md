# HashMap — Iteration

---

## 1. How Can We Iterate Over a HashMap?

### Using `entrySet()` ⭐⭐⭐⭐⭐

```java
Map<Integer, String> map = new HashMap<>();

for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " = " + entry.getValue());
}
```

Use `entrySet()` when you need both key and value.

---

## 2. `keySet()` vs `entrySet()`

### `keySet()`

Use when you only need keys:

```java
for (Integer key : map.keySet()) {
    System.out.println(key);
}
```

### `entrySet()`

Use when you need both key and value:

```java
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " = " + entry.getValue());
}
```

### Interview Answer

> If I need both key and value, I prefer `entrySet()` because each entry already contains both.

---

## 3. Using `forEach()` — Java 8

Java 8 provides a cleaner way:

```java
map.forEach((key, value) ->
        System.out.println(key + " = " + value));
```

### Java 8 Feature

```text
Lambda Expression
```

---

## 4. Can We Modify HashMap While Iterating?

Direct structural modification can cause:

```text
ConcurrentModificationException
```

Example:

```java
for (Integer key : map.keySet()) {
    map.remove(key); // Unsafe
}
```

---

## 5. How Can We Safely Remove During Iteration?

Use `Iterator.remove()`:

```java
Iterator<Map.Entry<Integer, String>> iterator =
        map.entrySet().iterator();

while (iterator.hasNext()) {

    Map.Entry<Integer, String> entry = iterator.next();

    if (entry.getKey() == 10) {
        iterator.remove();
    }
}
```

---

## 6. Is HashMap Ordered?

No.

HashMap does not guarantee:

- Insertion order
- Sorted order

If ordering is required:

```text
Insertion order → LinkedHashMap

Sorted order    → TreeMap
```

---

## 7. Does HashMap Maintain Insertion Order? ⭐⭐⭐⭐⭐

### Answer

No.

Never depend on the iteration order of a `HashMap`.

---

## 8. Iteration Complexity

For:

```text
Capacity = C
Size = N
```

HashMap iteration is approximately:

```text
O(C + N)
```

---

## 9. Quick Revision

```text
Both key + value → entrySet()

Only keys        → keySet()

Java 8           → forEach()

Safe removal     → Iterator.remove()

Insertion order  → LinkedHashMap

Sorted order     → TreeMap

HashMap order    → Not guaranteed
```

---

## 10. Interview Question

### Which is better: `keySet()` or `entrySet()`?

If both key and value are required:

```text
entrySet()
```

is preferred because the value is available directly from the entry.

If only keys are required:

```text
keySet()
```

is appropriate.
