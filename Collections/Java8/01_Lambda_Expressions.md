# Lambda Expressions

Lambda Expressions were introduced in **Java 8** to enable **functional programming** and make it easier to pass behavior as an argument.

They are mainly used with **Functional Interfaces**.

---

# 1. Lambda Concepts

## 1.1 What is a Lambda Expression?

A Lambda Expression is a short way of representing an **anonymous function** — a block of code that can be passed around and executed later.

### Without Lambda

```java
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello Java");
    }
};

runnable.run();
```

### With Lambda

```java
Runnable runnable = () -> {
    System.out.println("Hello Java");
};

runnable.run();
```

Lambda removes the unnecessary boilerplate of the anonymous class.

### Simple Definition

> Lambda Expression is a concise way to provide the implementation of a Functional Interface.

---

# 1.2 Why Was Lambda Introduced?

Before Java 8, passing behavior required anonymous inner classes.

For example:

```java
List<String> names = Arrays.asList("Chetan", "Rahul", "Amit");

Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
});
```

With Lambda:

```java
Collections.sort(names, (a, b) -> a.compareTo(b));
```

Lambda provides:

* Less boilerplate code
* Better readability
* Ability to pass behavior as data
* Support for functional programming
* Better integration with the Java 8 Stream API

---

# 1.3 Lambda Syntax

Basic syntax:

```text
(parameters) -> expression
```

or

```text
(parameters) -> {
    statements;
}
```

### No parameter

```java
() -> System.out.println("Hello");
```

### One parameter

```java
name -> System.out.println(name);
```

Parentheses are optional for a single implicitly typed parameter.

Both are valid:

```java
name -> System.out.println(name);

(name) -> System.out.println(name);
```

### Multiple parameters

```java
(a, b) -> a + b
```

### Multiple statements

```java
(a, b) -> {
    int sum = a + b;
    return sum;
}
```

---

# 1.4 Lambda with Functional Interface

A Lambda needs a **target type**.

Usually, that target type is a **Functional Interface**.

Example:

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

Lambda implementation:

```java
Calculator calculator = (a, b) -> a + b;

System.out.println(calculator.add(10, 20));
```

Output:

```text
30
```

The Lambda provides the implementation of the single abstract method.

> Detailed Functional Interfaces are covered separately in `02_Functional_Interfaces.md`.

---

# 1.5 Lambda with Parameters

Lambda can accept parameters.

### One parameter

```java
interface Printer {
    void print(String message);
}

Printer printer = message -> System.out.println(message);

printer.print("Hello");
```

### Multiple parameters

```java
interface Calculator {
    int calculate(int a, int b);
}

Calculator addition = (a, b) -> a + b;

System.out.println(addition.calculate(10, 20));
```

### Explicit parameter types

You can explicitly specify parameter types:

```java
Calculator addition = (int a, int b) -> a + b;
```

But you cannot mix implicit and explicit parameter types:

```java
// Invalid
// (int a, b) -> a + b
```

Use either:

```java
(a, b) -> a + b
```

or:

```java
(int a, int b) -> a + b
```

---

# 1.6 Lambda with Return Value

If the Lambda contains a single expression, the return is implicit.

```java
Calculator calculator = (a, b) -> a + b;
```

Equivalent to:

```java
Calculator calculator = (a, b) -> {
    return a + b;
};
```

### Multiple statements

When using a block body, `return` is required when the method returns a value.

```java
Calculator calculator = (a, b) -> {
    int result = a + b;
    return result;
};
```

This is invalid:

```java
// (a, b) -> {
//     int result = a + b;
//     result;
// }
```

---

# 1.7 Lambda vs Anonymous Inner Class

### Anonymous Inner Class

```java
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};
```

### Lambda

```java
Runnable runnable = () -> {
    System.out.println("Running");
};
```

## Difference

| Lambda                                         | Anonymous Inner Class                   |
| ---------------------------------------------- | --------------------------------------- |
| Introduced in Java 8                           | Available before Java 8                 |
| Less boilerplate                               | More boilerplate                        |
| Mainly used with Functional Interfaces         | Can implement interfaces/classes        |
| `this` refers to enclosing object              | `this` refers to anonymous class object |
| Cannot declare instance fields in the same way | Can have fields and methods             |
| Cannot have constructors                       | Can have initialization logic           |
| Focuses on behavior                            | Represents an anonymous class instance  |

### Important Interview Point

Lambda is **not simply another syntax for an anonymous class**.

Its semantics are different, especially regarding `this`.

---

# 1.8 Variable Capture

A Lambda can access variables from its enclosing scope.

Example:

```java
public class Demo {

    public static void main(String[] args) {

        String message = "Hello";

        Runnable runnable = () -> {
            System.out.println(message);
        };

        runnable.run();
    }
}
```

