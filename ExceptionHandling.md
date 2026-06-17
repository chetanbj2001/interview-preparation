# What is Exception Handling in Java?

## Interview Answer (1-2 Minutes)

Exception Handling is a mechanism in Java used to handle runtime errors gracefully so that the normal flow of the application is not interrupted.

Instead of terminating the program abruptly, Java provides constructs such as:

```java
try
catch
finally
throw
throws
```

to handle exceptional situations.

Exception Handling helps in:

- Maintaining application flow
- Providing meaningful error messages
- Improving application reliability
- Separating error handling code from business logic

---

# What is an Exception?

An Exception is an unwanted event that occurs during program execution and disrupts the normal flow of the program.

Example:

```java
int result = 10 / 0;
```

Output:

```text
ArithmeticException
```

---

# Why Do We Need Exception Handling?

Without Exception Handling:

```java
System.out.println("Start");

int result = 10 / 0;

System.out.println("End");
```

Output:

```text
Start
ArithmeticException
```

Program terminates.

---

With Exception Handling:

```java
System.out.println("Start");

try {

    int result = 10 / 0;

} catch (Exception e) {

    System.out.println("Exception Handled");
}

System.out.println("End");
```

Output:

```text
Start
Exception Handled
End
```

Program continues execution.

---

# Exception Hierarchy

```text
Object
   |
Throwable
   |
   +---- Error
   |
   +---- Exception
            |
            +---- Checked Exceptions
            |
            +---- RuntimeException
                      |
                      +---- Unchecked Exceptions
```

---

# Throwable

Root class of Java Exception Handling.

Two main subclasses:

```text
Error
Exception
```

---

# Error

Represents serious problems that applications should not handle.

Examples:

```java
OutOfMemoryError
StackOverflowError
VirtualMachineError
```

Usually caused by JVM issues.

---

# Exception

Represents conditions that applications can handle.

Examples:

```java
IOException
SQLException
NullPointerException
ArithmeticException
```

---

# Real World Example

Suppose:

```text
ATM Withdrawal
```

Possible situations:

```text
Invalid PIN
Insufficient Balance
Network Failure
```

These situations should be handled gracefully.

This is Exception Handling.

---

# Frequently Asked Interview Questions

## Q1: What is Exception Handling?

### Answer

A mechanism to handle runtime errors and maintain normal program flow.

---

## Q2: What is an Exception?

### Answer

An unwanted event that disrupts normal program execution.

---

## Q3: What is Throwable?

### Answer

Root class of Java Exception Handling hierarchy.

---

## Q4: Difference Between Error and Exception?

### Answer

| Error | Exception |
|---------|---------|
| JVM Related | Application Related |
| Cannot Usually Be Handled | Can Be Handled |
| Serious Problem | Recoverable Problem |

---

## Q5: Can We Handle Errors?

### Answer

Technically yes.

Practically no.

Examples:

```java
OutOfMemoryError
StackOverflowError
```

are usually not recoverable.

---

# Tricky Interview Questions

## Question

Is Exception a Class or Interface?

### Answer

Class.

```java
java.lang.Exception
```

---

## Question

Does Every Exception Crash the Program?

### Answer

No.

Handled exceptions do not crash the program.

---

## Question

What is the Parent of All Exceptions?

### Answer

```java
Throwable
```

---

# Common Interview Trap

## Question

Which is Parent?

```java
Exception
```

or

```java
Error
```

### Answer

Neither.

Both inherit from:

```java
Throwable
```

---

# Key Points For Revision

- Exception Handling manages runtime errors.
- Root class is Throwable.
- Throwable has Error and Exception.
- Errors are JVM-related.
- Exceptions are application-related.
- Exception Handling prevents abnormal termination.

---

# One-Line Summary

Exception Handling is a Java mechanism that allows applications to handle runtime errors gracefully and continue normal execution.
