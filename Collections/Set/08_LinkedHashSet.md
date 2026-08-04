# LinkedHashSet in Java

---

# 1. What is LinkedHashSet?

LinkedHashSet is an implementation of the **Set** interface that stores **unique elements** while maintaining **insertion order**.

Internally, LinkedHashSet is backed by a **LinkedHashMap**.

It combines the advantages of:

- HashSet → Fast lookup using hashing.
- LinkedList → Maintains insertion order using a doubly linked list.

LinkedHashSet allows one null element and is not synchronized.

---

# Why was LinkedHashSet Introduced?

HashSet provides fast operations but does not maintain insertion order.

Example:

```java
HashSet<String> set = new HashSet<>();

set.add("Java");
set.add("Spring");
set.add("Hibernate");
```

Output may be

```text
Hibernate
Java
Spring
```

Order is unpredictable.

Sometimes applications require:

- Unique elements
- Original insertion order

LinkedHashSet solves this problem.

---

# Why Do We Need LinkedHashSet?

Suppose we store recently visited pages.

```text
Home

Products

Cart

Payment
```

We want:

- No duplicates
- Same insertion order

LinkedHashSet is the perfect choice.

---

# Real Project Examples

## Browser History

```text
Home

Products

Cart

Checkout
```

---

## Recently Viewed Products

```text
Laptop

Mobile

Keyboard
```

---

## Unique Search History

```text
Java

Spring

Hibernate
```

Duplicates should not appear.

---

# Internal Working

LinkedHashSet internally uses:

```java
LinkedHashMap
```

Simplified source code hierarchy:

```text
LinkedHashSet

↓

HashSet

↓

LinkedHashMap
```

Each element is stored as:

```text
Key  -> Actual Element

Value -> Dummy Object (PRESENT)
```

Unlike HashSet, LinkedHashMap maintains a doubly linked list connecting all entries.

---

# Internal Architecture

```text
LinkedHashSet

↓

LinkedHashMap

↓

Hash Table

+

Doubly Linked List
```

Visualization

```text
Buckets

↓

Node

↓

Node

↓

Node

--------------------

Linked List

Java

↓

Spring

↓

Hibernate
```

Even if hashing changes bucket positions, iteration follows the linked list.

---

# Duplicate Detection

Same as HashSet.

Step 1

```java
hashCode()
```

↓

Step 2

Bucket lookup.

↓

Step 3

```java
equals()
```

↓

Duplicate ignored.

---

# Constructors

Default

```java
LinkedHashSet<String> set =
        new LinkedHashSet<>();
```

Capacity

```java
LinkedHashSet<String> set =
        new LinkedHashSet<>(20);
```

Capacity + Load Factor

```java
LinkedHashSet<String> set =
        new LinkedHashSet<>(20,0.75f);
```

Collection Constructor

```java
LinkedHashSet<String> set =
        new LinkedHashSet<>(list);
```

---

# Important Methods

```java
add()

remove()

contains()

size()

clear()

isEmpty()

iterator()
```

---

# Example

```java
Set<String> set = new LinkedHashSet<>();

set.add("Java");
set.add("Spring");
set.add("Hibernate");

System.out.println(set);
```

Output

```text
Java

Spring

Hibernate
```

Insertion order maintained.

---

# Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| add() | O(1) Average | Hashing |
| remove() | O(1) Average | Hashing |
| contains() | O(1) Average | Hashing |
| Iteration | O(n) | Visit linked nodes |

---

# Advantages

- Maintains insertion order
- Prevents duplicates
- Fast lookup
- Allows one null
- Easy iteration

---

# Disadvantages

- More memory than HashSet
- Slightly slower than HashSet
- Not sorted
- Not synchronized

---

# HashSet vs LinkedHashSet

| Feature | HashSet | LinkedHashSet |
|----------|----------|---------------|
| Duplicates | No | No |
| Order | No | Yes |
| Internal Structure | HashMap | LinkedHashMap |
| Memory | Less | More |
| Performance | Slightly Faster | Slightly Slower |

---

# LinkedHashSet vs TreeSet

| Feature | LinkedHashSet | TreeSet |
|----------|---------------|----------|
| Order | Insertion Order | Sorted Order |
| Null | One Allowed | Not Allowed (Natural Ordering) |
| Complexity | O(1) Avg | O(log n) |
| Internal Structure | LinkedHashMap | TreeMap |

---

# Frequently Asked Interview Questions

## Q1. What is LinkedHashSet?

LinkedHashSet is a Set implementation that stores unique elements while maintaining insertion order.

---

## Q2. Which class does LinkedHashSet use internally?

LinkedHashMap.

---

## Q3. Does LinkedHashSet maintain insertion order?

Yes.

---

## Q4. Does LinkedHashSet allow duplicates?

No.

---

## Q5. Does LinkedHashSet allow null?

Yes.

One null element.

---

## Q6. Is LinkedHashSet synchronized?

No.

---

# Cross Questions

## Why is LinkedHashSet slower than HashSet?

Because it maintains an additional doubly linked list to preserve insertion order.

---

## Why is LinkedHashSet faster than TreeSet?

Because hashing provides average O(1) operations, while TreeSet uses a Red-Black Tree with O(log n) operations.

---

## Does LinkedHashSet sort elements?

No.

It only preserves insertion order.

---

## Can LinkedHashSet replace HashSet?

Yes, if insertion order is required.

---

## Why does LinkedHashSet consume more memory?

Each entry stores extra references for the doubly linked list.

---

# Tricky Interview Questions

## Question

Can LinkedHashSet contain duplicate null values?

Answer

No.

Only one null is allowed.

---

## Question

Does LinkedHashSet automatically sort elements?

No.

It preserves insertion order only.

---

## Question

Can we access elements using an index?

No.

Like all Set implementations, LinkedHashSet does not support index-based access.

---

## Question

How does LinkedHashSet prevent duplicates?

Using:

1. hashCode()
2. equals()

---

# Best Practices

- Use LinkedHashSet when uniqueness and insertion order are both required.
- Override `equals()` and `hashCode()` for custom objects.
- Program to the `Set` interface.

```java
Set<String> set = new LinkedHashSet<>();
```

---

# Common Mistakes

- Expecting LinkedHashSet to sort elements.
- Assuming it performs exactly like HashSet without extra memory.
- Forgetting to override `equals()` and `hashCode()` for custom classes.

---

# Quick Revision

```text
Implements          → Set

Internal Structure  → LinkedHashMap

Duplicates          → Not Allowed

Null                → One Allowed

Insertion Order     → Maintained

Sorting             → No

Thread Safe         → No

Average Complexity  → O(1)
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to know if you can choose the correct Set implementation based on requirements.

Typical follow-up questions:

- Difference between HashSet and LinkedHashSet?
- Why does LinkedHashSet maintain insertion order?
- Which internal data structure is used?
- Why does it consume more memory than HashSet?
- When would you choose LinkedHashSet over TreeSet?

---

# One-Line Summary

**LinkedHashSet is a HashMap-based Set implementation backed by LinkedHashMap that stores unique elements while preserving insertion order with average O(1) performance.**