Output:

```text
Hello
```

The Lambda captures the local variable `message`.

This is called **variable capture**.

---

# 1.9 Effectively Final Variables

A local variable used inside a Lambda must be:

* `final`, or
* **effectively final**

Example:

```java
String message = "Hello";

Runnable runnable = () -> {
    System.out.println(message);
};
```

This works because `message` is never reassigned.

### Explicit final

```java
final String message = "Hello";

Runnable runnable = () -> {
    System.out.println(message);
};
```

Also valid.

### Not effectively final

```java
String message = "Hello";

message = "Hi";

Runnable runnable = () -> {
    // System.out.println(message); // Compile-time error
};
```

Because `message` was reassigned.

### Why?

Local variables live on the stack, while a Lambda may outlive the method invocation.

Java therefore captures the value rather than allowing unrestricted modification of the local variable.

---

# 1.10 Can Lambda Modify an Effectively Final Variable?

No.

```java
int count = 10;

Runnable runnable = () -> {
    // count++; // Compile-time error
};
```

Even though `count` is effectively final, it cannot be modified.

---

# 1.11 Can Lambda Modify an Object Referenced by an Effectively Final Variable?

Yes.

The **reference** must not change, but the object itself may be mutable.

```java
List<String> names = new ArrayList<>();

Runnable runnable = () -> {
    names.add("Chetan");
};

runnable.run();

System.out.println(names);
```

Output:

```text
[Chetan]
```

This works because the variable `names` still points to the same object.

This would not be allowed:

```java
List<String> names = new ArrayList<>();

Runnable runnable = () -> {
    // names = new ArrayList<>(); // Compile-time error
};
```

### Important

```text
Reference cannot change
        ↓
Object can potentially change
```

---

# 1.12 `this` Inside Lambda

One important difference from an anonymous inner class is the meaning of `this`.

### Lambda

```java
class Employee {

    private String name = "Chetan";

    void printName() {

        Runnable runnable = () -> {
            System.out.println(this.name);
        };

        runnable.run();
    }
}
```

Here:

```java
this
```

refers to the **enclosing `Employee` object**.

Lambda does not create a new `this`.

---

# 1.13 Lambda and `this` vs Anonymous Class

### Lambda

```java
class Demo {

    void test() {

        Runnable r = () -> {
            System.out.println(this);
        };
    }
}
```

`this` refers to the `Demo` object.

### Anonymous Class

```java
class Demo {

    void test() {

        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println(this);
            }
        };
    }
}
```

Here `this` refers to the **anonymous Runnable object**.

### Interview Question

> What does `this` refer to inside a Lambda?

Answer:

> `this` refers to the enclosing class instance because Lambda expressions do not introduce a new `this`.

---

# 1.14 Lambda and Local Variables

Lambda can access:

```java
class Demo {

    public static void main(String[] args) {

        String language = "Java";

        Runnable r = () -> {
            System.out.println(language);
        };

        r.run();
    }
}
```

But the local variable must be final or effectively final.

---

# 1.15 Lambda as Method Argument

Lambda allows behavior to be passed as an argument.

Example:

```java
interface Operation {
    int execute(int a, int b);
}

static int calculate(int a, int b, Operation operation) {
    return operation.execute(a, b);
}
```

Now different behavior can be passed:

```java
int addition = calculate(10, 20, (a, b) -> a + b);

int multiplication = calculate(10, 20, (a, b) -> a * b);

System.out.println(addition);
System.out.println(multiplication);
```

Output:

```text
30
200
```

This is one of the most important ideas behind Lambda:

> **Pass behavior as an argument.**

---

# 1.16 Lambda with Collections

Lambda is commonly used with Collections.

```java
List<String> names =
        Arrays.asList("Chetan", "Amit", "Rahul");

names.forEach(name -> System.out.println(name));
```

Before Java 8, iteration commonly looked like:

```java
for (String name : names) {
    System.out.println(name);
}
```

Lambda works especially well with Java 8 collection APIs and Streams.

---

# 1.17 Lambda with Sorting

Before Java 8:

```java
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
});
```

With Lambda:

```java
Collections.sort(names, (a, b) -> a.compareTo(b));
```

Java 8 also provides:

```java
names.sort((a, b) -> a.compareTo(b));
```

---

# 1.18 Lambda Limitations

Lambda expressions have some limitations.

### 1. Lambda requires a target type

This is not valid by itself:

```java
// () -> System.out.println("Hello");
```

It needs a compatible target type:

```java
Runnable r = () -> System.out.println("Hello");
```

---

### 2. Only works with Functional Interfaces

A Lambda provides implementation for a single abstract method.

