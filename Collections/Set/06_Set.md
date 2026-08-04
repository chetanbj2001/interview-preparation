# Set Interface in Java

---

# 1. What is Set?

The **Set** interface is a part of the Java Collections Framework that represents a collection of **unique elements**.

Unlike List, Set does **not allow duplicate elements**.

The Set interface extends the **Collection** interface and provides different implementations such as:

- HashSet
- LinkedHashSet
- TreeSet

Set is mainly used when uniqueness of data is important.

Examples include:

- User IDs
- Email IDs
- Employee IDs
- Aadhaar Numbers
- Product Codes

---

# Why was Set Introduced?

Imagine storing employee IDs.

```text
101

102

103

101
```

The duplicate employee ID is invalid.

Using List would allow duplicates.

To solve this problem, Java introduced the Set interface.

It automatically prevents duplicate elements.

---

# Why Do We Need Set?

Suppose we are building a Registration System.

Users register using their email.

```text
abc@gmail.com

xyz@gmail.com

abc@gmail.com
```

The same email should not exist twice.

Using Set ensures uniqueness.

---

# Real Project Examples

## User Registration

```text
abc@gmail.com

xyz@gmail.com

abc@gmail.com ❌
```

---

## Employee IDs

```text
101

102

103

101 ❌
```

---

## Product Codes

Every product code must be unique.

---

# Set Hierarchy

```text
Iterable
      │
Collection
      │
     Set
 ┌────┼───────────────┐
 │    │               │
HashSet LinkedHashSet TreeSet
```

---

# Characteristics of Set

- Stores unique elements
- No duplicate values
- Does not support index-based access
- Dynamic in size
- Allows null values depending on implementation
- Different implementations have different ordering behavior

---

# Internal Working

Set is only an interface.

It defines rules.

Actual storage depends on the implementation.

Example

```text
HashSet

↓

HashMap
```

```text
TreeSet

↓

TreeMap
```

```text
LinkedHashSet

↓

LinkedHashMap
```

We'll study each implementation separately.

---

# Common Methods

## Add

```java
set.add("Java");
```

---

## Remove

```java
set.remove("Java");
```

---

## Contains

```java
set.contains("Java");
```

---

## Size

```java
set.size();
```

---

## Clear

```java
set.clear();
```

---

## Iterate

```java
for(String language : set){
    System.out.println(language);
}
```

---

# Example

```java
import java.util.*;

public class Demo {

    public static void main(String[] args) {

        Set<String> languages = new HashSet<>();

        languages.add("Java");
        languages.add("Spring");
        languages.add("Java");
        languages.add("Hibernate");

        System.out.println(languages);
    }
}
```

Possible Output

```text
[Java, Spring, Hibernate]
```

Observation

Duplicate "Java" is ignored.

---

# Time Complexity (General)

| Operation | Complexity |
|------------|------------|
| add() | Depends on implementation |
| remove() | Depends on implementation |
| contains() | Depends on implementation |
| size() | O(1) |

The exact complexity depends on whether you use HashSet, LinkedHashSet, or TreeSet.

---

# Advantages

- Prevents duplicate elements
- Dynamic size
- Cleaner code
- Better data integrity
- Multiple implementations available

---

# Disadvantages

- No index-based access
- Ordering depends on implementation
- Performance depends on implementation

---

# Set vs List

| Feature | List | Set |
|----------|------|-----|
| Duplicates | Allowed | Not Allowed |
| Insertion Order | Maintained | Depends on implementation |
| Index Access | Yes | No |
| Null Values | Yes | Depends on implementation |

---

# Set Implementations

| Class | Description |
|---------|-------------|
| HashSet | Fastest, no ordering guarantee |
| LinkedHashSet | Maintains insertion order |
| TreeSet | Stores sorted elements |

---

# Frequently Asked Interview Questions

## Q1. What is Set?

### Answer

Set is an interface that stores only unique elements.

---

## Q2. Which interface does Set extend?

### Answer

```text
Iterable

↓

Collection

↓

Set
```

---

## Q3. Does Set allow duplicate elements?

### Answer

No.

---

## Q4. Can Set be instantiated?

### Answer

No.

It is an interface.

Correct way

```java
Set<String> set = new HashSet<>();
```

---

## Q5. Does Set support indexing?

### Answer

No.

There is no get(index) method.

---

# Cross Questions

## How does Set prevent duplicates?

### Answer

The Set interface defines the uniqueness rule.

The actual implementation decides **how** duplicates are detected.

For example:

- HashSet uses `hashCode()` and `equals()`
- TreeSet uses comparison (`compareTo()` or `Comparator`)
- LinkedHashSet uses HashSet logic while maintaining insertion order

---

## Why is there no get(index)?

### Answer

Set is not an indexed collection.

Elements are identified by value, not by position.

---

## Can Set contain null values?

### Answer

Depends on implementation.

- HashSet → One null allowed
- LinkedHashSet → One null allowed
- TreeSet → Null is not allowed when using natural ordering

---

## When should we use Set?

### Answer

Whenever duplicate values are not allowed.

Examples:

- Email IDs
- Usernames
- Employee IDs
- Unique Product Codes

---

# Tricky Interview Questions

## Question

Can Set contain duplicate null values?

### Answer

No.

Since null is also considered a value, only one null is allowed in HashSet and LinkedHashSet.

---

## Question

Does Set maintain insertion order?

### Answer

Depends.

- HashSet → No
- LinkedHashSet → Yes
- TreeSet → No (maintains sorted order instead)

---

## Question

Can we sort a Set?

### Answer

Not directly.

Use:

```java
TreeSet
```

or convert the Set into a List and sort it.

---

## Question

Can we access the first element of a HashSet?

### Answer

No.

HashSet does not guarantee any order.

---

# Best Practices

- Use Set whenever uniqueness is required.
- Program to the Set interface.

Good

```java
Set<String> names = new HashSet<>();
```

Bad

```java
HashSet<String> names = new HashSet<>();
```

- Choose the implementation based on your ordering requirements.

---

# Common Mistakes

- Expecting HashSet to maintain insertion order.
- Trying to access elements using an index.
- Assuming all Set implementations behave the same.

---

# Quick Revision

```text
Type                  → Interface

Parent                → Collection

Duplicates            → Not Allowed

Index                 → Not Supported

Dynamic Size          → Yes

Implementations       → HashSet
                        LinkedHashSet
                        TreeSet

Ordering              → Depends on implementation
```

---

# Interviewer's Perspective

### Why is this question asked?

Interviewers want to check whether you understand the **contract (Set)** separately from its implementations.

Common follow-up questions:

- How does HashSet prevent duplicates?
- Why doesn't Set have get(index)?
- Which Set implementation maintains insertion order?
- Which Set implementation keeps elements sorted?
- Which Set implementation is the fastest?

We'll answer all of these in the next three files.

---

# One-Line Summary

The **Set** interface represents a collection of unique elements, preventing duplicates while providing multiple implementations such as HashSet, LinkedHashSet, and TreeSet for different ordering and performance requirements.
