# HashMap Resize and Rehashing in Java

---

# 1. What is Resizing?

Resizing is the process of increasing the internal hash table capacity when the number of entries reaches the HashMap's resize threshold.

For example:

```text
Capacity = 16
Load Factor = 0.75

Threshold = 16 × 0.75
          = 12
```

When the map reaches the threshold, HashMap expands the table.

Conceptually:

```text
Capacity 16
     ↓
Resize
     ↓
Capacity 32
```

---

# 2. Why Does HashMap Resize?

HashMap resizes to maintain efficient distribution of entries across buckets.

As more entries are inserted:

```text
More entries
     ↓
More collisions possible
     ↓
Longer bucket chains
     ↓
Performance can degrade
```

Increasing the table capacity provides more buckets and generally reduces collisions.

---

# 3. What is Capacity?

Capacity is the number of buckets in the internal hash table.

For example:

```text
Capacity = 16
```

means the table has:

```text
16 buckets
```

Conceptually:

```text
0  1  2  3  4  5  ...  15
```

---

# 4. What is Load Factor?

Load factor determines how full the hash table is allowed to become before resizing.

The commonly used default is:

```text
0.75
```

Formula:

```text
Threshold = Capacity × Load Factor
```

---

# 5. Example

Suppose:

```text
Capacity = 16
Load Factor = 0.75
```

Then:

```text
Threshold = 16 × 0.75
          = 12
```

So the resize threshold is:

```text
12
```

---

# 6. What is Threshold?

Threshold is the number of entries at which HashMap decides that resizing is required.

Conceptually:

```text
size >= threshold
       ↓
Resize
```

For:

```text
capacity = 16
load factor = 0.75
```

we get:

```text
threshold = 12
```

---

# 7. Capacity vs Size vs Threshold

This is a common interview confusion.

### Size

Number of mappings currently stored.

```text
size = 10
```

means:

```text
10 key-value pairs
```

### Capacity

Number of buckets.

```text
capacity = 16
```

### Threshold

Point at which resizing is triggered.

```text
threshold = 12
```

So:

```text
Size      → Number of entries

Capacity  → Number of buckets

Threshold → Resize limit
```

---

# 8. What Happens When HashMap Resizes?

Suppose:

```text
Old Capacity = 16
```

HashMap grows the table:

```text
New Capacity = 32
```

The entries need to be redistributed according to the new table capacity.

Conceptually:

```text
Old table
   ↓
Create larger table
   ↓
Redistribute entries
   ↓
New table
```

---

# 9. Does HashMap Recalculate hashCode()?

This is a tricky interview question.

HashMap does not need to call the key's `hashCode()` method again simply because the table resized.

The node stores the processed hash.

During resizing, HashMap can use that stored hash to determine the new bucket position.

This is an important implementation optimization.

---

# 10. Why Does Bucket Position Change After Resize?

Suppose:

```text
Old capacity = 16
```

Bucket index:

```java
(hash & (16 - 1))
```

After resizing:

```text
New capacity = 32
```

Bucket index becomes:

```java
(hash & (32 - 1))
```

Because the mask changes, an entry can remain in the same bucket or move to another bucket.

---

# 11. Java 8 Resize Optimization ⭐⭐⭐⭐⭐

Java 8 uses an important optimization during resizing.

When capacity doubles:

```text
oldCap = 16
newCap = 32
```

an entry can only remain at:

```text
old index
```

or move to:

```text
old index + old capacity
```

So:

```text
newIndex = oldIndex
```

or:

```text
newIndex = oldIndex + oldCapacity
```

This is a very important Java 8 interview point.

---

# 12. Why Only Two Possible Positions?

Because the new capacity is double the old capacity.

Suppose:

```text
old capacity = 16
new capacity = 32
```

The new bucket calculation adds one additional significant bit to the mask.

The relevant bit determines whether the node:

```text
stays
```

or:

```text
moves by +16
```

---

# 13. Example

Suppose an entry was previously in:

```text
Bucket 5
```

Old capacity:

```text
16
```

After resize to:

```text
32
```

the entry can go to:

```text
Bucket 5
```

or:

```text
Bucket 21
```

because:

```text
5 + 16 = 21
```

---

# 14. High-Level Resize Flow

```text
HashMap reaches threshold
          ↓
Double capacity
          ↓
Create new table
          ↓
Process old buckets
          ↓
Redistribute entries
          ↓
Old table replaced
          ↓
Continue operations
```

---

# 15. Does HashMap Always Double Capacity?

