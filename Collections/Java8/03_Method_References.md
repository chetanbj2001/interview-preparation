# Method References in Java 8

## 1. What is a Method Reference?

A **Method Reference** is a shorthand syntax for a Lambda Expression when the Lambda only calls an existing method.

Instead of writing:

```java
name -> System.out.println(name)

we can write:

System.out::println
Simple Definition

Method Reference is a shorthand way of writing a Lambda Expression that simply refers to an existing method or constructor.

Method References were introduced in Java 8.

2. Why Do We Need Method References?

Method References make code:

shorter
cleaner
more readable
easier to understand
Lambda
List<String> names = Arrays.asList("Chetan", "Rahul", "Amit");

names.forEach(name -> System.out.println(name));
Method Reference
names.forEach(System.out::println);

Both perform the same operation.

3. Method Reference Syntax

The basic syntax is:

ClassName::methodName

or:

object::methodName

or:

ClassName::new

The :: operator is called the method reference operator.

4. Types of Method References

There are 4 main types of Method References:

Reference to a Static Method
Reference to an Instance Method of a Particular Object
Reference to an Instance Method of an Arbitrary Object
Reference to a Constructor
5. Reference to a Static Method
Syntax
ClassName::staticMethodName

The method must be static.

Example
Lambda
Function<String, Integer> function =
        value -> Integer.parseInt(value);
Method Reference
Function<String, Integer> function =
        Integer::parseInt;

Here:

Integer::parseInt

refers to the static method:

Integer.parseInt(String)
Another Example
Function<Integer, Integer> absolute =
        Math::abs;

System.out.println(absolute.apply(-10));

Output:

10

Equivalent Lambda:

Function<Integer, Integer> absolute =
        number -> Math.abs(number);
6. Reference to an Instance Method of a Particular Object

Here, we already have an object and want to reference one of its instance methods.

Syntax
object::instanceMethod
Example
String message = "Hello Java";

Supplier<Integer> supplier =
        message::length;

System.out.println(supplier.get());

Output:

10

Equivalent Lambda:

Supplier<Integer> supplier =
        () -> message.length();

The object is:

message

The method is:

length()

Therefore:

message::length
Another Example
String message = "hello";

Supplier<String> supplier =
        message::toUpperCase;

System.out.println(supplier.get());

Output:

HELLO

Equivalent Lambda:

Supplier<String> supplier =
        () -> message.toUpperCase();
7. Reference to an Instance Method of an Arbitrary Object

This is one of the most important and commonly asked concepts.

Syntax
ClassName::instanceMethod

Here, the object on which the method is called is supplied as the Lambda argument.

Example
Function<String, String> upperCase =
        String::toUpperCase;

System.out.println(upperCase.apply("java"));

Output:

JAVA

Equivalent Lambda:

Function<String, String> upperCase =
        value -> value.toUpperCase();

Here:

String::toUpperCase

means:

Take a String object as input and call toUpperCase() on that object.

Another Example
Function<String, Integer> length =
        String::length;

System.out.println(length.apply("Chetan"));

Output:

6

Equivalent Lambda:

Function<String, Integer> length =
        value -> value.length();
8. Particular Object vs Arbitrary Object

This is a very common interview question.

Particular Object
String name = "Chetan";

Supplier<Integer> supplier =
        name::length;

The object is already fixed:

name

So this is:

Instance method of a particular object.

Arbitrary Object
Function<String, Integer> function =
        String::length;

The object is provided later:

function.apply("Chetan");
function.apply("Rahul");
function.apply("Java");

So this is:

Instance method of an arbitrary object.

Quick Comparison
Type	Example	Object
Particular Object	name::length	Already fixed
Arbitrary Object	String::length	Supplied later
9. Reference to a Constructor

A constructor reference is used to create objects.

Syntax
ClassName::new
Example
Supplier<ArrayList<String>> supplier =
        ArrayList::new;

ArrayList<String> list = supplier.get();

Equivalent Lambda:

Supplier<ArrayList<String>> supplier =
        () -> new ArrayList<>();
Constructor with Parameters
Function<String, StringBuilder> function =
        StringBuilder::new;

StringBuilder builder = function.apply("Hello");

System.out.println(builder);

Equivalent Lambda:

Function<String, StringBuilder> function =
        value -> new StringBuilder(value);
10. Constructor Reference with Custom Class

Consider:

class Employee {

    private String name;

    public Employee(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

We can use:

Function<String, Employee> function =
        Employee::new;

Employee employee = function.apply("Chetan");

System.out.println(employee.getName());

Equivalent Lambda:

Function<String, Employee> function =
        name -> new Employee(name);
11. System.out::println

One of the most common method references:

System.out::println

Example:

List<String> names =
        Arrays.asList("Chetan", "Rahul", "Amit");

names.forEach(System.out::println);

Equivalent Lambda:

names.forEach(name -> System.out.println(name));
12. Method References with forEach()

Method references are frequently used with forEach().

Lambda
List<String> names =
        Arrays.asList("Chetan", "Rahul", "Amit");

names.forEach(name -> System.out.println(name));
Method Reference
names.forEach(System.out::println);

This is one of the most common Java 8 interview examples.

13. Method References with Streams

Method references are commonly used with the Stream API.

Example:

List<String> names =
        Arrays.asList("chetan", "rahul", "amit");

names.stream()
        .map(String::toUpperCase)
        .forEach(System.out::println);

Output:

CHETAN
RAHUL
AMIT

Equivalent Lambda:

names.stream()
        .map(name -> name.toUpperCase())
        .forEach(name -> System.out.println(name));
14. Method Reference with Predicate
Predicate<String> predicate =
        String::isEmpty;

Equivalent Lambda:

Predicate<String> predicate =
        value -> value.isEmpty();

Example:

System.out.println(predicate.test(""));

Output:

true
15. Method Reference with Function
Function<String, Integer> function =
        String::length;

Equivalent Lambda:

Function<String, Integer> function =
        value -> value.length();

Example:

System.out.println(function.apply("Java"));

Output:

4
16. Method Reference with Consumer
Consumer<String> consumer =
        System.out::println;

Equivalent Lambda:

Consumer<String> consumer =
        value -> System.out.println(value);
17. Method Reference with Supplier
String message = "Java";

Supplier<Integer> supplier =
        message::length;

Equivalent Lambda:

Supplier<Integer> supplier =
        () -> message.length();
18. Method References and Functional Interfaces

A Method Reference does not work independently.

It normally needs a target type, such as a Functional Interface.

Example:

Function<String, Integer> function =
        String::length;

Here:

Function<String, Integer>

provides the target type.

The compiler understands that:

String::length

must match:

Integer apply(String)
19. Why Functional Interface is Required

Consider:

String::length

By itself, the compiler doesn't know how the method reference should be used.

But:

Function<String, Integer> function =
        String::length;

provides the required context.

The Functional Interface tells Java:

input type → String
output type → Integer
method → apply()
20. Lambda vs Method Reference
Lambda
name -> name.toUpperCase()
Method Reference
String::toUpperCase
Comparison
Lambda	Method Reference
name -> name.toUpperCase()	String::toUpperCase
More explicit	More concise
Can contain additional logic	Usually directly delegates to existing method
Can perform multiple operations	Refers to an existing method/constructor
21. When Can Lambda Be Replaced by Method Reference?

A Lambda can usually be replaced when it simply calls an existing method.

Example:

name -> name.toUpperCase()

Can become:

String::toUpperCase

Another example:

value -> System.out.println(value)

Can become:

System.out::println
22. When Cannot Lambda Be Replaced?

If the Lambda contains additional logic, a method reference may not be appropriate.

Example:

name -> name.toUpperCase() + "!"

There is no direct single method reference for this complete operation.

So keep:

name -> name.toUpperCase() + "!"
Another Example
number -> number * 2

This is a calculation rather than simply delegating to an existing method.

So a method reference is not appropriate here.

23. Method Invocation vs Method Reference

This is an important interview question.

Method Invocation
System.out.println("Hello");

The method executes immediately.

Method Reference
System.out::println

The method is being referenced for later execution.

Example:

Consumer<String> consumer =
        System.out::println;

The method is executed when:

consumer.accept("Hello");

is called.

24. Method Reference Does Not Execute the Method

Consider:

String::toUpperCase

This does not immediately execute toUpperCase().

It creates a reference that can be used later.

Example:

Function<String, String> function =
        String::toUpperCase;

String result = function.apply("java");

The method executes when:

function.apply("java");

is called.

25. Method Reference with this

Inside an instance method, we can use:

this::methodName

Example:

class Employee {

    public void printName(String name) {
        System.out.println(name);
    }

    public void process() {

        Consumer<String> consumer =
                this::printName;

        consumer.accept("Chetan");
    }
}

Equivalent Lambda:

Consumer<String> consumer =
        name -> this.printName(name);
26. Method Reference with super

We can also use:

super::methodName

Example:

class Parent {

    public void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    public void process() {

        Runnable runnable =
                super::display;

        runnable.run();
    }
}

Equivalent Lambda:

Runnable runnable =
        () -> super.display();
27. Method Reference with Sorting

Method references can make sorting code cleaner.

Example:

List<String> names =
        Arrays.asList("Chetan", "Amit", "Rahul");

names.sort(String::compareTo);

System.out.println(names);

Equivalent Lambda:

names.sort((a, b) -> a.compareTo(b));
28. Method Reference with Custom Objects

Consider:

class Employee {

    private int id;
    private String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

Suppose we have:

List<Employee> employees = Arrays.asList(
        new Employee(1, "Chetan"),
        new Employee(2, "Rahul"),
        new Employee(3, "Amit")
);

We can print names using:

employees.stream()
        .map(Employee::getName)
        .forEach(System.out::println);

Equivalent Lambda:

employees.stream()
        .map(employee -> employee.getName())
        .forEach(name -> System.out.println(name));
29. Overloaded Methods and Method References

Method references can point to overloaded methods.

Java determines which method to use based on the target Functional Interface and method arguments.

Example:

class Printer {

    public void print(String value) {
        System.out.println(value);
    }

    public void print(int value) {
        System.out.println(value);
    }
}

If we write:

Printer printer = new Printer();

Consumer<String> consumer =
        printer::print;

Java selects:

print(String)

because the target type is:

Consumer<String>
30. Ambiguous Method References

Sometimes Java cannot determine which overloaded method should be used.

Example:

class Test {

    public void process(String value) {
    }

    public void process(Integer value) {
    }
}

If the target type does not provide enough information, the method reference may become ambiguous.

Interview Point

Overloaded method references are resolved using the target type and method signature. If Java cannot determine a unique match, compilation fails.

31. Constructor Reference vs Object Creation
Normal Object Creation
Employee employee =
        new Employee("Chetan");
Constructor Reference
Function<String, Employee> function =
        Employee::new;

The constructor is not executed when the reference is created.

It is executed when:

function.apply("Chetan");

is called.

32. Method Reference with Multiple Parameters

Method references can work with Functional Interfaces having multiple parameters.

Example:

BinaryOperator<Integer> max =
        Math::max;

System.out.println(max.apply(10, 20));

Output:

20

Equivalent Lambda:

BinaryOperator<Integer> max =
        (a, b) -> Math.max(a, b);
33. Static vs Instance Method Reference
Static
Integer::parseInt

Equivalent:

value -> Integer.parseInt(value)
Instance of Particular Object
name::toUpperCase

Equivalent:

() -> name.toUpperCase()
Instance of Arbitrary Object
String::toUpperCase

Equivalent:

value -> value.toUpperCase()
34. Four Types Quick Table
Type	Syntax	Example
Static Method	Class::staticMethod	Integer::parseInt
Particular Object	object::method	name::toUpperCase
Arbitrary Object	Class::method	String::toUpperCase
Constructor	Class::new	ArrayList::new
35. Important Difference: String::toUpperCase vs "java"::toUpperCase

This is a very common tricky question.

String::toUpperCase
Function<String, String> function =
        String::toUpperCase;

The String object is supplied later.

function.apply("java");
"java"::toUpperCase
Supplier<String> supplier =
        "java"::toUpperCase;

The object is already fixed.

supplier.get();
Remember
String::method

→ arbitrary String object

object::method

→ particular object

36. Method Reference and Java 8

Method References were introduced in:

Java 8

They are commonly used together with:

Lambda Expressions
Functional Interfaces
Stream API
Collections
java.util.function
37. Practical Example

Suppose:

List<String> names =
        Arrays.asList("chetan", "rahul", "amit");
Without Lambda
for (String name : names) {
    System.out.println(name);
}
Lambda
names.forEach(name -> System.out.println(name));
Method Reference
names.forEach(System.out::println);

Method Reference gives the shortest and cleanest version.

38. Practical Stream Example
List<String> names =
        Arrays.asList("chetan", "rahul", "amit", "rohit");

names.stream()
        .filter(name -> name.length() > 5)
        .map(String::toUpperCase)
        .forEach(System.out::println);

Here:

name -> name.length() > 5

is a Lambda because we are applying a condition.

String::toUpperCase

is a Method Reference because we are directly calling an existing method.

System.out::println

is also a Method Reference.

39. Why Not Convert Every Lambda to Method Reference?

Method References should improve readability.

Do not force a method reference when the Lambda is clearer.

For example:

name -> name.toUpperCase() + "!"

is clearer than trying to create complicated alternatives.

Interview Point

Use a method reference when it makes the code simpler and directly represents an existing method call. Otherwise, use a Lambda.

40. Common Interview Questions
Q1. What is a Method Reference?

Answer:

A Method Reference is a shorthand syntax for a Lambda Expression when the Lambda simply calls an existing method or constructor.

Example:

names.forEach(name -> System.out.println(name));

can be written as:

names.forEach(System.out::println);
Q2. Which operator is used for Method References?

Answer:

The double-colon operator:

::

is used.

Example:

System.out::println
Q3. How many types of Method References are there?

Answer:

There are four main types:

Static method
Instance method of a particular object
Instance method of an arbitrary object
Constructor reference
Q4. What is the syntax for a static method reference?

Answer:

ClassName::staticMethodName

Example:

Integer::parseInt
Q5. What is the syntax for an instance method of a particular object?

Answer:

object::instanceMethod

Example:

String name = "Chetan";

Supplier<Integer> supplier =
        name::length;
Q6. What is the syntax for an instance method of an arbitrary object?

Answer:

ClassName::instanceMethod

Example:

Function<String, Integer> function =
        String::length;

The String object is supplied later.

Q7. What is a constructor reference?

Answer:

A constructor reference is a method reference used to refer to a constructor.

Syntax:

ClassName::new

Example:

Supplier<ArrayList<String>> supplier =
        ArrayList::new;
Q8. What is the difference between a Lambda and a Method Reference?

Answer:

A Lambda can contain logic, while a Method Reference directly refers to an existing method or constructor.

Example:

name -> name.toUpperCase()

can be simplified to:

String::toUpperCase
Q9. Does a Method Reference execute the method immediately?

Answer:

No.

A Method Reference only refers to the method. The method executes when the Functional Interface method is invoked.

Example:

Consumer<String> consumer =
        System.out::println;

consumer.accept("Hello");
Q10. Can a Method Reference exist without a Functional Interface?

Answer:

Method References normally require a target type, typically a Functional Interface.

Example:

Function<String, Integer> function =
        String::length;

The Functional Interface provides the context needed to resolve the method reference.

Q11. Can every Lambda be converted into a Method Reference?

Answer:

No.

Only when the Lambda directly delegates to an existing method or constructor.

Example:

name -> name.toUpperCase()

can become:

String::toUpperCase

But:

name -> name.toUpperCase() + "!"

cannot be directly replaced by a simple method reference.

Q12. What is the difference between String::length and str::length?

Answer:

String::length

is a reference to the instance method for an arbitrary String object.

str::length

is a reference to the method of a specific String object.

Q13. Can Method References refer to static methods?

Answer:

Yes.

Example:

Function<String, Integer> function =
        Integer::parseInt;
Q14. Can Method References refer to constructors?

Answer:

Yes.

Example:

Function<String, StringBuilder> function =
        StringBuilder::new;
Q15. Can Method References be used with Streams?

Answer:

Yes.

They are commonly used with Stream operations such as:

map()
forEach()
sorted()

Example:

names.stream()
        .map(String::toUpperCase)
        .forEach(System.out::println);
41. Tricky Interview Questions
Q1. What is the difference between System.out::println and System.out.println()?
System.out::println

is a method reference.

It does not execute immediately.

System.out.println()

is a method invocation.

It executes immediately.

Q2. What does String::toUpperCase mean?

It means:

For a String object provided as input, invoke its toUpperCase() instance method.

Equivalent:

value -> value.toUpperCase()
Q3. What does "Java"::toUpperCase mean?

It means the object is already fixed:

"Java"

Equivalent:

() -> "Java".toUpperCase()

Therefore the appropriate Functional Interface can be:

Supplier<String>
Q4. Why does String::length work with Function<String, Integer>?

Because:

Function<String, Integer>

expects:

Integer apply(String value)

and:

String::length

represents:

value -> value.length()

which matches the required signature.

Q5. Can method references refer to overloaded methods?

Yes.

Java uses the target type and method arguments to determine the correct overloaded method.

If Java cannot determine a unique method, compilation fails due to ambiguity.

Q6. Is a Method Reference a Functional Interface?

No.

A Method Reference is an expression.

It can be assigned to a compatible Functional Interface.

Example:

Function<String, Integer> function =
        String::length;
Q7. Is a Method Reference always shorter than a Lambda?

Usually it is more concise, but the main purpose is readability.

If a Lambda contains additional logic, the Lambda may be clearer.

Q8. Can constructor references accept parameters?

Yes.

The parameters are passed when the Functional Interface method is called.

Example:

Function<String, StringBuilder> function =
        StringBuilder::new;

StringBuilder builder =
        function.apply("Java");
42. Interview-Level Practical Example

Consider:

class Employee {

    private int id;
    private String name;

    public Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

Now:

List<Employee> employees = Arrays.asList(
        new Employee(1, "Chetan"),
        new Employee(2, "Rahul"),
        new Employee(3, "Amit")
);

We can extract names:

List<String> names = employees.stream()
        .map(Employee::getName)
        .collect(Collectors.toList());

Print them:

names.forEach(System.out::println);

Equivalent Lambda:

List<String> names = employees.stream()
        .map(employee -> employee.getName())
        .collect(Collectors.toList());

names.forEach(name -> System.out.println(name));
Interview Explanation

Here:

Employee::getName

is an instance method reference of an arbitrary Employee object.

The Employee object is supplied by the Stream.

43. Method Reference Decision Rule

When you see a Lambda, ask:

Step 1

Does it simply call an existing method?

x -> x.method()

If yes, try:

ClassName::method
Step 2

Is the object already fixed?

x -> object.method()

If yes, try:

object::method
Step 3

Is it creating an object?

x -> new ClassName(x)

If yes, try:

ClassName::new
Step 4

Is it a static method?

x -> ClassName.method(x)

If yes, try:

ClassName::method
44. Lambda → Method Reference Conversion
Example 1
name -> name.toUpperCase()

↓

String::toUpperCase
Example 2
name -> name.length()

↓

String::length
Example 3
value -> System.out.println(value)

↓

System.out::println
Example 4
value -> Integer.parseInt(value)

↓

Integer::parseInt
Example 5
name -> new StringBuilder(name)

↓

StringBuilder::new
45. Lambda vs Method Reference — Interview Comparison
Feature	Lambda	Method Reference
Syntax	x -> x.method()	Class::method
Introduced	Java 8	Java 8
Can contain custom logic	Yes	No, it directly references an existing method/constructor
Readability	Good	Often better for simple delegation
Uses Functional Interface	Yes	Yes, as target type
Can reference constructor	Through new	Class::new
Can reference static method	Yes	Yes
Can reference instance method	Yes	Yes
46. Common Mistakes
Mistake 1: Thinking :: executes the method

Wrong:

String::toUpperCase

does not execute the method immediately.

Mistake 2: Confusing particular and arbitrary object
String::length

and:

str::length

are not the same kind of method reference.

Mistake 3: Trying to convert every Lambda

Not every Lambda has a suitable method reference.

Example:

x -> x * 2

does not directly correspond to a single existing method.

Mistake 4: Forgetting the target type

This:

String::length

needs contextual information to know how the method reference will be used.

Example:

Function<String, Integer> function =
        String::length;
47. Quick Revision
Method Reference
        ↓
Shorthand for Lambda
        ↓
Uses ::
        ↓
Java 8
Four Types
1. Static Method
   Class::staticMethod

2. Particular Object
   object::instanceMethod

3. Arbitrary Object
   Class::instanceMethod

4. Constructor
   Class::new
Examples
Integer::parseInt
System.out::println
String::toUpperCase
"java"::toUpperCase
ArrayList::new
48. Most Important Interview Points

Remember these points:

Method References were introduced in Java 8.
They use the :: operator.
They are shorthand for certain Lambda Expressions.
They usually work with Functional Interfaces.
There are four main types.
ClassName::staticMethod → static method.
object::method → particular object.
ClassName::instanceMethod → arbitrary object.
ClassName::new → constructor reference.
Method References do not execute the method immediately.
The target Functional Interface helps Java resolve the method.
Not every Lambda can be converted into a Method Reference.
String::length and str::length represent different forms of instance method references.
Method References are heavily used with Streams and Collections.
Use them when they make the code simpler and more readable.
49. One-Line Interview Answer

A Method Reference in Java 8 is a concise way of referring to an existing method or constructor using the :: operator, usually as an alternative to a Lambda Expression.

Example:

names.forEach(System.out::println);

instead of:

names.forEach(name -> System.out.println(name));
50. Final Example — Lambda vs Method Reference
Lambda
List<String> names =
        Arrays.asList("Chetan", "Rahul", "Amit");

names.stream()
        .filter(name -> !name.isEmpty())
        .map(name -> name.toUpperCase())
        .forEach(name -> System.out.println(name));
Method References where applicable
List<String> names =
        Arrays.asList("Chetan", "Rahul", "Amit");

names.stream()
        .filter(name -> !name.isEmpty())
        .map(String::toUpperCase)
        .forEach(System.out::println);

Notice that the filter() remains a Lambda because it contains a condition, while map() and forEach() can directly reference existing methods.

51. Final Interview Summary
Lambda:
x -> x.toUpperCase()

Method Reference:
String::toUpperCase
Lambda:
x -> System.out.println(x)

Method Reference:
System.out::println
Lambda:
x -> Integer.parseInt(x)

Method Reference:
Integer::parseInt
Lambda:
x -> new Employee(x)

Method Reference:
Employee::new
Golden Rule

If a Lambda only passes its argument to an existing method, check whether a Method Reference can make the code cleaner.
