# HashMap Internal Working in Java

---

# 1. How Does HashMap Work Internally?

HashMap internally uses an **array of buckets**.

Each bucket can contain one or more entries.

When we insert a key-value pair:

```text
Key
 ↓
hashCode()
 ↓
Hash
 ↓
Bucket Index
 ↓
Bucket
 ↓
Store Entry
```

When retrieving:

```text
Key
 ↓
hashCode()
 ↓
Hash
 ↓
Bucket Index
 ↓
Search Bucket
 ↓
equals()
 ↓
Return Value
```

In Java 8+, a bucket can contain:

```text
Linked List
```

or, under the required conditions,

```text
Red-Black Tree
```

---

# 2. Internal Structure

Conceptually:

```text
HashMap

        table[]
           |
    +------+------+------+------+------+
    |      |      |      |      |      |
    ↓      ↓      ↓      ↓      ↓
  null   Node    null   Node   Node
                    |
                  Node
```

The array is called the **table**.

Each position is called a **bucket**.

---

# 3. What is a Bucket?

A bucket is a position in the internal hash table where entries are stored.

For example:

```text
Bucket 0 → null

Bucket 1 → Node

Bucket 2 → null

Bucket 3 → Node → Node

Bucket 4 → null
```

Multiple entries can exist in the same bucket because of collisions.

---

# 4. What is a Node?

Conceptually, a HashMap entry contains:

```text
hash
key
value
next
```

Conceptually:

```java
static class Node<K,V> {

    int hash;

    K key;

    V value;

    Node<K,V> next;
}
```

The actual JDK implementation contains additional details, but this is the important interview-level structure.

---

# 5. What Happens During put()?

Suppose:

```java
map.put("Java", 100);
```

HashMap performs several important steps.

```text
"Java"
   ↓
hashCode()
   ↓
Hash
   ↓
Bucket Index
   ↓
Check Bucket
   ↓
Insert / Replace
```

Let's understand each step.

---

# 6. Step 1 — Calculate hashCode()

HashMap first obtains the key's hash code.

```java
key.hashCode();
```

For example:

```text
"Java"

↓

hashCode()

↓

Some integer
```

The hash code is used to determine where the entry should go.

---

# 7. Step 2 — Hash Spreading

Java 8 HashMap does not simply use the raw `hashCode()` directly.

Conceptually, it spreads the bits of the hash.

The JDK implementation uses a transformation equivalent to:

```java
h ^ (h >>> 16)
```

This helps distribute hash values more effectively across buckets, particularly because bucket indexing uses lower bits.

---

# 8. Step 3 — Calculate Bucket Index

Once HashMap has the hash, it determines the bucket index.

Conceptually:

```text
index = hash % capacity
```

But HashMap does **not** normally use `%`.

When the table capacity is a power of two, it uses:

```java
index = (n - 1) & hash;
```

where:

```text
n = table length
```

---

# 9. Why Does HashMap Use Power-of-2 Capacity?

Suppose:

```text
capacity = 16
```

Then:

```text
capacity - 1 = 15
```

Binary:

```text
15 = 1111
```

HashMap can therefore use:

```java
(hash & 15)
```

instead of:

```java
hash % 16
```

This makes bucket calculation efficient.

This is one reason HashMap capacities are maintained as powers of two.

---

# 10. Example of Bucket Calculation

Suppose:

```text
table length = 16

hash = 42
```

Then:

```text
index = (16 - 1) & 42
```

or:

```text
index = 15 & 42
```

Binary:

```text
15 = 1111
42 = 101010
```

Result:

```text
001010

= 10
```

So the entry goes into:

```text
Bucket 10
```

---

# 11. Step 4 — Check the Bucket

Suppose the calculated bucket is:

```text
Bucket 10
```

HashMap checks:

```text
Bucket 10

↓

Empty?
```

If empty:

```text
Insert Node
```

Example:

```text
10 → [Java, 100]
```

---

# 12. What If the Bucket Is Not Empty?

Suppose:

```text
Bucket 10

↓

Node A
```

Now we are inserting another key that maps to the same bucket.

This is called a:

```text
Collision
```

HashMap must determine whether:

1. It is the same key.
2. It is a different key with the same bucket.

---

# 13. How Does HashMap Check Whether Keys Are Equal?

HashMap uses both:

```text
hash
```

and:

```text
equals()
```

Conceptually:

```text
Same hash?
   ↓
Yes
   ↓
equals()?
   ↓
Yes
   ↓
Same key
```

