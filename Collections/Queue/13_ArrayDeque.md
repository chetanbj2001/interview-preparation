# ArrayDeque in Java

---

# 1. What is ArrayDeque?

ArrayDeque is a resizable-array implementation of the **Deque** interface introduced in Java 6.

It supports insertion and deletion from **both ends** of the collection with **O(1) amortized time complexity**.

Unlike Stack, ArrayDeque is **not synchronized**, making it faster in single-threaded applications.

Unlike LinkedList, ArrayDeque stores elements in a **contiguous array**, providing better cache locality and performance.

ArrayDeque can be used as both:

- Queue (FIFO)
- Stack (LIFO)

It does **not allow null elements**.

---

# Why was ArrayDeque Introduced?

Before Java 6,

developers mainly used

```java
Stack
```

or

```java
LinkedList
```

Problems:

### Stack

- Legacy Class
- Synchronized
- Slower

### LinkedList

- Extra Node Objects
- Higher Memory Usage
- Poor Cache Performance

ArrayDeque provides

- Faster Operations
- Better Memory Usage
- Modern API
- Queue + Stack Support

---

# Why Do We Need ArrayDeque?

Suppose an application performs

- Undo/Redo
- Browser History
- BFS Traversal
- DFS Traversal
- Sliding Window
- Expression Evaluation

ArrayDeque is usually the best choice.

---

# Internal Working ⭐⭐⭐⭐⭐

This is one of the most important interview questions.

ArrayDeque is implemented using a

```text
Resizable Circular Array
```

NOT

- Linked List
- Hash Table
- Tree
- Stack

Internally

```text
ArrayDeque

↓

Object[]

↓

Circular Array
```

---

# Internal Architecture

```text
          Object[]

+----+----+----+----+----+----+----+----+
|    | 10 | 20 | 30 |    |    |    |    |
+----+----+----+----+----+----+----+----+
       ↑              ↑
     Head           Tail
```

Unlike a normal array,

Head and Tail move in a circular manner.

---

# Circular Array Concept

Suppose

Capacity = 8

```
Index

0 1 2 3 4 5 6 7
```

Head reaches last index.

Instead of resizing immediately,

it wraps around.

```
7

↓

0
```

This is called

```text
Circular Buffer
```

---

# Why Circular Array?

Without a circular array,

after removing elements from the front,

unused space appears.

Example

```
10

20

30

40
```

Remove

```
10

20
```

Normal Array

```
_ _ 30 40
```

Space wasted.

Circular Array reuses free positions.

---

# Head and Tail

ArrayDeque maintains

```text
Head

↓

First Element
```

```text
Tail

↓

Next Insertion Position
```

Example

```
Head

↓

10

20

30

↑

Tail
```

---

# Dynamic Resizing

Suppose capacity

```
8
```

Array becomes full.

Java creates

```
16
```

Then copies elements.

Capacity doubles automatically.

---

# How addFirst() Works

```
Head--

↓

Store Element

↓

Update Head
```

---

# How addLast() Works

```
Store at Tail

↓

Tail++

↓

Done
```

---

# How removeFirst() Works

```
Read Head

↓

Return Element

↓

Head++
```

---

# How removeLast() Works

```
Tail--

↓

Return Element
```

---

# Constructors

Default

```java
Deque<Integer> deque =
        new ArrayDeque<>();
```

Capacity

```java
Deque<Integer> deque =
        new ArrayDeque<>(20);
```

Collection Constructor

```java
Deque<Integer> deque =
        new ArrayDeque<>(list);
```

---

# Important Methods

Insert Front

```java
addFirst()
```

Insert Rear

```java
addLast()
```

Remove Front

```java
removeFirst()
```

Remove Rear

```java
removeLast()
```

Offer Front

```java
offerFirst()
```

Offer Rear

```java
offerLast()
```

Peek Front

```java
peekFirst()
```

Peek Rear

```java
peekLast()
```

Push

```java
push()
```

Pop

```java
pop()
```

---

# Example as Queue

```java
Deque<Integer> queue =
        new ArrayDeque<>();

queue.offerLast(10);
queue.offerLast(20);
queue.offerLast(30);

System.out.println(queue.pollFirst());
```

Output

```
10
```

---

# Example as Stack

```java
Deque<Integer> stack =
        new ArrayDeque<>();

stack.push(10);
stack.push(20);
stack.push(30);

System.out.println(stack.pop());
```