It cannot directly implement an interface having multiple abstract methods.

---

### 3. Local variables must be final/effectively final

```java
int count = 10;

Runnable r = () -> {
    // count++; // ❌
};
```

---

### 4. Lambda cannot have its own constructor

Lambda is not a normal class definition.

---

### 5. Lambda can become difficult to read

Very complex Lambda expressions can reduce readability.

Prefer extracting complex logic into a method.

---

# 2. Lambda Interview Questions

## Q1. What is Lambda Expression?

**Answer:**

> Lambda Expression is a concise way to represent an anonymous function and provide an implementation for a Functional Interface. It was introduced in Java 8 to support functional programming and reduce boilerplate code.

---

## Q2. Why was Lambda introduced in Java 8?

Main reasons:

* Reduce anonymous class boilerplate
* Pass behavior as an argument
* Support functional programming
* Improve Collection APIs
* Enable cleaner Stream API operations

---

## Q3. What is the syntax of Lambda?

```java
(parameters) -> expression
```

or:

```java
(parameters) -> {
    statements;
}
```

Example:

```java
(a, b) -> a + b
```

---

## Q4. Can Lambda have zero parameters?

Yes.

```java
Runnable r = () -> System.out.println("Hello");
```

---

## Q5. Can Lambda have multiple parameters?

Yes.

```java
(a, b) -> a + b
```

---

## Q6. Can Lambda have a return value?

Yes.

```java
(a, b) -> a + b
```

For multiple statements:

```java
(a, b) -> {
    int result = a + b;
    return result;
}
```

---

## Q7. What is a Functional Interface?

A Functional Interface is an interface containing **exactly one abstract method**.

Example:

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

Lambda can provide its implementation.

> Functional Interfaces are covered in detail in `02_Functional_Interfaces.md`.

---

## Q8. Can Lambda be used without a Functional Interface?

A Lambda expression requires a **target type**. The common target type is a Functional Interface.

For example:

```java
Runnable r = () -> System.out.println("Hello");
```

The Lambda gets its target type from `Runnable`.

---

## Q9. What is effectively final?

A variable is effectively final if it is initialized once and never reassigned.

```java
String name = "Chetan";

Runnable r = () -> {
    System.out.println(name);
};
```

No explicit `final` is required.

---

## Q10. Can Lambda modify a local variable?

No.

```java
int count = 0;

Runnable r = () -> {
    // count++; // ❌
};
```

The variable must be final or effectively final.

---

## Q11. Can Lambda modify an object?

Yes, if the reference itself is not reassigned.

```java
List<String> list = new ArrayList<>();

Runnable r = () -> {
    list.add("Java");
};
```

This is valid.

---

## Q12. What does `this` refer to inside Lambda?

`this` refers to the **enclosing class instance**.

Lambda does not create a new `this`.

---

## Q13. Lambda vs Anonymous Inner Class?

### Lambda

```java
Runnable r = () -> System.out.println("Hello");
```

### Anonymous class

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello");
    }
};
```

Lambda is more concise, and `this` has different semantics.

---

## Q14. Can Lambda contain multiple statements?

Yes.

```java
(a, b) -> {
    int sum = a + b;
    System.out.println(sum);
    return sum;
}
```

When the Lambda has a block body and returns a value, `return` is required.

---

## Q15. Can Lambda throw exceptions?

Yes, but the exception must be compatible with the abstract method's `throws` declaration.

Example:

```java
@FunctionalInterface
interface Task {
    void execute() throws Exception;
}

Task task = () -> {
    throw new Exception("Error");
};
```

If the functional method doesn't declare a checked exception, the Lambda cannot directly throw that checked exception.

---

# 3. Lambda Tricky Questions

## Q1. Why can't Lambda modify local variables?

Because captured local variables must be final or effectively final.

```java
int count = 10;

Runnable r = () -> {
    // count++; // ❌
};
```

Java captures the value of the local variable rather than providing unrestricted access to a mutable stack variable.

---

## Q2. Why can Lambda modify an object but not the local variable reference?

Consider:

```java
List<String> list = new ArrayList<>();

