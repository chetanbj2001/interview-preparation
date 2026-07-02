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
# Custom Exception in Java

Custom Exceptions are user-defined exceptions created by developers to represent business-specific error conditions.

Java provides many built-in exceptions such as:

```java
NullPointerException
ArithmeticException
IOException
```

However, in real projects, we often need exceptions specific to business requirements.

Examples:

```text
InvalidAgeException
InsufficientBalanceException
InvalidAccountException
ProductNotFoundException
```

For such cases, we create Custom Exceptions.

---

# Why Do We Need Custom Exceptions?

Suppose we are building a Banking Application.

Requirement:

```text
Minimum Balance Must Be ₹1000
```

If user tries to withdraw more money:

```text
Insufficient Balance
```

Using:

```java
ArithmeticException
```

does not clearly represent the business problem.

Instead:

```java
InsufficientBalanceException
```

makes code more readable and meaningful.

---

# How To Create Custom Exception?

There are two ways:

```text
1. Checked Exception
2. Unchecked Exception
```

---

# Creating Custom Checked Exception

## Step 1

Extend Exception class.

```java
public class InvalidAgeException
        extends Exception {

    public InvalidAgeException(String message) {

        super(message);
    }
}
```

---

## Step 2

Use Exception

```java
public class Voting {

    public static void vote(int age)
            throws InvalidAgeException {

        if(age < 18) {

            throw new InvalidAgeException(
                    "Age Must Be 18 Or Above");
        }

        System.out.println("Eligible To Vote");
    }
}
```

---

## Step 3

Handle Exception

```java
public class Test {

    public static void main(String[] args) {

        try {

            Voting.vote(16);

        } catch (InvalidAgeException e) {

            System.out.println(e.getMessage());
        }
    }
}
```

Output:

```text
Age Must Be 18 Or Above
```

---

# Creating Custom Unchecked Exception

Extend:

```java
RuntimeException
```

Example:

```java
public class InvalidAgeException
        extends RuntimeException {

    public InvalidAgeException(String message) {

        super(message);
    }
}
```

Usage:

```java
if(age < 18) {

    throw new InvalidAgeException(
            "Not Eligible");
}
```

No mandatory handling required.

---

# Checked vs Unchecked Custom Exception

| Feature | Checked | Unchecked |
|----------|----------|----------|
| Parent Class | Exception | RuntimeException |
| Compiler Checks | Yes | No |
| Handling Mandatory | Yes | No |
| Example | InvalidAgeException | InvalidAgeException |

---

# Real Project Example

## Banking System

```java
public class InsufficientBalanceException
        extends Exception {

    public InsufficientBalanceException(
            String message) {

        super(message);
    }
}
```

Usage:

```java
if(balance < amount) {

    throw new InsufficientBalanceException(
            "Insufficient Balance");
}
```

---

# Frequently Asked Interview Questions

## Q1: What is a Custom Exception?

### Answer

A user-defined exception created to represent business-specific error conditions.

---

## Q2: How Do We Create a Custom Exception?

### Answer

Extend:

```java
Exception
```

or

```java
RuntimeException
```

---

## Q3: When Should We Create Custom Exceptions?

### Answer

When built-in exceptions do not clearly represent business requirements.

Examples:

```text
InvalidAgeException
InsufficientBalanceException
OrderNotFoundException
```

---

## Q4: Which Is Better?

### Answer

Depends on requirement.

```text
Mandatory Handling → Checked Exception

Optional Handling → Runtime Exception
```

---

## Q5: Can We Add Constructors?

### Answer

Yes.

```java
public InvalidAgeException(String msg) {

    super(msg);
}
```

---

# Tricky Interview Questions

## Question

Can We Create Exception Without Extending Exception Class?

### Answer

No.

Custom exception should extend:

```java
Exception
```

or

```java
RuntimeException
```

---

## Question

Can We Create Custom Runtime Exception?

### Answer

Yes.

```java
class InvalidUserException
        extends RuntimeException {

}
```

---

## Question

Can We Throw Custom Exception Using throw?

### Answer

Yes.

```java
throw new InvalidAgeException(
        "Age Must Be 18+");
```

---

## Question

Can Custom Exception Have Methods?

### Answer

Yes.

It is a normal Java class.

---

# Common Interview Trap

## Question

Should Every Validation Have a Custom Exception?

### Answer

No.

Create Custom Exceptions only when they improve readability and business understanding.

Bad:

```java
InvalidNumberException
InvalidNameException
InvalidEmailException
```

for every small validation.

Good:

```java
InsufficientBalanceException
OrderNotFoundException
UserNotAuthorizedException
```

---

# Key Points For Revision

- Custom Exceptions are user-defined exceptions.
- Extend Exception for Checked Exceptions.
- Extend RuntimeException for Unchecked Exceptions.
- Improve readability and business understanding.
- Frequently used in real-world applications.
- Can be thrown using throw keyword.

