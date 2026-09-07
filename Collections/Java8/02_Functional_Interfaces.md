Functional Interfaces

Functional Interfaces are a core Java 8 concept and are mainly used with Lambda Expressions.

1. What is a Functional Interface?

A Functional Interface is an interface that contains exactly one abstract method.

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}

Because calculate() is the only abstract method, Calculator is a Functional Interface.

2. Why do we need Functional Interfaces?

Functional Interfaces provide the target type for Lambda Expressions.

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}

Calculator addition = (a, b) -> a + b;

System.out.println(addition.calculate(10, 20));

Output:

30

Interview Answer

Functional Interfaces allow us to represent a single unit of behavior and pass that behavior using Lambda Expressions.

3. Is @FunctionalInterface mandatory?

No.

This is still a Functional Interface:

interface Calculator {

    int calculate(int a, int b);
}

However, using @FunctionalInterface is recommended because the compiler verifies that the interface has only one abstract method.

4. What is @FunctionalInterface?

@FunctionalInterface is an annotation introduced in Java 8 to indicate that an interface is intended to be a Functional Interface.

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}

If we add another abstract method:

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    int multiply(int a, int b);
}

The compiler gives an error.

5. Can a Functional Interface have multiple methods?

Yes, but only one method can be abstract.

It can contain:

One abstract method

Multiple default methods

Multiple static methods

Example:

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    default void print() {
        System.out.println("Calculator");
    }

    static void info() {
        System.out.println("Calculator Interface");
    }
}

6. Can a Functional Interface have a default method?

Yes.

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    default void print() {
        System.out.println("Calculator");
    }
}

A default method has an implementation, so it does not count as an abstract method.

7. Can a Functional Interface have a static method?

Yes.

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    static void info() {
        System.out.println("Calculator");
    }
}

Static methods do not count as abstract methods.

8. Can a Functional Interface extend another interface?

Yes, as long as the resulting interface still has exactly one abstract method.

Example:

interface Parent {

    void execute();
}

@FunctionalInterface
interface Child extends Parent {

}

Child still has only one abstract method: execute().

9. Can a Functional Interface extend two interfaces?

It can, provided the inherited abstract methods result in only one distinct abstract method.

Example:

interface A {
    void execute();
}

interface B {
    void execute();
}

@FunctionalInterface
interface C extends A, B {

}

This is valid because both interfaces declare the same abstract method signature.

10. What are the built-in Functional Interfaces in Java 8?

Java 8 provides common Functional Interfaces in:

java.util.function

The most important ones are:

Interface

Abstract Method

Purpose

Predicate<T>

boolean test(T t)

Checks a condition

Function<T,R>

R apply(T t)

Converts/transforms a value

Consumer<T>

void accept(T t)

Performs an action

Supplier<T>

T get()

Supplies a value

UnaryOperator<T>

T apply(T t)

One input → same type output

BinaryOperator<T>

T apply(T t1, T t2)

Two same-type inputs → same type output

11. What is Predicate?

Predicate<T> represents a condition.

Its abstract method is:

boolean test(T t);

Example:

Predicate<Integer> isEven = n -> n % 2 == 0;

System.out.println(isEven.test(10));

Output:

true

Remember

Predicate → input → boolean

12. What is Function?

Function<T, R> accepts one input and returns one result.

Its abstract method is:

R apply(T t);

Example:

Function<String, Integer> length = str -> str.length();

System.out.println(length.apply("Java"));

Output:

4

Here:

T = String
R = Integer

Remember

Function → input → output

13. What is Consumer?

Consumer<T> accepts an input and returns nothing.

Its abstract method is:

void accept(T t);

Example:

Consumer<String> print = str -> System.out.println(str);

print.accept("Java");

Output:

Java

Remember

Consumer → input → void

14. What is Supplier?

Supplier<T> does not accept any input but returns a value.

Its abstract method is:

T get();

Example:

Supplier<String> message = () -> "Hello Java";

System.out.println(message.get());

Output:

Hello Java

Remember

Supplier → no input → output

15. What is UnaryOperator?

UnaryOperator<T> is a specialized Function<T, T>.

It accepts one value and returns the same type.

UnaryOperator<Integer> square = n -> n * n;

System.out.println(square.apply(5));

Output:

25

Conceptually:

T → T

