# HashMap — Java 7 vs Java 8

---

# 1. Why Compare Java 7 and Java 8 HashMap?

This is a very common interview topic because Java 8 made important improvements to HashMap's collision handling and resizing behavior.

The biggest change is:

```text
Java 7
    ↓
Linked List

Java 8+
    ↓
Linked List
    +
Red-Black Tree
```

---

# 2. Main Difference

| Feature | Java 7 | Java 8+ |
|---|---|---|
| Collision structure | Linked List | Linked List + Red-Black Tree |
| Heavy collision lookup | O(n) | O(log n) after treeification |
| Treeification | ❌ | ✅ |
| Tree bin | ❌ | ✅ |
| Resize transfer | More prone to ordering issues | Improved transfer logic |
| Concurrent resize issue | Famous infinite-loop problem | Fixed by different transfer approach |

---

# 3. Java 7 Internal Structure

In Java 7, HashMap buckets were essentially linked lists.

```text
Bucket 5

Node A
  ↓
Node B
  ↓
Node C
  ↓
Node D
```

If many keys collided:

```text
Node A → Node B → Node C → Node D → Node E
```

Searching could become:

```text
O(n)
```

---

# 4. Java 8 Internal Structure

Java 8 introduced treeification.

A bucket can start as:

```text
Node A → Node B → Node C
```

and under the required conditions can become:

```text
        Node B
       /      \
   Node A    Node C
```

The tree is a:

```text
Red-Black Tree
```

---

# 5. Why Was Treeification Introduced?

Consider a heavily collided bucket.

### Java 7

```text
A → B → C → D → E → F → G → H
```

Search:

```text
O(n)
```

### Java 8

```text
       D
      / \
     B   F
    / \ / \
   A  C E  G
```

Search in the tree:

```text
O(log n)
```

This improves HashMap's behavior under heavy collisions.

---

# 6. Java 8 Treeification Thresholds

Important constants:

```text
TREEIFY_THRESHOLD = 8

UNTREEIFY_THRESHOLD = 6

MIN_TREEIFY_CAPACITY = 64
```

These are implementation details of Java's HashMap.

---

# 7. What Happens When a Bucket Becomes Large?

Suppose:

```text
Bucket contains 8 nodes
```

HashMap considers treeification.

But it also checks table capacity.

```text
Bucket crowded
       ↓
Capacity >= 64?
    /        \
  No          Yes
  ↓            ↓
Resize       Treeify
```

---

# 8. Why Resize Instead of Treeify?

Suppose:

```text
Capacity = 16
```

and one bucket has many entries.

The problem may simply be that the table is too small.

Instead of immediately creating a tree:

```text
Resize
```

can distribute the entries across more buckets.

Therefore:

```text
Small table
    +
Crowded bucket
    ↓
Prefer resize
```

---

# 9. Java 7 Resize Behavior

During resizing, Java 7 transferred entries from the old table into the new table.

The transfer process used linked-list manipulation.

This could reverse the ordering of nodes in a bucket.

Conceptually:

```text
Before:

A → B → C

After transfer:

C → B → A
```

This became particularly important when multiple threads modified a HashMap concurrently.

---

# 10. Java 7 Concurrent Resize Problem ⭐⭐⭐⭐⭐

HashMap is not thread-safe.

If multiple threads modify a HashMap concurrently without external synchronization, its internal structure can become corrupted.

One famous Java 7 problem involved concurrent resizing.

The linked-list transfer mechanism could create a cycle.

Conceptually:

```text
A → B → C
↑       ↓
└───────┘
```

Now traversal could continue forever.

This could result in:

```text
Infinite loop
```

and potentially very high CPU usage.

---

# 11. Important Clarification

Do not say:

> "HashMap in Java 7 always causes an infinite loop."

That is incorrect.

The issue was associated with:

```text
Concurrent modification
+
Resize
+
Unsynchronized HashMap
```

It was not normal single-threaded HashMap behavior.

---

# 12. Why Does This Problem Exist?

