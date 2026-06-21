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
# try-catch-finally in Java

Java provides the try-catch-finally mechanism to handle exceptions gracefully.

- try → Contains risky code.
- catch → Handles exception.
- finally → Executes whether exception occurs or not.

This helps prevent application crashes and ensures cleanup code executes properly.

---

# try Block

## Definition

The try block contains code that may throw an exception.

Example:

```java
try {

    int result = 10 / 0;

}
```

If an exception occurs, JVM immediately stops executing the remaining code inside try and transfers control to the appropriate catch block.

---

# catch Block

## Definition

The catch block handles exceptions thrown from the try block.

Example:

```java
try {

    int result = 10 / 0;

} catch (ArithmeticException e) {

    System.out.println("Cannot divide by zero");
}
```

Output:

```text
Cannot divide by zero
```

---

# finally Block

## Definition

The finally block always executes regardless of whether an exception occurs or not.

Example:

```java
try {

    int result = 10 / 0;

} catch (ArithmeticException e) {

    System.out.println("Exception Handled");

} finally {

    System.out.println("Finally Executed");
}
```

Output:

```text
Exception Handled
Finally Executed
```

---

# Normal Execution Flow

## Example

```java
try {

    System.out.println("Inside Try");

} catch (Exception e) {

    System.out.println("Inside Catch");

} finally {

    System.out.println("Inside Finally");
}
```

Output:

```text
Inside Try
Inside Finally
```

---

# Exception Execution Flow

## Example

```java
try {

    int result = 10 / 0;

} catch (ArithmeticException e) {

    System.out.println("Inside Catch");

} finally {

    System.out.println("Inside Finally");
}
```

Output:

```text
Inside Catch
Inside Finally
```

---

# Can We Use try Without catch?

Yes.

If finally is present.

Example:

```java
try {

    System.out.println("Hello");

} finally {

    System.out.println("Cleanup");
}
```

Valid code.

---

# Can We Use try Without finally?

Yes.

If catch is present.

Example:

```java
try {

    int result = 10 / 0;

} catch (Exception e) {

    System.out.println("Handled");
}
```

Valid code.

---

# Can We Use try Alone?

No.

Invalid:

```java
try {

    System.out.println("Hello");

}
```

Compilation Error.

At least one of the following is required:

```text
catch
finally
```

---

# Execution Flow Summary

## No Exception

```text
try → finally
```

---

## Exception Handled

```text
try → catch → finally
```

---

## Exception Not Handled

```text
try → finally → JVM Default Handler
```

---

# Real World Example

Database Connection:

```java
Connection con = null;

try {

    con = getConnection();

} catch (Exception e) {

    e.printStackTrace();

} finally {

    con.close();
}
```

finally ensures resources are released.

---

# Frequently Asked Interview Questions

## Q1: What is the purpose of try block?

### Answer

Contains code that may throw an exception.

---

## Q2: What is the purpose of catch block?

### Answer

Handles exceptions thrown from try block.

---

## Q3: What is the purpose of finally block?

### Answer

Executes cleanup code regardless of exception occurrence.

---

## Q4: Can We Have Multiple Catch Blocks?

### Answer

Yes.

```java
try {

} catch (ArithmeticException e) {

} catch (NullPointerException e) {

}
```

---

## Q5: Is finally Always Executed?

### Answer

Almost always.

There are some exceptions discussed later.

---

# Tricky Interview Questions

## Question

What Will Be Output?

```java
try {

    System.out.println("Try");

} finally {

    System.out.println("Finally");
}
```

### Output

```text
Try
Finally
```

---

## Question

What Will Be Output?

```java
try {

    int result = 10 / 0;

} catch (ArithmeticException e) {

    System.out.println("Catch");

} finally {

    System.out.println("Finally");
}
```

### Output

```text
Catch
Finally
```

---

## Question

Can finally Exist Without catch?

### Answer

Yes.

---

## Question

Can catch Exist Without finally?

### Answer

Yes.

---

## Question

Can try Exist Without catch and finally?

### Answer

No.

Compilation Error.

---

# Common Interview Trap

## Question

Does finally Execute If Exception Occurs?

### Answer

Yes.

finally executes whether an exception occurs or not.

---

## Question

Does finally Execute If Exception Is Handled?

### Answer

Yes.

finally still executes.

---

# Key Points For Revision

