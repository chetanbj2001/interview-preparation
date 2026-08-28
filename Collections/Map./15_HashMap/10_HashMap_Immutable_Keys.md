# HashMap — Immutable Keys

## 1. Why should HashMap keys be immutable?

A `HashMap` uses the key's `hashCode()` to find the bucket.

If the key is modified after insertion, its `hashCode()` may change. Then `HashMap` may look in a different bucket and fail to find the key.

```java
Map<User, String> map = new HashMap<>();

User user = new User(1, "Chetan");
map.put(user, "Developer");

user.setId(2);

System.out.println(map.get(user)); // may return null
```

### Interview Answer

> HashMap keys should be immutable because changing a key after insertion can change its hash code, causing the map to search in the wrong bucket.

---

## 2. Why is String a good HashMap key?

`String` is immutable.

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 10);
```

Once `"Java"` is created, its value cannot be changed.

Therefore:

* `hashCode()` remains stable
* `equals()` result remains consistent
* HashMap can safely locate the key

---

## 3. Can a mutable object be a HashMap key?

Yes, Java allows it.

But it is **dangerous** if fields used by `equals()` or `hashCode()` are modified.

```java
class User {
    int id;

    User(int id) {
        this.id = id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }

    @Override
    public boolean equals(Object obj) {
        return obj instanceof User
                && this.id == ((User) obj).id;
    }
}
```

Example:

```java
User user = new User(1);

Map<User, String> map = new HashMap<>();
map.put(user, "Developer");

user.id = 2;

System.out.println(map.get(user)); // null
```

The object is still inside the map, but its hash code changed.

---

## 4. What makes a class immutable?

Typically:

* Make the class `final`
* Make fields `private final`
* Initialize fields through the constructor
* Don't provide setters
* Don't expose mutable internal objects directly

Example:

```java
final class User {

    private final int id;
    private final String name;

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

Now the object's state cannot be changed after creation.

---

## 5. Why does immutability make HashMap safer?

Suppose:

```java
map.put(user, "Developer");
```

HashMap calculates:

```text
hashCode(key)
      ↓
bucket
      ↓
store key-value
```

Later:

```java
map.get(user);
```

HashMap calculates the hash again.

If the key is immutable:

```text
Insertion hash = 100
Retrieval hash = 100
```

So HashMap can find the correct bucket.

With a mutable key:

```text
Insertion hash = 100
Retrieval hash = 200
```

HashMap searches another bucket.

---

## 6. Important interview trap

### Is an immutable key mandatory for HashMap?

**No.**

HashMap does not require keys to be immutable.

But if a key's state used by `equals()` or `hashCode()` changes after insertion, lookup/removal can fail.

### Best practice

Use immutable keys such as:

```java
String
Integer
Long
Enum
```

or create your own immutable class.

---

## 7. What happens if we mutate a HashMap key?

```java
User user = new User(1);

map.put(user, "Java");

user.id = 2;

map.get(user);
```

Possible result:

```text
null
```

The entry has **not necessarily disappeared**.

It is still stored in the old bucket, but HashMap now calculates a different hash and searches somewhere else.

---

## 8. Interview Cross Questions

### Q1. Why is String commonly used as a HashMap key?

Because `String` is immutable and has a stable `hashCode()`.

### Q2. Can Integer be a HashMap key?

Yes. `Integer` is immutable.

### Q3. Can ArrayList be a HashMap key?

Yes, but it is risky because `ArrayList` is mutable.

### Q4. What happens if a key's hashCode changes?

HashMap may not be able to find the existing entry.

### Q5. Does HashMap copy the key?

No.

It stores the reference to the key object.

### Q6. Is `final` alone enough to make a class immutable?

No.

```java
final class User {
    private int id;
}
```

`final` prevents inheritance, but `id` can still change.

### Q7. What is the safest type of HashMap key?

An immutable object whose `equals()` and `hashCode()` are consistent.

---

## ⭐ Interview Summary

> A HashMap key should ideally be immutable because HashMap depends on the key's `hashCode()` and `equals()` for insertion and retrieval. If the key is modified after insertion and its hash code changes, HashMap may search in a different bucket and fail to find the entry. String, Integer, Long, and Enum are common safe key types.

### Java 8 Concepts Used

* `final` classes
* `private final` fields
* Constructor-based initialization
* `equals()`
* `hashCode()`
* `HashMap`
