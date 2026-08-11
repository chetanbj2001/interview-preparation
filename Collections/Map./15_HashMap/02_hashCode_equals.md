# hashCode() and equals() in HashMap

---

# 1. Why Are hashCode() and equals() Important?

HashMap uses both `hashCode()` and `equals()` to identify keys.

When we call:

```java
map.get(key);
```

HashMap first uses the key's `hashCode()` to determine the bucket.

Then, if multiple keys are present in that bucket, HashMap uses `equals()` to determine the exact matching key.

The basic flow is:

```text
Key
 ↓
hashCode()
 ↓
Bucket
 ↓
equals()
 ↓
Matching Key
 ↓
Value
```

---

# 2. What is hashCode()?

`hashCode()` is a method defined in the `Object` class that returns an integer hash value representing the object.

Example:

```java
String name = "Java";

System.out.println(name.hashCode());
```

The hash code is used by hash-based collections such as:

- HashMap
- HashSet
- Hashtable

---

# 3. What is equals()?

`equals()` is used to determine whether two objects are logically equal.

Example:

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

Even though `a` and `b` are different objects, their contents are equal.

---

# 4. hashCode() vs equals()

| hashCode() | equals() |
|---|---|
| Returns an `int` | Returns `boolean` |
| Used for hashing | Used for logical equality |
| Helps locate bucket | Identifies matching key |
| Defined in Object | Defined in Object |

---

# 5. The Most Important Contract ⭐⭐⭐⭐⭐

The most important rule is:

```text
If a.equals(b) == true

then

a.hashCode() == b.hashCode()
```

This must always be true.

---

# 6. Can Equal Objects Have Different HashCodes?

No.

Example:

```java
Employee e1 = new Employee(101, "Chetan");
Employee e2 = new Employee(101, "Chetan");
```

If:

```java
e1.equals(e2)
```

returns:

```text
true
```

then:

```java
e1.hashCode()
```

and:

```java
e2.hashCode()
```

must return the same value.

---

# 7. Can Different Objects Have the Same HashCode?

Yes.

This is completely valid.

Example:

```text
Object A → hashCode = 100

Object B → hashCode = 100
```

They can still be different according to `equals()`.

This is called a:

```text
Hash Collision
```

---

# 8. Very Important Rule

Remember this table:

| Situation | Valid? |
|---|---|
| Equal → Same hashCode | Must be true |
| Unequal → Same hashCode | Yes |
| Unequal → Different hashCode | Yes |
| Equal → Different hashCode | ❌ No |

---

# 9. Why Does HashMap Need Both?

Suppose:

```text
Key A → hash = 100

Key B → hash = 100
```

Both keys go to the same bucket.

HashMap cannot decide based only on hashCode.

It then uses:

```java
equals()
```

to distinguish them.

Conceptually:

```text
Same hash?
    ↓
   Yes
    ↓
equals()?
  /     \
Yes      No
 ↓        ↓
Same     Collision
key
```

---

# 10. Example With String

```java
Map<String, Integer> map = new HashMap<>();

map.put(new String("Java"), 100);

System.out.println(
        map.get(new String("Java"))
);
```

Output:

```text
100
```

Why?

Because `String` correctly overrides both:

```java
hashCode()
```

and:

```java
equals()
```

Both String objects have equal content.

---

# 11. Custom Object Example ⭐⭐⭐⭐⭐

Consider:

```java
class Employee {

    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

Now:

```java
Employee e1 =
        new Employee(101, "Chetan");

Employee e2 =
        new Employee(101, "Chetan");
```

Put:

```java
Map<Employee, String> map =
        new HashMap<>();

map.put(e1, "Developer");
```

Now:

```java
System.out.println(map.get(e2));
```

What should we expect?

Many developers answer:

```text
Developer
```

But without overriding `equals()` and `hashCode()`, the result will normally be:

```text
null
```

Why?

Because `e1` and `e2` are different object references.

The default `Object.equals()` compares object identity.

---

# 12. Why Does This Happen?

We inserted:

```text
e1
```

But we searched using:

```text
e2
```

Although their data is the same:

```text
e1:
id = 101
name = Chetan

