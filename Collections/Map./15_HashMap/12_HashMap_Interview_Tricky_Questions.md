# HashMap — Interview Tricky Questions

## 1. What happens if two keys have the same hashCode()?

They go to the **same bucket**, but HashMap uses `equals()` to distinguish them.

```text
Same hashCode()
      ↓
Same bucket
      ↓
equals()
      ↓
Different keys → both stored
```

**Interview answer:**

> Hash collision occurs. HashMap stores both entries in the same bucket and uses `equals()` to identify the correct key.

---

## 2. What happens if equals() returns true but hashCode() is different?

This violates the Java contract.

```java
a.equals(b) == true
a.hashCode() != b.hashCode()
```

HashMap may put them into different buckets, causing incorrect lookup behavior.

### Rule

> If two objects are equal according to `equals()`, they must have the same `hashCode()`.

---

## 3. What happens if hashCode() is same but equals() returns false?

That's completely valid.

```java
a.hashCode() == b.hashCode()
a.equals(b) == false
```

This is a **hash collision**.

HashMap can store both keys.

---

## 4. What happens if I override equals() but not hashCode()?

Bad practice.

Two logically equal objects may have different hash codes.

```java
User u1 = new User(1);
User u2 = new User(1);

u1.equals(u2); // true
```

If their hash codes differ, HashMap may put them in different buckets.

```java
map.put(u1, "A");

map.get(u2); // may return null
```

---

## 5. What happens if I override hashCode() but not equals()?

Also dangerous.

Two different objects can have the same hash code.

That's allowed, but without correctly implementing `equals()`, HashMap treats them as different keys.

**Important:**

> Same hashCode does NOT mean objects are equal.

---

## 6. Can HashMap have duplicate keys?

No.

```java
map.put("Java", 10);
map.put("Java", 20);
```

There is only one `"Java"` key.

The value is replaced:

```text
Java → 20
```

---

## 7. Can HashMap have duplicate values?

Yes.

```java
map.put("A", 100);
map.put("B", 100);
```

Both entries are valid.

---

## 8. What happens if I put the same key twice?

The old value is replaced.

```java
map.put("Java", 10);
map.put("Java", 20);
```

Result:

```text
Java → 20
```

`put()` returns the previous value.

```java
Integer old = map.put("Java", 30);
System.out.println(old); // 20
```

---

## 9. Why does HashMap allow one null key?

`HashMap` explicitly supports one `null` key and multiple `null` values.

```java
map.put(null, 100);
map.put(null, 200);
```

Result:

```text
null → 200
```

The second insertion replaces the first value.

---

## 10. Why doesn't ConcurrentHashMap allow null?

Because in concurrent code, `null` could make it ambiguous whether:

```text
key is absent
```

or

```text
key exists with null value
```

Therefore `ConcurrentHashMap` does not permit null keys or values.

---

## 11. Is HashMap insertion order guaranteed?

**No.**

Do not depend on the order returned by:

```java
map.entrySet()
```

If you need insertion order, use:

```java
LinkedHashMap
```

---

## 12. Is HashMap sorted?

No.

If you need sorted keys, use:

```java
TreeMap
```

---

## 13. HashMap vs LinkedHashMap vs TreeMap

| Map             | Ordering               | Typical lookup |
| --------------- | ---------------------- | -------------- |
| `HashMap`       | No guaranteed order    | O(1) average   |
| `LinkedHashMap` | Insertion/access order | O(1) average   |
| `TreeMap`       | Sorted order           | O(log n)       |

---

## 14. Can HashMap keys be mutable?

Yes, but **avoid it**.

If fields used in `hashCode()` or `equals()` change after insertion, lookup may fail.

```java
map.put(user, "Developer");

user.setId(2);

map.get(user); // may return null
```

---

## 15. What is the default initial capacity?

```text
16
```

The default load factor is:

```text
0.75
```

So the initial resize threshold is approximately:

```text
16 × 0.75 = 12
```

---

## 16. Does HashMap resize exactly when size becomes 12?

Be careful.

The resize decision depends on the implementation's threshold and insertion process. For the default Java 8 configuration, the initial threshold is effectively **12** after the table is initialized.

For interview purposes:

> Default capacity = 16, load factor = 0.75, so resize threshold is 12.