If the keys are equal:

```text
Replace old value
```

---

# 14. Example — Same Key

```java
map.put("Java", 100);

map.put("Java", 200);
```

The second operation finds the existing key.

Result:

```text
Java → 200
```

The key is not duplicated.

The value is replaced.

---

# 15. Example — Collision

Imagine:

```text
Key A → Bucket 5

Key B → Bucket 5
```

but:

```text
Key A != Key B
```

Then both entries must be stored.

Conceptually:

```text
Bucket 5

Node A
   ↓
Node B
```

This is collision handling.

---

# 16. Collision Handling in Java 8+

Initially, a bucket can use a linked structure:

```text
Bucket 5

Node A → Node B → Node C
```

If a bucket becomes sufficiently large and the table is sufficiently large, Java 8+ can convert the bucket into a:

```text
Red-Black Tree
```

Conceptually:

```text
Bucket 5

        Node B
       /      \
   Node A    Node C
```

This process is called:

```text
Treeification
```

---

# 17. Why Use Red-Black Tree?

Suppose a bucket contains many entries.

Linked list search:

```text
O(n)
```

A balanced Red-Black Tree can provide:

```text
O(log n)
```

lookup within that bucket.

This protects HashMap from severe collision chains.

---

# 18. What Happens During get()?

Suppose:

```java
map.get("Java");
```

HashMap performs:

```text
Key
 ↓
hashCode()
 ↓
Hash spreading
 ↓
Bucket Index
 ↓
Bucket
 ↓
Compare hash
 ↓
Compare key using equals()
 ↓
Return value
```

---

# 19. Detailed get() Flow

Suppose:

```text
Bucket 5

Node A
   ↓
Node B
   ↓
Node C
```

We search:

```text
"Java"
```

HashMap first determines:

```text
hash
```

Then:

```text
bucket = 5
```

It checks the nodes in that bucket.

For each possible match:

```text
hash matches?
```

If yes:

```text
equals() matches?
```

If yes:

```text
Return value
```

---

# 20. Why Does HashMap Need Both hashCode() and equals()?

Because `hashCode()` alone cannot uniquely identify an object.

Different objects can have the same hash code.

Example:

```text
Key A
hash = 100

Key B
hash = 100
```

This is allowed.

Therefore HashMap uses:

```text
hashCode()
```

to narrow down the location.

Then:

```text
equals()
```

to determine whether the keys are actually equal.

---

# 21. Important Rule

If:

```java
a.equals(b)
```

is:

```text
true
```

then:

```java
a.hashCode() == b.hashCode()
```

must also be true.

But the reverse is not required.

Two objects can have the same hash code and still not be equal.

---

# 22. What Happens During remove()?

Suppose:

```java
map.remove("Java");
```

HashMap:

```text
"Java"
   ↓
hashCode()
   ↓
Hash
   ↓
Bucket Index
   ↓
Find Node
   ↓
Compare Key
   ↓
Remove Node
```

If the bucket contains multiple nodes:

```text
Node A → Node B → Node C
```

and Node B is removed:

```text
Node A → Node C
```

---

# 23. Complete put() Flow

This is extremely important for interviews.

```text
map.put(key, value)

        ↓

Is table initialized?

        ↓

Calculate hash

        ↓

Calculate bucket index

        ↓

Is bucket empty?

   ┌────┴────┐
  Yes        No
   ↓          ↓
Insert     Check existing key
             ↓
       Same key?
        ┌────┴────┐
       Yes        No
        ↓          ↓
    Replace      Collision
     value          ↓
                Add Node
                   ↓
              Check treeification
```

---

# 24. Complete get() Flow

```text
map.get(key)

      ↓

Calculate hash

      ↓

Calculate bucket index

      ↓

Find bucket

      ↓

Compare hash

      ↓

Compare keys using equals()

      ↓

Found?

   ┌────┴────┐
  Yes        No
   ↓          ↓
Return      null
value
```

---

# 25. Complete remove() Flow

```text
map.remove(key)

      ↓

Calculate hash

      ↓

Calculate bucket

      ↓

Search bucket

      ↓

Compare hash + equals()

      ↓

Found?

   ┌────┴────┐
  Yes        No
   ↓          ↓
Remove       No-op
Node
```

---

# 26. What is Lazy Initialization?

A new HashMap does not necessarily allocate its internal table immediately.

For example:

```java
HashMap<Integer, String> map =
        new HashMap<>();
```

The actual table is initialized when needed, typically on the first insertion.

This is called:

```text
Lazy Initialization
```

---

# 27. What Happens on First put()?

```java
HashMap<Integer, String> map =
        new HashMap<>();
```

At this point, the internal table may not yet be allocated.

Then:

```java
map.put(1, "Java");
```

HashMap initializes the table and performs the insertion.

---

# 28. Important Interview Point

Do not say:

> "When I create `new HashMap<>(16)`, Java immediately creates an array of 16 buckets."

For modern Java implementations, table allocation is lazy.

The constructor records configuration, and the internal table is initialized when required.

---

# 29. HashMap Internal Structure — Final Picture

```text
                         HashMap
                            |
                         table[]
                            |
       +---------+----------+----------+---------+
       |         |          |          |         |
       ↓         ↓          ↓          ↓         ↓
     null      Node       null       Node      null
                 |                     |
                 ↓                     ↓
               Node                  Node
                 |
                 ↓
               Node
```

If a bucket becomes treeified:

```text
Bucket

   ↓

Red-Black Tree
```

---

# 30. Complexity

| Operation | Average | Collision-heavy case |
|---|---:|---:|
| get() | O(1) | O(log n) after treeification |
| put() | O(1) | O(log n) after treeification |
| remove() | O(1) | O(log n) after treeification |

The exact worst-case behavior depends on the bucket structure and JDK implementation details.

---

# 31. Frequently Asked Questions

## Q1. What data structure does HashMap use?

A hash table implemented using an array of buckets.

Buckets can contain linked nodes and, under treeification conditions, Red-Black Trees.

---

## Q2. Does HashMap use a LinkedList?

It can use linked nodes within a bucket when collisions occur.

It is not simply implemented as one LinkedList.

---

## Q3. Does HashMap use a Binary Search Tree?

No.

When treeification occurs, Java uses a **Red-Black Tree**, not a normal BST.

---

## Q4. Why is HashMap generally O(1)?

Hashing allows HashMap to directly identify the expected bucket instead of searching the entire collection.

---

## Q5. Why does HashMap use equals()?

To determine whether a key found in the bucket is actually equal to the requested key.

---

# 32. Tricky Interview Questions

## Can two different keys have the same hashCode()?

Yes.

This creates a collision.

---

## Can two equal objects have different hashCodes?

No.

That violates the `hashCode()` contract.

---

## Can two unequal objects have the same hashCode?

Yes.

---

## Does the same hashCode() mean two objects are equal?

No.

Same hash code does not guarantee equality.

---

## Does HashMap compare only hashCode()?

No.

It uses the hash to locate the bucket and `equals()` to identify the key.

---

# 33. Common Mistakes

### Mistake 1

> HashMap directly uses `hashCode()` as the array index.

Incorrect.

It performs hash processing and then calculates the bucket index.

---

### Mistake 2

> HashMap uses `%` to calculate the bucket every time.

Incorrect for the standard power-of-two table implementation.

It uses:

```java
(n - 1) & hash
```

---

### Mistake 3

> Same hash means same key.

Incorrect.

A collision can occur.

---

### Mistake 4

> HashMap always uses a Red-Black Tree.

Incorrect.

Treeification happens only under specific conditions.

---

### Mistake 5

> HashMap is always O(1).

Incorrect.

O(1) is the average expected complexity.

---

# 34. Interviewer's Follow-Up Chain

If the interviewer asks:

> How does HashMap work?

A strong answer should naturally lead through:

```text
HashMap
   ↓
Hash Table
   ↓
Bucket Array
   ↓
hashCode()
   ↓
Hash Spreading
   ↓
Bucket Index
   ↓
Collision
   ↓
equals()
   ↓
Linked Nodes
   ↓
Treeification
   ↓
Red-Black Tree
   ↓
Resize
   ↓
Load Factor
```

If you can explain this flow confidently, you understand the core of HashMap.

---

# 35. Quick Revision

```text
put(key, value)

        ↓

hashCode()

        ↓

Hash Spreading

        ↓

Bucket Index

        ↓

Find Bucket

        ↓

Empty?
   ↓       ↓
 Yes      No
  ↓        ↓
Insert   Compare
           ↓
       hash + equals()
           ↓
      Same Key?
       ↓       ↓
      Yes      No
       ↓        ↓
   Replace    Collision
               ↓
            New Node
```

---

# One-Line Summary

**HashMap uses hashing to locate a bucket, then uses key comparison to find the correct entry; collisions are handled through linked nodes and, under Java 8+ treeification conditions, Red-Black Trees.**
