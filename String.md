# What is String? Why is String Immutable in Java?

## 

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

# What is String Constant Pool (SCP)? How Does It Work?

## Interview Answer (1-2 Minutes)

The String Constant Pool (SCP) is a special memory area inside the Heap where JVM stores String literals.

The main purpose of the String Pool is:

- Memory Optimization
- Reusability
- Improved Performance

When a String literal is created, JVM first checks whether the same String already exists in the String Pool.

If it exists:

```text
Reuse Existing Object
```

If it does not exist:

```text
Create New Object
```

and store it in the String Pool.

---

# Example

```java
String s1 = "Java";
String s2 = "Java";
```

Memory:

```text
String Pool

+--------+
| "Java" |
+--------+
   ↑
  / \
s1   s2
```

Both variables point to the same object.

---

# Proof

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

Because both references point to the same object.

---

# Why Was String Pool Introduced?

Suppose:

```java
String s1 = "Java";
String s2 = "Java";
String s3 = "Java";
String s4 = "Java";
```

Without String Pool:

```text
4 Objects
```

With String Pool:

```text
1 Object
```

This saves memory.

---

# How JVM Handles String Literals

Example:

```java
String s1 = "Java";
```

Step 1:

JVM checks String Pool.

```text
Does "Java" exist?
```

Step 2:

If No:

```text
Create Object
Store In Pool
```

Step 3:

If Yes:

```text
Return Existing Reference
```

---

# String Literal vs new String()

This is one of the most asked interview questions.

---

## Using String Literal

```java
String s1 = "Java";
String s2 = "Java";
```

Memory:

```text
String Pool

+--------+
| "Java" |
+--------+
```

Only one object created.

---

## Using new String()

```java
String s1 = new String("Java");
```

Memory:

```text
String Pool

+--------+
| "Java" |
+--------+

Heap

+--------+
| "Java" |
+--------+
```

Two objects are created.

---

# Example

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

Reason:

Different memory locations.

---

# How Many Objects Are Created?

## Question 1

```java
String s = "Java";
```

Answer:

```text
1 Object
```

If not already present in pool.

---

## Question 2

```java
String s = new String("Java");
```

Answer:

```text
2 Objects
```

1 in String Pool

1 in Heap

---

## Question 3

```java
String s1 = "Java";
String s2 = "Java";
```

Answer:

```text
1 Object
```

Because pool reuses object.

---

## Question 4

```java
String s1 = new String("Java");
String s2 = new String("Java");
```

Answer:

```text
3 Objects
```

1 in Pool

2 in Heap

---

# What is intern() Method?

The intern() method adds a String to the String Pool if it is not already present.

Example:

```java
String s1 = new String("Java");

String s2 = s1.intern();

String s3 = "Java";
```

```java
System.out.println(s2 == s3);
```

Output:

```text
true
```

Because both refer to the pooled String.

---

# Frequently Asked Interview Questions

## Q1: What is String Constant Pool?

### Answer

A special memory area in Heap where JVM stores String literals to avoid duplicate object creation.

---

## Q2: Why is String Pool Used?

### Answer

To save memory and improve performance.

---

## Q3: Where Is String Pool Stored?

### Answer

Inside Heap Memory.

(Java 7 onwards)

---

## Q4: What is intern() Method?

### Answer

It returns the reference from the String Pool.

---

## Q5: Which Is Better?

```java
String s = "Java";
```

or

```java
String s = new String("Java");
```

### Answer

String Literal.

Because it utilizes the String Pool.

---

# Tricky Interview Questions

## Question

What Will Be Output?

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

### Answer

```text
true
```

Same pooled object.

---

## Question

What Will Be Output?

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

### Answer

```text
false
```

Different Heap objects.

---

## Question

What Will Be Output?

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);
```

### Answer

```text
false
```

Different memory locations.

---

# Common Interview Trap

## Question

Is String Pool Stored In Stack Memory?

### Answer

No.

String Pool is stored inside Heap Memory.

(Java 7+)

---

# Key Points For Revision

- String Pool stores String literals.
- Duplicate literals reuse existing objects.
- String Pool saves memory.
- String literals are preferred over new String().
- intern() returns pooled reference.
- String Pool resides in Heap.

---

# One-Line Summary

String Constant Pool is a special area in Heap that stores String literals and reuses existing objects to improve memory efficiency and performance.
