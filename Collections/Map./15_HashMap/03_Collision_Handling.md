# HashMap Collision Handling in Java

---

# 1. What is a Hash Collision?

A hash collision occurs when two different keys produce the same bucket index in a HashMap.

For example:

```text
Key A
  ↓
Bucket 5

Key B
  ↓
Bucket 5
```

Both keys need to be stored in the same bucket.

HashMap must therefore have a mechanism to handle collisions.

---

# 2. Why Do Collisions Happen?

A HashMap has a finite number of buckets, but there can be a very large number of possible hash values.

Therefore, different keys can eventually map to the same bucket.

Remember:

```text
Different hashCode()
        ↓
Can still produce
        ↓
Same bucket index
```

The bucket index depends on the current table capacity.

---

# 3. Important Distinction

These two situations are different:

### Same hash code

```text
key1.hashCode() == key2.hashCode()
```

### Same bucket

```text
index(key1) == index(key2)
```

Two different hash codes can still result in the same bucket index.

Therefore:

> A bucket collision does not necessarily mean the two keys have identical hash codes.

---

# 4. Example

Suppose:

```text
Capacity = 16
```

HashMap calculates:

```java
index = (n - 1) & hash;
```

So:

```text
index = hash & 15
```

Different hashes can produce the same result.

For example, conceptually:

```text
Hash A → Bucket 5

Hash B → Bucket 5
```

This is a bucket collision.

---

# 5. How Does HashMap Handle Collisions?

HashMap stores multiple entries in the same bucket.

Historically, this was handled using linked nodes.

Conceptually:

```text
Bucket 5

Node A
  ↓
Node B
  ↓
Node C
```

In Java 8+, under appropriate conditions, a heavily populated bucket can be converted into a Red-Black Tree.

```text
Bucket 5

       Node B
       /    \
   Node A  Node C
```

---

# 6. Collision Handling — High-Level Flow

```text
put(key, value)
       ↓
Calculate hash
       ↓
Calculate bucket index
       ↓
Is bucket empty?
    /       \
  Yes       No
   ↓         ↓
Insert     Collision
             ↓
      Compare existing key
             ↓
       Same key?
        /       \
      Yes       No
       ↓         ↓
 Replace       Add node
 value           ↓
          Check treeification
```

---

# 7. What Happens When Bucket Is Empty?

Suppose:

```text
Bucket 5 → null
```

We execute:

```java
map.put(key, value);
```

HashMap simply creates an entry:

```text
Bucket 5

Node A
```

No collision exists.

---

# 8. What Happens When Bucket Already Contains a Node?

Suppose:

```text
Bucket 5

Node A
```

We insert another key.

HashMap checks whether the existing key is actually the same key.

Conceptually:

```text
Same hash?
     ↓
   Yes
     ↓
equals()?
```

If:

```text
equals() == true
```

the value is replaced.

If:

```text
equals() == false
```

we have a collision and another entry must be stored.

---

# 9. Collision With Linked Nodes

Suppose:

```text
Bucket 5

Node A → Node B → Node C
```

Each node contains information about:

```text
hash
key
value
next
```

The `next` reference connects nodes in the same bucket.

---

# 10. How get() Works With a Collision

Suppose:

```java
map.get(keyC);
```

HashMap:

```text
keyC
 ↓
hashCode()
 ↓
hash spreading
 ↓
bucket index = 5
 ↓
Bucket 5
 ↓
Node A
 ↓
Node B
 ↓
Node C
 ↓
Matching hash + equals()
 ↓
Return value
```

If the bucket is a linked structure, HashMap may have to traverse nodes until it finds the matching key.

---

# 11. Why Can Linked Collision Handling Be Slow?

Suppose a bucket contains:

```text
Node A → Node B → Node C → Node D → Node E
```

Searching for Node E requires traversal through several nodes.

The search within the bucket can therefore become:

```text
O(n)
```

in a long linked chain.

This is why Java 8 introduced treeification.

---

# 12. Java 7 Collision Handling

In Java 7, collision chains were handled using linked lists.

Conceptually:

```text
Bucket

Node A → Node B → Node C → Node D
```

If many keys collided in one bucket, lookup could degrade toward:

```text
O(n)
```

---

# 13. Java 8 Collision Handling

Java 8 introduced a major improvement.

When a bucket becomes sufficiently large, HashMap can convert the linked structure into a:

```text
Red-Black Tree
```

Conceptually:

```text
Before:

Node A → Node B → Node C → Node D
```

After treeification:

```text
        Node B
       /      \
   Node A    Node C
                \
                Node D
```

The exact tree structure depends on the keys and hashes.

---

# 14. Why Red-Black Tree?

A Red-Black Tree is a self-balancing binary search tree.

Its height remains logarithmic relative to the number of nodes.

Therefore:

```text
Linked List

Search → O(n)
```

while a balanced tree provides approximately:

```text
Red-Black Tree

Search → O(log n)
```

