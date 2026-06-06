# What is OOP? Explain the Four Pillars of OOP with Real-World Examples.


Object-Oriented Programming (OOP) is a programming paradigm that organizes software around objects rather than functions.

An object contains:

- State (Attributes/Variables)
- Behavior (Methods)

Java follows OOP principles to improve:

- Reusability
- Maintainability
- Scalability
- Security

The four pillars of OOP are:

1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism

---

# What is a Class?

A class is a blueprint used to create objects.

### Example

```java
class Employee {
    String name;
    int age;
}
```

Here, Employee is a class.

---

# What is an Object?

An object is an instance of a class.

### Example

```java
Employee emp = new Employee();
```

Here:

- Employee → Class
- emp → Object

---

# Four Pillars of OOP

---

# 1. Encapsulation

## Definition

Encapsulation is the process of binding data and methods together into a single unit and restricting direct access to data.

We achieve encapsulation using:

- Private variables
- Public getter/setter methods

---

## Example

```java
class Employee {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

Usage:

```java
Employee emp = new Employee();

emp.setName("Chetan");

System.out.println(emp.getName());
```

---

## Real-World Example

ATM Machine

You can:

```text
Withdraw Money
Deposit Money
Check Balance
```

But you cannot directly access:

```text
Bank Database
```

Data is hidden.

This is Encapsulation.

---

## Advantages

- Data Security
- Better Control
- Easy Maintenance

---

# 2. Abstraction

## Definition

Abstraction means hiding implementation details and exposing only essential functionality.

---

## Example

```java
interface Vehicle {

    void start();
}
```

Implementation:

```java
class Car implements Vehicle {

    @Override
    public void start() {
        System.out.println("Car Started");
    }
}
```

Usage:

```java
Vehicle vehicle = new Car();

vehicle.start();
```

User only knows:

```java
vehicle.start();
```

Not how the engine starts internally.

---

## Real-World Example

Car Driving

You know:

```text
Accelerator
Brake
Steering
```

You don't know:

```text
Engine Combustion
Fuel Injection
Gear Mechanism
```

This is Abstraction.

---

## Achieved Using

- Interface
- Abstract Class

---

# 3. Inheritance

## Definition

Inheritance allows one class to acquire properties and methods of another class.

It promotes code reusability.

---

## Example

Parent Class:

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}
```

Child Class:

```java
class Dog extends Animal {

    void bark() {
        System.out.println("Barking");
    }
}
```

Usage:

```java
Dog dog = new Dog();

dog.eat();
dog.bark();
```

---

## Real-World Example

```text
Animal
   |
   +-- Dog
   +-- Cat
```

Dog and Cat inherit common behavior from Animal.

---

## Advantages

- Code Reusability
- Reduced Duplication
- Easy Maintenance

---

# 4. Polymorphism

## Definition

Polymorphism means "One Thing, Many Forms."

The same method can behave differently based on the object.

---

## Types of Polymorphism

### 1. Compile-Time Polymorphism

Method Overloading

### 2. Runtime Polymorphism

Method Overriding

---

# Compile-Time Polymorphism

## Example

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

Same method:

```java
add()
```

Different parameters.

This is Method Overloading.

---

# Runtime Polymorphism

## Example

Parent:

```java
class Animal {

    void sound() {
        System.out.println("Animal Sound");
    }
}
```

Child:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

Usage:

```java
Animal animal = new Dog();

animal.sound();
```

Output:

```text
Bark
```

Method is decided at runtime.

This is Method Overriding.

---

# Summary of Four Pillars

| Pillar | Purpose | Example |
|----------|----------|----------|
| Encapsulation | Hide Data | ATM |
| Abstraction | Hide Implementation | Car |
| Inheritance | Reuse Code | Animal → Dog |
| Polymorphism | Multiple Behaviors | Animal → Dog → Bark |

---

# Frequently Asked Interview Questions

## Q1: Why does Java follow OOP?

### Answer

To improve:

- Reusability
- Maintainability
- Security
- Scalability

---

## Q2: Which OOP concept provides security?

### Answer

Encapsulation.

---

## Q3: Which OOP concept provides code reusability?

### Answer

Inheritance.

---

## Q4: Which OOP concept hides implementation details?

### Answer

Abstraction.

---

