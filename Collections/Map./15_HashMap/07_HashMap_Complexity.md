# HashMap — Time & Space Complexity

---

## 1. `put()` Complexity

### Average Case

```text
O(1)
```

HashMap calculates the hash, finds the bucket, and inserts the entry.

### Worst Case

With a treeified bucket:

```text
O(log n)
```

With a long linked collision chain:

```text
O(n)
```

---

## 2. `get()` Complexity

### Average

```text
O(1)
```

### Treeified Bucket

```text
O(log n)
```

### Poor Collision Chain

```text
O(n)
```

---

## 3. `remove()` Complexity

Same general behavior as `get()`:

```text
Average            → O(1)
Treeified bucket   → O(log n)
Long chain         → O(n)
```

---

## 4. Resize Complexity

When HashMap resizes, existing entries need to be redistributed.

```text
O(n)
```

However, resizing happens only occasionally.

Therefore:

```text
put() → O(1) amortized
```

---

## 5. Iteration Complexity

Iterating through a HashMap is approximately:

```text
O(capacity + size)
```

because iteration may inspect the bucket array as well as the stored entries.

---

## 6. Space Complexity

For `n` entries:

```text
O(n)
```

HashMap stores:

- Keys
- Values
- Nodes
- Bucket array

---

## 7. Complexity Summary

| Operation | Average | Worst / Special Case |
|---|---:|---:|
| `put()` | O(1) | O(log n) tree / O(n) chain |
| `get()` | O(1) | O(log n) tree / O(n) chain |
| `remove()` | O(1) | O(log n) tree / O(n) chain |
| Resize | — | O(n) |
| Iteration | — | O(capacity + size) |
| Space | O(n) | O(n) |

---

## 8. Important Interview Question ⭐⭐⭐⭐⭐

### Is HashMap always O(1)?

**No.**

A better interview answer is:

> HashMap provides O(1) average-time `get`, `put`, and `remove`. Heavy collisions can degrade performance, while Java 8 treeified buckets can provide O(log n) lookup. Resizing itself is O(n), but `put()` is O(1) amortized.

---

## 9. Quick Revision

```text
put()       → O(1) average
get()       → O(1) average
remove()    → O(1) average

Tree bucket → O(log n)

Long chain  → O(n)

Resize      → O(n)

Iteration   → O(capacity + size)

Space       → O(n)
