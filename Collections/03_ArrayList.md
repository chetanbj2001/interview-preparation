# ArrayList in Java

---

# 1. What is ArrayList?

ArrayList is a resizable implementation of the **List** interface provided in the Java Collections Framework.

Internally, ArrayList uses a **dynamic array** (`Object[]`) to store elements.

Unlike arrays, ArrayList grows automatically when its capacity is exceeded.

ArrayList maintains insertion order, allows duplicate elements, allows multiple null values, and provides fast random access using indexes.

It is the most commonly used List implementation because of its simplicity and excellent read performance.

---

# Why was ArrayList Introduced?

Arrays in Java have a fixed size.

Example:

```java
int[] arr = new int[5];
```

If the array becomes full, we cannot add more elements without creating a new array manually.

To solve this limitation, Java introduced ArrayList.

ArrayList automatically increases its capacity whenever required.

---

# Why Do We Need ArrayList?

Suppose we are building an Employee Management System.

Initially:

```text
10 Employees
```

Later:

```text
100 Employees

500 Employees

5000 Employees
```

The number of employees is not fixed.

Instead of creating large arrays, we can use ArrayList because it grows dynamically.

---

# Internal Working

This is one of the most frequently asked interview questions.

Internally, ArrayList stores data inside an array.

```java
Object[] elementData;
```

Visualization

```text
ArrayList

↓

Object[]

+-----+-----+-----+-----+-----+
| A   | B   | C   | D   | E   |
+-----+-----+-----+-----+-----+
```

When the array becomes full, Java creates a larger array.

Steps:

1. Create a new array with larger capacity.
2. Copy all existing elements.
3. Add the new element.
4. Old array becomes eligible for Garbage Collection.

---

# Capacity Growth

Default constructor:

```java
ArrayList<String> list = new ArrayList<>();
```

Initially:

```text
Capacity = 0
```

When the first element is added:

```text
Capacity becomes 10
```

If the array becomes full:

```text
New Capacity

=

Old Capacity + (Old Capacity / 2)
```

Example:

```text
10

↓

15

↓

22

↓

33

↓

49
```

(Java implementation details may vary slightly between JDK versions, but the growth strategy is approximately 1.5×.)

---

# Why 1.5x Growth?

If Java doubled the array every time:

```text
10

20

40

80
```

Too much memory would be wasted.

If Java increased by only one element:

```text
10

11

12

13
```

Frequent copying would make insertion very slow.

A growth factor of about **1.5×** provides a good balance between memory usage and performance.

---

# ArrayList Architecture

```text
Iterable
      │
Collection
      │
List
      │
ArrayList
```

---

# Important Constructors

```java
ArrayList()
```

Default constructor.

---

```java
ArrayList(int capacity)
```

Creates an ArrayList with a specified initial capacity.

---

```java
ArrayList(Collection c)
```

Creates an ArrayList from another collection.

---

# Important Methods

## Add

```java
list.add("Java");
```

---

## Get

```java
list.get(0);
```

---

## Update

```java
list.set(0, "Spring");
```

---

## Remove

```java
list.remove(0);
```

---

## Search

```java
list.contains("Java");
```

---

## Size

```java
list.size();
```

---

## Clear

```java
list.clear();
```

---

# Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| add(E) (end) | O(1) (Amortized) | Usually appends at the end |
| add(index) | O(n) | Elements are shifted |
| get(index) | O(1) | Direct array access |
| set(index) | O(1) | Direct array update |
| remove(index) | O(n) | Elements are shifted |
| contains() | O(n) | Linear search |

---

# Real Project Example

## Shopping Cart

```java
List<Product> cart = new ArrayList<>();
```

Customer adds products.

Products remain in insertion order.

Random access is very fast.

---

## Student Records

```java
List<Student> students =
        new ArrayList<>();
```

Students can be added dynamically.

---

# Advantages

- Dynamic size
- Fast random access
- Maintains insertion order
- Allows duplicates
- Allows null values
- Easy to use

---

# Disadvantages

- Slow insertion in the middle
- Slow deletion in the middle
- Array resizing requires copying elements
- Not synchronized

---

# ArrayList vs Array

| Array | ArrayList |
|--------|-----------|
| Fixed Size | Dynamic Size |
| Primitive Supported | Objects Only |
| No Built-in Methods | Rich API |
| Faster for fixed-size data | Flexible |

---

# Frequently Asked Interview Questions

## Q1. What is ArrayList?

### Answer

ArrayList is a resizable implementation of the List interface backed by a dynamic array.

---

## Q2. Does ArrayList maintain insertion order?

### Answer

Yes.

---

## Q3. Does ArrayList allow duplicate elements?

### Answer

Yes.

---

## Q4. Does ArrayList allow null values?

### Answer

Yes.

It allows multiple null values.

---

## Q5. Is ArrayList synchronized?

### Answer

No.

It is not thread-safe.

---

## Q6. What is the default capacity of ArrayList?

### Answer

When you create an empty ArrayList using the default constructor, its internal array is initially empty. On adding the **first element**, it grows to a default capacity of **10**.

---

# Cross Questions

## Why is get() O(1)?

### Answer

Because ArrayList uses an array.

Index calculation directly accesses the element.

---

## Why is remove() O(n)?

### Answer

After removing an element, all subsequent elements must shift one position to the left.

---

## Why is insertion in the middle O(n)?

### Answer

Existing elements need to shift to make space.

---

## Can ArrayList store primitive data types?

### Answer

No.

Use wrapper classes.

```java
ArrayList<Integer>
```

---

## Why is ArrayList faster than LinkedList for reading?

### Answer

Because arrays provide direct index-based access.

LinkedList must traverse nodes.

---

# Tricky Interview Questions

## Question

Can ArrayList contain duplicate null values?

### Answer

Yes.

```java
list.add(null);
list.add(null);
```

Valid.

---

## Question

Can we create an ArrayList of primitive int?

### Answer

No.

Use Integer.

---

## Question

Can ArrayList grow automatically?

### Answer

Yes.

It automatically resizes when capacity is exceeded.

---

## Question

Is ArrayList thread-safe?

### Answer

No.

Use synchronization or a concurrent collection if multiple threads modify it.

---

# Best Practices

- Use ArrayList when read operations are more frequent than insertions or deletions.
- Specify an initial capacity if you know the approximate number of elements to reduce resizing.
- Program to the `List` interface.

```java
List<String> list = new ArrayList<>();
```

---

# Common Mistakes

- Using ArrayList for frequent middle insertions.
- Assuming ArrayList is synchronized.
- Confusing size with capacity.
- Believing every `add()` operation is O(1); resizing occasionally makes it more expensive, which is why we say **amortized O(1)**.

---

# Quick Revision

```text
Implements            → List

Internal Structure    → Object[]

Insertion Order       → Yes

Duplicates            → Yes

Null Values           → Yes

Random Access         → O(1)

Middle Insertion      → O(n)

Middle Deletion       → O(n)

Thread Safe           → No

Default Capacity      → Grows to 10 on first insertion
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers are checking whether you know **how ArrayList works internally**, not just how to use it.

Very common follow-up questions include:

- Why is `get()` O(1)?
- Why is `remove()` O(n)?
- What happens when ArrayList becomes full?
- What is the difference between size and capacity?
- Why is `add()` amortized O(1)?

If you can answer these confidently, it demonstrates a solid understanding of one of Java's most widely used collection classes.

---

# One-Line Summary

**ArrayList is a dynamic array implementation of the List interface that provides fast random access, maintains insertion order, allows duplicates and null values, and automatically grows as elements are added.**
