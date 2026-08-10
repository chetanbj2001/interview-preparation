
# HashMap in Java — Overview

---

# 1. What is HashMap?

`HashMap` is a class in Java that implements the `Map` interface and stores data in **key-value pairs**.

Each key must be unique, while multiple keys can have the same value.

HashMap uses **hashing** internally to provide average **O(1)** time complexity for `put()`, `get()`, and `remove()` operations.

HashMap does not guarantee any ordering of its elements.

It allows:

* One `null` key
* Multiple `null` values
* Duplicate values

HashMap is **not thread-safe**.

---

# 2. Why was HashMap Introduced?

Suppose we have employee data:

```text
101 → Rahul
102 → Amit
103 → Chetan
```

If we use a List, finding an employee by ID may require searching through elements.

With HashMap:

```java
employees.get(102);
```

we can retrieve the employee directly using the key.

HashMap is designed for **fast key-based lookup**.

---

# 3. Why Do We Need HashMap?

HashMap is useful when:

* We have a unique identifier.
* We need fast lookup.
* Ordering is not required.
* We want key-value relationships.

Examples:

```text
Employee ID → Employee

Product ID → Product

Username → User

Country Code → Country

Word → Frequency
```

---

# 4. Basic Example

```java
Map<Integer, String> employees = new HashMap<>();

employees.put(101, "Rahul");
employees.put(102, "Amit");
employees.put(103, "Chetan");

System.out.println(employees.get(102));
```

Output:

```text
Amit
```

---

# 5. HashMap Characteristics

| Feature            | HashMap             |
| ------------------ | ------------------- |
| Interface          | Map                 |
| Stores             | Key-Value pairs     |
| Duplicate Keys     | Not allowed         |
| Duplicate Values   | Allowed             |
| Null Key           | One                 |
| Null Values        | Multiple            |
| Ordering           | No guaranteed order |
| Thread Safe        | No                  |
| Average `get()`    | O(1)                |
| Average `put()`    | O(1)                |
| Average `remove()` | O(1)                |

---

# 6. What Happens When We Put the Same Key?

Consider:

```java
Map<Integer, String> map = new HashMap<>();

map.put(101, "Rahul");
map.put(101, "Amit");
```

The second `put()` replaces the previous value.

Final Map:

```text
101 → Amit
```

There is still only one key:

```text
101
```

---

# 7. Can HashMap Have Duplicate Values?

Yes.

```java
map.put(101, "Java");
map.put(102, "Java");
```

This is completely valid.

Result:

```text
101 → Java
102 → Java
```

Keys must be unique.

Values do not have to be unique.

---

# 8. Can HashMap Have Null?

Yes.

HashMap allows:

```text
One null key

Multiple null values
```

Example:

```java
Map<Integer, String> map = new HashMap<>();

map.put(null, "Java");
map.put(101, null);
map.put(102, null);
```

This is valid.

---

# 9. Is HashMap Thread-Safe?

No.

HashMap is not designed for concurrent modification by multiple threads without external synchronization.

For concurrent access, Java provides alternatives such as:

```java
ConcurrentHashMap
```

We will study this later.

---

# 10. Internal Working — High Level

This is where HashMap becomes important.

HashMap internally uses a **hash table**.

Conceptually:

```text
HashMap

      ↓

Bucket Array

      ↓

Bucket

      ↓

Node
```

A simplified structure looks like:

```text
table[]

  0 → null

  1 → Node

  2 → null

  3 → Node → Node

  4 → null

  5 → Node
```

Multiple nodes can exist in the same bucket because of **hash collisions**.

We will study this in extreme detail in:

```text
01_Internal_Working.md
```

---

# 11. How Does HashMap Find a Bucket?

When we execute:

```java
map.put("Java", 100);
```

HashMap uses the key's:

```java
hashCode()
```

The hash is processed to determine a bucket index.

Conceptually:

```text
Key
 ↓
hashCode()
 ↓
Hash
 ↓
Bucket Index
 ↓
Bucket
```

The exact hashing and index calculation are important interview topics and will be covered separately.

---

# 12. What Happens During get()?

Suppose:

```java
map.get("Java");
```

Conceptually:

```text
"Java"
   ↓
hashCode()
   ↓
Hash
   ↓
Bucket Index
   ↓
Bucket
   ↓
Compare Keys
   ↓
Return Value
```

HashMap uses both:

```java
hashCode()
```

and

```java
equals()
```

to identify the correct key.

---

# 13. Why Are hashCode() and equals() Important?

Suppose:

```java
map.put(key1, value1);
```

Later:

```java
map.get(key2);
```

Even if `key1` and `key2` are different object references, HashMap can find the value if they are logically equal.

This depends on the contract between:

```java
hashCode()
```

and

```java
equals()
```

Important rule:

> If two objects are equal according to `equals()`, they must return the same `hashCode()`.

This topic deserves its own detailed file.

---

# 14. What is a Collision?

A collision occurs when multiple keys end up mapped to the same bucket.

Conceptually:

```text
Key A
  ↓
Bucket 5

Key B
  ↓
Bucket 5
```

The bucket may then contain:

```text
Bucket 5

Node A → Node B
```

HashMap has mechanisms to handle collisions.

We will study collision handling separately.

---

# 15. What is the Load Factor?

Load factor controls when HashMap should resize its internal table.

The default load factor is:

```text
0.75
```

Conceptually:

```text
Threshold = Capacity × Load Factor
```

For example:

```text
Capacity = 16

Load Factor = 0.75

Threshold = 12
```

When the number of stored entries reaches the resize threshold, HashMap may resize its table.

---

# 16. What is Initial Capacity?

The initial capacity determines the initial size of the hash table when it is allocated.

A commonly used default capacity is:

```text
16
```

Important interview distinction:

```text
Initial Capacity
        ≠
Current Number of Elements
```

Also, creating:

```java
new HashMap<>(16);
```

does not necessarily mean the internal table is immediately allocated at that moment. Modern HashMap uses lazy table initialization.

---

# 17. Why Does HashMap Resize?

As more elements are inserted, buckets become increasingly populated.

Too many collisions can reduce performance.

HashMap therefore expands its table and redistributes entries.

Conceptually:

```text
Small Table

↓

Resize

↓

Larger Table

↓

Redistribute Entries
```

This is called **resizing**.

The detailed process is covered in:

```text
04_Resize_Rehashing.md
```

---

# 18. Java 7 vs Java 8

This is a very common interview topic.

### Java 7

HashMap collision chains were represented using linked lists.

```text
Bucket

↓

Node → Node → Node
```

### Java 8+

When a bucket becomes sufficiently large and other conditions are satisfied, HashMap can transform the bucket into a **Red-Black Tree**.

```text
Bucket

↓

Tree
```

This improves lookup performance for heavily-collided buckets.

We will study this in:

```text
06_Treeification.md
```

---

# 19. Why Is HashMap O(1)?

The average complexity of:

```java
put()
get()
remove()
```

is:

```text
O(1)
```

because hashing allows HashMap to locate the expected bucket directly.

However, O(1) is an **average-case expectation**, not a guarantee for every possible situation.

Collisions can affect performance.

---

# 20. HashMap vs Hashtable

| Feature     | HashMap          | Hashtable        |
| ----------- | ---------------- | ---------------- |
| Thread Safe | No               | Yes              |
| Null Key    | One              | No               |
| Null Values | Yes              | No               |
| Performance | Generally faster | Generally slower |
| Legacy      | Modern           | Legacy           |
| Recommended | Yes              | Usually no       |

For modern concurrent applications, `ConcurrentHashMap` is generally preferred over `Hashtable`.

---

# 21. HashMap vs LinkedHashMap

| Feature            | HashMap                      | LinkedHashMap                                               |
| ------------------ | ---------------------------- | ----------------------------------------------------------- |
| Ordering           | No guarantee                 | Maintains insertion/access order depending on configuration |
| Performance        | Generally slightly faster    | Slight overhead                                             |
| Internal Structure | Hash table                   | Hash table + linked structure                               |
| Use Case           | Fast lookup without ordering | Fast lookup with predictable iteration order                |

---

# 22. HashMap vs TreeMap

| Feature            | HashMap      | TreeMap                             |
| ------------------ | ------------ | ----------------------------------- |
| Ordering           | No guarantee | Sorted                              |
| Internal Structure | Hash table   | Red-Black Tree                      |
| Average Lookup     | O(1)         | O(log n)                            |
| Null Key           | One          | Not supported with natural ordering |
| Use Case           | Fast lookup  | Sorted keys                         |

---

# 23. Common HashMap Methods

### put()

```java
map.put("Java", 100);
```

Adds or replaces a key-value mapping.

---

### get()

```java
map.get("Java");
```

Returns the value associated with the key.

---

### remove()

```java
map.remove("Java");
```

Removes the mapping.

---

### containsKey()

```java
map.containsKey("Java");
```

Checks whether a key exists.

---

### containsValue()

```java
map.containsValue(100);
```

Checks whether a value exists.

---

### putIfAbsent()

```java
map.putIfAbsent("Java", 100);
```

Adds the mapping only if the key does not already have a mapping.

---

### getOrDefault()

```java
map.getOrDefault("Java", 0);
```

Returns the mapped value or a default value.

---

# 24. How Can We Iterate Over HashMap?

### Using entrySet()

```java
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(
            entry.getKey() + " = " + entry.getValue()
    );
}
```

When both key and value are needed, `entrySet()` is generally the preferred approach.

---

### Using keySet()

```java
for (Integer key : map.keySet()) {
    System.out.println(key);
}
```

Useful when only keys are required.

---

### Using values()

```java
for (String value : map.values()) {
    System.out.println(value);
}
```

Useful when only values are required.

---

# 25. Frequently Asked Interview Questions

## Q1. What is HashMap?

HashMap is a Map implementation that stores key-value pairs using hashing and provides average O(1) lookup, insertion, and deletion.