---

## 17. What happens when HashMap exceeds its threshold?

HashMap resizes its internal table.

Typically:

```text
16
 ↓
32
 ↓
64
 ↓
128
```

The capacity generally doubles.

---

## 18. Why is resizing expensive?

Existing entries may need to be redistributed into the new table.

Therefore, resizing can be costly.

### Optimization

If you know approximately how many entries you'll store, providing a suitable initial capacity can reduce unnecessary resizing.

---

## 19. What is a hash collision?

When different keys produce the same bucket index.

Example:

```text
Key A → bucket 5
Key B → bucket 5
```

HashMap must handle both entries in that bucket.

---

## 20. Does same hashCode() mean same bucket?

Not directly.

HashMap applies its hash-spreading calculation and then derives the bucket index from the table size.

Conceptually:

```text
key
 ↓
hashCode()
 ↓
hash spreading
 ↓
bucket index
```

So don't oversimplify it as:

> "hashCode directly equals bucket number."

---

## 21. Why is HashMap lookup O(1) average?

Because hashing usually allows HashMap to directly locate the appropriate bucket.

Conceptually:

```text
key
 ↓
hash
 ↓
bucket
 ↓
entry
```

So average lookup is approximately:

```text
O(1)
```

Worst-case behavior depends on collisions and the implementation.

---

## 22. What changed in Java 8 for heavily-collided buckets?

Java 8 can convert a sufficiently large collision chain into a **Red-Black Tree** under certain conditions.

This improves lookup performance for heavily-collided buckets.

Instead of potentially:

```text
O(n)
```

the tree-based bucket can provide approximately:

```text
O(log n)
```

lookup within that bucket.

---

## 23. Is HashMap thread-safe?

No.

Multiple threads modifying a `HashMap` concurrently can cause incorrect behavior.

For concurrent access, consider:

```java
ConcurrentHashMap
```

or appropriate external synchronization.

---

## 24. Is Collections.synchronizedMap() the same as ConcurrentHashMap?

No.

```java
Collections.synchronizedMap(new HashMap<>());
```

provides synchronized access around map operations.

`ConcurrentHashMap` is specifically designed for concurrent access and generally offers better scalability.

---

## 25. What happens if a key's hashCode changes after insertion?

The entry may remain in its original bucket, while future lookups calculate a different location.

Therefore:

```java
map.get(key)
```

may fail to find the entry.

This is one of the most important mutable-key interview questions.

---

# ⭐ Rapid-Fire Tricky Questions

### Q: Can HashMap contain multiple null values?

**Yes.**

### Q: Can HashMap contain multiple null keys?

**No. One null key.**

### Q: Can two different keys have the same hashCode?

**Yes.**

### Q: Can two equal objects have different hashCodes?

**No. That violates the contract.**

### Q: Can two different objects have the same hashCode?

**Yes. That's a collision.**

### Q: Does HashMap maintain insertion order?

**No.**

### Q: Is HashMap sorted?

**No.**

### Q: Is HashMap synchronized?

**No.**

### Q: Is ConcurrentHashMap synchronized?

It is thread-safe, but don't describe it simply as "the whole map is synchronized." It uses concurrency mechanisms such as CAS and fine-grained synchronization.

### Q: What is the default HashMap capacity?

**16.**

### Q: Default load factor?

**0.75.**

### Q: What happens when the threshold is exceeded?

**HashMap resizes its table.**

### Q: Does HashMap use a LinkedList forever for collisions?

**No.** Java 8 can treeify sufficiently large collision buckets under the implementation's conditions.

---

# 🎯 Most Important Interview Traps

Remember these five:

```text
1. Same hashCode ≠ same object

2. equals() true → hashCode() MUST be same

3. Mutable keys can break HashMap lookup

4. HashMap does NOT guarantee ordering

5. HashMap is NOT thread-safe
```

---

# Final Interview Answer

> HashMap stores key-value pairs using hashing. It uses `hashCode()` to locate a bucket and `equals()` to identify the correct key. Hash collisions are handled within the bucket, and Java 8 can treeify heavily-collided buckets. HashMap allows one null key and multiple null values, does not guarantee ordering, and is not thread-safe. Keys should preferably be immutable so their hash code and equality behavior remain stable after insertion.