For normal resizing of the table, HashMap generally doubles the capacity.

Example:

```text
16 → 32
32 → 64
64 → 128
128 → 256
```

This power-of-two growth is a key part of the implementation.

There are also implementation limits for very large capacities.

---

# 16. Why Power of Two?

HashMap uses:

```java
(n - 1) & hash
```

to calculate the bucket index.

For this to distribute the lower bits efficiently, table capacities are maintained as powers of two.

Examples:

```text
16
32
64
128
256
```

---

# 17. Why Not Use Prime Numbers?

Some hash table implementations use prime capacities.

HashMap takes a different approach.

It uses:

```text
Power-of-two capacity
+
Bitwise AND
+
Hash spreading
```

This provides efficient bucket index calculation.

---

# 18. Why Is Load Factor 0.75?

This is a balance between:

```text
Memory Usage
```

and:

```text
Collision Rate
```

### Lower load factor

```text
More buckets
↓
Fewer collisions
↓
More memory
```

### Higher load factor

```text
Fewer buckets
↓
More collisions
↓
Less memory
```

`0.75` is a practical compromise chosen by the JDK implementation.

---

# 19. What If Load Factor Is Too Low?

Suppose:

```text
Load Factor = 0.25
```

The map resizes more aggressively.

Advantages:

```text
Fewer collisions
```

Disadvantage:

```text
More memory usage
```

---

# 20. What If Load Factor Is Too High?

Suppose:

```text
Load Factor = 0.90
```

The table can become more densely populated before resizing.

Advantages:

```text
Less memory
```

Disadvantages:

```text
Potentially more collisions
```

---

# 21. Can We Specify Initial Capacity?

Yes.

Example:

```java
Map<String, Integer> map =
        new HashMap<>(100);
```

This allows us to provide an initial capacity hint.

However, the actual internal table capacity follows HashMap's implementation rules and is not necessarily exactly 100 buckets.

---

# 22. Why Specify Initial Capacity?

Suppose we know that a map will contain many entries.

Example:

```text
Expected entries = 1000
```

Starting with a suitable capacity can reduce repeated resizing as the map grows.

This can improve performance.

---

# 23. Important Interview Question

## Does initial capacity mean exactly that many buckets?

Not necessarily.

For example:

```java
new HashMap<>(100);
```

does not mean:

```text
Exactly 100 buckets
```

The implementation rounds the relevant capacity to an appropriate power-of-two size.

Also, table allocation itself is lazy.

---

# 24. Lazy Initialization + Resize

Suppose:

```java
HashMap<Integer, String> map =
        new HashMap<>(16);
```

The internal table may not be allocated immediately.

Then:

```java
map.put(1, "Java");
```

causes table initialization.

Later, when the threshold is reached:

```text
Resize
```

occurs.

---

# 25. What Happens to Existing Entries During Resize?

Existing entries are redistributed into the new table.

Example:

```text
Before:

Bucket 5
  ↓
A → B

Bucket 10
  ↓
C
```

After resizing:

```text
Larger table

Bucket 5
  ↓
A

Bucket 21
  ↓
B

Bucket 10
  ↓
C
```

The exact distribution depends on the hashes.

---

# 26. Does Resize Change the Keys?

No.

The key objects remain the same.

What changes is their position in the internal bucket array.

---

# 27. Does Resize Change the Values?

No.

The values remain associated with their keys.

Only the internal table organization changes.

---

# 28. What is Rehashing?

The term "rehashing" is commonly used to describe redistributing entries after the hash table grows.

In HashMap discussions, people often say:

```text
Resize + Rehash
```

However, in Java 8's implementation, HashMap does not simply call every key's `hashCode()` again.

It stores the processed hash in each node and uses that during resize.

So, in interview discussions, be precise:

> HashMap resizes the table and redistributes existing nodes according to the new capacity.

---

# 29. Resize Example

Suppose:

```text
Capacity = 16
Load Factor = 0.75
Threshold = 12
```

Insert entries:

```text
1
2
3
...
11
12
```

The map has reached its threshold.

A subsequent insertion causes the table to resize.

Conceptually:

```text
16 buckets
     ↓
32 buckets
```

Then entries are redistributed.

---

# 30. Important Detail About the Resize Trigger

Do not memorize the process as simply:

```text
size == threshold → resize immediately
```

The actual insertion logic checks the map size and table state as part of `put()` processing.

For interview purposes, the useful model is:

```text
Insertion causes size to reach the resize threshold
        ↓
HashMap grows the table
```

---

# 31. What Happens to Treeified Buckets During Resize?

