# PriorityQueue in Java

---

# 1. What is PriorityQueue?

PriorityQueue is an implementation of the **Queue** interface that orders elements according to their **priority** instead of insertion order.

By default, PriorityQueue follows **Natural Ordering** (ascending order).

Internally, PriorityQueue is implemented using a **Binary Heap (Min Heap)**.

Unlike a normal Queue, the first inserted element is **not necessarily removed first**.

Instead, the element with the **highest priority** is removed first.

For numbers,

Smallest Number = Highest Priority

---

# Why was PriorityQueue Introduced?

Suppose a hospital receives patients.

```text
Patient A (Critical)

Patient B (Normal)

Patient C (Emergency)
```

Patients should not be treated in arrival order.

Instead,

Higher priority should be served first.

PriorityQueue solves this problem.

---

# Why Do We Need PriorityQueue?

Whenever processing depends upon priority.

Examples

- CPU Scheduling
- Hospital Management
- Task Scheduling
- Job Scheduling
- Dijkstra Algorithm
- Huffman Coding
- Event Processing

---

# Real Project Examples

## Hospital Queue

```text
Critical

↓

Emergency

↓

Normal
```

---

## CPU Scheduler

```text
High Priority

↓

Medium Priority

↓

Low Priority
```

---

## Online Gaming

```text
VIP Player

↓

Premium Player

↓

Normal Player
```

---

# Internal Working ⭐⭐⭐⭐⭐

PriorityQueue internally uses a **Binary Heap**.

It is **NOT** a TreeSet.

It is **NOT** a LinkedList.

It is **NOT** a HashMap.

Internally

```text
PriorityQueue

↓

Binary Heap

↓

Array
```

---

# What is a Binary Heap?

A Binary Heap is a **Complete Binary Tree** stored inside an array.

Example

```text
        10
      /    \
    20      30
   /  \
 40   50
```

Internally stored as

```text
Index

0 → 10

1 → 20

2 → 30

3 → 40

4 → 50
```

---

# Why Array?

Binary Heap does not require node objects.

Array provides

- Less Memory
- Better Cache Performance
- Faster Access

---

# Internal Architecture

```text
PriorityQueue

↓

Object[]

↓

Binary Heap
```

---

# How offer() Works

Suppose

```java
pq.offer(40);

pq.offer(20);

pq.offer(10);

pq.offer(50);
```

Step 1

Insert at end.

```text
40

20

10

50
```

Step 2

Perform Heapify Up.

Result

```text
10

20

40

50
```

Heap property maintained.

---

# How poll() Works

Suppose Heap

```text
10

20

40

50
```

Remove Root

↓

Move Last Element to Root

↓

Heapify Down

Result

```text
20

50

40
```

---

# Constructors

Default

```java
PriorityQueue<Integer> pq =
        new PriorityQueue<>();
```

Capacity

```java
PriorityQueue<Integer> pq =
        new PriorityQueue<>(20);
```

Comparator

```java
PriorityQueue<Integer> pq =
        new PriorityQueue<>(Comparator.reverseOrder());
```

Collection

```java
PriorityQueue<Integer> pq =
        new PriorityQueue<>(list);
```

---

# Important Methods

Offer

```java
pq.offer(20);
```

Poll

```java
pq.poll();
```

Peek

```java
pq.peek();
```

Size

```java
pq.size();
```

Contains

```java
pq.contains(20);
```

Clear

```java
pq.clear();
```

---

# Example

```java
PriorityQueue<Integer> pq =
        new PriorityQueue<>();

pq.offer(40);
pq.offer(10);
pq.offer(30);
pq.offer(20);

while (!pq.isEmpty()) {
    System.out.println(pq.poll());
}
```

Output

```text
10

20

30

40
```

---

# Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| offer() | O(log n) | Heapify Up |
| poll() | O(log n) | Heapify Down |
| peek() | O(1) | Root Element |
| contains() | O(n) | Linear Search |

---

# Advantages

- Automatic priority ordering
- Efficient insertion
- Fast removal of highest priority
- Heap-based implementation
- Excellent for scheduling

---

# Disadvantages

- Does not maintain insertion order
- No index-based access
- Iteration is **not sorted**
- Not synchronized

---

# Queue vs PriorityQueue

| Feature | Queue | PriorityQueue |
|----------|---------------|---------------|
| Order | FIFO | Priority |
| Internal Structure | Depends | Binary Heap |
| poll() | First Inserted | Highest Priority |
| Sorting | No | Priority Only |

---

# Frequently Asked Interview Questions

## Q1. What is PriorityQueue?

PriorityQueue is a Queue implementation that removes elements based on priority instead of insertion order.

---

## Q2. Which data structure is used internally?

Binary Heap.

---

## Q3. Does PriorityQueue maintain insertion order?

No.

---

## Q4. Is PriorityQueue sorted?

No.

Only the head element is guaranteed to be the highest priority.

The remaining elements are arranged according to the heap structure, **not full sorting**.

---

## Q5. Does PriorityQueue allow duplicates?

Yes.

---

## Q6. Does PriorityQueue allow null?

No.

Adding null throws a NullPointerException.

---

# Cross Questions

## Why is peek() O(1)?

Because the highest-priority element is always stored at the root of the heap (array index 0).

---

## Why is offer() O(log n)?

Because after inserting at the end, the element may move upward (Heapify Up).

---

## Why is poll() O(log n)?

Because after removing the root, the last element moves to the root and Heapify Down restores the heap property.

---

## Does PriorityQueue use Red-Black Tree?

No.

It uses a Binary Heap.

---

## Does PriorityQueue sort the whole array?

No.

Only the heap property is maintained.

---

# Tricky Interview Questions

## Question

Can PriorityQueue store custom objects?

Answer

Yes.

Provide

```java
Comparable
```

or

```java
Comparator
```

---

## Question

Can we iterate in sorted order?

No.

Iteration order is not guaranteed to be sorted.

To retrieve elements in priority order, repeatedly call:

```java
poll()
```

---

## Question

How do we create a Max Heap?

```java
PriorityQueue<Integer> pq =
        new PriorityQueue<>(Comparator.reverseOrder());
```

---

## Question

Which element is always accessible in O(1)?

The root (highest-priority element).

---

# Best Practices

- Use PriorityQueue for scheduling and priority-based processing.
- Use `Comparator` for custom priority rules.
- Do not assume iteration returns sorted elements.

---

# Common Mistakes

- Assuming PriorityQueue behaves like FIFO.
- Assuming iteration order is sorted.
- Confusing Binary Heap with Binary Search Tree.
- Inserting null values.

---

# Quick Revision

```text
Implements          → Queue

Internal Structure  → Binary Heap

Storage             → Array

Ordering            → Priority

Duplicates          → Allowed

Null                → Not Allowed

offer()             → O(log n)

poll()              → O(log n)

peek()              → O(1)

Thread Safe         → No
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to check whether you understand that **PriorityQueue is heap-based**, not tree-based.

Common follow-up questions:

- Binary Heap vs Binary Search Tree?
- Why is `peek()` O(1)?
- Why is `offer()` O(log n)?
- How is a Max Heap created?
- Why isn't iteration sorted?
- Why doesn't PriorityQueue maintain insertion order?

---

# One-Line Summary

**PriorityQueue is a Queue implementation backed by a Binary Heap that processes elements based on priority instead of insertion order, providing O(log n) insertion and deletion while keeping the highest-priority element at the root.**
