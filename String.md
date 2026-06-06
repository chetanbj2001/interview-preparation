# What is String? Why is String Immutable in Java?

## Interview Answer (1-2 Minutes)

A String in Java is an object that represents a sequence of characters.

It is one of the most commonly used classes in Java and belongs to the `java.lang` package.

Example:

```java
String name = "Chetan";
```

Internally, String is implemented as a class and not as a primitive data type.

One of the most important characteristics of String is that it is immutable.

Immutable means:

```text
Once a String object is created, its value cannot be changed.
```

If we modify a String, Java creates a new String object instead of modifying the existing one.

---

# Example of Immutability

```java
String s = "Java";

s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java
```

Reason:

```java
s.concat(" Programming");
```

creates a new String object but does not assign it back to `s`.

---

# Correct Way

```java
String s = "Java";

s = s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java Programming
```

---

# How String Immutability Works

Example:

```java
String s1 = "Java";
```

Memory:

```text
String Pool
+--------+
| "Java" |
+--------+
```

Now:

```java
s1 = s1.concat("8");
```

JVM creates a new object:

```text
String Pool
+--------+
| "Java" |
+--------+

Heap
+---------+
| "Java8" |
+---------+
```

Original String remains unchanged.

---

# Why String is Immutable?

This is one of the most asked interview questions.

Java designers made String immutable for several reasons.

---

# 1. Security

String is heavily used in:

```text
Database URLs
File Paths
Network Connections
Usernames
Passwords
```

Example:

```java
String url = "jdbc:mysql://localhost:3306/test";
```

If Strings were mutable, someone could modify the URL after validation.

Immutability prevents this security issue.

---

# 2. String Constant Pool

Java stores String literals inside the String Pool.

Example:

```java
String s1 = "Java";
String s2 = "Java";
```

Both variables point to the same object.

Memory:

```text
String Pool

+--------+
| "Java" |
+--------+
   ↑
 s1,s2
```

This saves memory.

If Strings were mutable, changing one reference would affect others.

---

# 3. Thread Safety

Immutable objects are naturally thread-safe.

Example:

```java
String message = "Welcome";
```

Multiple threads can use the same String object without synchronization.

Because no thread can modify it.

---

# 4. HashMap Performance

String is commonly used as a key.

Example:

```java
Map<String, Integer> map = new HashMap<>();
```

HashMap relies on:

```java
hashCode()
```

If String were mutable:

```text
HashCode could change
```

and HashMap lookup would fail.

Immutability ensures consistent hashCode.

---

# How String is Made Immutable?

String class is declared as:

```java
public final class String
```

`final` prevents inheritance.

Internal data is private.

Example:

```java
private final byte[] value;
```

The value cannot be modified after creation.

---

# Frequently Asked Interview Questions

## Q1: What is String?

### Answer

String is an immutable object used to represent a sequence of characters.

---

## Q2: What does Immutable mean?

### Answer

Once an object is created, its state cannot be changed.

---

## Q3: Why is String Immutable?

### Answer

- Security
- String Pool
- Thread Safety
- HashMap Performance

---

## Q4: Is String a Primitive Data Type?

### Answer

No.

String is a class from:

```java
java.lang.String
```

---

## Q5: Is String Thread Safe?

### Answer

Yes.

Because String is immutable.

---

## Q6: Why is String Declared Final?

### Answer

To prevent inheritance and maintain immutability.

---

# Tricky Interview Questions

## Question

Does concat() modify the original String?

### Answer

No.

It creates a new String object.

---

## Question

Can We Make String Mutable?

### Answer

No.

String itself is immutable.

For mutable strings use:

```java
StringBuilder
```

or

```java
StringBuffer
```

---

## Question

What Happens Here?

```java
String s = "Java";

s.concat("8");

System.out.println(s);
```

Output:

```text
Java
```

Reason:

The newly created String is not assigned back.

---

# Common Interview Trap

## Question

Why not make String mutable and save memory?

### Answer

Making String mutable would break:

- Security
- String Pool
- Thread Safety
- HashMap Key Stability

The benefits of immutability outweigh the memory cost.

---

# Key Points for Revision

- String is a class, not a primitive.
- String objects are immutable.
- Any modification creates a new object.
- String is final.
- String is thread-safe due to immutability.
- Immutability helps Security, String Pool, and HashMap.

---

# One-Line Summary

String is an immutable, final class in Java whose value cannot be changed after creation, making it secure, thread-safe, and memory efficient through String Pooling.
