# HashSet in Java

---

# 1. What is HashSet?

HashSet is an implementation of the **Set** interface that stores **unique elements** and does **not maintain insertion order**.

Internally, HashSet is backed by a **HashMap**. Every element added to a HashSet is actually stored as a **key** in a HashMap, while a constant dummy object is used as its value.

HashSet provides **O(1) average time complexity** for add(), remove(), and contains() operations by using a hashing mechanism.

HashSet allows **one null element** and is **not synchronized**.

---

# Why was HashSet Introduced?

Suppose we want to store:

```text
101

102

103

101
```

Duplicate values should not be allowed.

Using a List:

```java
List<Integer> list = new ArrayList<>();
```

would store duplicates.

To automatically prevent duplicate elements and provide fast lookup, Java introduced HashSet.

---

# Why Do We Need HashSet?

Real-world examples:

- Registered Email IDs
- Employee IDs
- Product Codes
- Coupon Codes
- Unique Usernames

All these require uniqueness.

---

# Internal Working ⭐⭐⭐⭐⭐

This is one of the most important interview questions.

Most people think HashSet has its own internal data structure.

**It does not.**

HashSet internally uses **HashMap**.

Source code (simplified):

```java
private transient HashMap<E,Object> map;
```

Whenever we write:

```java
HashSet<String> set = new HashSet<>();

set.add("Java");
```

Internally Java executes something similar to:

```java
map.put("Java", PRESENT);
```

where

```java
private static final Object PRESENT = new Object();
```

Notice:

```text
HashSet Element

↓

HashMap Key

↓

Dummy Object (PRESENT)
```

So the actual storage is:

```text
HashMap

Key          Value

Java   ---> PRESENT

Spring ---> PRESENT

SQL    ---> PRESENT
```

The value is ignored.

Only the key matters.

---

# Internal Architecture

```text
HashSet

↓

HashMap

↓

Bucket Array

↓

Node

↓

Key + Value
```

---

# How add() Works Internally

Suppose:

```java
set.add("Java");
```

Step 1

```text
hashCode()

↓

2301506
```

Step 2

HashMap calculates bucket index.

```text
Bucket = hash % capacity
```

(Internally Java uses a faster bitwise calculation.)

Step 3

Check bucket.

```text
Empty?

↓

Yes
```

Store new node.

If bucket already contains an element:

Compare

```java
equals()
```

If equal:

Duplicate.

Return false.

Otherwise:

Store in another node (collision handling).

---

# Duplicate Detection

Suppose

```java
set.add("Java");
set.add("Java");
```

HashSet performs

Step 1

```java
hashCode()
```

Step 2

If hash is same

↓

Step 3

```java
equals()
```

If equals() returns true

↓

Duplicate ignored.

---

# Why hashCode() and equals()?

Interviewers love this question.

Imagine:

```java
Employee e1 = new Employee(101);

Employee e2 = new Employee(101);
```

Different objects.

Same employee.

Without properly overriding:

```java
equals()

hashCode()
```

HashSet treats them as different objects.

Result

```text
Duplicate data stored ❌
```

We'll study equals() and hashCode() in depth later.

---

# Constructors

Default

```java
HashSet<String> set =
        new HashSet<>();
```

Initial Capacity

```java
HashSet<String> set =
        new HashSet<>(20);
```

Capacity + Load Factor

```java
HashSet<String> set =
        new HashSet<>(20,0.75f);
```

Collection Constructor

```java
HashSet<String> set =
        new HashSet<>(list);
```

---

# Important Methods

Add

```java
set.add("Java");
```

Remove

```java
set.remove("Java");
```

Contains

```java
set.contains("Java");
```

Size

```java
set.size();
```

Is Empty

```java
set.isEmpty();
```

Clear

```java
set.clear();
```

---

# Example

```java
Set<String> technologies = new HashSet<>();

technologies.add("Java");
technologies.add("Spring");
technologies.add("Java");

System.out.println(technologies);
```

Output

```text
[Java, Spring]
```

Duplicate removed.

---

# Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| add() | O(1) Average | Hashing |
| contains() | O(1) Average | Hashing |
| remove() | O(1) Average | Hashing |
| Worst Case | O(n) | Heavy collisions |

---

# Real Project Example

Employee IDs

```text
1001

1002

1003

1002
```

HashSet automatically prevents duplicate IDs.

---

# Advantages

- No duplicate elements
- Fast searching
- Fast insertion
- Fast deletion
- Allows one null
- Backed by HashMap

---

# Disadvantages

- No insertion order
- No sorting
- Not thread-safe
- Depends on proper hashCode() and equals()

---

# HashSet vs ArrayList

| Feature | HashSet | ArrayList |
|----------|----------|-----------|
| Duplicates | Not Allowed | Allowed |
| Order | Not Guaranteed | Maintained |
| Internal Structure | HashMap | Dynamic Array |
| Search | O(1) Avg | O(n) |

---

# Frequently Asked Interview Questions

## Q1. What is HashSet?

### Answer

HashSet is a Set implementation backed by HashMap that stores unique elements.

---

## Q2. Does HashSet allow duplicate elements?

No.

---

## Q3. Does HashSet allow null?

Yes.

Only one null element.

---

## Q4. Is HashSet synchronized?

No.

---

## Q5. Which data structure does HashSet use internally?

HashMap.

---

## Q6. Does HashSet maintain insertion order?

No.

Use LinkedHashSet if insertion order is required.

---

# Cross Questions

## Why does HashSet use HashMap internally?

Because HashMap already provides efficient hashing, bucket management, collision handling, and fast lookup.

HashSet only needs unique keys, so it reuses HashMap instead of implementing hashing logic again.

---

## Why is only the key stored?

Because Set stores only values.

Internally, those values become HashMap keys.

The value part is a constant dummy object.

---

## What is PRESENT?

```java
private static final Object PRESENT = new Object();
```

A dummy object used as the value for every key in the internal HashMap.

---

## Why is add() O(1)?

Because hashing allows Java to directly locate the correct bucket in average cases.

---

## Why can HashSet become O(n)?

If many elements collide into the same bucket, Java may need to scan multiple nodes.

(Java 8 improves this with treeification in large buckets—we'll cover that in `HashMap.md`.)

---

# Tricky Interview Questions

## Question

Can HashSet contain duplicate null values?

Answer

No.

Only one null is allowed.

---

## Question

Can HashSet store primitive types?

No.

Use Wrapper Classes.

Example

```java
HashSet<Integer>
```

---

## Question

Does HashSet preserve insertion order?

No.

---

## Question

How does HashSet identify duplicates?

Using:

1. hashCode()
2. equals()

---

# Best Practices

- Always override both `equals()` and `hashCode()` for custom objects.
- Use HashSet when uniqueness is required and ordering is not important.
- Program to the `Set` interface.

```java
Set<String> set = new HashSet<>();
```

---

# Common Mistakes

- Forgetting to override `equals()` and `hashCode()`.
- Expecting insertion order.
- Assuming HashSet stores elements in sorted order.
- Using mutable objects as keys without understanding the impact on hashing.

---

# Quick Revision

```text
Implements          → Set

Internal Structure  → HashMap

Duplicates          → Not Allowed

Null                → One Allowed

Order               → Not Guaranteed

Thread Safe         → No

Average Complexity  → O(1)

Duplicate Check     → hashCode() + equals()
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers are checking whether you know that **HashSet is not an independent data structure**.

Typical follow-up questions:

- How does HashSet work internally?
- Why does it use HashMap?
- What is the `PRESENT` object?
- How are duplicates detected?
- Why are `equals()` and `hashCode()` important?
- Why is HashSet faster than ArrayList for searching?

If you answer these well, you'll be well-prepared for the much deeper `HashMap` discussion.

---

# One-Line Summary

**HashSet is a HashMap-backed implementation of the Set interface that stores unique elements using hashing, providing O(1) average-time operations while preventing duplicates through `hashCode()` and `equals()`.**
