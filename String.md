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

# Difference Between == and equals() in Java



Both `==` and `equals()` are used for comparison in Java, but they work differently.

- `==` compares memory references (addresses).
- `equals()` compares actual content (values).

For primitive data types:

```java
int a = 10;
int b = 10;

System.out.println(a == b);
```

Output:

```text
true
```

Because primitive values are compared directly.

For objects:

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

Because both objects have different memory locations.

---

# == Operator

## Definition

The `==` operator compares references for objects.

It checks whether both references point to the same memory location.

---

## Example

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

Memory:

```text
Heap

+--------+      +--------+
| Java   |      | Java   |
+--------+      +--------+
    ↑               ↑
   s1              s2
```

Different objects.

---

# equals() Method

## Definition

The `equals()` method compares actual content.

---

## Example

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1.equals(s2));
```

Output:

```text
true
```

Because both Strings contain:

```text
Java
```

---

# Example 1

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
true
true
```

Reason:

Both refer to same pooled object.

---

# Example 2

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
false
true
```

Reason:

Different references but same content.

---

# Example 3

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
false
true
```

Reason:

Different memory locations.

---

# Why equals() Works For String?

String class overrides Object class equals() method.

Object class:

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

String class overrides it to compare character content.

---

# Custom Class Example

Without overriding equals():

```java
class Employee {

    int id;

    Employee(int id) {
        this.id = id;
    }
}
```

```java
Employee e1 = new Employee(1);
Employee e2 = new Employee(1);

System.out.println(e1.equals(e2));
```

Output:

```text
false
```

Because Object class implementation compares references.

---

# After Overriding equals()

```java
class Employee {

    int id;

    Employee(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {

        Employee emp = (Employee) obj;

        return this.id == emp.id;
    }
}
```

Now:

```java
Employee e1 = new Employee(1);
Employee e2 = new Employee(1);

System.out.println(e1.equals(e2));
```

Output:

```text
true
```

---

# Frequently Asked Interview Questions

## Q1: What is the difference between == and equals()?

### Answer

```text
==       → Reference Comparison
equals() → Content Comparison
```

---

## Q2: Can == Compare Strings?

### Answer

Yes.

But it compares references, not content.

---

## Q3: Why Does String Override equals()?

### Answer

To compare actual character content instead of memory addresses.

---

## Q4: Can We Override == Operator?

### Answer

No.

Java does not support operator overloading for ==.

---

## Q5: Can equals() Be Overridden?

### Answer

Yes.

Many classes override equals().

Example:

```java
String
Integer
Employee
User
```

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

Because both point to same String Pool object.

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

System.out.println(s1.equals(s2));
```

### Answer

```text
true
```

Contents are same.

---

## Question

What Will Be Output?

```java
String s1 = null;

System.out.println(s1.equals("Java"));
```

### Answer

```text
NullPointerException
```

---

## Safer Approach

```java
System.out.println("Java".equals(s1));
```

Output:

```text
false
```

No exception.

---

# Common Interview Trap

## Question

Which is Faster?

```java
==
```

or

```java
equals()
```

### Answer

`==` is slightly faster because it compares references only.

`equals()` compares content.

However, use the correct operator based on requirement, not performance.

---

# Key Points For Revision

- `==` compares references.
- `equals()` compares content.
- String overrides equals().
- Object class equals() compares references.
- equals() can be overridden.
- `==` cannot be overridden.
- Use `"value".equals(variable)` to avoid NullPointerException.

---

# One-Line Summary

`==` checks whether two references point to the same object, whereas `equals()` checks whether two objects contain the same data.

# Difference Between String, StringBuilder, and StringBuffer


String, StringBuilder, and StringBuffer are all used to represent and manipulate character sequences in Java.

The main difference is:

- String is immutable.
- StringBuilder is mutable and not thread-safe.
- StringBuffer is mutable and thread-safe.

If frequent modifications are required, StringBuilder or StringBuffer should be preferred over String.

---

# String

## Definition

String is an immutable class.

Once created, its value cannot be changed.

---

## Example

```java
String str = "Java";

str.concat(" Programming");