e2:
id = 101
name = Chetan
```

they are different objects.

Without overriding equality behavior:

```text
e1.equals(e2)

↓

false
```

Therefore HashMap does not consider them the same key.

---

# 13. Correct Implementation

We should override both methods.

```java
class Employee {

    private int id;
    private String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {

        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Employee)) {
            return false;
        }

        Employee other = (Employee) obj;

        return id == other.id &&
               Objects.equals(name, other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}
```

We need:

```java
import java.util.Objects;
```

---

# 14. Now HashMap Works Correctly

```java
Employee e1 =
        new Employee(101, "Chetan");

Employee e2 =
        new Employee(101, "Chetan");

Map<Employee, String> map =
        new HashMap<>();

map.put(e1, "Developer");

System.out.println(map.get(e2));
```

Output:

```text
Developer
```

Because:

```text
e1.equals(e2)
        ↓
      true

e1.hashCode() == e2.hashCode()
        ↓
      true
```

---

# 15. What Happens Internally?

When:

```java
map.put(e1, "Developer");
```

HashMap calculates:

```text
e1.hashCode()
       ↓
Bucket
       ↓
Store e1
```

When:

```java
map.get(e2);
```

HashMap calculates:

```text
e2.hashCode()
       ↓
Same Bucket
       ↓
equals()
       ↓
e1.equals(e2)
       ↓
true
       ↓
Return "Developer"
```

---

# 16. What If We Override Only equals()?

This is a very common interview question.

Suppose:

```java
@Override
public boolean equals(Object obj) {
    ...
}
```

but we don't override:

```java
hashCode()
```

This is dangerous.

Two logically equal objects may produce different hash codes.

Then HashMap may put them into different buckets.

Conceptually:

```text
e1.equals(e2) → true

but

e1.hashCode() != e2.hashCode()
```

This violates the contract.

---

# 17. What If We Override Only hashCode()?

Suppose:

```java
@Override
public int hashCode() {
    return id;
}
```

but we don't override `equals()`.

This is also insufficient.

Two different objects can have the same hash code, but `equals()` may still return false.

Therefore:

```text
hashCode() alone
       ❌
```

is not enough.

---

# 18. Correct Rule

For HashMap keys:

```text
equals() + hashCode()
```

must be implemented consistently.

Think:

```text
equals()
   +
hashCode()
   ↓
Correct HashMap Key
```

---

# 19. Important Interview Question

## What happens if two keys have the same hashCode?

They can be placed in the same bucket.

HashMap then uses `equals()` to determine whether they are actually the same key.

If `equals()` returns false:

```text
Collision
```

Both entries can exist.

---

# 20. Important Interview Question

## What happens if two keys have different hashCodes?

They normally map to different buckets for the same table state.

Therefore HashMap does not need to consider them equal keys.

---

# 21. The Four Possible Cases

## Case 1

```text
Same hashCode
+
equals() = true
```

Result:

```text
Same key
```

The value is replaced during `put()`.

---

## Case 2

```text
Same hashCode
+
equals() = false
```

Result:

```text
Collision
```

Both keys can exist.

---

## Case 3

```text
Different hashCode
+
equals() = false
```

Normal case.

Different logical keys.

---

## Case 4

```text
Different hashCode
+
equals() = true
```

Invalid contract.

This should never happen for a correctly implemented class.

---

# 22. equals() Contract

The `equals()` method follows several important properties.

## Reflexive

```text
x.equals(x) == true
```

---

## Symmetric

If:

```text
x.equals(y) == true
```

then:

```text
y.equals(x) == true
```

---

## Transitive

If:

```text
x.equals(y)
y.equals(z)
```

then:

```text
x.equals(z)
```

should also be true.

---

## Consistent

Repeated calls should return the same result as long as the relevant state has not changed.

---

## Non-null

For a non-null reference:

```text
x.equals(null)
```

should return:

```text
false
```

---

# 23. hashCode() Contract

If:

```java
a.equals(b)
```

is true, then:

```java
a.hashCode() == b.hashCode()
```

must be true.

Repeated calls should return the same hash code as long as information used in equality comparisons has not changed.

Unequal objects may have the same hash code.

---

# 24. Why Should HashMap Keys Be Immutable?

This is a very important practical interview question.

Suppose a key is inserted:

```java
map.put(employee, "Developer");
```

HashMap uses the key's hash code to determine its bucket.

If we later change a field used by:

```text
hashCode()
```

or:

```text
equals()
```

the key may effectively belong to a different bucket.

Then:

```java
map.get(employee)
```

may fail to find it.

---

# 25. Example of a Mutable Key Problem

Suppose:

```java
class Employee {

