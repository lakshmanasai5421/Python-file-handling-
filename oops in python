# Python OOPs Concepts

Python Object-Oriented Programming (OOP) is a programming paradigm that organizes code using **classes** and **objects**. A class is a blueprint that defines attributes and methods, while an object is an instance of a class. OOP helps developers write modular, reusable, and maintainable code.

The four main principles of OOP are encapsulation, inheritance, polymorphism, and abstraction. By using OOP concepts, programmers can build scalable applications, improve code organization, reduce redundancy, and enhance overall software development efficiency and flexibility in large projects.

---

## Table of Contents

- [Class](#class)
- [Object](#object)
- [Methods](#methods)
  - [Instance Methods](#1-instance-methods)
  - [Class Methods](#2-class-methods)
  - [Static Methods](#3-static-methods)
- [Constructors](#constructors)
  - [Default Constructor](#1-default-constructor)
  - [Parameterized Constructor](#2-parameterized-constructor)
- [Inheritance](#inheritance)
  - [Single Inheritance](#1-single-inheritance)
  - [Multiple Inheritance](#2-multiple-inheritance)
  - [Multilevel Inheritance](#3-multilevel-inheritance)
  - [Hierarchical Inheritance](#4-hierarchical-inheritance)
  - [Hybrid Inheritance](#5-hybrid-inheritance)

---

## Class

A class in Python is a user-defined blueprint or template used to create objects. It defines the structure, properties, and behaviors that the objects created from it will possess. A class acts as a logical representation of a real-world entity by combining data (attributes) and functions (methods) into a single unit.

A class is created using the `class` keyword and can contain variables, methods, constructors, and other class members. It does not occupy memory for individual data until objects are created from it. Each object created from a class is called an **instance** and has its own set of attribute values while sharing the common structure and behavior defined by the class.

For example, if `Student` is a class, it may define attributes such as `name`, `age`, and `roll_number`, along with methods such as `study()` and `display_details()`. Each student object created from this class will have its own values for these attributes while sharing the same structure and behaviors.

```python
# Creating a class
class Student:
    pass
```

---

## Object

An **object** is an instance of a class. It is a real entity created from a class that occupies memory and contains actual data. While a class serves as a blueprint, an object represents the implementation of that blueprint. Objects allow access to the attributes and methods defined within a class.

Each object has its own identity, state, and behavior. The state of an object is represented by its attribute values, while its behavior is defined by the methods of the class. Multiple objects can be created from the same class, and each object can store different data while sharing the same structure and functionality.

Objects are fundamental to OOP because they enable programmers to model real-world entities such as students, employees, cars, and bank accounts. In Python, objects are created by calling the class name followed by parentheses. Once created, an object can access the class's attributes and methods using the dot (`.`) operator.

```python
class Student:

    def display(self):
        print("Class created successfully")


# Creating object
s1 = Student()

# Calling method
s1.display()
```

**Output:**
```
Class created successfully
```

---

## Methods

A method is a function that is defined inside a class and is used to describe the behavior or actions of an object. Methods allow objects to perform specific tasks and operate on the data stored within the object.

Methods are created inside a class using the `def` keyword and typically use the `self` parameter to access the attributes and other methods of the current object.

**Why `self` is used:**

- To access instance variables (object data)
- To call other methods inside the class
- To differentiate between instance variables and local variables

```python
class Student:

    def display(self):
        print("Name:", self.name)
        print("Age:", self.age)

    def study(self):
        print(self.name, "is studying")


# Creating object
s1 = Student("John", 20)

# Calling methods
s1.display()
s1.study()
```

---

### Types of Methods

#### 1. Instance Methods

Instance methods are the most commonly used methods in Python. They work with object data (instance variables) and can access or modify it using the `self` parameter.

- First parameter is `self`
- Access instance variables
- Called using an object
- Can modify object state

```python
class Student:

    def show(self):
        print("This is an instance method")


s1 = Student()
s1.show()
```

#### 2. Class Methods

Class methods work with class variables instead of object data. They are used when we want to affect the class as a whole rather than individual objects.

- First parameter is `cls`
- Defined using `@classmethod`
- Can access and modify class variables
- Called using the class name or an object

```python
class Student:
    school = "ABC School"

    @classmethod
    def show_school(cls):
        print("School:", cls.school)


Student.show_school()
```

#### 3. Static Methods

Static methods do not depend on class or object data. They behave like normal functions but are placed inside a class for logical grouping.

- No `self` or `cls`
- Defined using `@staticmethod`
- Cannot access instance or class variables
- Used for utility functions

```python
class Math:

    @staticmethod
    def add(a, b):
        print("Sum:", a + b)


Math.add(5, 3)
```

---

## Constructors

A constructor is a special method in Python that is **automatically executed** when a new object of a class is created. Its main purpose is to initialize the state (values) of an object so that the object is ready to use immediately after creation.

In Python, the constructor is defined using the method name `__init__()`. The word "constructor" is used because it constructs or sets up the object by assigning values to its attributes.

**How it works:** when you create an object like `s1 = Student("John", 20)`, Python automatically calls `Student.__init__(s1, "John", 20)` — the constructor runs without being called manually.

**Why Constructors are Important:**

- They automatically initialize object data
- They reduce repetitive code
- They ensure every object starts in a valid state
- They improve code readability and structure
- They make object creation simple and clean

### 1. Default Constructor

A constructor that does not take any parameters except `self`. It initializes objects with default values.

```python
class Student:

    def __init__(self):
        print("Default Constructor Called")


s1 = Student()
```

### 2. Parameterized Constructor

A constructor that takes parameters to initialize object attributes with specific values.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        print(self.name, self.age)


s1 = Student("John", 20)
s1.show()
```

---

## Inheritance

Inheritance is an important concept of OOP in which one class (called the **child class**) acquires the properties and methods of another class (called the **parent class**). It helps in code reusability, meaning we can use existing code without rewriting it.

In inheritance, the child class can use the features of the parent class and can also add its own new features or modify existing ones.

- **Parent Class (Base Class):** The class whose properties are inherited.
- **Child Class (Derived Class):** The class that inherits properties from the parent class.

```python
class Animal:
    def sound(self):
        print("Animals make sound")


class Dog(Animal):
    def bark(self):
        print("Dog barks")


d1 = Dog()
d1.sound()
d1.bark()
```

**Output:**
```
Animals make sound
Dog barks
```

---

### Types of Inheritance

#### 1. Single Inheritance

A child class inherits from only one parent class. The child class can access all the properties and methods of the parent class and can also add its own new features.

*Real-life analogy:* A Car inherits basic features from a Vehicle.

```python
class Vehicle:
    def start(self):
        print("Vehicle is starting")


class Car(Vehicle):
    def stop(self):
        print("Car is stopping")


c = Car()
c.start()   # Inherited from Vehicle
c.stop()    # Own method
```

#### 2. Multiple Inheritance

A single child class inherits from more than one parent class. This allows the child class to combine features from multiple classes.

*Real-life analogy:* A child inherits driving skills from their father and cooking skills from their mother.

```python
class Father:
    def driving(self):
        print("Father can drive")


class Mother:
    def cooking(self):
        print("Mother can cook")


class Child(Father, Mother):
    pass


c = Child()
c.driving()
c.cooking()
```

#### 3. Multilevel Inheritance

A chain of inheritance where a class is derived from another derived class. It follows a **parent → child → grandchild** structure.

```python
class Grandparent:
    def home(self):
        print("Grandparent's home")


class Parent(Grandparent):
    def car(self):
        print("Parent's car")


class Child(Parent):
    def bike(self):
        print("Child's bike")


c = Child()
c.home()   # From Grandparent
c.car()    # From Parent
c.bike()   # Own method
```

#### 4. Hierarchical Inheritance

One parent class is inherited by multiple child classes. All child classes share the same base class but can have their own features.

*Real-life analogy:* One `Animal` class is shared by `Dog`, `Cat`, and `Cow`.

```python
class Animal:
    def sound(self):
        print("Animals make sound")


class Dog(Animal):
    def bark(self):
        print("Dog barks")


class Cat(Animal):
    def meow(self):
        print("Cat meows")


d = Dog()
c = Cat()

d.sound()   # Inherited
c.sound()   # Inherited
```

#### 5. Hybrid Inheritance

Hybrid inheritance is a combination of two or more types of inheritance. It is the most complex form of inheritance in Python and may include multiple, multilevel, and hierarchical inheritance together.

```python
class A:
    def showA(self):
        print("Class A")


class B(A):
    def showB(self):
        print("Class B")


class C(A):
    def showC(self):
        print("Class C")


class D(B, C):
    pass


d = D()
d.showA()   # From A (through B and C)
d.showB()   # From B
d.showC()   # From C
```

In this example, `D` inherits from both `B` and `C`, which in turn both inherit from `A`, forming a diamond-shaped inheritance structure.

---

## Quick Reference

| Concept | Keyword / Decorator | Purpose |
|---|---|---|
| Class | `class` | Blueprint for objects |
| Object | `ClassName()` | Instance of a class |
| Constructor | `__init__()` | Initialize object state |
| Instance Method | `self` | Operate on object data |
| Class Method | `@classmethod`, `cls` | Operate on class data |
| Static Method | `@staticmethod` | Utility logic, no access to class/object |
| Inheritance | `class Child(Parent)` | Reuse and extend parent class features |