System.out.println(str);
```

Output:

```text
Java
```

Reason:

A new object is created but not assigned.

---

## Internal Working

```java
String str = "Java";

str = str + "8";
```

Memory:

```text
Java
  ↓
Create New Object
  ↓
Java8
```

Every modification creates a new object.

---

# StringBuilder

## Definition

StringBuilder is a mutable class introduced in Java 5.

Changes happen on the same object.

It is not synchronized.

Therefore:

```text
Not Thread Safe
```

---

## Example

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

Same object gets modified.

---

# StringBuffer

## Definition

StringBuffer is a mutable class.

It is synchronized.

Therefore:

```text
Thread Safe
```

---

## Example

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

---

# Why Were StringBuilder and StringBuffer Introduced?

Consider:

```java
String str = "";

for(int i=0;i<1000;i++) {
    str += i;
}
```

Every iteration creates a new String object.

Result:

```text
High Memory Usage
Poor Performance
```

Solution:

```java
StringBuilder
```

or

```java
StringBuffer
```

---

# Difference Between String, StringBuilder, and StringBuffer

| Feature | String | StringBuilder | StringBuffer |
|----------|----------|----------|----------|
| Mutable | No | Yes | Yes |
| Thread Safe | Yes | No | Yes |
| Performance | Slow | Fastest | Slower than StringBuilder |
| Synchronization | Not Required | No | Yes |
| Introduced In | Java 1.0 | Java 5 | Java 1.0 |

---

# Memory Example

## String

```java
String str = "Java";

str += "8";
```

Memory:

```text
Java
 ↓
Java8
```

New object created.

---

## StringBuilder

```java
StringBuilder sb = new StringBuilder("Java");

sb.append("8");
```

Memory:

```text
Java
 ↓
Modify Same Object
 ↓
Java8
```

No new object.

---

# Performance Comparison

Example:

```java
for(int i=0;i<100000;i++) {
}
```

Appending data:

```java
StringBuilder
```

is fastest because:

```text
No Synchronization
Mutable
```

---

# When Should We Use String?

Use when:

```text
Data will not change frequently.
```

Example:

```java
String name = "Chetan";
```

---

# When Should We Use StringBuilder?

Use when:

```text
Frequent modifications
Single Threaded Application
```

Example:

```java
Generating Reports
Building SQL Queries
Building JSON Responses
```

---

# When Should We Use StringBuffer?

Use when:

```text
Frequent modifications
Multi-Threaded Application
```

---

# Frequently Asked Interview Questions

## Q1: Why is StringBuilder Faster Than String?

### Answer

Because StringBuilder modifies the same object.

String creates a new object for every modification.

---

## Q2: Why is StringBuilder Faster Than StringBuffer?

### Answer

StringBuilder is not synchronized.

No locking overhead.

---

## Q3: Which Is Thread Safe?

### Answer

```java
StringBuffer
```

---

## Q4: Is String Thread Safe?

### Answer

Yes.

Because String is immutable.

---

## Q5: Which Is Faster?

### Answer

```text
StringBuilder > StringBuffer > String
```

---

# Tricky Interview Questions

## Question

What Will Be Output?

```java
String str = "Java";

str.concat("8");

System.out.println(str);
```

### Answer

```text
Java
```

String is immutable.

---

## Question

What Will Be Output?

```java
StringBuilder sb = new StringBuilder("Java");

sb.append("8");

System.out.println(sb);
```

### Answer

```text
Java8
```

Same object modified.

---

## Question

Which Class Should Be Used For String Concatenation Inside Loops?

### Answer

```java
StringBuilder
```

---

## Question

Why Is StringBuffer Slower?

### Answer

Because methods are synchronized.

Example:

```java
public synchronized StringBuffer append(String str)
```

Synchronization adds overhead.

---

## Question

Can StringBuilder Be Used In Multithreading?

### Answer

Yes.

But it is not thread-safe.

External synchronization is required.

---

# Common Interview Trap

## Question

Which One Should Always Be Preferred?

### Wrong Answer

```text
StringBuilder
```

### Correct Answer

Depends on the use case.

```text
No Modification → String

Frequent Modification + Single Thread → StringBuilder