This is an advanced question.

A treeified bucket is also redistributed during resizing.

Depending on how the entries split between the old and new positions, each resulting bucket may remain treeified or may be converted back to a linked structure when its population becomes small enough.

---

# 32. Resize and Treeification

Remember:

```text
Resize
  +
Redistribution
  +
Bucket population changes
```

Therefore resizing can affect whether a bucket remains a tree or becomes a linked structure.

---

# 33. Why Is Resizing Expensive?

Resizing requires processing existing entries.

If there are:

```text
n entries
```

the resize operation involves work proportional to the number of entries being redistributed.

Therefore an individual resize can be:

```text
O(n)
```

---

# 34. Then Why Is put() Still O(1) Average?

Because resizing does not happen on every insertion.

Most insertions are constant-time.

Occasional expensive resize operations are spread across many insertions.

Therefore insertion is described as:

```text
Average / Amortized → O(1)
```

---

# 35. What Does Amortized O(1) Mean?

It means:

```text
Most operations → O(1)

Occasionally → O(n)

Overall average cost → O(1)
```

This is an important concept for dynamic data structures.

---

# 36. Capacity Growth Example

```text
Initial

16
 ↓
32
 ↓
64
 ↓
128
 ↓
256
 ↓
512
```

The table generally doubles during growth.

---

# 37. Capacity vs Load Factor Example

Suppose:

```text
Capacity = 32
Load Factor = 0.75
```

Then:

```text
Threshold = 32 × 0.75
          = 24
```

When the map grows to the relevant threshold during insertion:

```text
32 → 64
```

and entries are redistributed.

---

# 38. Tricky Interview Questions

## Q1. What is the default load factor?

```text
0.75
```

---

## Q2. What is the default initial capacity?

The commonly documented default initial capacity is:

```text
16
```

But remember that the internal table is lazily allocated.

---

## Q3. What happens when HashMap reaches its threshold?

The table is resized and existing entries are redistributed.

---

## Q4. Does resizing call hashCode() again for every key?

Not necessarily.

Java 8 HashMap stores the processed hash in each node and uses it during resize.

---

## Q5. Why does capacity double?

Doubling works efficiently with the power-of-two table design and allows optimized redistribution.

---

## Q6. Why is HashMap capacity a power of two?

It enables efficient bucket calculation using:

```java
(n - 1) & hash
```

---

## Q7. Is resizing O(1)?

No.

A resize processes existing entries and can take:

```text
O(n)
```

---

## Q8. Why is put() still considered O(1) average?

Because resizing occurs only occasionally, so its cost is amortized over many insertions.

---

# 39. Advanced Interview Question ⭐⭐⭐⭐⭐

## Why can an entry only stay at oldIndex or move to oldIndex + oldCapacity after doubling?

Suppose:

```text
oldCapacity = 16
newCapacity = 32
```

The new mask has one additional bit.

That bit determines whether the entry:

```text
stays at oldIndex
```

or:

```text
moves to oldIndex + 16
```

Therefore Java 8 can redistribute entries efficiently without recalculating the entire bucket index from scratch.

---

# 40. Complete Resize Flow

```text
             put(key, value)
                    ↓
              Insert Entry
                    ↓
              Size reaches
               threshold
                    ↓
               Resize
                    ↓
          Double table capacity
                    ↓
          Create larger table
                    ↓
        Process existing buckets
                    ↓
       Redistribute each entry
                    ↓
       oldIndex or oldIndex + oldCap
                    ↓
             New table
```

---

# 41. Quick Revision

```text
Capacity
    ↓
Number of buckets

Load Factor
    ↓
How full the table can become

Threshold
    ↓
Capacity × Load Factor

Resize
    ↓
Increase table capacity

Redistribution
    ↓
Move entries according to new capacity

Resize Cost
    ↓
O(n)

put() Average
    ↓
O(1) amortized
```

---

# 42. Golden Interview Answer

If the interviewer asks:

> "What happens when HashMap reaches its threshold?"

Answer:

> HashMap increases the table capacity, normally doubling it, and redistributes the existing entries according to the new capacity. In Java 8, the stored hash in each node allows the resize process to efficiently determine whether an entry remains at its old bucket or moves by the old capacity. Although resizing itself is O(n), it happens occasionally, so `put()` remains O(1) amortized on average.

---

# 43. One-Line Summary

**HashMap resizes when its entry count reaches the threshold determined by capacity and load factor; the table grows, usually doubles, and existing entries are efficiently redistributed across the new buckets.**

---
