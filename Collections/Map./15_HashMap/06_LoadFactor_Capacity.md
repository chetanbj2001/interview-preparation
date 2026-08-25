# HashMap — Load Factor, Capacity and Threshold

---

# 1. What is Capacity?

Capacity is the number of buckets available in the internal HashMap table.

For example:

```text
Capacity = 16
```

means the internal table can have:

```text
16 buckets
```

Conceptually:

```text
0   1   2   3   4   5   ...   15
↓   ↓   ↓   ↓   ↓   ↓         ↓
B   B   B   B   B   B   ...   B
```

---

# 2. What is Load Factor?

Load factor determines how full the HashMap is allowed to become before resizing.

The default load factor is:

```text
0.75
```

It is a balance between:

```text
Memory usage
        +
Collision rate
```

---

# 3. What is Threshold?

The threshold determines when HashMap should resize.

Conceptually:

```text
Threshold = Capacity × Load Factor
```

Example:

```text
Capacity = 16
Load Factor = 0.75

Threshold = 16 × 0.75
          = 12
```

So:

```text
Capacity  = 16
Load Factor = 0.75
Threshold = 12
```

---

# 4. Capacity vs Size vs Threshold

This is a very common interview question.

| Term | Meaning |
|---|---|
| Size | Number of key-value mappings |
| Capacity | Number of buckets |
| Load Factor | How full the table is allowed to become |
| Threshold | Point at which resizing occurs |

Example:

```text
Capacity = 16
Size = 10
Load Factor = 0.75
Threshold = 12
```

---

# 5. Why Default Load Factor is 0.75?

A lower load factor means:

```text
More buckets
↓
Fewer collisions
↓
More memory
```

A higher load factor means:

```text
Fewer buckets
↓
More collisions
↓
Less memory
```

Therefore:

```text
0.75
```

is a practical compromise between memory and performance.

---

# 6. What Happens When Load Factor is Low?

Suppose:

```text
Load Factor = 0.50
```

For:

```text
Capacity = 16
```

threshold becomes:

```text
16 × 0.50 = 8
```

The map resizes earlier.

Advantages:

```text
Fewer collisions
```

Disadvantages:

```text
More memory usage
```

---

# 7. What Happens When Load Factor is High?

Suppose:

```text
Load Factor = 0.90
```

For:

```text
Capacity = 16
```

threshold becomes approximately:

```text
16 × 0.90 = 14.4
```

The table can hold more entries before resizing.

Advantages:

```text
Less memory overhead
```

Potential disadvantage:

```text
More collisions
```

---

# 8. What is the Default Initial Capacity?

The commonly documented default initial capacity is:

```text
16
```

But there is an important detail:

> HashMap uses lazy initialization.

Creating:

```java
HashMap<Integer, String> map =
        new HashMap<>();
```

does not necessarily allocate the internal table immediately.

The table is initialized when needed, typically during the first insertion.

---

# 9. What Happens During First put()?

```java
HashMap<Integer, String> map =
        new HashMap<>();
```

Then:

```java
map.put(1, "Java");
```

Conceptually:

```text
HashMap created
     ↓
Table not yet allocated
     ↓
put()
     ↓
Initialize table
     ↓
Insert entry
```

---

# 10. What Does new HashMap<>(16) Mean?

Consider:

```java
HashMap<Integer, String> map =
        new HashMap<>(16);
```

This means:

```text
16 is provided as an initial capacity hint
```

It does not mean that the constructor necessarily allocates exactly 16 buckets immediately.

The internal table is still lazily initialized.

---

# 11. Tricky Question ⭐⭐⭐⭐⭐

## Does `new HashMap<>(100)` create 100 buckets?

No.

The requested capacity is processed internally and the actual table capacity follows HashMap's power-of-two sizing rules.

Also, table allocation is lazy.

So don't simply say:

```text
new HashMap<>(100)
→ exactly 100 buckets
```

That is incorrect.

---

# 12. Why Power-of-Two Capacity?

HashMap uses bucket calculation based on:

```java
(n - 1) & hash
```

where:

```text
n = table capacity
```

For this to work efficiently, HashMap maintains capacities as powers of two.

Examples:

```text
16
32
64
128
256
512
```

---

# 13. Capacity Rounding

Suppose we request:

```java
new HashMap<>(10);
```

HashMap does not need to use exactly:

```text
10
```

as the internal table length.

The capacity is rounded to an appropriate power of two.

Conceptually:

```text
Requested capacity
       ↓
Find suitable power of 2
       ↓
Actual table capacity
```