Frequent Modification + Multi Thread → StringBuffer
```

---

# Real Project Examples

## String

```java
String username = "admin";
```

---

## StringBuilder

```java
StringBuilder query = new StringBuilder();

query.append("SELECT * FROM EMPLOYEE");
```

---

## StringBuffer

```java
Shared Log Message Generation
Shared Report Generation
```

Multi-threaded environment.

---

# Key Points For Revision

- String is immutable.
- StringBuilder is mutable and not thread-safe.
- StringBuffer is mutable and thread-safe.
- StringBuilder is faster than StringBuffer.
- Use StringBuilder for frequent modifications.
- Use StringBuffer in multi-threaded scenarios.

---

# One-Line Summary

String is immutable, StringBuilder is mutable and fast, while StringBuffer is mutable and thread-safe due to synchronization.

# Difference Between String Literal and new String() in Java

In Java, Strings can be created in two ways:

1. Using String Literal
2. Using new String() Keyword

The main difference is:

- String Literal uses the String Constant Pool.
- new String() creates a new object in Heap memory.

String literals are preferred because they save memory by reusing existing objects from the String Pool.

---

# String Literal

## Example

```java
String s1 = "Java";
String s2 = "Java";
```

JVM first checks the String Pool.

If the String already exists:

```text
Reuse Existing Object
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

Only one object is created.

---

# new String()

## Example

```java
String s1 = new String("Java");
String s2 = new String("Java");
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

+--------+
| "Java" |
+--------+
```

Each call creates a new Heap object.

---

# Comparison Example

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

Reason:

Both references point to same pooled object.

---

# Example Using new String()

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

Reason:

Both references point to different Heap objects.

---

# Why String Literal Is Preferred?

## Memory Efficient

```java
String s1 = "Java";
String s2 = "Java";
String s3 = "Java";
```

Only one object exists in memory.

---

## Better Performance

No unnecessary object creation.

Object reuse improves performance.

---

# Internal Working

## String Literal

```java
String s = "Java";
```

JVM:

```text
1. Check String Pool
2. If Present → Reuse
3. If Not Present → Create
```

---

## new String()

```java
String s = new String("Java");
```

JVM:

```text
1. Check String Pool
2. Create Object In Pool (if absent)
3. Create New Object In Heap
4. Return Heap Reference
```

---

# Difference Between String Literal and new String()

| Feature | String Literal | new String() |
|----------|----------|----------|
| Memory Location | String Pool | Heap |
| Object Reuse | Yes | No |
| Memory Efficient | Yes | No |
| Performance | Better | Slightly Slower |
| Recommended | Yes | Usually No |

---

# Real-World Example

## Preferred

```java
String role = "ADMIN";
```

Reason:

Fixed values should use String Pool.

---

## Less Common

```java
String role = new String("ADMIN");
```

Creates unnecessary object.

---

# Frequently Asked Interview Questions

## Q1: Which Is Better?

```java
String s = "Java";
```

or

```java
String s = new String("Java");
```

### Answer

String Literal.

Because it uses String Pool and saves memory.

---

## Q2: Where Is String Literal Stored?

### Answer

String Constant Pool.

---

## Q3: Where Is new String() Object Stored?

### Answer

Heap Memory.

---

## Q4: Does new String() Use String Pool?

### Answer

Yes.

But it additionally creates a new Heap object.

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

Does new String("Java") Create Only One Object?

### Answer

No.

Generally:

```text
1 Object In String Pool
1 Object In Heap
```

Total:

```text
2 Objects
```

(If "Java" is not already present in the Pool.)

---

# Key Points For Revision

- String Literal uses String Pool.
- new String() creates Heap object.
- String Literal is memory efficient.
- String Literal is preferred.
- new String() may create two objects.
- String Pool avoids duplicate object creation.

---

# One-Line Summary

String literals reuse objects from the String Constant Pool, while new String() always creates a new object in Heap memory.
# How Many Objects Are Created in String?


This is one of the most popular Java interview questions.

The number of String objects created depends on:

- Whether the String already exists in the String Pool
- Whether String Literal or new String() is used

To answer correctly, we must understand:

```text
String Pool
+
Heap Memory
```

and how JVM creates String objects.

---

# Scenario 1

## Code