    int id;

    Employee(int id) {
        this.id = id;
    }

    @Override
    public int hashCode() {
        return id;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Employee)) {
            return false;
        }

        Employee other = (Employee) obj;

        return id == other.id;
    }
}
```

Now:

```java
Employee e =
        new Employee(101);

map.put(e, "Developer");
```

Suppose:

```java
e.id = 200;
```

Now the hash code has changed.

The entry is still physically stored in the bucket determined using the old hash.

But lookup uses the new hash.

This can cause:

```java
map.get(e)
```

to return:

```text
null
```

---

# 26. Best Practice for Keys

Prefer:

```text
Immutable objects
```

as HashMap keys.

Good examples:

```text
String
Integer
Long
UUID
```

Custom immutable classes can also be excellent keys.

---

# 27. Why String Is a Good HashMap Key

String is effectively immutable.

Once created:

```java
String key = "Java";
```

its content cannot be changed.

Therefore its:

```text
equals()
```

and:

```text
hashCode()
```

behavior remains stable.

---

# 28. Tricky Interview Questions

## Q1. Can two unequal objects have the same hashCode?

Yes.

That is a collision.

---

## Q2. Can two equal objects have different hashCodes?

No.

That violates the contract.

---

## Q3. Is same hashCode enough to prove equality?

No.

`equals()` must also return true.

---

## Q4. Why override hashCode() when overriding equals()?

Because equal objects must have equal hash codes.

---

## Q5. Why are immutable keys preferred?

Because changing fields used by `equals()` or `hashCode()` after insertion can make the entry difficult or impossible to retrieve.

---

## Q6. What happens if equals() is correct but hashCode() is wrong?

HashMap may place logically equal keys into different buckets, causing incorrect lookup behavior.

---

## Q7. What happens if hashCode() is correct but equals() is wrong?

Hash collisions may not be resolved correctly, and logically equal keys may be treated as different keys.

---

# 29. Interview Scenario ⭐⭐⭐⭐⭐

Interviewer:

> I have an Employee class. I insert Employee(101, "Chetan") into HashMap. Later I create another Employee(101, "Chetan") and call get(). Why am I getting null?

Answer:

Because the two objects are different object instances and the custom class has not correctly overridden `equals()` and `hashCode()`.

HashMap uses `hashCode()` to locate the bucket and `equals()` to identify the matching key.

Both methods must be implemented consistently.

---

# 30. Interview Scenario 2

Interviewer:

> Can two objects have the same hashCode but not be equal?

Answer:

Yes.

Hash collisions are allowed.

`hashCode()` is used to narrow the search area; `equals()` determines actual logical equality.

---

# 31. Interview Scenario 3

Interviewer:

> Can two objects be equal but have different hashCodes?

Answer:

No.

That violates the `hashCode()` contract and can cause hash-based collections to behave incorrectly.

---

# 32. Quick Revision

```text
HashMap Key
    |
    +---- hashCode()
    |        |
    |        ↓
    |      Bucket
    |
    +---- equals()
             |
             ↓
       Exact Key Match
```

Remember:

```text
Equal objects
      ↓
Same hashCode

Same hashCode
      ↓
NOT necessarily equal
```

---

# 33. Golden Rule ⭐

Memorize this for interviews:

> **If two objects are equal according to equals(), they must have the same hashCode(). But two objects having the same hashCode() does not mean they are equal.**

---

# 34. One-Line Summary

**HashMap relies on the `hashCode()` and `equals()` contract: `hashCode()` helps locate the bucket, while `equals()` determines the exact key, so custom HashMap keys must implement both methods consistently.**