## Q5: Which OOP concept allows one interface with multiple implementations?

### Answer

Polymorphism.

---

## Q6: Can Java support multiple inheritance?

### Answer

Java does not support multiple inheritance through classes.

Example (Not Allowed):

```java
class C extends A, B {
}
```

Java supports multiple inheritance through interfaces.

---

## Q7: Is Java 100% Object-Oriented?

### Answer

No.

Because Java supports primitive data types:

```java
int
char
double
boolean
```

Pure OOP languages do not have primitive types.

---

# Difference Between Encapsulation and Abstraction

| Encapsulation | Abstraction |
|--------------|------------|
| Hides Data | Hides Implementation |
| Uses Private Variables | Uses Interface/Abstract Class |
| Focuses on Security | Focuses on Simplicity |

---

# Common Interview Traps

## Question

Can private methods be overridden?

### Answer

No.

Private methods are not inherited.

---

## Question

Which OOP concept is achieved using getters and setters?

### Answer

Encapsulation.

---

## Question

Which OOP concept is achieved using interfaces?

### Answer

Abstraction.

---

# Key Points for Revision

- OOP = Object-Oriented Programming.
- Four Pillars:
  - Encapsulation
  - Abstraction
  - Inheritance
  - Polymorphism
- Encapsulation = Hide Data.
- Abstraction = Hide Implementation.
- Inheritance = Reuse Code.
- Polymorphism = Multiple Behaviors.
- Java supports multiple inheritance through interfaces.
- Java is not 100% Object-Oriented because of primitive types.

---

# One-Line Summary

OOP is a programming paradigm based on objects, and its four pillars—Encapsulation, Abstraction, Inheritance, and Polymorphism—help build reusable, maintainable, secure, and scalable applications.

# Tricky Interview Questions on OOP

---

## Q1: Is Java 100% Object-Oriented?

### Answer

No.

Java supports primitive data types:

```java
int
char
double
boolean
```

Pure Object-Oriented languages do not have primitive types.

Therefore Java is not considered 100% Object-Oriented.

---

## Q2: Why is String considered an Object if String literals are used?

### Answer

String is a class in Java.

Example:

```java
String name = "Chetan";
```

Even though it looks like a primitive value, JVM internally creates a String object.

---

## Q3: Can We Achieve Encapsulation Without Getters and Setters?

### Answer

Yes.

Encapsulation means restricting direct access to data.

Getters and setters are just one way to achieve it.

Example:

```java
class Employee {

    private String name;

    public void updateName(String newName) {
        this.name = newName;
    }
}
```

Still encapsulated.

---

## Q4: Is Encapsulation Only About Making Variables Private?

### Answer

No.

Making variables private is one implementation.

Actual goal:

```text
Data Hiding
Controlled Access
```

---

## Q5: Can We Achieve Abstraction Without Interfaces?

### Answer

Yes.

Using Abstract Classes.

Example:

```java
abstract class Vehicle {

    abstract void start();
}
```

Abstraction can be achieved using:

- Interface
- Abstract Class

---

## Q6: Can We Create an Object of an Abstract Class?

### Answer

No.

Invalid:

```java
abstract class Vehicle {}

Vehicle v = new Vehicle();
```

Compilation Error.

---

## Q7: Can an Abstract Class Have Constructors?

### Answer

Yes.

Example:

```java
abstract class Vehicle {

    Vehicle() {
        System.out.println("Constructor");
    }
}
```

Abstract classes can have constructors.

---

## Q8: Why Do Abstract Classes Have Constructors If We Cannot Create Objects?

### Answer

Because constructors are executed when child classes are instantiated.

Example:

```java
class Car extends Vehicle {

}
```

```java
Car car = new Car();
```

Parent constructor executes first.

---

## Q9: Can an Interface Have Method Implementations?

### Answer

Yes.

Since Java 8:

```java
default void test() {

}
```

and

```java
static void test() {

}
```

Since Java 9:

```java
private void helper() {

}
```

---

## Q10: Why Does Java Not Support Multiple Inheritance Through Classes?

### Answer

To avoid Diamond Problem.

Example:

```java
class A {
    void show() {}
}

class B extends A {
}

class C extends A {
}

class D extends B, C {
}
```

JVM becomes confused about which implementation to use.

