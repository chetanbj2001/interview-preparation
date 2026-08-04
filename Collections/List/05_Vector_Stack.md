# Vector and Stack in Java

---

# 1. What is Vector?

Vector is a legacy class in the Java Collections Framework that implements the **List** interface.

Like ArrayList, Vector stores elements in a **dynamic array**.

The major difference is that **Vector is synchronized**, making it thread-safe.

Because synchronization introduces additional overhead, Vector is generally slower than ArrayList in single-threaded applications.

Today, Vector is considered a **legacy class**, and ArrayList is preferred unless thread safety is specifically required.

---

# Why was Vector Introduced?

Before the Java Collections Framework (JDK 1.2), Java needed a dynamic array implementation.

Vector provided:

- Dynamic resizing
- Indexed access
- Thread safety

Later, ArrayList was introduced to provide better performance without synchronization overhead.

---

# Internal Working

Internally, Vector uses:

```java
Object[] elementData;
```

It automatically resizes when capacity is full.

Unlike ArrayList, every public method is synchronized.

Example

```java
public synchronized boolean add(E e)
```

Only one thread can execute synchronized methods at a time.

---

# Important Methods

```java
add()

get()

set()

remove()

size()

capacity()

firstElement()

lastElement()
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| add() | O(1) Amortized |
| get() | O(1) |
| remove(index) | O(n) |
| contains() | O(n) |

---

# Advantages

- Thread-safe
- Dynamic resizing
- Maintains insertion order
- Allows duplicates
- Allows null values

---

# Disadvantages

- Slower than ArrayList
- Synchronization overhead
- Legacy class

---

# Vector vs ArrayList

| Feature | Vector | ArrayList |
|---------|---------|-----------|
| Thread Safe | Yes | No |
| Synchronized | Yes | No |
| Performance | Slower | Faster |
| Legacy | Yes | No |
| Internal Structure | Dynamic Array | Dynamic Array |

---

# Frequently Asked Interview Questions

## Q1. What is Vector?

Vector is a synchronized dynamic array implementation of the List interface.

---

## Q2. Is Vector thread-safe?

Yes.

All major methods are synchronized.

---

## Q3. Why is Vector slower?

Because synchronization adds locking overhead.

---

## Q4. Should we use Vector today?

Generally, no.

Prefer ArrayList unless legacy code or specific requirements dictate otherwise.

---

# What is Stack?

## Interview Answer (1-2 Minutes)

Stack is a legacy class that extends **Vector** and follows the **LIFO (Last In, First Out)** principle.

The last element inserted is the first one removed.

Example

```text
Push

10

20

30

Pop

30
```

---

# Why was Stack Introduced?

Many applications require processing the most recently added element first.

Examples:

- Undo operation
- Browser Back
- Function Call Stack
- Expression Evaluation

---

# Internal Working

```text
Vector

↓

Stack
```

Stack inherits all Vector methods.

It adds stack-specific methods like:

```java
push()

pop()

peek()

empty()

search()
```

---

# Example

```java
Stack<String> stack = new Stack<>();

stack.push("Java");
stack.push("Spring");
stack.push("Boot");

System.out.println(stack.pop());
```

Output

```text
Boot
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| push() | O(1) |
| pop() | O(1) |
| peek() | O(1) |
| search() | O(n) |

---

# Real Project Example

## Undo Feature

```text
Write A

↓

Write B

↓

Write C

Undo

↓

Write B
```

---

## Browser History

```text
Google

↓

GitHub

↓

ChatGPT

Back

↓

GitHub
```

---

# Stack vs Queue

| Stack | Queue |
|--------|-------|
| LIFO | FIFO |
| push() | offer() |
| pop() | poll() |
| peek() | peek() |

---

# Stack vs Deque

| Stack | ArrayDeque |
|--------|------------|
| Legacy | Modern |
| Slower | Faster |
| Extends Vector | Implements Deque |
| Synchronized | Not synchronized |

**Interview Tip:** For new applications, prefer **ArrayDeque** instead of **Stack** when you need stack behavior.

---

# Frequently Asked Interview Questions

## Q1. What is Stack?

A legacy LIFO data structure that extends Vector.

---

## Q2. Which class does Stack extend?

```text
Vector
```

---

## Q3. What principle does Stack follow?

```text
LIFO
```

---

## Q4. Is Stack synchronized?

Yes.

Because it extends Vector.

---

## Q5. Why is Stack considered legacy?

Because modern Java recommends using **Deque (ArrayDeque)** for stack operations.

---

# Cross Questions

## Why is Stack slower?

Because it inherits synchronized methods from Vector.

---

## Why does Stack extend Vector?

Historically, Stack was built by adding LIFO operations on top of Vector's dynamic array implementation.

---

## What is the modern replacement for Stack?

```java
Deque<Integer> stack = new ArrayDeque<>();
```

---

## Does Stack allow duplicate elements?

Yes.

---

## Does Stack allow null values?

Yes, but avoid storing null values in stack-based logic because they can complicate code and some modern alternatives (like `ArrayDeque`) do not permit nulls.

---

# Tricky Interview Questions

## Question

Is Stack part of the Java Collections Framework?

Yes.

---

## Question

Can we use ArrayList as a Stack?

Technically yes, by adding and removing from the end.

However, Java provides better alternatives such as `Deque`.

---

## Question

Which is better for implementing a Stack?

```text
ArrayDeque
```

---

## Question

Why do interviewers rarely recommend Stack?

Because it is a legacy class.

---

# Best Practices

- Prefer `ArrayList` over `Vector` unless synchronization is required.
- Prefer `ArrayDeque` over `Stack` for new development.
- Use the interface (`Deque`) where appropriate instead of concrete implementations.

---

# Common Mistakes

- Assuming Vector and ArrayList have the same performance.
- Using Stack in new projects without considering `ArrayDeque`.
- Believing synchronization is always beneficial.

---

# Quick Revision

```text
Vector

Implements        → List

Internal Structure → Dynamic Array

Thread Safe       → Yes

Legacy            → Yes

----------------------------

Stack

Extends           → Vector

Principle         → LIFO

Thread Safe       → Yes

Modern Alternative → ArrayDeque
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to see if you understand the evolution of Java Collections.

Typical follow-up questions include:

- Why is Vector slower than ArrayList?
- Is Stack still recommended?
- What would you use instead of Stack?
- Why is ArrayDeque preferred?

---

# One-Line Summary

**Vector is a synchronized dynamic array implementation of List, while Stack is a legacy LIFO class built on top of Vector. In modern Java, ArrayList is preferred over Vector, and ArrayDeque is preferred over Stack.**
