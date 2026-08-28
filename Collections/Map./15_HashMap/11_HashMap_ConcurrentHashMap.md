# HashMap — ConcurrentHashMap

## 1. What is ConcurrentHashMap?

`ConcurrentHashMap` is a thread-safe implementation of `Map` designed for concurrent access.

```java
Map<String, Integer> map = new ConcurrentHashMap<>();

map.put("Java", 10);
```

Multiple threads can read and update it safely.

---

## 2. HashMap vs ConcurrentHashMap

| Feature          | HashMap                       | ConcurrentHashMap            |
| ---------------- | ----------------------------- | ---------------------------- |
| Thread-safe      | ❌ No                          | ✅ Yes                        |
| Multiple threads | Unsafe for concurrent updates | Designed for concurrency     |
| `null` key       | ✅ One                         | ❌ Not allowed                |
| `null` values    | ✅ Allowed                     | ❌ Not allowed                |
| Performance      | Faster in single-threaded use | Better for concurrent access |
| Package          | `java.util`                   | `java.util.concurrent`       |

---

## 3. Why doesn't ConcurrentHashMap allow null?

This avoids ambiguity in concurrent operations.

For example:

```java
map.get("Java");
```

If the result is `null`, it can clearly mean:

> `"Java"` does not have a mapping.

There is no ambiguity between **"key absent"** and **"key mapped to null"**.

---

## 4. How does ConcurrentHashMap achieve thread safety?

The implementation has changed across Java versions.

### Java 7

It used **Segment-based locking**.

```text
ConcurrentHashMap
      |
      +-- Segment 1
      +-- Segment 2
      +-- Segment 3
      +-- ...
```

Different segments could be locked independently.

### Java 8

Java 8 removed the `Segment` architecture.

It uses:

* CAS (Compare-And-Swap)
* `synchronized` on individual bins when needed
* Fine-grained locking
* Volatile/atomic operations

This allows better concurrency than locking the entire map.

---

## 5. Does ConcurrentHashMap lock the entire map?

**No.**

It does not use one global lock for normal updates.

Java 8 uses fine-grained synchronization, allowing multiple threads to work on different parts of the map concurrently.

---

## 6. Is ConcurrentHashMap completely lock-free?

**No.**

It uses CAS for some operations and `synchronized` for certain updates.

So don't say:

> "ConcurrentHashMap is completely lock-free."

Better interview answer:

> ConcurrentHashMap uses a combination of CAS, volatile operations, and fine-grained synchronization to provide thread-safe concurrent access.

---

## 7. What happens when multiple threads update different keys?

They can generally proceed concurrently.

```java
map.put("Java", 10);
map.put("Spring", 20);
map.put("Docker", 30);
```

ConcurrentHashMap is designed to minimize contention between independent operations.

---

## 8. Does ConcurrentHashMap allow null keys?

```java
ConcurrentHashMap<String, Integer> map =
        new ConcurrentHashMap<>();

map.put(null, 10); // NullPointerException
```

❌ `null` key is not allowed.

Similarly:

```java
map.put("Java", null); // NullPointerException
```

❌ `null` values are not allowed.

---

## 9. ConcurrentHashMap vs Collections.synchronizedMap()

```java
Map<String, Integer> map =
        Collections.synchronizedMap(new HashMap<>());
```

vs

```java
Map<String, Integer> map =
        new ConcurrentHashMap<>();
```

### synchronizedMap

Uses synchronization around map operations.

### ConcurrentHashMap

Designed specifically for concurrent access and generally provides better scalability under contention.

---

## 10. Is this operation atomic?

Consider:

```java
if (!map.containsKey("Java")) {
    map.put("Java", 10);
}
```

❌ The complete sequence is **not atomic**.

Another thread can modify the map between `containsKey()` and `put()`.

Instead, use:

```java
map.putIfAbsent("Java", 10);
```

This provides the required atomic operation.

---

## 11. Important ConcurrentHashMap methods

### `putIfAbsent()`

```java
map.putIfAbsent("Java", 10);
```

Adds the value only if the key doesn't already exist.

### `computeIfAbsent()`

```java
map.computeIfAbsent("Java", k -> 10);
```

Computes and inserts a value only when the key is absent.

### `compute()`

```java
map.compute("Java", (k, v) -> v == null ? 1 : v + 1);
```

Atomically computes the value for the key.

### `merge()`

```java
map.merge("Java", 1, Integer::sum);
```

Useful for counters.

---

## 12. Common Interview Trap

### Q: Can ConcurrentHashMap guarantee that every operation is atomic?

**No.**

Individual supported atomic operations such as:

```java
putIfAbsent()
compute()
merge()
```

are atomic.

But a sequence of separate operations is not automatically atomic.

---

## 13. Quick Interview Questions

### Q: Is ConcurrentHashMap thread-safe?

Yes.

### Q: Does it allow null?

No, neither null keys nor null values.

### Q: Is ConcurrentHashMap faster than HashMap?

Not necessarily.

For single-threaded code, `HashMap` generally has less overhead.

`ConcurrentHashMap` is useful when multiple threads access the map concurrently.

### Q: Does Java 8 ConcurrentHashMap use Segments?

No.

The Java 8 implementation removed the old segment-based architecture.

### Q: What should you use for a shared map accessed by multiple threads?

Usually `ConcurrentHashMap`.

### Q: Is `HashMap` safe if multiple threads only read it?

It can be safely read concurrently **if the map is safely published and never modified afterward**. Concurrent modification requires proper synchronization/concurrency control.

---

## ⭐ Interview Summary

> `ConcurrentHashMap` is a thread-safe map designed for high-concurrency scenarios. Unlike `HashMap`, it does not allow null keys or values. In Java 8, it uses CAS, volatile operations, and fine-grained synchronization rather than the old Segment-based locking approach. Methods such as `putIfAbsent()`, `compute()`, and `merge()` provide useful atomic operations.

### Java 8 Concepts Used

* `ConcurrentHashMap`
* CAS
* `volatile`
* `synchronized`
* `putIfAbsent()`
* `computeIfAbsent()`
* `compute()`
* `merge()`