---

## Q11: What Is Diamond Problem?

### Answer

When multiple parent classes provide the same method and child inherits from both.

Example:

```text
      A
     / \
    B   C
     \ /
      D
```

If both B and C have:

```java
show()
```

Which method should D use?

This ambiguity is called Diamond Problem.

---

## Q12: Why Does Java Allow Multiple Inheritance Through Interfaces?

### Answer

Interfaces do not force implementation inheritance.

Example:

```java
interface A {
    void show();
}

interface B {
    void show();
}
```

Child class must provide implementation.

No ambiguity exists.

---

## Q13: Can Constructors Be Overridden?

### Answer

No.

Constructors are not inherited.

Only inherited methods can be overridden.

---

## Q14: Can Static Methods Be Overridden?

### Answer

No.

Static methods belong to class.

They are hidden, not overridden.

Example:

```java
class Parent {

    static void show() {}
}

class Child extends Parent {

    static void show() {}
}
```

This is Method Hiding.

---

## Q15: Can Private Methods Be Overridden?

### Answer

No.

Private methods are not inherited.

Therefore overriding is impossible.

---

## Q16: Which OOP Pillar Is Most Important?

### Answer

There is no "most important."

All four pillars work together:

```text
Encapsulation → Security
Abstraction → Simplicity
Inheritance → Reusability
Polymorphism → Flexibility
```

---

## Q17: What Is the Difference Between IS-A and HAS-A Relationship?

### Answer

IS-A → Inheritance

Example:

```text
Dog IS-A Animal
```

HAS-A → Composition/Aggregation

Example:

```text
Car HAS-A Engine
```

---

## Q18: What Is More Preferred in Real Projects: Inheritance or Composition?

### Answer

Composition.

Reason:

- Loose Coupling
- Better Maintainability
- Better Testing

This follows:

```text
Favor Composition Over Inheritance
```

which is a very popular interview point.

---

## Q19: Why Is Composition Preferred Over Inheritance?

### Answer

Inheritance creates tight coupling.

Composition provides flexibility.

Example:

```java
class Car {

    Engine engine;
}
```

Instead of:

```java
class Car extends Engine {
}
```

---

## Q20: Which OOP Concept Is Used Most in Spring Boot?

### Answer

All four are used.

Examples:

### Encapsulation

```java
@Entity
```

Private fields + getters/setters.

### Abstraction

```java
JpaRepository
```

### Inheritance

```java
RuntimeException
```

hierarchy.

### Polymorphism

```java
@Autowired
```

injects different implementations.

---

# Interviewer's Favorite Rapid Fire Questions

### Can Constructor Be Final?

No.

---

### Can Constructor Be Static?

No.

---

### Can Abstract Class Have Static Methods?

Yes.

---

### Can Interface Have Variables?

Yes.

All variables are:

```java
public static final
```

by default.

---

### Can We Instantiate Interface?

No.

---

### Can Final Class Be Inherited?

No.

Example:

```java
String
```

is final.

---

### Can Final Method Be Overridden?

No.

---

### Can We Have Multiple Interfaces?

Yes.

```java
class Employee implements A, B, C {

}
```

---

# Most Asked OOP Interview Question

### Why Does Java Support Multiple Inheritance Through Interfaces But Not Classes?

### Answer
   
To avoid Diamond Problem.
  
Interfaces provide contracts  , not implementation inheritance, so ambiguity is avoided.



# Method Overloading vs Method Overriding

## Interview Answer

Method Overloading means having multiple methods with the same name but different parameters within the same class.

Method Overriding means redefining a parent class method in the child class with the same method signature.

---

# Method Overloading

## Example

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

This is Method Overloading.

---

# Method Overriding

## Example

```java
class Animal {

    void sound() {
        System.out.println("Animal Sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

This is Method Overriding.

---

# Difference

| Feature | Overloading | Overriding |
|----------|------------|------------|
| Occurs In | Same Class | Parent & Child |
| Parameters | Must Differ | Must Be Same |
| Return Type | Can Differ | Must Be Compatible |
| Polymorphism | Compile Time | Runtime |
| Annotation | Not Required | @Override Recommended |

---

# Frequently Asked Questions

## Can We Overload main() Method?

### Answer

Yes.

```java
public static void main(String[] args) {

}