---

# One-Line Summary

A Custom Exception is a user-defined exception created to represent specific business errors that built-in Java exceptions cannot clearly express.
# Exception Propagation in Java

Exception Propagation is the process where an exception travels from the method where it occurred to the calling method until it is handled.

If a method does not handle an exception, JVM passes the exception to its caller.

This process continues through the call stack until:

```text
Exception Gets Handled
```

or

```text
Program Terminates
```

Exception propagation mainly happens for:

```java
Unchecked Exceptions
```

such as:

```java
ArithmeticException
NullPointerException
ArrayIndexOutOfBoundsException
```

---

# Understanding With Example

## Code

```java
public class Test {

    public static void method3() {

        int result = 10 / 0;
    }

    public static void method2() {

        method3();
    }

    public static void method1() {

        method2();
    }

    public static void main(String[] args) {

        method1();
    }
}
```

Output:

```text
ArithmeticException
```

---

# Flow

```text
main()
   |
method1()
   |
method2()
   |
method3()
   |
ArithmeticException
```

Exception occurs in:

```java
method3()
```

Not handled.

Moves to:

```java
method2()
```

Not handled.

Moves to:

```java
method1()
```

Not handled.

Moves to:

```java
main()
```

Not handled.

JVM Default Exception Handler executes.

Program terminates.

---

# Handling Propagated Exception

## Example

```java
public class Test {

    public static void method3() {

        int result = 10 / 0;
    }

    public static void method2() {

        method3();
    }

    public static void method1() {

        try {

            method2();

        } catch (ArithmeticException e) {

            System.out.println("Handled");
        }
    }

    public static void main(String[] args) {

        method1();
    }
}
```

Output:

```text
Handled
```

---

# Visual Representation

```text
method3()
   |
Exception
   ↓
method2()
   ↓
method1()
   ↓
catch Block
```

---

# Checked Exception Propagation

## Example

```java
public void method3()
        throws IOException {

}
```

```java
public void method2()
        throws IOException {

    method3();
}
```

```java
public void method1()
        throws IOException {

    method2();
}
```

Eventually:

```java
try {

    method1();

} catch(IOException e) {

}
```

---

# Why Is Exception Propagation Useful?

Without propagation:

Every method would need:

```java
try-catch
```

This would make code messy.

Instead:

```text
Handle Exception At Appropriate Layer
```

Example:

```text
DAO Layer
Service Layer
Controller Layer
```

Exception can travel upward.

---

# Real Project Example

```java
Controller
   |
Service
   |
Repository
```

Suppose:

```java
SQLException
```

occurs in Repository.

Instead of handling there:

```text
Repository → Service → Controller
```

Controller handles exception and sends proper API response.

---

# Frequently Asked Interview Questions

## Q1: What is Exception Propagation?

### Answer

The process of passing an exception from one method to its caller until it is handled.

---

## Q2: Why Does Exception Propagation Happen?

### Answer

Because the current method does not handle the exception.

---

## Q3: Which Exceptions Propagate Automatically?

### Answer

```java
Runtime Exceptions
```

Examples:

```java
ArithmeticException
NullPointerException
```

---

## Q4: Can Checked Exceptions Propagate?

### Answer

Yes.

Using:

```java
throws
```

---

## Q5: What Happens If Nobody Handles Exception?

### Answer

JVM Default Exception Handler executes.

Program terminates.

---

# Tricky Interview Questions

## Question

Will This Compile?

```java
public void test() {

    throw new IOException();
}
```

### Answer

No.

IOException is checked exception.

Must be:

```java
try-catch
```

or

```java
throws IOException
```

---

## Question

Can Runtime Exceptions Propagate Without throws?

### Answer

Yes.

Example:

```java
ArithmeticException
```

---

## Question

Does Exception Propagation Work Bottom To Top?

### Answer

Yes.

It moves upward through method call stack.

---

## Question

Can We Catch Exception In Main Method?

### Answer

Yes.

```java
public static void main(String[] args) {

    try {

    } catch(Exception e) {

    }
}
```

---

# Common Interview Trap

## Question

Does Every Exception Automatically Propagate?

### Answer

No.

Checked exceptions require:

```java
throws
```

or

```java
try-catch
```

Runtime exceptions propagate automatically.

---

# Key Points For Revision

- Exception Propagation moves exception up the call stack.
- Happens when current method does not handle exception.
- Runtime exceptions propagate automatically.
- Checked exceptions use throws.
- JVM handles exception if nobody catches it.
- Improves code maintainability.

---

# One-Line Summary

Exception Propagation is the process of passing an exception from the method where it occurs to its caller until it is handled or reaches the JVM.
# Multiple Catch Blocks in Java

In Java, a single `try` block can have multiple `catch` blocks.

Each catch block handles a specific type of exception.

When an exception occurs, JVM checks each catch block from top to bottom.

