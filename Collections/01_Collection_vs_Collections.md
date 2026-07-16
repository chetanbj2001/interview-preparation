# Collection vs Collections in Java

---

# 1. What is the difference between Collection and Collections?

One of the most common interview questions in Java is the difference between **Collection** and **Collections**.

Although their names are similar, they are completely different.

- **Collection** is an **interface** that represents a group of objects.
- **Collections** is a **utility class** that provides static methods to perform operations on collections.

In simple words,

> Collection defines **what a collection can do**, whereas Collections provides **ready-made utility methods to work with collections**.

---

# Why was Collection Interface Introduced?

The Collection interface provides a common contract for all collection classes.

Instead of learning different APIs for different classes, Java provides a common interface.

For example,

```java
List
Set
Queue
```

all implement the Collection interface.

---

# Why was Collections Class Introduced?

Imagine every developer writing their own sorting logic.

Instead, Java provides utility methods like

```java
Collections.sort()

Collections.reverse()

Collections.shuffle()

Collections.max()

Collections.min()

Collections.binarySearch()
```

This avoids duplicate code and improves reusability.

---

# Collection Interface

Package

```java
java.util
```

Declaration

```java
public interface Collection<E>
        extends Iterable<E>
```

It is the root interface for

- List
- Set
- Queue

Map is NOT part of this hierarchy.

---

# Common Methods of Collection Interface

```java
add()

remove()

contains()

size()

isEmpty()

iterator()

clear()
```

Example

```java
Collection<String> names = new ArrayList<>();

names.add("Chetan");
names.add("Rahul");

System.out.println(names.size());
```

Output

```text
2
```

---

# Collections Utility Class

Package

```java
java.util
```

Declaration

```java
public class Collections
```

Notice

It is a **class**, not an interface.

All methods are

```java
static
```

Example

```java
Collections.sort(list);
```

No object creation is required.

---

# Common Methods of Collections Class

Sorting

```java
Collections.sort(list);
```

Reverse

```java
Collections.reverse(list);
```

Shuffle

```java
Collections.shuffle(list);
```

Maximum

```java
Collections.max(list);
```

Minimum

```java
Collections.min(list);
```

Binary Search

```java
Collections.binarySearch(list, value);
```

Frequency

```java
Collections.frequency(list, element);
```

Swap

```java
Collections.swap(list, 0, 2);
```

---

# Example

```java
List<Integer> numbers = new ArrayList<>();

numbers.add(40);
numbers.add(10);
numbers.add(30);
numbers.add(20);

Collections.sort(numbers);

System.out.println(numbers);
```

Output

```text
[10, 20, 30, 40]
```

---

# Internal Working

## Collection

```text
Interface

↓

Implemented By

↓

ArrayList

LinkedList

HashSet

TreeSet

PriorityQueue
```

---

## Collections

```text
Utility Class

↓

Contains Static Methods

↓

sort()

reverse()

shuffle()

max()

min()

frequency()

swap()
```

---

# Comparison Table

| Feature | Collection | Collections |
|----------|------------|-------------|
| Type | Interface | Utility Class |
| Package | java.util | java.util |
| Introduced | JDK 1.2 | JDK 1.2 |
| Purpose | Store Objects | Utility Operations |
| Object Creation | No | No |
| Methods | Non-static | Static |
| Extended By | List, Set, Queue | Cannot be Extended for Collection hierarchy |
| Example | List | Collections.sort() |

---

# Real Project Example

Suppose we have a list of employees.

```java
List<Employee> employees =
        new ArrayList<>();
```

Store employees using

```java
Collection
```

Sort employees using

```java
Collections.sort(employees);
```

Here,

Collection stores data.

Collections performs operations.

---

# Advantages

## Collection

- Common interface
- Code reusability
- Dynamic data storage

## Collections

- Ready-made algorithms
- Better performance
- Cleaner code
- Less development effort

---

# Disadvantages

## Collection

Cannot be instantiated directly.

Example

```java
Collection list =
        new Collection();
```

Compilation Error.

---

## Collections

Only utility methods.

Cannot store data.

---

# Frequently Asked Interview Questions

## Q1. What is Collection?

### Answer

Collection is the root interface of the Java Collections Framework used to represent a group of objects.

---

## Q2. What is Collections?

### Answer

Collections is a utility class containing static methods to perform operations on collections.

---

## Q3. Is Collection a class?

### Answer

No.

It is an interface.

---

## Q4. Is Collections an interface?

### Answer

No.

It is a final utility class with static methods.

---

## Q5. Which one stores data?

### Answer

Collection.

---

## Q6. Which one performs sorting?

### Answer

Collections.

Example

```java
Collections.sort(list);
```

---

# Cross Questions

## Can we create an object of Collection?

### Answer

No.

It is an interface.

---

## Can we create an object of Collections?

### Answer

No.

All methods are static.

No object creation is required.

---

## Which interface does ArrayList implement?

```text
ArrayList

↓

List

↓

Collection

↓

Iterable
```

---

## Does Map extend Collection?

### Answer

No.

Map has a separate hierarchy.

---

## Which one provides iterator()?

### Answer

Collection

because it extends

```java
Iterable
```

---

# Tricky Interview Questions

## Question

Which one is inherited?

### Answer

Collection.

Because it is an interface.

Collections is just a utility class.

---

## Question

Can Collections store objects?

### Answer

No.

It only provides utility methods.

---

## Question

Can Collection sort data?

### Answer

No.

Sorting is provided by

```java
Collections.sort()
```

---

## Question

Which one contains static methods?

### Answer

Collections.

---

# Best Practices

- Program against the Collection interface whenever possible.

Example

```java
List<String> list =
        new ArrayList<>();
```

- Use Collections utility methods instead of writing custom sorting logic.
- Use Generics to ensure type safety.

---

# Common Mistakes

- Confusing Collection with Collections.
- Thinking Collections stores data.
- Assuming Collection is a class.
- Assuming Map extends Collection.

---

# Quick Revision

```text
Collection

Interface

Stores Objects

Parent of List, Set, Queue

------------------------------------

Collections

Utility Class

Static Methods

sort()

reverse()

shuffle()

max()

min()
```

---

# One-Line Summary

Collection is an interface used to store groups of objects, whereas Collections is a utility class that provides static methods to manipulate and process those collections.
