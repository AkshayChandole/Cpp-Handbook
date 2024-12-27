# [Explain virtual function.](#explain-virtual-function)

## **Virtual Function in C++**

A **virtual function** in C++ is a member function in a base class that you expect to be overridden in derived classes. It enables **runtime polymorphism**, allowing the program to decide at runtime which function to invoke, based on the type of the object that is being pointed to, rather than the type of the pointer.

---

## **Why Use Virtual Functions?**

1. To achieve **runtime polymorphism**.
2. To allow derived classes to provide specific implementations of functions.
3. To ensure that the correct function is invoked for an object, regardless of the type of pointer or reference used to call it.

---

## **Syntax**
```cpp
class Base {
public:
    virtual void display() { // Virtual function
        cout << "Display from Base class" << endl;
    }
};

class Derived : public Base {
public:
    void display() override { // Overriding the virtual function
        cout << "Display from Derived class" << endl;
    }
};
```

---

## **Key Characteristics of Virtual Functions**
1. **Declared with the `virtual` keyword** in the base class.
2. **Overridden in derived classes** using the same function signature.
3. A **virtual function table (vtable)** is created for classes containing virtual functions, and a pointer to this table (`vptr`) determines which function to call at runtime.
4. A **base class pointer or reference** can call a derived class's function if the function is virtual.

---

## **Example: Virtual Function in Action**

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual void display() { // Virtual function
        cout << "Display from Base class" << endl;
    }
};

class Derived : public Base {
public:
    void display() override { // Overriding the virtual function
        cout << "Display from Derived class" << endl;
    }
};

int main() {
    Base *basePtr;
    Derived derivedObj;

    basePtr = &derivedObj;

    basePtr->display(); // Runtime polymorphism: Calls Derived's display()
    return 0;
}
```

**Output:**
```
Display from Derived class
```

---

## **Pure Virtual Function**

A **pure virtual function** is a virtual function with no implementation in the base class. Declaring a pure virtual function makes the class **abstract**, and you cannot instantiate it.

### **Syntax**
```cpp
class AbstractClass {
public:
    virtual void pureVirtualFunction() = 0; // Pure virtual function
};
```

### **Example**
```cpp
#include <iostream>
using namespace std;

class Shape {
public:
    virtual void draw() = 0; // Pure virtual function
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle" << endl;
    }
};

class Rectangle : public Shape {
public:
    void draw() override {
        cout << "Drawing Rectangle" << endl;
    }
};

int main() {
    Shape *shape;

    Circle circle;
    Rectangle rectangle;

    shape = &circle;
    shape->draw(); // Calls Circle's draw()

    shape = &rectangle;
    shape->draw(); // Calls Rectangle's draw()

    return 0;
}
```

**Output:**
```
Drawing Circle
Drawing Rectangle
```

---

## **Virtual Functions and Destructors**
When using inheritance, you should make destructors virtual to ensure proper cleanup of derived class objects.

### **Example**
```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual ~Base() { // Virtual destructor
        cout << "Base Destructor" << endl;
    }
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "Derived Destructor" << endl;
    }
};

int main() {
    Base *basePtr = new Derived();
    delete basePtr; // Calls Derived's destructor first, then Base's
    return 0;
}
```

**Output:**
```
Derived Destructor
Base Destructor
```

---

## **Advantages of Virtual Functions**
1. Enables **runtime polymorphism**.
2. Provides flexibility and extensibility for object-oriented programming.
3. Ensures the correct function is called based on the actual object type.

---

## **Disadvantages**
1. **Performance Overhead**: Slightly slower due to the use of vtable for function lookups.
2. **Memory Overhead**: Each object of a class with virtual functions requires a vtable pointer (`vptr`).

---

## **Key Points to Remember**
1. Use `virtual` to enable runtime polymorphism.
2. Always declare destructors as `virtual` in base classes to avoid memory leaks.
3. Pure virtual functions force derived classes to implement specific behavior, making the base class abstract.

By using virtual functions, you can design flexible and reusable object-oriented systems.
