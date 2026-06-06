# What is OOP? Explain the Four Pillars of OOP with Real-World Examples.

## Interview Answer (1-2 Minutes)

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