HashMap was not designed for concurrent modification.

Consider:

```text
Thread 1
   ↓
Resize

Thread 2
   ↓
Resize / modify
```

Both threads can manipulate the same internal bucket structures.

Without synchronization, their operations can interfere with each other.

---

# 13. Java 8 Improvement

Java 8 changed the resize transfer implementation.

Instead of the old head-insertion approach that could reverse chains, Java 8 uses a different transfer strategy that preserves the relative ordering of nodes within the low and high partitions during resize.

This avoids the famous Java 7 resize-cycle problem.

---

# 14. Important Interview Point

Java 8's fix does **not** mean:

```text
HashMap became thread-safe
```

It did not.

This is extremely important.

```text
Java 7 HashMap
    ↓
Not thread-safe

Java 8 HashMap
    ↓
Still not thread-safe
```

The implementation was improved, but concurrent unsynchronized access is still unsafe.

---

# 15. What Should We Use for Concurrent Access?

If multiple threads need to modify a map concurrently, use:

```java
ConcurrentHashMap
```

rather than relying on:

```java
HashMap
```

without synchronization.

---

# 16. Java 7 vs Java 8 Collision Handling

### Java 7

```text
Collision
   ↓
Linked List
   ↓
Long chain
   ↓
O(n)
```

### Java 8

```text
Collision
   ↓
Linked Nodes
   ↓
Heavy collision
   ↓
Treeification
   ↓
Red-Black Tree
   ↓
O(log n)
```

---

# 17. Java 7 vs Java 8 Resize

### Java 7

```text
Old table
   ↓
Create new table
   ↓
Transfer nodes
   ↓
Linked-list insertion
```

### Java 8

```text
Old table
   ↓
Create new table
   ↓
Split nodes into:
   ↓
low bucket
high bucket
   ↓
Preserve relative order
```

---

# 18. Low and High Buckets in Java 8

Suppose:

```text
old capacity = 16
new capacity = 32
```

An entry can go to:

```text
old index
```

or:

```text
old index + 16
```

Therefore a bucket is conceptually split into:

```text
LOW list
HIGH list
```

Example:

```text
Old bucket:

A → B → C → D
```

After resize:

```text
LOW:
A → C

HIGH:
B → D
```

The exact entries depend on the relevant hash bit.

---

# 19. Why Is This Efficient?

Java 8 doesn't need to calculate a completely new bucket index using expensive operations for every node.

It can inspect the relevant hash bit.

Conceptually:

```text
hash & oldCapacity
```

determines whether the entry stays in the low bucket or moves to the high bucket.

---

# 20. Java 8 Resize Optimization

Suppose:

```text
old capacity = 16
new capacity = 32
```

For an entry at:

```text
oldIndex = 5
```

it can become:

```text
5
```

or:

```text
5 + 16 = 21
```

Therefore:

```text
newIndex =
    oldIndex
    OR
    oldIndex + oldCapacity
```

depending on the relevant hash bit.

---

# 21. Why Does Java 8 Use Red-Black Trees?

A Red-Black Tree is self-balancing.

Therefore the height remains approximately logarithmic.

This provides better worst-case behavior for a heavily collided bucket.

Remember:

```text
Linked List → O(n)

Red-Black Tree → O(log n)
```

---

# 22. Does Java 8 Make HashMap O(log n)?

No.

This is a trick question.

Normal HashMap operations are still generally:

```text
Average → O(1)
```

The tree is only relevant for heavily collided buckets.

---

# 23. Java 7 vs Java 8 — Quick Table

| Topic | Java 7 | Java 8 |
|---|---|---|
| Bucket | Linked list | Linked list / tree |
| Heavy collisions | O(n) | O(log n) after treeification |
| Red-Black Tree | ❌ | ✅ |
| Treeification | ❌ | ✅ |
| Minimum treeify capacity | N/A | 64 |
| Treeify threshold | N/A | 8 |
| Untreeify threshold | N/A | 6 |
| Resize transfer | Head insertion | Improved split/preservation |
| Famous resize cycle issue | Possible under unsafe concurrent use | Avoided by changed transfer logic |
| Thread-safe? | ❌ | ❌ |