The exact internal initialization behavior depends on the JDK implementation.

---

# 14. Example

Suppose the requested capacity is:

```text
10
```

A suitable power-of-two capacity is:

```text
16
```

So conceptually:

```text
Requested → 10

Internal capacity → 16
```

---

# 15. Another Example

```java
new HashMap<>(100);
```

The next suitable power of two is:

```text
128
```

So the eventual internal table capacity can be based on:

```text
128
```

rather than exactly:

```text
100
```

---

# 16. Why Not Use Exactly 100?

Because HashMap's bucket calculation is optimized around powers of two:

```java
(hash & (n - 1))
```

Using powers of two makes the bucket calculation efficient.

---

# 17. Threshold Example

Suppose the effective capacity is:

```text
128
```

and load factor is:

```text
0.75
```

Then:

```text
Threshold = 128 × 0.75
          = 96
```

Conceptually:

```text
Capacity = 128
Threshold = 96
```

---

# 18. Common Capacity Values

With default load factor:

| Capacity | Threshold |
|---:|---:|
| 16 | 12 |
| 32 | 24 |
| 64 | 48 |
| 128 | 96 |
| 256 | 192 |
| 512 | 384 |
| 1024 | 768 |

Formula:

```text
Threshold = Capacity × 0.75
```

---

# 19. What Happens After Resize?

Suppose:

```text
Capacity = 16
Threshold = 12
```

HashMap grows:

```text
16 → 32
```

New threshold becomes:

```text
32 × 0.75
= 24
```

So:

```text
Before:

Capacity = 16
Threshold = 12


After:

Capacity = 32
Threshold = 24
```

---

# 20. Capacity Growth Pattern

Typically:

```text
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

The table normally doubles during resizing.

---

# 21. Important Difference: Initial Capacity vs Threshold

Suppose:

```java
new HashMap<>(100);
```

Do not say:

```text
Threshold = 100
```

Initial capacity and resize threshold are different concepts.

The threshold is related to the effective table capacity and load factor.

---

# 22. Example

Suppose the effective capacity becomes:

```text
128
```

and load factor:

```text
0.75
```

Then:

```text
Threshold = 96
```

So:

```text
Capacity = 128

Threshold = 96
```

They are not the same thing.

---

# 23. What Happens If We Change Load Factor?

Example:

```java
HashMap<Integer, String> map =
        new HashMap<>(16, 0.5f);
```

Now:

```text
Capacity = approximately 16 initially
Load Factor = 0.5
```

Threshold is based on:

```text
16 × 0.5
= 8
```

So resizing occurs earlier than with the default:

```text
0.75
```

---

# 24. Constructor Syntax

HashMap provides constructors such as:

```java
HashMap()
```

```java
HashMap(int initialCapacity)
```

```java
HashMap(int initialCapacity, float loadFactor)
```

Example:

```java
Map<Integer, String> map =
        new HashMap<>(32, 0.75f);
```

---

# 25. When Should We Specify Initial Capacity?

If we have a reasonable estimate of the number of entries, specifying an appropriate initial capacity can reduce repeated resizing.

Example:

```text
Expected entries ≈ 1000
```

Instead of starting very small and repeatedly growing:

```text
16
 ↓
32
 ↓
64
 ↓
128
 ↓
...
```

we can provide a suitable initial capacity.

---

# 26. Important Practical Formula

If you expect approximately `n` entries and want to avoid resizing under a load factor `lf`, a useful capacity estimate is:

```text
required capacity ≥ n / lf
```

For example:

```text
Expected entries = 1000
Load factor = 0.75
```

Then:

```text
1000 / 0.75
≈ 1333
```

Choose the next suitable power of two:

```text
2048
```

This is a practical sizing idea.

---

# 27. Why Is This Useful?

Suppose an application knows:

```text
We will store around 10,000 records.
```

Choosing an appropriate initial capacity can reduce the number of resize operations.

This can improve performance during bulk insertion.

---

# 28. Tricky Interview Question

## Is load factor a percentage of buckets that are occupied?

Not exactly.

It is a threshold factor used to determine when the table should resize.

The simplified interview formula is:

```text
Threshold = Capacity × Load Factor
```

It should not be interpreted as a guarantee that exactly that percentage of buckets will be occupied.

---

# 29. Tricky Interview Question

## Does resizing happen when every bucket is full?

No.

Resizing is based on the map's size reaching the resize threshold, not on every bucket being occupied.

---

# 30. Tricky Interview Question

## Can a HashMap have more entries than its capacity?

Yes.

This is very important.

Suppose:

```text
Capacity = 16
```

The map can have more than:

```text
16 entries
```

because multiple entries can exist in the same bucket due to collisions.

The load factor controls when resizing happens.

---

# 31. Example

```text
Capacity = 16
```

Suppose:

```text
Size = 10
```

There could be:

```text
Bucket 1 → 4 entries
Bucket 2 → 1 entry
Bucket 3 → 2 entries
...
```

Multiple entries can share buckets.

Therefore:

```text
Capacity ≠ Maximum number of entries
```

---

# 32. Capacity Does Not Mean Maximum Size

This is a common interview trap.

Wrong:

> "A HashMap with capacity 16 can contain only 16 entries."

Correct:

> Capacity represents the number of buckets, not the maximum number of key-value mappings.

---

# 33. Load Factor Trade-Off

Think of it like this:

```text
Low Load Factor
      ↓
