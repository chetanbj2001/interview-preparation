# List Interface in Java

---

# 1. What is List in Java?

The **List** interface is a part of the Java Collections Framework that represents an **ordered collection of elements**.

It allows:

- Duplicate elements
- Null values
- Insertion order to be maintained
- Elements to be accessed using an index

The List interface extends the **Collection** interface and is implemented by classes such as:

- ArrayList
- LinkedList
- Vector
- Stack

List is used when the order of elements is important and duplicate values are allowed.

---

# Why was List Introduced?

Before the List interface, there was no standard way to store an ordered collection.

Developers needed a common interface that could:

- Preserve insertion order
- Allow duplicate elements
- Support indexed access

The List interface provides this standard contract.

---

# Why Do We Need List?

Suppose we are developing an E-Commerce application.

Customer adds products to a cart.

Example:

```text
Laptop

Mouse

Keyboard

Mouse
```

Here,

- Order matters.
- Duplicate products are allowed.

List is the perfect choice.

---

# Real Project Examples

## Shopping Cart

```text
Laptop

Mouse

Keyboard
```

Order matters.

---

## Chat Messages

```text
Hi

Hello

How are you?
```

Messages should appear in the order they were sent.

---

## Playlist

```text
Song A

Song B

Song C
```

Songs should play in order.

---

# List Hierarchy

```text
Iterable
    │
Collection
    │
   List
 ┌──┼─────────────┐
 │  │             │
ArrayList   LinkedList
 │
Vector
 │
Stack
```

---

# Characteristics of List

- Ordered collection
- Maintains insertion order
- Allows duplicate elements
- Allows multiple null values
- Supports index-based access
- Dynamic in size

---

# Internal Working

The List interface itself does not store data.

It only defines a contract.

Actual storage depends on its implementation.

Example:

```text
ArrayList

↓

Dynamic Array
```

```text
LinkedList

↓

Doubly Linked List
```

```text
Vector

↓

Synchronized Dynamic Array
```

---

# Common Methods

## Add Element

```java
list.add("Java");
```

---

## Insert at Index

```java
list.add(1, "Spring");
```

---

## Get Element

```java
list.get(0);
```

---

## Update Element

```java
list.set(0, "Spring Boot");
```

---

## Remove Element

```java
list.remove(0);
```

---

## Find Size

```java
list.size();
```

---

## Check Element

```java
list.contains("Java");
```

---

## Clear List

```java
list.clear();
```

---

# Example

```java
import java.util.*;

public class Test {

    public static void main(String[] args) {

        List<String> courses = new ArrayList<>();

        courses.add("Java");
        courses.add("Spring");
        courses.add("Java");
        courses.add(null);

        System.out.println(courses);
    }
}
```

Output

```text
[Java, Spring, Java, null]
```

Observation:

- Duplicate values allowed
- Null allowed
- Order maintained

---

# Time Complexity (General)

| Operation | Complexity |
|------------|-----------|
| Add | Depends on implementation |
| Remove | Depends on implementation |
| Search | O(n) |
| Get | Depends on implementation |

> **Note:** The exact complexity depends on whether you're using `ArrayList`, `LinkedList`, or another implementation. We'll cover those in their respective files.

---

# Advantages

- Ordered collection
- Supports duplicates
- Easy traversal
- Index-based operations
- Dynamic size

---

# Disadvantages

- Cannot store primitive types directly
- Performance depends on implementation
- Choosing the wrong implementation can affect application performance

---

# List Implementations

| Class | Description |
|---------|-------------|
| ArrayList | Dynamic Array |
| LinkedList | Doubly Linked List |
| Vector | Thread-safe Dynamic Array |
| Stack | LIFO Stack (Legacy) |

---

# List vs Set

| List | Set |
|------|-----|
| Ordered | Unordered (implementation-dependent) |
| Duplicates Allowed | Duplicates Not Allowed |
| Index Available | No Index |
| Multiple Nulls | Depends on implementation (HashSet allows one null) |

---

# Frequently Asked Interview Questions

## Q1. What is List?

### Answer

List is an ordered collection that allows duplicate elements and supports index-based access.

---

## Q2. Which interface does List extend?

### Answer

```text
Iterable

↓

Collection

↓

List
```

---

## Q3. Does List maintain insertion order?

### Answer

Yes.

---

## Q4. Does List allow duplicate elements?

### Answer

Yes.

---

## Q5. Can List store null values?

### Answer

Yes.

Most List implementations allow multiple null values.

---

## Q6. Can we instantiate List?

### Answer

No.

It is an interface.

Correct way:

```java
List<String> list = new ArrayList<>();
```

---

# Cross Questions

## Why is List an interface?

### Answer

Because Java provides multiple implementations like:

- ArrayList
- LinkedList
- Vector

The interface defines the contract, while implementations provide different internal data structures.

---

## Why use List instead of Array?

### Answer

Arrays have a fixed size.

List grows dynamically and provides many utility methods.

---

## Can List store primitive data types?

### Answer

No.

Use Wrapper classes.

Example:

```java
List<Integer> numbers = new ArrayList<>();
```

---

## Is List synchronized?

### Answer

No.

However, `Vector` is synchronized.

---

## Which implementation should I choose?

### Answer

- Frequent random access → ArrayList
- Frequent insertions/deletions → LinkedList
- Thread-safe legacy requirement → Vector

---

# Tricky Interview Questions

## Question

Can List contain duplicate null values?

### Answer

Yes.

Example:

```java
List<String> list = new ArrayList<>();

list.add(null);
list.add(null);
```

Valid.

---

## Question

Can List contain primitive types?

### Answer

No.

Collections work with objects.

Use wrapper classes.

---

## Question

Can we create an object of List?

### Answer

No.

List is an interface.

---

## Question

Is List ordered or sorted?

### Answer

Ordered.

It maintains insertion order.

It does **not** automatically sort elements.

---

# Best Practices

- Program to the interface.

Good

```java
List<String> list = new ArrayList<>();
```

Bad

```java
ArrayList<String> list = new ArrayList<>();
```

- Choose the implementation based on your use case.
- Use Generics for type safety.

---

# Common Mistakes

- Confusing ordered with sorted.
- Assuming all List implementations have the same performance.
- Using ArrayList for heavy insert/delete operations without considering LinkedList.
- Instantiating implementation types directly in variable declarations when the interface is sufficient.

---

# Quick Revision

```text
Package               → java.util

Type                  → Interface

Parent                → Collection

Maintains Order       → Yes

Duplicates            → Yes

Null Values           → Yes

Index Based           → Yes

Dynamic Size          → Yes

Implementations       → ArrayList, LinkedList, Vector, Stack
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to check whether you understand the **contract (List)** separately from its **implementations (ArrayList, LinkedList, etc.)**.

A common follow-up is:

> "If List is only an interface, where is the actual data stored?"

Correct answer:

The data is stored by the implementation class (such as `ArrayList` using a dynamic array or `LinkedList` using a doubly linked list), not by the `List` interface itself.

---

# One-Line Summary

The **List** interface represents an ordered, dynamic collection that allows duplicate and null elements, supports index-based access, and is implemented by classes such as ArrayList, LinkedList, Vector, and Stack.