for the bucket's tree structure.

---

# 15. Important Thresholds ⭐⭐⭐⭐⭐

Java 8 HashMap has two important constants related to treeification:

```text
TREEIFY_THRESHOLD = 8

UNTREEIFY_THRESHOLD = 6

MIN_TREEIFY_CAPACITY = 64
```

These values are extremely common interview questions.

---

# 16. TREEIFY_THRESHOLD

The commonly documented threshold is:

```text
8
```

When a bucket becomes sufficiently populated, HashMap considers treeification.

However:

> Reaching 8 nodes does not mean the bucket will always immediately become a tree.

There is another important condition.

---

# 17. MIN_TREEIFY_CAPACITY

HashMap generally does not treeify a bucket when the table is still too small.

The minimum table capacity for treeification is:

```text
64
```

If the table is smaller than this, HashMap prefers resizing the table.

Conceptually:

```text
Bucket becomes crowded
        ↓
Table capacity < 64?
     /          \
   Yes           No
    ↓             ↓
Resize        Treeify
```

---

# 18. Why Resize Instead of Treeify?

A crowded bucket in a small table can often be caused simply by insufficient table capacity.

Increasing the table size distributes entries across more buckets.

Therefore:

```text
Small table
+
Crowded bucket
        ↓
Resize first
```

This can reduce collisions without immediately creating tree structures.

---

# 19. TREEIFY_THRESHOLD vs MIN_TREEIFY_CAPACITY

Do not confuse these.

```text
TREEIFY_THRESHOLD
        =
approximately 8 nodes
```

while:

```text
MIN_TREEIFY_CAPACITY
        =
64 buckets
```

One relates to:

```text
bucket population
```

The other relates to:

```text
table capacity
```

---

# 20. UNTREEIFY_THRESHOLD

The commonly documented value is:

```text
6
```

When a tree becomes sufficiently small during certain removal/resizing operations, HashMap can convert it back into a linked structure.

This is called:

```text
Untreeification
```

Conceptually:

```text
Red-Black Tree
      ↓
Too few entries
      ↓
Linked Nodes
```

---

# 21. Treeification Flow

```text
Bucket becomes crowded
        ↓
Is table capacity >= 64?
     /          \
   No            Yes
   ↓              ↓
Resize       Consider Treeify
                 ↓
          Linked → Tree
```

---

# 22. Untreeification

Suppose a treeified bucket loses enough entries.

HashMap can convert the tree back to a linked structure.

Conceptually:

```text
Before:

       B
      / \
     A   C
          \
           D

After:

A → B → C → D
```

The actual node arrangement is implementation-specific.

---

# 23. Why Doesn't HashMap Treeify Immediately?

Because a tree has additional overhead.

For a small number of entries, a linked structure can be simpler and efficient.

HashMap therefore uses thresholds rather than turning every bucket into a tree.

---

# 24. Collision vs Duplicate Key

This is a very important distinction.

### Duplicate Key

```text
Same key
```

Example:

```java
map.put("Java", 100);
map.put("Java", 200);
```

Result:

```text
Java → 200
```

---

### Collision

```text
Different keys
+
Same bucket
```

Example:

```text
Key A → Bucket 5
Key B → Bucket 5
```

Both can exist.

---

# 25. Same HashCode vs Same Key

Remember:

```text
Same hashCode
       ≠
Same key
```

For example:

```text
keyA.hashCode() == keyB.hashCode()

but

keyA.equals(keyB) == false
```

This is a collision.

---

# 26. How equals() Resolves a Collision

Suppose:

```text
Bucket 5

Node A
Node B
```

We search for Key B.

HashMap checks the candidate node's hash and then key equality.

Conceptually:

```text
Hash matches?
     ↓
   Yes
     ↓
equals()?
     ↓
   Yes
     ↓
Found key
```

If:

```text
equals() == false
```

HashMap continues searching.

---

# 27. What If equals() Is Broken?

Suppose two logically equal keys return:

```text
equals() = true
```

but their hash codes are different.

Then they can end up in different buckets.

HashMap may fail to locate the key correctly.

This is why the contract matters:

```text
equals() + hashCode()
```

must work together.

---

# 28. Worst-Case Improvement in Java 8+

With linked collision chains:

```text
O(n)
```

With a treeified bucket:

```text
O(log n)
```

Therefore Java 8 significantly improves HashMap's behavior under heavy collisions.

---

# 29. Does Treeification Make HashMap Always O(log n)?

No.

This is a common trick question.

HashMap is still generally described as:

```text
Average → O(1)
```

Treeification is a collision-management mechanism.

Only a heavily collided bucket may use a tree.

---

# 30. What Causes Many Collisions?

Poor hash functions can produce many collisions.

For example, a badly designed custom key class might return:

```java
@Override
public int hashCode() {
    return 1;
}
```

for every object.

Then all keys may go into the same bucket.

