# Deque Interface in Java

---

# 1. What is Deque?


The **Deque (Double Ended Queue)** interface is a part of the Java Collections Framework that allows insertion and deletion of elements from **both the front and the rear**.

Deque extends the **Queue** interface.

Unlike Queue, which follows only **FIFO**, Deque supports both:

- FIFO (Queue)
- LIFO (Stack)

This makes Deque more flexible than both Queue and Stack.

Java provides two major implementations:

- ArrayDeque
- LinkedList

---

# Why was Deque Introduced?

A normal Queue allows insertion at one end and removal from the other.

```text
Insert  → Rear

Remove → Front
```

Sometimes applications require insertion and deletion from **both ends**.

Examples:

- Browser History
- Undo/Redo
- Sliding Window Algorithm
- Task Scheduling

Deque was introduced to solve these problems.

---

# Why Do We Need Deque?

Suppose we have a train.

```text
Engine

Coach1

Coach2

Coach3
```

Sometimes we add a coach at the front.

Sometimes at the rear.

Queue cannot do this efficiently.

Deque can.

---

# Real Project Examples

## Browser History

```text
Back

↓

Current Page

↓

Forward
```

---

## Undo / Redo

```text
Undo

↓

Current

↓

Redo
```

---

## Sliding Window Algorithm

Frequently used in:

- Maximum in Sliding Window
- Minimum in Sliding Window
- Monotonic Queue

---

## Task Scheduler

High-priority tasks can be inserted at the front.

Normal tasks at the rear.

---

# Deque Hierarchy

```text
Iterable
      │
Collection
      │
Queue
      │
Deque
   ┌───────┐
   │       │
LinkedList ArrayDeque
```

---

# Internal Working

Deque is an **interface**.

Its internal working depends on the implementation.

Examples:

```text
ArrayDeque

↓

Resizable Circular Array
```

```text
LinkedList

↓

Doubly Linked List
```

We will study ArrayDeque internals in the next chapter.

---

# FIFO and LIFO

## Queue Mode (FIFO)

```text
offerLast(A)

offerLast(B)

offerLast(C)

↓

pollFirst()

↓

A Removed
```

---

## Stack Mode (LIFO)

```text
push(A)

push(B)

push(C)

↓

pop()

↓

C Removed
```

Same interface.

Different behavior.

---

# Important Methods

## Insert at Front

```java
deque.addFirst(10);
```

---

## Insert at Rear

```java
deque.addLast(20);
```

---

## Remove First

```java
deque.removeFirst();
```

---

## Remove Last

```java
deque.removeLast();
```

---

## Peek First

```java
deque.peekFirst();
```

---

## Peek Last

```java
deque.peekLast();
```

---

## Offer First

```java
deque.offerFirst(10);
```

---

## Offer Last

```java
deque.offerLast(20);
```

---

# Example

```java
Deque<Integer> deque =
        new ArrayDeque<>();

deque.addFirst(20);
deque.addLast(30);
deque.addFirst(10);

System.out.println(deque);
```

Output

```text
[10,20,30]
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| addFirst() | O(1) |
| addLast() | O(1) |
| removeFirst() | O(1) |
| removeLast() | O(1) |
| peekFirst() | O(1) |
| peekLast() | O(1) |

---

# Advantages

- Supports FIFO and LIFO
- Insertion/removal from both ends
- Flexible interface
- Better replacement for Stack (when using ArrayDeque)

---

# Disadvantages

- No index-based access
- Behavior depends on implementation
- Some implementations do not allow null elements

---

# Queue vs Deque

| Feature | Queue | Deque |
|----------|-------|-------|
| Insert | Rear | Front & Rear |
| Remove | Front | Front & Rear |
| FIFO | Yes | Yes |
| LIFO | No | Yes |

---

# Stack vs Deque

| Feature | Stack | Deque |
|----------|--------|-------|
| LIFO | Yes | Yes |
| Legacy Class | Yes | No |
| Recommended | No | Yes (ArrayDeque) |

---

# Frequently Asked Interview Questions

## Q1. What is Deque?

Deque is a double-ended queue that supports insertion and deletion at both ends.

---

## Q2. Which interface does Deque extend?

Queue.

---

## Q3. Can Deque work as a Stack?

Yes.

Using:

```java
push()

pop()

peek()
```

---

## Q4. Can Deque work as a Queue?

Yes.

Using:

```java
offerLast()

pollFirst()
```

---

## Q5. Which classes implement Deque?

- ArrayDeque
- LinkedList

---

# Cross Questions

## Why was Deque introduced?

To support efficient insertion and removal from both ends.

---

## Why is Deque better than Stack?

- Stack is a legacy class.
- ArrayDeque is faster.
- No synchronization overhead.
- Better performance.

---

## Can Deque replace Queue?

Yes.

Deque supports all Queue operations and provides additional functionality.

---

## Can Deque replace Stack?

Yes.

In modern Java, ArrayDeque is recommended instead of Stack.

---

# Tricky Interview Questions

## Question

Can Deque behave as both Queue and Stack?

Answer

Yes.

Queue:

```java
offerLast()

pollFirst()
```

Stack:

```java
push()

pop()
```

---

## Question

Does Deque allow duplicates?

Yes.

---

## Question

Can Deque store null values?

Depends on the implementation.

For example:

- ArrayDeque → No
- LinkedList → Yes

---

## Question

Can we access the middle element?

No.

Deque does not provide index-based access.

---

# Best Practices

- Use the `Deque` interface instead of concrete implementations.

```java
Deque<Integer> deque = new ArrayDeque<>();
```

- Prefer `ArrayDeque` over `Stack` for LIFO operations.
- Choose `LinkedList` only when linked-node behavior is specifically required.

---

# Common Mistakes

- Confusing Deque with Queue.
- Assuming Deque only works as a Queue.
- Using Stack instead of ArrayDeque in new code.
- Expecting index-based access.

---

# Quick Revision

```text
Type                → Interface

Parent              → Queue

Behavior            → FIFO + LIFO

Insert              → Both Ends

Remove              → Both Ends

Implementations     → ArrayDeque
                      LinkedList

Recommended         → ArrayDeque
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to know whether you understand that Deque is more flexible than Queue and Stack.

Common follow-up questions:

- Queue vs Deque?
- Stack vs Deque?
- Why is ArrayDeque preferred over Stack?
- Which implementations support Deque?
- Can Deque work as both Queue and Stack?

---

# One-Line Summary

**Deque is a double-ended queue interface that supports insertion and deletion at both ends, allowing it to function efficiently as both a Queue (FIFO) and a Stack (LIFO).**