More buckets
      ↓
Less collision
      ↓
More memory


High Load Factor
      ↓
Fewer buckets
      ↓
More collision
      ↓
Less memory
```

---

# 34. Default Configuration

For interview purposes, remember:

```text
Default initial capacity = 16

Default load factor = 0.75
```

And:

```text
Threshold ≈ capacity × load factor
```

---

# 35. Tricky Interview Questions

## Q1. What is the default load factor?

```text
0.75
```

---

## Q2. What is the default initial capacity?

```text
16
```

with the important caveat that the internal table is lazily initialized.

---

## Q3. Is capacity the same as size?

No.

```text
Capacity = buckets

Size = mappings
```

---

## Q4. Is capacity the maximum number of entries?

No.

Multiple entries can be stored in the same bucket.

---

## Q5. Why does HashMap use a power-of-two capacity?

To efficiently calculate bucket indexes using bitwise operations.

---

## Q6. What happens if load factor is decreased?

The map resizes earlier.

This generally reduces collisions but increases memory usage.

---

## Q7. What happens if load factor is increased?

The map can become denser before resizing.

This can reduce memory overhead but may increase collisions.

---

## Q8. Why might we specify initial capacity?

To reduce repeated resizing when we know approximately how many entries will be stored.

---

## Q9. Does `new HashMap<>(100)` create 100 buckets immediately?

No.

The requested capacity is processed according to HashMap's sizing rules, and the internal table is lazily initialized.

---

## Q10. Can HashMap contain more entries than its capacity?

Yes.

Capacity is the number of buckets, not the maximum number of entries.

---

# 36. Interview Scenario ⭐⭐⭐⭐⭐

### Interviewer:

> I know my application will store approximately 1,000 entries. Why would you specify an initial capacity?

### Answer:

To reduce repeated resizing and redistribution as the map grows.

A suitable initial capacity can be chosen based on the expected number of entries and the load factor.

For example:

```text
1000 / 0.75 ≈ 1333
```

Then choose an appropriate power-of-two capacity, such as:

```text
2048
```

---

# 37. Interview Scenario

### Interviewer:

> Why is the default load factor 0.75 instead of 1.0?

### Answer:

It provides a practical balance between memory usage and collision rate.

A lower load factor reduces collisions but consumes more memory, while a higher load factor saves memory but can increase collisions.

---

# 38. Interview Scenario

### Interviewer:

> Does HashMap resize when 75% of its buckets are occupied?

### Answer:

Not exactly.

The load factor is used to calculate the resize threshold:

```text
threshold = capacity × load factor
```

The trigger is based on the map's number of mappings relative to that threshold, not simply the percentage of non-empty buckets.

---

# 39. Quick Revision

```text
Capacity
    ↓
Number of buckets

Default Capacity
    ↓
16

Load Factor
    ↓
0.75

Threshold
    ↓
Capacity × Load Factor

Resize
    ↓
Increase capacity

Capacity
    ↓
Normally doubles

Capacity
    ≠
Maximum entries
```

---

# 40. Golden Interview Answer

> **Capacity is the number of buckets in a HashMap, while load factor determines how full the table is allowed to become before resizing. The default initial capacity is 16 and the default load factor is 0.75. The resize threshold is approximately capacity multiplied by the load factor. HashMap uses power-of-two capacities for efficient bucket indexing, and providing an appropriate initial capacity can reduce repeated resizing when the expected number of entries is known.**

---

# 41. One-Line Summary

**Capacity tells us how many buckets the HashMap has, load factor controls when it grows, and threshold is calculated from the two to trigger resizing.**
