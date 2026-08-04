# Queue Interface in Java

---

# 1. What is Queue?

The **Queue** interface is a part of the Java Collections Framework that follows the **FIFO (First In, First Out)** principle.

This means the element inserted first is removed first.

The Queue interface extends the **Collection** interface and is mainly used when elements need to be processed in the order they arrive.

Java provides multiple Queue implementations such as:

- LinkedList
- PriorityQueue
- ArrayDeque
- ConcurrentLinkedQueue

Queue is commonly used in scheduling systems, message queues, task processing, and producer-consumer applications.

---

# Why was Queue Introduced?

Suppose customers are waiting at a billing counter.

```text
Rahul

Amit

Chetan
```

Rahul came first.

Rahul should be served first.

A Queue models this behavior.

Unlike Stack (LIFO), Queue follows FIFO.

---

# Why Do We Need Queue?

Whenever requests arrive one after another.

Examples:

- Printer Queue
- CPU Scheduling
- Ticket Booking
- Call Center
- Order Processing
- Message Queue

---

# Real Project Examples

## Printer Queue

```text
Print Job 1

↓

Print Job 2

↓

Print Job 3
```

Jobs are processed in arrival order.

---

## Food Delivery Orders

```text
Order 101

↓

Order 102

↓

Order 103
```

Orders are prepared in FIFO order.

---

## Customer Support

```text
Customer A

↓

Customer B

↓

Customer C
```

---

# Queue Hierarchy

```text
Iterable
      │
Collection
      │
Queue
├─────────────┬──────────────┐
│             │              │
LinkedList  PriorityQueue  Deque
                             │
                        ArrayDeque
```

---

# FIFO Principle

```text
Offer(A)

Offer(B)

Offer(C)

Queue

Front

A

B

C

Rear

poll()

↓

A Removed
```

---

# Important Methods

## Insert

```java
queue.offer("Java");
```

---

## Remove

```java
queue.poll();
```

---

## View Head

```java
queue.peek();
```

---

## Remove (Throws Exception)

```java
queue.remove();
```

---

## View Head (Throws Exception)

```java
queue.element();
```

---

# offer() vs add()

| Method | Queue Full |
|----------|------------|
| offer() | Returns false |
| add() | Throws Exception |

Interview Tip:

Prefer `offer()` because it avoids exceptions in capacity-restricted queues.

---

# poll() vs remove()

| Method | Empty Queue |
|----------|-------------|
| poll() | Returns null |
| remove() | Throws Exception |

---

# peek() vs element()

| Method | Empty Queue |
|----------|-------------|
| peek() | Returns null |
| element() | Throws Exception |

---

# Example

```java
Queue<String> queue =
        new LinkedList<>();

queue.offer("Java");
queue.offer("Spring");
queue.offer("Hibernate");

System.out.println(queue.poll());
```

Output

```text
Java
```

FIFO maintained.

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| offer() | O(1) |
| poll() | O(1) |
| peek() | O(1) |

(Depends on implementation, but these are typical for LinkedList and ArrayDeque.)

---

# Advantages

- FIFO processing
- Efficient insertion/removal at ends
- Ideal for scheduling
- Multiple implementations

---

# Disadvantages

- No index-based access
- Ordering rules depend on implementation
- Some implementations do not allow null

---

# Queue Implementations

| Class | Description |
|---------|-------------|
| LinkedList | General-purpose Queue |
| PriorityQueue | Priority-based Queue |
| ArrayDeque | Queue + Stack |
| ConcurrentLinkedQueue | Thread-safe Queue |

---

# Frequently Asked Interview Questions

## Q1. What is Queue?

Queue is an interface that follows the FIFO principle.

---

## Q2. Which interface does Queue extend?

```text
Collection
```

---

## Q3. Which classes implement Queue?

- LinkedList
- PriorityQueue
- ArrayDeque
- ConcurrentLinkedQueue

---

## Q4. Does Queue allow duplicate elements?

Yes.

Most Queue implementations allow duplicates.

---

## Q5. Does Queue allow null values?

It depends on the implementation.

For example:

- LinkedList → Yes
- PriorityQueue → No
- ArrayDeque → No

---

## Q6. Can we instantiate Queue?

No.

Queue is an interface.

Correct:

```java
Queue<Integer> queue =
        new LinkedList<>();
```

---

# Cross Questions

## Why is Queue an interface?

To allow multiple implementations optimized for different use cases.

---

## Difference between Queue and Stack?

| Queue | Stack |
|--------|-------|
| FIFO | LIFO |
| offer() | push() |
| poll() | pop() |
| peek() | peek() |

---

## Difference between Queue and Deque?

Queue inserts/removes at one end.

Deque allows insertion/removal at both ends.

---

## Why use offer() instead of add()?

Because `offer()` returns `false` instead of throwing an exception when insertion fails.

---

## Why use poll() instead of remove()?

Because `poll()` returns `null` instead of throwing an exception on an empty queue.

---

# Tricky Interview Questions

## Question

Can Queue be traversed?

Answer

Yes.

Using:

```java
for (String value : queue) {
    System.out.println(value);
}
```

---

## Question

Can Queue be sorted?

Not directly.

Use a `PriorityQueue` or copy elements into another collection and sort them.

---

## Question

Can Queue store duplicate elements?

Yes.

---

## Question

Can Queue store null?

Depends on the implementation.

---

# Best Practices

- Program to the Queue interface.

```java
Queue<String> queue =
        new LinkedList<>();
```

- Prefer `offer()`, `poll()`, and `peek()` over `add()`, `remove()`, and `element()` when possible.
- Choose the implementation based on your requirements (ordering, priority, concurrency).

---

# Common Mistakes

- Confusing FIFO with LIFO.
- Using `remove()` without checking if the queue is empty.
- Assuming all Queue implementations allow null values.
- Assuming Queue always maintains insertion order (PriorityQueue is an exception).

---

# Quick Revision

```text
Type                → Interface

Parent              → Collection

Principle           → FIFO

Duplicates          → Allowed

Null                → Depends on Implementation

Common Methods      → offer()
                      poll()
                      peek()

Implementations     → LinkedList
                      PriorityQueue
                      ArrayDeque
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to verify that you understand the Queue contract before discussing specific implementations.

Typical follow-up questions:

- Difference between Queue and Deque?
- offer() vs add()?
- poll() vs remove()?
- peek() vs element()?
- Which Queue implementation would you choose for a scheduler?

---

# One-Line Summary

**Queue is an interface that follows the FIFO principle and is used to process elements in the order they arrive, with implementations such as LinkedList, PriorityQueue, ArrayDeque, and ConcurrentLinkedQueue.**