The first matching catch block is executed, and the remaining catch blocks are skipped.

Multiple catch blocks help us handle different exceptions in different ways.

---

# Why Do We Need Multiple Catch Blocks?

Suppose a program performs multiple operations:

- Division
- Array Access
- File Reading

Each operation may throw a different exception.

Instead of writing one generic catch block, we can handle each exception separately.

---

# Example

```java
public class Test {

    public static void main(String[] args) {

        try {

            int[] arr = {10,20,30};

            System.out.println(arr[5]);

            int result = 10 / 0;

        } catch (ArrayIndexOutOfBoundsException e) {

            System.out.println("Invalid Array Index");

        } catch (ArithmeticException e) {

            System.out.println("Cannot Divide By Zero");
        }
    }
}
```

Output

```text
Invalid Array Index
```

Reason:

The first exception occurs while accessing the array.

Execution immediately moves to the matching catch block.

---

# Execution Flow

```text
try
 |
Exception Occurs
 |
Check Catch 1
 |
Matched ?
 |
Yes
 |
Execute Catch
 |
Skip Remaining Catch Blocks
```

---

# Example 2

```java
public class Test {

    public static void main(String[] args) {

        try {

            int result = 10 / 0;

        } catch (ArrayIndexOutOfBoundsException e) {

            System.out.println("Array Error");

        } catch (ArithmeticException e) {

            System.out.println("Arithmetic Error");
        }
    }
}
```

Output

```text
Arithmetic Error
```

---

# Can We Catch Parent Exception First?

No.

Example

```java
try {

} catch (Exception e) {

} catch (ArithmeticException e) {

}
```

Compilation Error

```text
Unreachable catch block
```

Reason:

`Exception` is the parent of `ArithmeticException`.

The parent catch block already handles every subclass.

The second catch block will never execute.

---

# Correct Order

```java
try {

} catch (ArithmeticException e) {

} catch (Exception e) {

}
```

Always write:

```text
Specific Exception
        ↓
General Exception
```

---

# Multi-Catch (Java 7)

If multiple exceptions require the same handling logic, Java allows them to be combined using the `|` operator.

Example

```java
try {

    int[] arr = {1,2,3};

    System.out.println(arr[5]);

} catch (ArithmeticException | ArrayIndexOutOfBoundsException e) {

    System.out.println("Exception Handled");
}
```

Output

```text
Exception Handled
```

---

# Advantages of Multi-Catch

- Reduces duplicate code.
- Improves readability.
- Easier to maintain.

---

# Real Project Example

```java
try {

    // Database Operation

} catch (SQLException e) {

    // Database Error

} catch (IOException e) {

    // File Error

} catch (Exception e) {

    // Unexpected Error
}
```

Each exception is handled differently based on the business requirement.

---

# Frequently Asked Interview Questions

## Q1: Can a try block have multiple catch blocks?

### Answer

Yes.

One try block can have multiple catch blocks.

---

## Q2: Which catch block executes?

### Answer

The first matching catch block.

---

## Q3: Can two catch blocks execute for one exception?

### Answer

No.

Once a matching catch block is executed, the remaining catch blocks are skipped.

---

## Q4: What happens if no catch block matches?

### Answer

The exception propagates to the caller.

If nobody handles it, JVM's Default Exception Handler terminates the program.

---

## Q5: Can we use Exception as the last catch block?

### Answer

Yes.

It should always be the last catch block.

---

# Tricky Interview Questions

## Question

Will this compile?

```java
try {

} catch (Exception e) {

} catch (ArithmeticException e) {

}
```

### Answer

No.

Compilation Error.

Reason:

The second catch block is unreachable.

---

## Question

What is the correct order?

### Answer

```java
catch (ArithmeticException e)

catch (NullPointerException e)

catch (Exception e)
```

Specific exceptions first.

General exception last.

---

## Question

Can we write multiple Exception catch blocks?

```java
catch(Exception e){

}

catch(Exception e){

}
```

### Answer

No.

Compilation Error.

Duplicate catch blocks are not allowed.

---

## Question

Can we combine exceptions in one catch block?

### Answer

Yes.

```java
catch(IOException | SQLException e){

}
```

---

# Common Interview Trap

## Question

Which catch block should always be written last?

### Answer

```java
catch(Exception e)
```

Because it is the parent of almost all exceptions.

---

# Best Practices

- Catch the most specific exception first.
- Catch the general `Exception` last.
- Avoid empty catch blocks.
- Use multi-catch when handling logic is the same.
- Never ignore exceptions silently.

---

# Key Points For Revision

- One try block can have multiple catch blocks.
- JVM executes only the first matching catch block.
- Specific exception first, general exception last.
- Multi-catch was introduced in Java 7.
- Exception propagation occurs if no catch block matches.

---

# One-Line Summary

Multiple catch blocks allow a single try block to handle different exception types separately, and the JVM executes only the first matching catch block.