16. What is BinaryOperator?

BinaryOperator<T> is a specialized BiFunction<T, T, T>.

It accepts two values of the same type and returns the same type.

BinaryOperator<Integer> sum = (a, b) -> a + b;

System.out.println(sum.apply(10, 20));

Output:

30

Conceptually:

(T, T) → T

17. What is the difference between Predicate, Function, Consumer and Supplier?

Interface

Input

Output

Method

Predicate<T>

1

boolean

test()

Function<T,R>

1

R

apply()

Consumer<T>

1

void

accept()

Supplier<T>

0

T

get()

Easy way to remember:

Predicate → Check
Function  → Transform
Consumer  → Consume
Supplier  → Supply

18. What is the difference between Function and UnaryOperator?

Function<T, R> can have different input and output types.

Function<String, Integer> length = str -> str.length();

UnaryOperator<T> must have the same input and output type.

UnaryOperator<Integer> doubleValue = n -> n * 2;

So:

Function<T,R>   → T → R
UnaryOperator<T> → T → T

19. What is the difference between BiFunction and BinaryOperator?

BiFunction<T, U, R> can have different input and output types.

BiFunction<Integer, Integer, String> result =
        (a, b) -> String.valueOf(a + b);

BinaryOperator<T> requires both inputs and the output to be the same type.

BinaryOperator<Integer> sum = (a, b) -> a + b;

So:

BiFunction<T,U,R> → (T,U) → R
BinaryOperator<T> → (T,T) → T

20. What is the relationship between Lambda and Functional Interface?

A Lambda Expression provides the implementation of the single abstract method of a Functional Interface.

Predicate<Integer> isPositive = n -> n > 0;

Here:

Predicate<Integer>
        ↓
Functional Interface
        ↓
test(Integer)
        ↓
n -> n > 0
        ↓
Lambda implementation

21. Can we create our own Functional Interface?

Yes.

@FunctionalInterface
interface Validator {

    boolean validate(String value);
}

Usage:

Validator validator = value -> value != null && !value.isEmpty();

System.out.println(validator.validate("Java"));

22. When should we create a custom Functional Interface?

Use a custom Functional Interface when:

The required method signature is not covered by existing Java interfaces.

The interface represents meaningful business behavior.

A custom name improves readability.

Otherwise, prefer the built-in interfaces from java.util.function.

23. What is the difference between Custom and Built-in Functional Interfaces?

Custom

@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}

Built-in

BinaryOperator<Integer> addition = (a, b) -> a + b;

Built-in interfaces should generally be preferred when they already fit the requirement.

Interview Questions

Q1. What is a Functional Interface?

A Functional Interface is an interface that contains exactly one abstract method. It can contain multiple default and static methods.

Q2. Why are Functional Interfaces important in Java 8?

They provide the target type required by Lambda Expressions and enable functional-style programming in Java.

Q3. Is @FunctionalInterface mandatory?

No. It is optional.

It provides compile-time validation that the interface has only one abstract method.

Q4. Can a Functional Interface have multiple methods?

Yes.

It can have multiple default and static methods, but only one abstract method.

Q5. Can a Functional Interface contain a default method?

Yes. Default methods do not count as abstract methods.

Q6. Can a Functional Interface contain a static method?

Yes. Static methods do not count as abstract methods.

Q7. Which package contains Java's built-in Functional Interfaces?

java.util.function

Q8. Name the four most commonly used Functional Interfaces.

Predicate
Function
Consumer
Supplier

Q9. What is Predicate used for?

It is used when we need to evaluate a condition and return true or false.

Predicate<Integer> p = n -> n > 10;

Q10. What is Function used for?

It is used to take an input and transform it into another value.

Function<String, Integer> f = str -> str.length();

Q11. What is Consumer used for?

It is used when we need to consume an input and perform an action without returning a result.

Consumer<String> c = System.out::println;

Q12. What is Supplier used for?

It is used when we need to generate or provide a value without receiving an input.

Supplier<Double> s = Math::random;

Q13. What is the difference between Predicate and Function?

Predicate returns boolean.

Function returns a result of type R.

Predicate<T>   → T → boolean
Function<T,R>  → T → R

Q14. What is the difference between Consumer and Supplier?

Consumer<T> → input → void
Supplier<T> → no input → T