public static void main(int a) {

}
```

JVM only calls:

```java
main(String[] args)
```

---

## Can Static Methods Be Overridden?

### Answer

No.

Static methods are hidden, not overridden.

---

## Can Private Methods Be Overridden?

### Answer

No.

Private methods are not inherited.

---

## Can Constructors Be Overloaded?

### Answer

Yes.

---

## Can Constructors Be Overridden?

### Answer

No.

Constructors are not inherited.
# Abstract Class vs Interface

## Interview Answer

Both Abstract Classes and Interfaces are used to achieve abstraction.

Use Abstract Class when classes share common state and behavior.

Use Interface when classes need to follow a common contract.

---

# Abstract Class Example

```java
abstract class Vehicle {

    String brand;

    abstract void start();

    void stop() {
        System.out.println("Stopped");
    }
}
```

---

# Interface Example

```java
interface Vehicle {

    void start();
}
```

---

# Difference Between Abstract Class and Interface

| Feature | Abstract Class | Interface |
|----------|---------------|-----------|
| Constructor | Yes | No |
| Instance Variables | Yes | No |
| Multiple Inheritance | No | Yes |
| Method Implementation | Yes | Yes (Java 8+) |
| Access Modifier | Any | Public by default |
| Keyword | extends | implements |

---

# When To Use Abstract Class?

Use when:

```text
Classes share common code.
```

Example:

```text
Vehicle
   |
   +-- Car
   +-- Bike
```

---

# When To Use Interface?

Use when:

```text
Different classes need same capability.
```

Example:

```text
Flyable
```

Implemented by:

```text
Bird
Aeroplane
Drone
```

---

# Frequently Asked Questions

## Can Interface Have Methods With Body?

### Answer

Yes.

Since Java 8:

```java
default void test() {

}
```

```java
static void test() {

}
```

---

## Can Interface Have Private Methods?

### Answer

Yes.

Since Java 9.

---

## Can Abstract Class Have Constructor?

### Answer

Yes.

---

## Can We Create Object Of Abstract Class?

### Answer

No.
# IS-A vs HAS-A Relationship

## IS-A Relationship

Represents Inheritance.

Example:

```text
Dog IS-A Animal
```

```java
class Animal {

}

class Dog extends Animal {

}
```

---

# HAS-A Relationship

Represents Composition/Aggregation.

Example:

```text
Car HAS-A Engine
```

```java
class Engine {

}

class Car {

    private Engine engine;
}
```

---

# Difference

| IS-A | HAS-A |
|--------|--------|
| Inheritance | Composition |
| Tight Coupling | Loose Coupling |
| extends Keyword | Object Reference |
| Strong Relationship | Flexible Relationship |

---

# Interview Favorite Question

## Which Is Preferred?

### Answer

HAS-A Relationship.

Reason:

```text
Favor Composition Over Inheritance
```

Because it provides:

- Loose Coupling
- Better Testing
- Better Maintainability
# Association vs Aggregation vs Composition

## Association

A general relationship between two classes.

Example:

```text
Teacher ↔ Student
```

Both can exist independently.

---

## Example

```java
class Teacher {

}

class Student {

}
```

---

# Aggregation

A weak HAS-A relationship.

Child can exist without Parent.

Example:

```text
Department HAS-A Teacher
```

Teacher can exist even if Department is deleted.

---

## Example

```java
class Teacher {

}

class Department {

    List<Teacher> teachers;
}
```

---

# Composition

A strong HAS-A relationship.

Child cannot exist without Parent.

Example:

```text
House HAS-A Room
```

If House is destroyed:

```text
Room also ceases to exist.
```

---

## Example

```java
class Room {

}

class House {

    private Room room = new Room();
}
```

---

# Difference

| Association | Aggregation | Composition |
|------------|------------|------------|
| General Relation | Weak HAS-A | Strong HAS-A |
| Independent Objects | Child Independent | Child Dependent |
| Ownership | No | Partial | Full |

---

# Real World Examples

Association

```text
Doctor ↔ Patient
```

Aggregation

```text
Department → Employee
```

Composition

```text
House → Room
```

---

# Interview Favorite Question

## Which Is Stronger?

### Answer

Composition.

Because child lifecycle depends on parent lifecycle.