Runnable r = () -> {
    list.add("Java"); // ✅
};
```

The reference `list` doesn't change.

But:

```java
Runnable r = () -> {
    // list = new ArrayList<>(); // ❌
};
```

changes the reference.

Therefore:

```text
Reference → must remain effectively final
Object    → may be mutable
```

---

## Q3. Does Lambda create an anonymous class?

Not conceptually.

Lambda expressions have different runtime and language semantics from anonymous inner classes.

For example, `this` behaves differently.

### Lambda

```java
() -> this
```

`this` refers to the enclosing object.

### Anonymous class

```java
new Runnable() {
    public void run() {
        System.out.println(this);
    }
}
```

`this` refers to the anonymous class instance.

---

## Q4. Can we use the same Lambda for different Functional Interfaces?

Sometimes, if the target types are compatible, but Lambda expressions themselves don't have a standalone type.

For example:

```java
Runnable r = () -> System.out.println("Hello");
```

The Lambda gets its type from the target context.

This is called **target typing**.

---

## Q5. Why is target typing important?

Consider:

```java
Runnable r = () -> System.out.println("Hello");
```

Java knows the Lambda must implement:

```java
void run()
```

because the target type is `Runnable`.

A Lambda therefore depends on the surrounding context to determine its type.

---

## Q6. Can Lambda access instance variables?

Yes.

```java
class Employee {

    private String name = "Chetan";

    void print() {

        Runnable r = () -> {
            System.out.println(name);
        };

        r.run();
    }
}
```

Instance variables don't have the same effectively-final restriction as local variables.

---

## Q7. Can Lambda access static variables?

Yes.

```java
class Demo {

    static int count = 10;

    public static void main(String[] args) {

        Runnable r = () -> {
            System.out.println(count);
        };

        r.run();
    }
}
```

---

## Q8. Can we overload methods using Lambda arguments?

Potentially, but overloaded functional-interface methods can create ambiguity.

Example:

```java
interface A {
    void execute();
}

interface B {
    void execute();
}

class Demo {

    static void process(A a) {
    }

    static void process(B b) {
    }
}
```

This becomes ambiguous:

```java
// process(() -> System.out.println("Hello")); // ❌ ambiguous
```

The Lambda itself doesn't provide enough information to determine whether it should target `A` or `B`.

You can resolve it with a cast:

```java
process((A) () -> System.out.println("Hello"));
```

---

## Q9. Can a Lambda have a return statement without braces?

No.

This is invalid:

```java
// (a, b) -> return a + b;
```

Correct:

```java
(a, b) -> a + b
```

or:

```java
(a, b) -> {
    return a + b;
}
```

---

## Q10. What happens if Lambda has multiple statements but no `return`?

If the functional interface method returns a value, compilation fails.

Invalid:

```java
// (a, b) -> {
//     int result = a + b;
//     System.out.println(result);
// }
```

Correct:

```java
(a, b) -> {
    int result = a + b;
    System.out.println(result);
    return result;
}
```

---

# 4. Quick Revision

## Lambda Definition

> Lambda is a concise way to provide behavior for a Functional Interface.

---

## Syntax

```java
(parameters) -> expression
```

```java
(parameters) -> {
    statements;
}
```

---

## Examples

### No parameter

```java
() -> System.out.println("Hello");
```

### One parameter

```java
name -> System.out.println(name);
```

### Multiple parameters

```java
(a, b) -> a + b;
```

### Multiple statements

```java
(a, b) -> {
    int result = a + b;
    return result;
};
```

---

# Lambda Cheat Sheet

| Concept           | Key Point                                    |
| ----------------- | -------------------------------------------- |
| Introduced        | Java 8                                       |
| Main purpose      | Functional programming + less boilerplate    |
| Lambda target     | Functional Interface                         |
| Parameters        | Zero, one, or multiple                       |
| Return            | Implicit for expression body                 |
| Block body        | Requires `return` for value-returning method |
| Local variables   | Must be final/effectively final              |
| Object mutation   | Allowed if reference doesn't change          |
| `this`            | Refers to enclosing object                   |
| Anonymous class   | Has its own `this`                           |
| Target typing     | Lambda gets type from context                |
| Checked exception | Must match functional method's `throws`      |
| Complex Lambda    | Can hurt readability                         |

---

# Most Important Interview Questions ⭐⭐⭐⭐⭐

1. What is Lambda Expression?
2. Why was Lambda introduced?
3. Explain Lambda syntax.
4. What is a Functional Interface?
5. Why does Lambda require a target type?
6. Lambda vs Anonymous Inner Class.
7. What is effectively final?
8. Why can't Lambda modify local variables?
9. Can Lambda modify an object?
10. What does `this` refer to inside Lambda?
11. What is target typing?
12. Can Lambda throw checked exceptions?
13. Can overloaded methods with Functional Interfaces cause ambiguity?

---

# Final Memory Trick

```text
Java 8
   ↓
Lambda
   ↓
Functional Interface
   ↓
Pass Behavior
   ↓
Less Boilerplate
   ↓
Collections + Streams
```

### Remember

```text
Lambda
    ↓
(parameters) -> expression

? Local variable
    ↓
final / effectively final

? this
    ↓
enclosing object

? Multiple statements
    ↓
{ statements }

? Return from block
    ↓
return required

? Lambda type
    ↓
Target type / Functional Interface
```
