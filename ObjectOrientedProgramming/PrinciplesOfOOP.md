# [4.1 Principles of OOP](#41-principles-of-oop)

The four core principles of Object-Oriented Programming (OOP)—**Encapsulation**, **Abstraction**, **Inheritance**, and **Polymorphism**—form the foundation of designing robust, modular, and reusable code in C++.

---

## [**4.1.1 Encapsulation**](#411-encapsulation)
Encapsulation is the mechanism of bundling data (variables) and methods (functions) that operate on the data into a single unit, i.e., a **class**. It also restricts direct access to some of the object's components, which is essential for maintaining control over the internal state of an object.

**Key Points:**
- Data is hidden using access modifiers (`private`, `protected`, `public`).
- Access to the hidden data is provided through getter and setter methods.

**Example:**

```cpp
#include <iostream>
#include <string>

class Person {
private:
    std::string name; // Encapsulated data
    int age;

public:
    // Setter methods
    void setName(const std::string& newName) { name = newName; }
    void setAge(int newAge) { age = newAge; }

    // Getter methods
    std::string getName() const { return name; }
    int getAge() const { return age; }
};

int main() {
    Person person;
    person.setName("John Doe");
    person.setAge(30);

    std::cout << "Name: " << person.getName() << "\n"; // Output: Name: John Doe
    std::cout << "Age: " << person.getAge() << "\n";   // Output: Age: 30

    return 0;
}
```

**Best Practices:**
- Always keep sensitive data private.
- Use getter and setter functions for controlled access.

---

## [**4.1.2 Abstraction**](#412-abstraction)
Abstraction is the process of hiding the implementation details of an object and exposing only the essential features. It helps in reducing complexity and increasing code usability.

**Key Points:**
- Achieved through abstract classes and interfaces.
- Focuses on **what** an object does rather than **how** it does it.

**Example:**

```cpp
#include <iostream>

// Abstract class
class Shape {
public:
    virtual void draw() const = 0; // Pure virtual function
};

class Circle : public Shape {
public:
    void draw() const override {
        std::cout << "Drawing a Circle\n"; // Output: Drawing a Circle
    }
};

class Rectangle : public Shape {
public:
    void draw() const override {
        std::cout << "Drawing a Rectangle\n"; // Output: Drawing a Rectangle
    }
};

int main() {
    Shape* shape1 = new Circle();
    Shape* shape2 = new Rectangle();

    shape1->draw();
    shape2->draw();

    delete shape1;
    delete shape2;

    return 0;
}
```

**Best Practices:**
- Use abstraction to separate interface and implementation.
- Avoid exposing unnecessary details.

---

## [**4.1.3 Inheritance**](#413-inheritance)
Inheritance allows a class (child or derived class) to acquire the properties and behavior of another class (parent or base class). This promotes **code reusability** and the creation of hierarchical relationships.

**Key Points:**
- `public`, `protected`, and `private` inheritance control access to inherited members.
- Supports single and multiple inheritance.

**Example:**

```cpp
#include <iostream>

// Base class
class Animal {
public:
    void eat() { std::cout << "This animal eats food.\n"; }
};

// Derived class
class Dog : public Animal {
public:
    void bark() { std::cout << "The dog barks.\n"; }
};

int main() {
    Dog dog;
    dog.eat();  // Output: This animal eats food.
    dog.bark(); // Output: The dog barks.

    return 0;
}
```

**Best Practices:**
- Use inheritance when there is a clear **"is-a"** relationship.
- Avoid excessive inheritance; prefer composition if applicable.

---

## [**4.1.4 Polymorphism**](#414-polymorphism)
Polymorphism allows the same interface to represent different types of objects. It supports dynamic (runtime) and static (compile-time) behavior.

**Types of Polymorphism:**
1. **Compile-Time Polymorphism:**
   - Achieved through function overloading and operator overloading.
   
2. **Runtime Polymorphism:**
   - Achieved through virtual functions.

**Example: Compile-Time Polymorphism (Function Overloading):**

```cpp
#include <iostream>

class Calculator {
public:
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
};

int main() {
    Calculator calc;
    std::cout << "Integer addition: " << calc.add(3, 5) << "\n";       // Output: 8
    std::cout << "Double addition: " << calc.add(2.5, 3.7) << "\n";   // Output: 6.2

    return 0;
}
```

**Example: Compile-Time Polymorphism (Operator Overloading):**

```cpp
#include <iostream>
using namespace std;

class Complex {
private:
    double real;
    double imag;

public:
    // Constructor
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}

    // Overload the + operator
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }

    // Function to display the complex number
    void display() const {
        cout << real << " + " << imag << "i" << endl;
    }
};

int main() {
    Complex c1(3.5, 2.5);
    Complex c2(1.5, 4.5);

    // Using the overloaded + operator
    Complex c3 = c1 + c2;

    cout << "Complex Number 1: ";
    c1.display(); // Output: 3.5 + 2.5i

    cout << "Complex Number 2: ";
    c2.display(); // Output: 1.5 + 4.5i

    cout << "Sum of Complex Numbers: ";
    c3.display(); // Output: 5.0 + 7.0i

    return 0;
}
```

**Example: Runtime Polymorphism (Virtual Functions):**

```cpp
#include <iostream>

class Animal {
public:
    virtual void sound() const {
        std::cout << "Animal makes a sound.\n";
    }
};

class Dog : public Animal {
public:
    void sound() const override {
        std::cout << "Dog barks.\n";
    }
};

int main() {
    Animal* animal = new Dog();
    animal->sound(); // Output: Dog barks.

    delete animal;
    return 0;
}
```

**Best Practices:**
- Use polymorphism to enable flexible and maintainable code.
- Always use `override` to prevent errors during method overriding.
- Ensure proper use of virtual destructors in polymorphic base classes.

---

These principles, when applied effectively, make C++ programs more robust, modular, and easier to maintain.

---