- try contains risky code.
- catch handles exceptions.
- finally executes cleanup code.
- finally executes in both normal and exceptional cases.
- try must have either catch or finally.
- Multiple catch blocks are allowed.

---

# One-Line Summary

The try-catch-finally mechanism allows Java applications to handle exceptions gracefully while ensuring cleanup code always executes.
# Difference Between throw and throws in Java

Both `throw` and `throws` are used in Exception Handling, but they serve different purposes.

- `throw` is used to explicitly throw an exception.
- `throws` is used to declare exceptions that a method may throw.

In simple words:

```text
throw  -> Actually Throws Exception

throws -> Declares Exception
```

This is one of the most frequently asked Java interview questions.

---

# What is throw?

## Definition

The `throw` keyword is used to explicitly create and throw an exception.

---

## Syntax

```java
throw new ExceptionType("Message");
```

---

## Example

```java
public class Test {

    public static void main(String[] args) {

        throw new ArithmeticException("Divide by zero");
    }
}
```

Output:

```text
Exception in thread "main"
java.lang.ArithmeticException: Divide by zero
```

---

# Real World Example

```java
public class Voting {

    public static void vote(int age) {

        if(age < 18) {

            throw new RuntimeException("Not Eligible For Voting");
        }

        System.out.println("Eligible");
    }
}
```

---

# What is throws?

## Definition

The `throws` keyword is used in method declaration to indicate that the method may throw an exception.

Responsibility of handling exception is transferred to the caller.

---

## Syntax

```java
public void method() throws Exception {

}
```

---

## Example

```java
import java.io.*;

public class Test {

    public static void readFile() throws IOException {

        FileReader file =
                new FileReader("test.txt");
    }
}
```

The method is informing the caller:

```text
I may throw IOException.
Handle it yourself.
```

---

# Difference Between throw and throws

| Feature | throw | throws |
|----------|----------|----------|
| Keyword Type | Statement | Declaration |
| Purpose | Throw Exception | Declare Exception |
| Location | Method Body | Method Signature |
| Number of Exceptions | One | Multiple |
| Followed By | Exception Object | Exception Class |

---

# Example Using throw

```java
throw new NullPointerException();
```

Actual exception is thrown.

---

# Example Using throws

```java
public void test()
throws IOException {

}
```

Exception is only declared.

---

# Can We Use Multiple Exceptions With throws?

Yes.

Example:

```java
public void test()
throws IOException,
       SQLException,
       ClassNotFoundException {

}
```

---

# Can We Use Multiple Exceptions With throw?

No.

```java
throw new IOException();
```

Only one exception object at a time.

---

# Real Project Example

Suppose a banking application:

```java
public void withdraw(double amount) {

    if(amount <= 0) {

        throw new IllegalArgumentException(
                "Invalid Amount");
    }
}
```

Used for validation.

---

# Frequently Asked Interview Questions

## Q1: What is throw?

### Answer

Used to explicitly throw an exception.

---

## Q2: What is throws?

### Answer

Used to declare exceptions that a method may throw.

---

## Q3: Which Keyword Creates an Exception?

### Answer

```java
throw
```

---

## Q4: Which Keyword Appears in Method Signature?

### Answer

```java
throws
```

---

## Q5: Can We Use throws With RuntimeException?

### Answer

Yes.

But it is not mandatory.

---

# Tricky Interview Questions

## Question

Can We Use throw Without throws?

### Answer

Yes.

For unchecked exceptions.

Example:

```java
throw new ArithmeticException();
```

---

## Question

Can We Use throws Without throw?

### Answer

Yes.

Method may declare exception without explicitly throwing it.

---

## Question

Which One Comes After Method Name?

### Answer

```java
throws
```

Example:

```java
public void test()
throws IOException {

}
```

---

## Question

Which One Creates Exception Object?

### Answer

```java
throw
```

Example:

```java
throw new IOException();
```

---

# Common Interview Trap

## Question

What Is Output?

```java
public static void main(String[] args)
throws Exception {

    System.out.println("Hello");
}
```

### Answer

```text
Hello
```

No exception occurs.

`throws` only declares possibility.

---

# Key Points For Revision

- throw throws exception explicitly.
- throws declares exception.
- throw is used inside method body.
- throws is used in method signature.
- throw uses exception object.
- throws uses exception class.
- throw can throw one exception at a time.
- throws can declare multiple exceptions.

---

# One-Line Summary

`throw` is used to explicitly throw an exception, whereas `throws` is used to declare exceptions that a method may throw.