```text
Bucket X

Node
 ↓
Node
 ↓
Node
 ↓
Node
 ↓
Node
```

This is extremely poor hashing.

---

# 31. Why Should hashCode() Distribute Values Well?

A good hash function should distribute keys across buckets.

Ideally:

```text
Keys

 ↓

Hash values

 ↓

Different buckets
```

rather than:

```text
Many keys

 ↓

Same bucket
```

Better distribution means fewer collisions and better performance.

---

# 32. Bad hashCode() Example

```java
class Employee {

    private int id;

    @Override
    public int hashCode() {
        return 1;
    }
}
```

Every employee has:

```text
hash = 1
```

This creates excessive collisions.

Even though HashMap can still function correctly, performance can suffer.

---

# 33. Good hashCode() Example

Use the fields that participate in equality.

For example:

```java
@Override
public int hashCode() {
    return Objects.hash(id, name);
}
```

This generally provides much better distribution than returning a constant.

---

# 34. Interview Question

## Does a collision mean HashMap is broken?

No.

Collisions are expected in hash tables.

A correct HashMap must handle collisions.

The problem is not the existence of collisions.

The problem is **excessive collisions**.

---

# 35. Interview Question

## Can two different buckets contain keys with the same hashCode?

For a given table state and normal bucket-index calculation, equal processed hashes map to the same bucket index.

However, the important point is that bucket index depends on the current table capacity.

After resizing, the same hash can map to a different bucket.

---

# 36. Interview Question

## What happens to collisions after resizing?

When the table grows, bucket indexes are recalculated according to the new capacity.

Entries are redistributed across the larger table.

This can reduce collisions.

Detailed resizing will be covered in:

```text
04_Resize_Rehashing.md
```

---

# 37. Java 7 vs Java 8

| Feature | Java 7 | Java 8+ |
|---|---|---|
| Collision structure | Linked list | Linked list + Tree |
| Heavy collision handling | O(n) traversal | Can become O(log n) |
| Treeification | No | Yes |
| Red-Black Tree | No | Yes |

---

# 38. Important Thresholds

Remember:

```text
TREEIFY_THRESHOLD
        ↓
        8

UNTREEIFY_THRESHOLD
        ↓
        6

MIN_TREEIFY_CAPACITY
        ↓
        64
```

These are implementation details of Java's HashMap and are useful interview knowledge, but exact internals can vary across JDK versions.

---

# 39. Tricky Interview Questions

## Q1. Does same hashCode mean same bucket?

Not necessarily across different table capacities.

The bucket index is calculated from the processed hash and the current table length.

---

## Q2. Does same bucket mean same key?

No.

Different keys can collide into the same bucket.

---

## Q3. Does HashMap use a tree for every collision?

No.

Treeification happens only when the relevant conditions are met.

---

## Q4. Why is MIN_TREEIFY_CAPACITY important?

Because HashMap prefers resizing a small table rather than immediately treeifying a crowded bucket.

---

## Q5. Why doesn't HashMap use a tree from the beginning?

Because trees have additional memory and processing overhead and are unnecessary for small buckets.

---

## Q6. Can a treeified bucket become a linked structure again?

Yes.

Under appropriate conditions, it can be untreeified.

---

# 40. Complete Collision Flow

```text
                  put(key, value)
                         |
                         ↓
                     hashCode()
                         |
                         ↓
                  Hash Processing
                         |
                         ↓
                   Bucket Index
                         |
                         ↓
                  Is bucket empty?
                    /          \
                  Yes           No
                   |             |
                Insert       Check key
                                 |
                          hash + equals()
                                 |
                       Is it the same key?
                         /             \
                       Yes             No
                        |               |
                  Replace value     Collision
                                        |
                                        ↓
                                 Add linked node
                                        |
                                        ↓
                              Bucket sufficiently large?
                                   /          \
                                 No            Yes
                                 |              |
                               Done       Capacity >= 64?
                                               /      \
                                             No        Yes
                                             |          |
                                           Resize     Treeify
```

---

# 41. Quick Revision

```text
Collision
    ↓
Different keys
    ↓
Same bucket
    ↓
Linked nodes
    ↓
Heavy collision
    ↓
Check table capacity
    ↓
Small table → Resize
    ↓
Large enough table → Treeify
    ↓
Red-Black Tree
```

---

# 42. One-Line Summary

**HashMap handles collisions by storing multiple entries in the same bucket using linked nodes and, when the bucket becomes sufficiently large and the table is large enough, converting the bucket into a Red-Black Tree to improve lookup performance.**

---

# Next

```text
15_HashMap/
├── 00_HashMap_Overview.md
├── 01_Internal_Working.md
├── 02_hashCode_equals.md
├── 03_Collision_Handling.md     ← completed
└── 04_Resize_Rehashing.md       ← NEXT
```

`04_Resize_Rehashing.md` will cover **capacity, load factor, threshold, resizing, why capacity is a power of 2, and what actually happens when HashMap grows**.
