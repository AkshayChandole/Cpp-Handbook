# [What is an abstract class and when do you use it?](#what-is-an-abstract-class-and-when-do-you-use-it)

## **Abstract Class in C++**

An **abstract class** in C++ is a class that cannot be instantiated and is designed to be a base class for other classes. It provides a blueprint for derived classes to implement its behavior.

---

## **Key Characteristics of an Abstract Class**
1. **Contains at least one pure virtual function**:
   - A **pure virtual function** is declared by assigning `0` to a virtual function, e.g., `virtual void display() = 0;`.
   - This makes the class abstract.
2. **Cannot be instantiated**:
   - You cannot create an object of an abstract class. It can only be used as a base class.
3. **Derived classes must override pure virtual functions**:
   - A derived class must provide implementations for all pure virtual functions unless the derived class is also abstract.
4. **May contain non-abstract (regular) members**:
   - An abstract class can have data members, fully implemented member functions, and constructors.

---

## **When to Use an Abstract Class**

1. **To enforce a contract**:
   - Use an abstract class when you want to ensure that derived classes implement specific behaviors.
   - For example, when creating a hierarchy of related objects that must have a certain set of operations.

2. **Polymorphism**:
   - Abstract classes enable polymorphism, where you can interact with objects of derived classes through base class pointers or references.

3. **Code Reusability**:
   - An abstract class allows you to implement shared functionality in the base class while leaving the specific behavior to the derived classes.

4. **Framework or Interface Design**:
   - Abstract classes serve as a foundation for designing frameworks and interfaces where behavior is defined but not implemented.

---

## **Example of an Abstract Class**

### Scenario: Shapes Hierarchy
You want to define different shapes (e.g., Circle, Rectangle, Triangle) with a common interface to compute the area and perimeter.

```cpp
#include <iostream>
#include <cmath>
using namespace std;

// Abstract class
class Shape {
public:
    virtual double area() const = 0;      // Pure virtual function
    virtual double perimeter() const = 0; // Pure virtual function

    void display() const {
        cout << "Area: " << area() << endl;
        cout << "Perimeter: " << perimeter() << endl;
    }

    virtual ~Shape() = default; // Virtual destructor
};

// Derived class: Circle
class Circle : public Shape {
    double radius;
public:
    Circle(double r) : radius(r) {}

    double area() const override {
        return M_PI * radius * radius;
    }

    double perimeter() const override {
        return 2 * M_PI * radius;
    }
};

// Derived class: Rectangle
class Rectangle : public Shape {
    double length, width;
public:
    Rectangle(double l, double w) : length(l), width(w) {}

    double area() const override {
        return length * width;
    }

    double perimeter() const override {
        return 2 * (length + width);
    }
};

int main() {
    Shape* circle = new Circle(5.0);
    Shape* rectangle = new Rectangle(4.0, 6.0);

    cout << "Circle: " << endl;
    circle->display();

    cout << "\nRectangle: " << endl;
    rectangle->display();

    delete circle;
    delete rectangle;

    return 0;
}
```

**Output**:
```
Circle: 
Area: 78.5398
Perimeter: 31.4159

Rectangle: 
Area: 24
Perimeter: 20
```

---

## **Key Points in the Example**
1. `Shape` is an abstract class with pure virtual functions `area` and `perimeter`.
2. `Circle` and `Rectangle` are derived classes that implement the `area` and `perimeter` methods.
3. Polymorphism allows you to use a `Shape*` pointer to interact with both `Circle` and `Rectangle`.

---

## **Advantages of Abstract Classes**
1. **Encapsulation of Common Functionality**:
   - Shared behaviors can be implemented in the abstract class.
2. **Polymorphism**:
   - Enables writing flexible and reusable code by working with base class pointers/references.
3. **Enforces Interface Consistency**:
   - Ensures that all derived classes adhere to the defined contract.

---

## **Abstract Class vs Interface**
1. **Abstract Class**:
   - Can have both abstract and non-abstract members (e.g., implemented methods).
   - Supports single inheritance (in C++).
   - Can have constructors and data members.

2. **Interface** (in other languages or through pure abstract classes):
   - All methods are pure virtual.
   - Does not contain any implemented methods.
   - Often used when multiple inheritance of behavior is required.
  
---
