# What is Exception Handling in Java?

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

# Checked Exception vs Unchecked Exception

In Java, exceptions are categorized into:

1. Checked Exceptions
2. Unchecked Exceptions

The main difference is:

```text
Checked Exceptions → Checked at Compile Time

Unchecked Exceptions → Occur at Runtime
```

Checked exceptions must be handled using:

```java
try-catch
```

or

```java
throws
```

Unchecked exceptions are not checked by the compiler.

---

# Checked Exceptions

## Definition

Exceptions checked by the compiler during compilation.

If not handled, code will not compile.

---

## Example

```java
import java.io.*;

public class Test {

    public static void main(String[] args) {

        FileReader file = new FileReader("test.txt");
    }
}
```

Compilation Error:

```text
Unhandled exception type FileNotFoundException
```

---

## Solution

```java
try {

    FileReader file = new FileReader("test.txt");

} catch (FileNotFoundException e) {

    e.printStackTrace();
}
```

---

# Common Checked Exceptions

```java
IOException
SQLException
ClassNotFoundException
FileNotFoundException
InterruptedException
```

---

# Unchecked Exceptions

## Definition

Exceptions not checked by compiler.

They occur during program execution.

Also called:

```text
Runtime Exceptions
```

---

## Example

```java
int result = 10 / 0;
```

Output:

```text
ArithmeticException
```

Code compiles successfully.

Exception occurs at runtime.

---

# Common Unchecked Exceptions

```java
NullPointerException
ArithmeticException
ArrayIndexOutOfBoundsException
NumberFormatException
ClassCastException
```

---

# Exception Hierarchy

```text
Throwable
   |
   +---- Exception
            |
            +---- RuntimeException
                      |
                      +---- Unchecked Exceptions
            |
            +---- Checked Exceptions
```

---

# Difference Between Checked and Unchecked Exceptions

| Feature | Checked Exception | Unchecked Exception |
|----------|----------|----------|
| Checked By | Compiler | JVM |
| Occurs At | Compile Time | Runtime |
| Handling Required | Yes | No |
| Parent Class | Exception | RuntimeException |
| Example | IOException | NullPointerException |

---

# Real World Example

## Checked Exception

```text
Opening a File
```

File may not exist.

Compiler forces handling.

---

## Unchecked Exception

```text
Divide By Zero
```

Programming mistake.

Developer should fix code.

---

# Frequently Asked Interview Questions

## Q1: What is Checked Exception?

### Answer

An exception checked by compiler during compilation.

Handling is mandatory.

---

## Q2: What is Unchecked Exception?

### Answer

An exception that occurs at runtime.

Handling is optional.

---

## Q3: Parent Class of Unchecked Exceptions?

### Answer

```java
RuntimeException
```

---

## Q4: Is NullPointerException Checked?

### Answer

No.

It is an unchecked exception.

---

## Q5: Is IOException Checked?

### Answer

Yes.

Compiler forces handling.

---

## Q6: Can We Catch Unchecked Exceptions?

### Answer

Yes.

```java
try {
    // code
} catch (RuntimeException e) {

}
```

---

# Tricky Interview Questions

## Question

Will This Compile?

```java
FileReader file = new FileReader("test.txt");
```

### Answer

No.

FileNotFoundException must be handled.

---

## Question

Will This Compile?

```java
int result = 10 / 0;
```

### Answer

Yes.

ArithmeticException occurs at runtime.

---

## Question

Is RuntimeException Checked or Unchecked?

### Answer

Unchecked.

All subclasses of RuntimeException are unchecked.

---

## Question

Can We Create Our Own Checked Exception?

### Answer

Yes.

```java
class InvalidAgeException extends Exception {

}
```

---

## Question

Can We Create Our Own Unchecked Exception?

### Answer

Yes.

```java
class InvalidAgeException extends RuntimeException {

}
```

---

# Common Interview Trap

## Question

Are All Exceptions Checked?

### Answer

No.

RuntimeException and its subclasses are unchecked.

Examples:

```java
NullPointerException
ArithmeticException
ArrayIndexOutOfBoundsException
```

---

# Key Points For Revision

- Checked Exceptions are checked by compiler.
- Unchecked Exceptions occur at runtime.
- Checked Exceptions must be handled.
- RuntimeException and its subclasses are unchecked.
- IOException is checked.
- NullPointerException is unchecked.

---

# One-Line Summary

Checked Exceptions are verified by the compiler and must be handled, whereas Unchecked Exceptions occur at runtime and are usually caused by programming mistakes.