Q15. What is the difference between Function and UnaryOperator?

Function<T,R>    → T → R
UnaryOperator<T> → T → T

UnaryOperator is used when input and output are the same type.

Q16. What is the difference between BiFunction and BinaryOperator?

BiFunction<T,U,R> → (T,U) → R
BinaryOperator<T> → (T,T) → T

Q17. Can a Lambda Expression exist without a Functional Interface?

A Lambda needs a target type. A Functional Interface is the most common target type.

For example:

Runnable task = () -> System.out.println("Running");

The Lambda gets its target type from Runnable.

Q18. Can we use an abstract class as a target for a Lambda?

No.

Lambda Expressions work with Functional Interfaces, not abstract classes.

Q19. Can a Functional Interface have methods from Object?

Methods corresponding to public methods of Object, such as equals(Object), do not count as additional abstract methods for functional-interface purposes.

Q20. Can a Functional Interface extend another interface?

Yes, provided the resulting interface has exactly one abstract method.

Tricky Interview Questions

Q1. Is an interface with one method always a Functional Interface?

Not necessarily if that method is not abstract.

A Functional Interface must have exactly one abstract method.

For example:

interface Test {

    default void execute() {
    }
}

This has zero abstract methods, so it is not a Functional Interface.

Q2. Does a default method count as an abstract method?

No.

default void print() {
}

already has an implementation.

Q3. Does a static method count as an abstract method?

No.

Static methods have an implementation and belong to the interface itself.

Q4. What happens if @FunctionalInterface is used and there are two abstract methods?

Compilation error.

@FunctionalInterface
interface Test {

    void method1();

    void method2();
}

Q5. Can two inherited abstract methods still result in a Functional Interface?

Yes, if they represent the same method signature.

interface A {
    void execute();
}

interface B {
    void execute();
}

@FunctionalInterface
interface C extends A, B {
}

C still has one distinct abstract method.

Q6. Why can't we use a Lambda with an interface containing two abstract methods?

Because the compiler cannot determine which abstract method the Lambda is supposed to implement.

interface Test {

    void method1();

    void method2();
}

// Test t = () -> System.out.println("Hello"); // Invalid

Q7. Can we have overloaded abstract methods in a Functional Interface?

No.

Overloaded abstract methods are still multiple abstract methods.

@FunctionalInterface
interface Test {

    void execute();

    void execute(String value);
}

This is invalid.

Q8. Can a Functional Interface have a private method?

Yes, Java 9+ allows private interface methods.

They do not count as abstract methods.

For Java 8 interview preparation, remember that private interface methods were not available in Java 8.

Q9. Can a Functional Interface have a generic abstract method?

Yes.

@FunctionalInterface
interface Processor {

    <T> T process(T value);
}

It still has one abstract method.

Q10. Can a Functional Interface have a generic type parameter?

Yes.

@FunctionalInterface
interface Converter<T, R> {

    R convert(T value);
}

Practical Interview Example

Question

Which Functional Interface would you choose for each requirement?

1. Check whether an employee is active

Predicate<Employee> isActive = Employee::isActive;

2. Convert Employee to Employee Name

Function<Employee, String> getName = Employee::getName;

3. Print Employee

Consumer<Employee> print = System.out::println;

4. Generate a random Employee ID

Supplier<Integer> idGenerator = () -> new Random().nextInt(10000);

Quick Revision

Functional Interface
        ↓
Exactly ONE abstract method
        ↓
Lambda can implement it

Main Java 8 Functional Interfaces

Predicate<T>
    → T → boolean
    → test()

Function<T,R>
    → T → R
    → apply()

Consumer<T>
    → T → void
    → accept()

Supplier<T>
    → () → T
    → get()

UnaryOperator<T>
    → T → T

BinaryOperator<T>
    → (T,T) → T

Most Important Interview Points

Functional Interface → exactly one abstract method.

@FunctionalInterface → optional but recommended.

Default methods are allowed.

Static methods are allowed.

Built-in interfaces are in java.util.function.

Lambda Expressions use Functional Interfaces as target types.

Predicate → condition.

Function → transformation.

Consumer → action.

Supplier → value generation.

UnaryOperator → same input/output type.

BinaryOperator → two same-type inputs and same-type output.

Custom Functional Interfaces are useful when built-in interfaces do not clearly fit the requirement.