---

# 24. Tricky Interview Questions

## Q1. Did Java 8 make HashMap thread-safe?

No.

HashMap remains non-thread-safe.

---

## Q2. Why did Java 8 introduce Red-Black Trees?

To improve performance when many keys collide into the same bucket.

---

## Q3. What was the famous Java 7 HashMap problem?

Concurrent unsynchronized resizing could corrupt linked bucket chains and potentially create a cycle, causing an infinite traversal.

---

## Q4. Can the Java 7 infinite-loop issue happen in normal single-threaded code?

The famous cycle problem was associated with concurrent unsynchronized modification during resize.

It was not normal behavior of a correctly used single-threaded HashMap.

---

## Q5. What changed in Java 8 resize?

The transfer logic was redesigned so that entries are split into low/high partitions while preserving their relative order, avoiding the old head-insertion cycle problem.

---

## Q6. What is the biggest HashMap change from Java 7 to Java 8?

The most interview-relevant change is:

```text
Linked List
     ↓
Linked List + Red-Black Tree
```

for heavily collided buckets.

---

## Q7. What should we use instead of HashMap for concurrent updates?

```java
ConcurrentHashMap
```

---

# 25. Interview Scenario ⭐⭐⭐⭐⭐

### Interviewer:

> Why did Java 8 change HashMap's collision handling?

### Answer:

Java 7 used linked lists for collision chains. If many keys collided in the same bucket, lookup could degrade toward O(n).

Java 8 introduced treeification, allowing a heavily populated bucket to become a Red-Black Tree, improving lookup within that bucket to approximately O(log n).

---

# 26. Interview Scenario

### Interviewer:

> Was Java 8 HashMap made thread-safe?

### Answer:

No.

Java 8 improved the internal resize and collision-handling implementation, but HashMap itself is still not thread-safe.

For concurrent access, `ConcurrentHashMap` should be considered.

---

# 27. Interview Scenario

### Interviewer:

> Explain the Java 7 HashMap infinite-loop issue.

### Answer:

HashMap is not thread-safe. During concurrent resizing in Java 7, multiple threads could manipulate linked-list bucket structures simultaneously. The transfer logic could create a cyclic linked list, causing traversal to loop indefinitely.

Java 8 changed the transfer mechanism to avoid this specific cycle problem.

---

# 28. Important Interview Trap

### Question:

> If Java 8 fixed the HashMap infinite-loop issue, can I safely use HashMap from multiple threads?

### Answer:

**No.**

The correct conclusion is:

```text
Java 8 fixed a specific resize corruption issue
        ≠
HashMap became thread-safe
```

For concurrent modifications:

```java
ConcurrentHashMap
```

is the appropriate collection.

---

# 29. Quick Revision

```text
Java 7
   |
   +-- Linked List
   |
   +-- Heavy collision → O(n)
   |
   +-- Concurrent resize could create cycle
   |
   +-- Not thread-safe


Java 8
   |
   +-- Linked List
   |
   +-- Red-Black Tree
   |
   +-- Heavy collision → O(log n)
   |
   +-- Improved resize transfer
   |
   +-- Still not thread-safe
```

---

# 30. Golden Interview Answer

> **The major HashMap improvement in Java 8 was the introduction of treeification. Java 7 used linked lists for collision chains, which could degrade lookup to O(n). Java 8 can convert heavily populated buckets into Red-Black Trees, improving lookup within those buckets to O(log n). Java 8 also redesigned the resize transfer logic, avoiding the famous cyclic-list problem that could occur during concurrent unsynchronized resizing in Java 7. However, HashMap remains non-thread-safe.**

---

# 31. One-Line Summary

**Java 8 improved HashMap by introducing Red-Black Tree buckets for heavy collisions and safer resize-transfer logic, but HashMap itself remains non-thread-safe.**
