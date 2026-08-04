# TreeSet in Java

---

# 1. What is TreeSet?

TreeSet is an implementation of the **Set** interface that stores **unique elements in sorted order**.

Unlike HashSet and LinkedHashSet, TreeSet automatically sorts elements using either:

- Natural Ordering (Comparable)
- Custom Ordering (Comparator)

Internally, TreeSet is backed by a **TreeMap**, which is implemented using a **Red-Black Tree**.

TreeSet provides **O(log n)** time complexity for add(), remove(), and contains() operations.

TreeSet does not allow duplicate elements and does not allow null elements when using natural ordering.

---

# Why was TreeSet Introduced?

HashSet provides fast searching.

LinkedHashSet maintains insertion order.

Neither provides automatic sorting.

Suppose we store employee IDs.

```text
105

102

109

101
```

Expected Output

```text
101

102

105

109
```

TreeSet automatically sorts the elements.

---

# Why Do We Need TreeSet?

Whenever data must remain sorted.

Examples:

- Student Marks
- Employee IDs
- Product Prices
- Dictionary Words
- Leaderboards
- Rankings

---

# Real Project Examples

## Student Roll Numbers

```text
105

101

103

102
```

Output

```text
101

102

103

105
```

---

## Dictionary

```text
Cat

Apple

Dog
```

Output

```text
Apple

Cat

Dog
```

---

# Internal Working ⭐⭐⭐⭐⭐

TreeSet internally uses:

```java
TreeMap
```

Simplified Source Code

```java
private transient NavigableMap<E,Object> m;
```

Actually

```text
TreeSet

↓

TreeMap

↓

Red-Black Tree
```

Every element is stored as

```text
Key

↓

Actual Element

Value

↓

PRESENT (Dummy Object)
```

Exactly like HashSet uses HashMap.

---

# Internal Architecture

```text
TreeSet

↓

TreeMap

↓

Red-Black Tree
```

Visualization

```text
          50
        /    \
      30      70
     /  \    /  \
   20   40 60   80
```

Unlike HashSet,

there are

- No buckets
- No hashing

Everything depends upon tree traversal.

---

# How add() Works Internally

Suppose

```java
set.add(50);
set.add(30);
set.add(70);
```

Step 1

Compare values.

↓

Step 2

Insert at correct position.

↓

Step 3

Balance tree.

↓

Maintain Red-Black Tree properties.

---

# Duplicate Detection

TreeSet does NOT use

```java
hashCode()
```

Instead

```java
compareTo()

or

Comparator
```

If comparison returns

```text
0
```

↓

Duplicate.

Element ignored.

---

# Constructors

Default

```java
TreeSet<Integer> set =
        new TreeSet<>();
```

Comparator

```java
TreeSet<Integer> set =
        new TreeSet<>(Comparator.reverseOrder());
```

Collection Constructor

```java
TreeSet<Integer> set =
        new TreeSet<>(list);
```

---

# Important Methods

Add

```java
set.add(10);
```

Remove

```java
set.remove(10);
```

Contains

```java
set.contains(10);
```

First

```java
set.first();
```

Last

```java
set.last();
```

Higher

```java
set.higher(20);
```

Lower

```java
set.lower(20);
```

Ceiling

```java
set.ceiling(20);
```

Floor

```java
set.floor(20);
```

---

# Example

```java
Set<Integer> numbers =
        new TreeSet<>();

numbers.add(50);
numbers.add(10);
numbers.add(30);
numbers.add(20);

System.out.println(numbers);
```

Output

```text
[10,20,30,50]
```

Automatically sorted.

---

# Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| add() | O(log n) | Tree traversal |
| remove() | O(log n) | Tree traversal |
| contains() | O(log n) | Tree traversal |
| first() | O(log n) | Left traversal |
| last() | O(log n) | Right traversal |

---

# Advantages

- Automatically sorted
- No duplicates
- Supports range operations
- Predictable ordering