```java
String s = "Java";
```

## Objects Created

```text
1 Object
```

Memory:

```text
String Pool

+--------+
| "Java" |
+--------+
```

If "Java" does not already exist in the pool.

---

# Scenario 2

## Code

```java
String s1 = "Java";
String s2 = "Java";
```

## Objects Created

```text
1 Object
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

JVM reuses the existing pooled object.

---

# Scenario 3

## Code

```java
String s = new String("Java");
```

## Objects Created

```text
2 Objects
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

Explanation:

1. JVM creates "Java" in String Pool (if not already present)
2. JVM creates another object in Heap
3. Reference points to Heap object

---

# Scenario 4

## Code

```java
String s1 = new String("Java");
String s2 = new String("Java");
```

## Objects Created

```text
3 Objects
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

+--------+
| "Java" |
+--------+
```

Explanation:

```text
1 Object in Pool
2 Objects in Heap
```

Total:

```text
3 Objects
```

---

# Scenario 5

## Code

```java
String s1 = "Java";
String s2 = new String("Java");
```

## Objects Created

```text
2 Objects
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

Explanation:

```text
1 Pool Object
1 Heap Object
```

---

# Scenario 6

## Code

```java
String s1 = new String("Java");
String s2 = s1.intern();
```

## Objects Created

```text
2 Objects
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

intern() does not create a new object.

It simply returns the pooled reference.

---

# Scenario 7

## Code

```java
String s1 = "Ja";
String s2 = "va";

String s3 = "Java";
```

## Objects Created

```text
3 Objects
```

Memory:

```text
"Ja"
"va"
"Java"
```

All stored in String Pool.

---

# Scenario 8

## Code

```java
String s1 = "Ja" + "va";
```

## Objects Created

```text
1 Object
```

Reason:

Compiler performs optimization.

Compile-time conversion:

```java
String s1 = "Java";
```

---

# Scenario 9

## Code

```java
String s1 = "Ja";

String s2 = s1 + "va";
```

## Objects Created

More than one object.

Reason:

Concatenation happens at runtime.

Internally JVM uses:

```java
StringBuilder
```

---

# Frequently Asked Interview Questions

## Q1: How Many Objects Are Created?

```java
String s = "Java";
```

### Answer

```text
1 Object
```

---

## Q2: How Many Objects Are Created?

```java
String s = new String("Java");
```

### Answer

```text
2 Objects
```

(If not already present in String Pool)

---

## Q3: How Many Objects Are Created?

```java
String s1 = "Java";
String s2 = "Java";
```

### Answer

```text
1 Object
```

---

## Q4: How Many Objects Are Created?

```java
String s1 = new String("Java");
String s2 = new String("Java");
```

### Answer

```text
3 Objects
```

---

## Q5: Does intern() Create New Object?

### Answer

No.

It returns a reference from String Pool.

---

# Tricky Interview Questions

## Question

How Many Objects Are Created?

```java
String s1 = "Java";
String s2 = new String("Java");
```

### Answer

```text
2 Objects
```

---

## Question

How Many Objects Are Created?

```java
String s1 = "Java";
String s2 = "Java";
String s3 = "Java";
```

### Answer

```text
1 Object
```

---

## Question

How Many Objects Are Created?

```java
String s1 = "Ja" + "va";
```

### Answer

```text
1 Object
```

Compiler optimization.

---

## Question

How Many Objects Are Created?

```java
String s1 = "Ja";

String s2 = s1 + "va";
```

### Answer

Runtime concatenation occurs.

Additional objects may be created internally using StringBuilder.

---

# Common Interview Trap

## Question

Is the Answer Always "2 Objects" for new String()?

### Answer

No.

If the String already exists in the Pool:

```java
new String("Java");
```

creates only:

```text
1 New Heap Object
```

because Pool object already exists.

---

# Key Points For Revision

- String literals use String Pool.
- Duplicate literals do not create new objects.
- new String() creates Heap object.
- intern() does not create new object.
- Compile-time concatenation is optimized.
- Runtime concatenation creates extra objects.

---

# One-Line Summary

The number of String objects created depends on whether the String already exists in the String Pool and whether the String is created using literals, new String(), or concatenation.