---

## Q2. Does HashMap allow duplicate keys?

No.

A new value replaces the previous value for the same key.

---

## Q3. Does HashMap allow duplicate values?

Yes.

---

## Q4. Does HashMap allow null?

Yes.

One null key and multiple null values.

---

## Q5. Is HashMap thread-safe?

No.

---

## Q6. What is the default load factor?

```text
0.75
```

---

## Q7. What is the commonly used default initial capacity?

```text
16
```

---

## Q8. What is the average complexity of get()?

```text
O(1)
```

---

## Q9. What is the worst-case complexity?

It depends on the bucket structure and implementation details.

With Java 8+ treeification, heavily-collided buckets can use Red-Black Trees, giving O(log n) lookup within such a tree after treeification. Before treeification or in other collision scenarios, linked traversal can be O(n).

---

# 26. Cross Questions

## Why is HashMap faster than TreeMap?

HashMap uses hashing and provides average O(1) lookup.

TreeMap uses a Red-Black Tree and provides O(log n) lookup.

HashMap should be preferred when sorted ordering is not required.

---

## Why doesn't HashMap maintain insertion order?

HashMap is designed around hashing and bucket placement rather than maintaining an ordering structure.

If predictable insertion order is required, use LinkedHashMap.

---

## Why is the key required to be unique?

Because a Map represents a mapping from a key to a value.

When the same key is inserted again, its associated value is replaced.

---

# 27. Tricky Interview Questions

## Can two different keys have the same hashCode()?

Yes.

That is called a hash collision.

```text
Key A → hash 100

Key B → hash 100
```

HashMap must handle this correctly.

---

## Can two equal objects have different hashCodes?

They should not.

If:

```java
a.equals(b) == true
```

then:

```java
a.hashCode() == b.hashCode()
```

must be true.

---

## Can two unequal objects have the same hashCode?

Yes.

This is completely legal.

It produces a collision.

---

## Does HashMap compare keys using only hashCode()?

No.

`hashCode()` helps locate the bucket.

`equals()` is then used to identify the matching key.

---

## Is HashMap always O(1)?

No.

O(1) is the average expected complexity.

Actual performance depends on hashing, collisions, and the internal bucket structure.

---

# 28. Best Practices

### Prefer the interface

```java
Map<String, Integer> map =
        new HashMap<>();
```

instead of:

```java
HashMap<String, Integer> map =
        new HashMap<>();
```

when the implementation itself is not required by the API.

---

### Use stable keys

Keys should ideally be immutable or should not change in a way that affects `equals()` or `hashCode()` while they are stored in the map.

---

### Override equals() and hashCode() together

For custom key classes, follow the contract correctly.

---

### Choose the correct Map

```text
No ordering required
        ↓
HashMap

Insertion/access ordering required
        ↓
LinkedHashMap

Sorted keys required
        ↓
TreeMap

Concurrent access required
        ↓
ConcurrentHashMap
```

---

# 29. Common Mistakes

* Saying HashMap is thread-safe.
* Saying HashMap maintains insertion order.
* Saying duplicate values are not allowed.
* Forgetting the `equals()` / `hashCode()` contract.
* Assuming O(1) is guaranteed in every situation.
* Confusing HashMap with Hashtable.
* Assuming `new HashMap<>(16)` immediately creates a 16-bucket table.
* Saying HashMap uses a Red-Black Tree for every bucket.

---

# 30. Quick Revision

```text
HashMap

↓

Map Implementation

↓

Key + Value

↓

Unique Keys

↓

Hashing

↓

Bucket Array

↓

Collision Handling

↓

Linked List / Tree Structure

↓

Average O(1)

↓

Not Thread Safe

↓

1 Null Key

↓

Multiple Null Values

↓

Default Load Factor = 0.75

↓

Common Default Capacity = 16
```

---

# 31. Interviewer's Perspective

HashMap is one of the most important Java Collections interview topics.

An interviewer may start with:

> "What is HashMap?"

Then progressively ask:

```text
What is hashing?
        ↓
How is bucket calculated?
        ↓
What is hashCode()?
        ↓
What is equals()?
        ↓
How are collisions handled?
        ↓
What happens when two keys collide?
        ↓
What is load factor?
        ↓
When does HashMap resize?
        ↓
Why capacity is a power of 2?
        ↓
Why is load factor 0.75?
        ↓
What changed in Java 8?
        ↓
What is treeification?
        ↓
Why Red-Black Tree?
        ↓
What happens internally during put()?
        ↓
What happens internally during get()?
```

You should be able to answer each of these before considering HashMap fully understood.

---

# 32. One-Line Summary

**HashMap is a hash-table-based Map implementation that stores unique keys and provides average O(1) key-based operations through hashing, with collision handling, resizing, and treeification in modern Java.**


The **next file, `01_Internal_Working.md`, is where the real HashMap interview preparation begins**. We will trace `put()` and `get()` step-by-step from key → `hashCode()` → hash spreading → bucket index → node → `equals()`.
