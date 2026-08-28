# HashMap — Null Keys and Null Values

---

## 1. Can HashMap Have a Null Key? ⭐⭐⭐⭐⭐

Yes.

A `HashMap` allows:

```text
One null key
```

Example:

```java
Map<String, Integer> map = new HashMap<>();

map.put(null, 100);

System.out.println(map.get(null));
```

Output:

```text
100
```

---

## 2. Can HashMap Have Multiple Null Values?

Yes.

A `HashMap` can contain multiple `null` values.

```java
map.put("A", null);
map.put("B", null);
map.put("C", null);
```

All three are valid.

```text
A → null
B → null
C → null
```

---

## 3. Can HashMap Have Multiple Null Keys?

No.

Only **one null key** can exist because keys must be unique.

```java
map.put(null, 100);
map.put(null, 200);
```

The second `put()` replaces the first value.

Result:

```text
null → 200
```

---

## 4. Why Only One Null Key?

Because a `Map` cannot contain duplicate keys.

This:

```java
map.put(null, 100);
map.put(null, 200);
```

is effectively:

```text
Same key → update value
```

So:

```text
null → 200
```

---

## 5. How Does HashMap Handle `null` Key?

`HashMap` has special handling for a `null` key.

Conceptually:

```text
null key
   ↓
special bucket handling
   ↓
store entry
```

You don't need to memorize the exact internal implementation for normal interviews.

---

## 6. Can `get()` Return `null`?

Yes.

But there are **two possibilities**:

```text
1. Key does not exist
2. Key exists and its value is null
```

Example:

```java
Map<String, Integer> map = new HashMap<>();

map.put("A", null);

System.out.println(map.get("A")); // null
System.out.println(map.get("B")); // null
```

Both return:

```text
null
```

---

## 7. How Do We Distinguish Them? ⭐⭐⭐⭐⭐

Use `containsKey()`.

```java
if (map.containsKey("A")) {
    System.out.println("Key exists");
}
```

For:

```java
map.put("A", null);
```

we get:

```text
get("A")         → null
containsKey("A") → true
```

For a missing key:

```text
get("B")         → null
containsKey("B") → false
```

---

## 8. Quick Example

```java
Map<String, Integer> map = new HashMap<>();

map.put(null, 10);
map.put("A", null);
map.put("B", null);

System.out.println(map.size());
```

Output:

```text
3
```

Because:

```text
null → 10
A    → null
B    → null
```

---

## 9. HashMap vs Hashtable

Important interview question:

| Feature | HashMap | Hashtable |
|---|---|---|
| Null key | Yes, one | No |
| Null values | Yes | No |
| Thread-safe | No | Yes (legacy synchronization) |

---

## 10. HashMap vs ConcurrentHashMap

Another common question:

| Feature | HashMap | ConcurrentHashMap |
|---|---|---|
| Null key | Yes | No |
| Null values | Yes | No |
| Thread-safe | No | Yes |

### Why does ConcurrentHashMap reject null?

Because `null` could make it ambiguous whether:

```text
key is absent
```

or:

```text
key exists with null value
```

This is especially problematic for concurrent operations.

---

## 11. Tricky Interview Questions

### Q1. How many null keys can HashMap contain?

```text
One
```

---

### Q2. How many null values can HashMap contain?

```text
Multiple
```

---

### Q3. What happens when we insert the same null key twice?

The second value replaces the first.

---

### Q4. `get()` returns null. Does that mean the key doesn't exist?

No.

The value itself may be `null`.

Use:

```java
containsKey()
```

to distinguish the cases.

---

### Q5. Does ConcurrentHashMap allow null?

No.

It allows neither:

```text
null key
```

nor:

```text
null value
```

---

## 12. Quick Revision

```text
HashMap

Null key     → One
Null values  → Multiple

get() = null
    ↓
Could mean:
    ↓
Missing key OR null value

containsKey()
    ↓
Distinguishes them

ConcurrentHashMap
    ↓
No null keys
No null values