Output

```
30
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| addFirst() | O(1) Amortized |
| addLast() | O(1) Amortized |
| removeFirst() | O(1) |
| removeLast() | O(1) |
| peekFirst() | O(1) |
| peekLast() | O(1) |

---

# Advantages

- Faster than Stack
- Faster than LinkedList for most Queue operations
- Less Memory
- Better Cache Performance
- Queue + Stack
- Dynamic Resizing

---

# Disadvantages

- No Random Access
- No Null Elements
- Not Thread Safe

---

# ArrayDeque vs Stack

| Feature | ArrayDeque | Stack |
|----------|------------|-------|
| Synchronization | No | Yes |
| Performance | Faster | Slower |
| Legacy | No | Yes |
| Recommended | Yes | No |

---

# ArrayDeque vs LinkedList

| Feature | ArrayDeque | LinkedList |
|----------|------------|------------|
| Internal Structure | Circular Array | Doubly Linked List |
| Memory | Less | More |
| Cache Performance | Better | Poor |
| Queue Performance | Better | Good |

---

# Frequently Asked Interview Questions

## Q1. What is ArrayDeque?

ArrayDeque is a resizable circular-array implementation of the Deque interface.

---

## Q2. Which data structure does ArrayDeque use internally?

Resizable Circular Array.

---

## Q3. Does ArrayDeque allow null elements?

No.

Adding null throws a NullPointerException.

---

## Q4. Is ArrayDeque synchronized?

No.

---

## Q5. Can ArrayDeque be used as a Stack?

Yes.

Using:

```java
push()

pop()
```

---

## Q6. Can ArrayDeque be used as a Queue?

Yes.

Using:

```java
offerLast()

pollFirst()
```

---

# Cross Questions

## Why is ArrayDeque faster than Stack?

Because Stack extends Vector, which synchronizes every operation.

ArrayDeque avoids synchronization overhead.

---

## Why is ArrayDeque faster than LinkedList?

Because elements are stored in a contiguous array, improving CPU cache locality and reducing object allocation.

---

## Why doesn't ArrayDeque allow null?

Methods like `pollFirst()` return `null` when the deque is empty. Allowing `null` elements would make it impossible to distinguish between an empty deque and a stored `null` value.

---

## Why is resizing called amortized O(1)?

Most insertions take constant time. Occasionally, resizing requires copying all elements (O(n)), but spread over many insertions, the average cost per insertion remains O(1).

---

# Tricky Interview Questions

## Question

Can ArrayDeque replace Stack?

Answer

Yes.

It is the recommended replacement in modern Java.

---

## Question

Can ArrayDeque replace Queue?

Yes.

It fully implements the Deque interface, which extends Queue.

---

## Question

Does ArrayDeque maintain insertion order?

Yes.

Elements are processed according to how you use it:

- FIFO when used as a Queue
- LIFO when used as a Stack

---

## Question

Can ArrayDeque grow automatically?

Yes.

It resizes when its internal array becomes full.

---

# Best Practices

- Use `Deque` as the reference type.

```java
Deque<Integer> deque = new ArrayDeque<>();
```

- Prefer ArrayDeque over Stack for LIFO operations.
- Prefer ArrayDeque over LinkedList for general Queue/Deque operations unless linked-node behavior is specifically needed.

---

# Common Mistakes

- Assuming ArrayDeque is backed by a LinkedList.
- Trying to store `null`.
- Using Stack in new code instead of ArrayDeque.
- Assuming it provides random access like an ArrayList.

---

# Quick Revision

```text
Implements          → Deque

Internal Structure  → Resizable Circular Array

Null                → Not Allowed

Thread Safe         → No

Queue               → Yes

Stack               → Yes

Resizing            → Automatic

Performance         → Better than Stack
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to see whether you understand why ArrayDeque is the recommended replacement for Stack and a better general-purpose queue than LinkedList in many scenarios.

Typical follow-up questions:

- Why is ArrayDeque faster than Stack?
- Why is it faster than LinkedList?
- How does the circular array work?
- Why doesn't it allow null?
- What does amortized O(1) mean?
- When would you choose ArrayDeque over LinkedList?

---

# One-Line Summary

**ArrayDeque is a high-performance implementation of the Deque interface backed by a resizable circular array, supporting efficient FIFO and LIFO operations with O(1) amortized performance.**
