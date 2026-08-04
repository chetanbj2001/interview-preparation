# LinkedList in Java

---

# 1. What is LinkedList?

LinkedList is a class in the Java Collections Framework that implements the **List**, **Deque**, and **Queue** interfaces.

Unlike ArrayList, LinkedList stores elements as **nodes** connected using references instead of storing them in a continuous array.

Java's LinkedList is implemented as a **Doubly Linked List**, where each node contains:

- Previous Node Reference
- Data
- Next Node Reference

Because of this structure, insertion and deletion operations are very efficient, especially at the beginning or middle of the list.

However, random access is slower because elements must be traversed node by node.

---

# Why was LinkedList Introduced?

ArrayList performs very well for reading data.

However, inserting or deleting elements in the middle requires shifting many elements.

Example

```text
ArrayList

A B C D E

Insert X at index 2

↓

A B X C D E

Shift C, D, E
```

This shifting makes insertion expensive.

LinkedList solves this problem by simply changing references between nodes.

---

# Why Do We Need LinkedList?

Suppose we are developing:

- Music Playlist
- Browser History
- Undo/Redo Feature
- Task Scheduler

These applications frequently insert or remove elements.

LinkedList performs these operations efficiently.

---

# Internal Working

LinkedList is implemented as a **Doubly Linked List**.

Each node contains three parts.

```text
+-----------------------------+
| Previous | Data | Next      |
+-----------------------------+
```

Visualization

```text
null
  │
  ▼
+-----+     +-----+     +-----+
| A | ●────►| B | ●────►| C | X
| ▲ |       | ▲ |       | ▲ |
+─┼─+       +─┼─+       +─┼─+
  │           │           │
  └───────────┴───────────┘
```

Java internally maintains:

```java
first
last
size
```

This allows efficient insertion at both ends.

---

# Architecture

```text
Iterable
      │
Collection
      │
List
      │
LinkedList
      │
Also Implements
      │
Queue
Deque
```

---

# Important Constructors

```java
LinkedList<String> list =
        new LinkedList<>();
```

---

# Important Methods

## Add

```java
list.add("Java");
```

---

## Add First

```java
list.addFirst("Spring");
```

---

## Add Last

```java
list.addLast("Boot");
```

---

## Remove First

```java
list.removeFirst();
```

---

## Remove Last

```java
list.removeLast();
```

---

## Get First

```java
list.getFirst();
```

---

## Get Last

```java
list.getLast();
```

---

## Peek

```java
list.peek();
```

---

## Poll

```java
list.poll();
```

---

# Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| addFirst() | O(1) | Update head reference |
| addLast() | O(1) | Update tail reference |
| removeFirst() | O(1) | Remove head node |
| removeLast() | O(1) | Remove tail node |
| get(index) | O(n) | Traverse node by node |
| search | O(n) | Linear traversal |
| add(index) | O(n) | Traverse to the position first |
| remove(index) | O(n) | Traverse to the node first |

> **Interview Tip:** Although insertion itself is just pointer manipulation (O(1)), inserting at an arbitrary index is **O(n)** because Java must first traverse to that position.

---

# Real Project Example

## Browser History

```text
Google

↓

YouTube

↓

GitHub

↓

ChatGPT
```

Previous and Next navigation becomes efficient.

---

## Music Playlist

```text
Song 1

↓

Song 2

↓

Song 3
```

Adding or removing songs is efficient.

---

## Undo/Redo Feature

Editors maintain previous and next states using linked structures.

---

# Advantages

- Fast insertion at beginning and end
- Fast deletion at beginning and end
- Dynamic size
- No array resizing
- Implements List, Queue, and Deque

---

# Disadvantages

- Slow random access
- More memory usage due to node references
- Poor cache locality compared to arrays

---

# LinkedList vs ArrayList

| Feature | ArrayList | LinkedList |
|---------|-----------|------------|
| Internal Structure | Dynamic Array | Doubly Linked List |
| Random Access | O(1) | O(n) |
| Middle Insertion | O(n) | O(n) traversal + O(1) link update |
| End Insertion | Amortized O(1) | O(1) |
| Memory Usage | Lower | Higher |
| Cache Locality | Better | Poor |

---

# Frequently Asked Interview Questions

## Q1. What is LinkedList?

### Answer

LinkedList is a doubly linked list implementation of the List interface that also implements Queue and Deque.

---

## Q2. Which data structure is used internally?

### Answer

Doubly Linked List.

---

## Q3. Does LinkedList allow duplicate elements?

### Answer

Yes.

---

## Q4. Does LinkedList allow null values?

### Answer

Yes.

---

## Q5. Is LinkedList synchronized?

### Answer

No.

---

## Q6. Why is get(index) slower than ArrayList?

### Answer

Because LinkedList must traverse nodes sequentially until it reaches the requested index.

---

# Cross Questions

## Why is addFirst() O(1)?

### Answer

Only the head reference and a few node links are updated.

---

## Why is removeLast() O(1)?

### Answer

The LinkedList maintains a reference to the tail node.

---

## Why is LinkedList slower for searching?

### Answer

Because there is no direct indexing.

Each node must be visited one by one.

---

## Why does LinkedList consume more memory?

### Answer

Each node stores:

- Data
- Previous reference
- Next reference

These additional references increase memory usage.

---

## Why can LinkedList implement Queue and Deque?

### Answer

Because it supports efficient insertion and deletion from both ends using head and tail references.

---

# Tricky Interview Questions

## Question

Is insertion in LinkedList always O(1)?

### Answer

No.

Insertion is O(1) **only if you already have a reference to the target node** (or you're inserting at the head/tail).

Insertion by index is O(n) due to traversal.

---

## Question

Does LinkedList use a Singly or Doubly Linked List?

### Answer

Doubly Linked List.

---

## Question

Can we access the 500th element directly?

### Answer

No.

LinkedList must traverse from the beginning or end.

---

## Question

Can LinkedList replace ArrayList?

### Answer

No.

Choose based on the use case.

- Frequent reads → ArrayList
- Frequent insertions/deletions → LinkedList

---

# Best Practices

- Use LinkedList when insertions and deletions are frequent.
- Avoid LinkedList for heavy random-access operations.
- Program against the `List` interface when only list operations are required.

---

# Common Mistakes

- Assuming LinkedList provides O(1) access by index.
- Using LinkedList for applications dominated by random reads.
- Believing every insertion is O(1); traversal often dominates.

---

# Quick Revision

```text
Implements           → List, Queue, Deque

Internal Structure   → Doubly Linked List

Maintains Order      → Yes

Duplicates           → Yes

Null Values          → Yes

Random Access        → O(n)

addFirst()           → O(1)

addLast()            → O(1)

removeFirst()        → O(1)

removeLast()         → O(1)

Thread Safe          → No
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to know whether you understand the trade-offs between **ArrayList** and **LinkedList** rather than simply memorizing time complexities.

Typical follow-up questions include:

- Why is `get(index)` O(n)?
- Why is `addFirst()` O(1)?
- Why is LinkedList implemented as a doubly linked list?
- When would you choose LinkedList over ArrayList?
- Why does LinkedList consume more memory?

---

# One-Line Summary

LinkedList is a doubly linked list implementation of the List interface that provides efficient insertion and deletion operations while sacrificing random access performance.