---

# Disadvantages

- Slower than HashSet
- More memory usage
- Does not allow null (natural ordering)
- Requires elements to be comparable or a Comparator

---

# TreeSet vs HashSet

| Feature | TreeSet | HashSet |
|----------|----------|----------|
| Order | Sorted | Unordered |
| Internal Structure | TreeMap | HashMap |
| Data Structure | Red-Black Tree | Hash Table |
| add() | O(log n) | O(1) Avg |
| Search | O(log n) | O(1) Avg |
| Null | No (Natural Ordering) | One Allowed |

---

# TreeSet vs LinkedHashSet

| Feature | TreeSet | LinkedHashSet |
|----------|----------|---------------|
| Order | Sorted | Insertion Order |
| Internal Structure | TreeMap | LinkedHashMap |
| Search | O(log n) | O(1) Avg |
| Null | No | One Allowed |

---

# Frequently Asked Interview Questions

## Q1. What is TreeSet?

TreeSet is a Set implementation that stores unique elements in sorted order using a Red-Black Tree.

---

## Q2. Which data structure is used internally?

Red-Black Tree.

---

## Q3. Does TreeSet allow duplicates?

No.

---

## Q4. Does TreeSet allow null?

No (when using natural ordering).

---

## Q5. Does TreeSet maintain insertion order?

No.

It maintains sorted order.

---

## Q6. Which class backs TreeSet?

TreeMap.

---

# Cross Questions

## Why is TreeSet slower than HashSet?

Because TreeSet performs tree traversal (O(log n)), whereas HashSet uses hashing (average O(1)).

---

## Why does TreeSet not use hashCode()?

Because elements are organized by comparison, not hashing.

---

## How does TreeSet identify duplicates?

Using:

```java
compareTo()

or

Comparator
```

If the comparison result is `0`, the element is treated as a duplicate.

---

## Why must elements implement Comparable?

Without a Comparator, TreeSet needs a way to compare elements to determine their sorted position.

---

## When should we use TreeSet?

When both uniqueness and automatic sorting are required.

---

# Tricky Interview Questions

## Question

Can TreeSet contain null?

Answer

With natural ordering, no.

Adding `null` throws a `NullPointerException`.

---

## Question

Does TreeSet use hashCode()?

No.

---

## Question

Can TreeSet store custom objects?

Yes.

But the objects must either implement `Comparable` or a `Comparator` must be provided.

---

## Question

Can TreeSet replace HashSet?

Only if sorting is required.

Otherwise, HashSet is usually faster.

---

# Best Practices

- Use TreeSet when sorted unique data is required.
- Prefer HashSet if ordering is not needed and performance is the priority.
- Implement `Comparable` carefully or provide a `Comparator` for custom objects.

---

# Common Mistakes

- Expecting TreeSet to preserve insertion order.
- Assuming TreeSet uses hashing.
- Forgetting to implement `Comparable` or provide a `Comparator` for custom objects.
- Trying to insert `null` with natural ordering.

---

# Quick Revision

```text
Implements          → Set

Internal Structure  → TreeMap

Data Structure      → Red-Black Tree

Duplicates          → Not Allowed

Null                → Not Allowed (Natural Ordering)

Sorting             → Yes

Thread Safe         → No

Average Complexity  → O(log n)

Duplicate Check     → compareTo()/Comparator
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to verify that you understand the three major Set implementations and can choose the correct one.

Typical follow-up questions:

- HashSet vs LinkedHashSet vs TreeSet?
- Why is TreeSet O(log n)?
- Does TreeSet use hashCode()?
- How are duplicates detected?
- Why can't TreeSet store null with natural ordering?
- What happens if compareTo() returns 0?

---

# One-Line Summary

**TreeSet is a TreeMap-backed implementation of the Set interface that stores unique elements in sorted order using a Red-Black Tree, providing O(log n) operations through element comparison rather than hashing.**
