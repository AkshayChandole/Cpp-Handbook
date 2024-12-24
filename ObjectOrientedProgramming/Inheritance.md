# [4.3 Inheritance](#43-inheritance)

Inheritance is a core concept in Object-Oriented Programming (OOP) that allows a class to inherit properties and behaviors (methods) from another class. It promotes reusability, extensibility, and maintainability of code by enabling the creation of a hierarchical relationship between classes.

---

## [4.3.1 What is Inheritance?](#431-what-is-inheritance)

Inheritance allows a class, called the **derived class**, to reuse the properties and methods of another class, called the **base class**.

**Example**:
```cpp
#include <iostream>
using namespace std;

class Base {
public:
    void show() {
        cout << "This is the Base class." << endl;
    }
};

class Derived : public Base {
};

int main() {
    Derived obj;
    obj.show();  // Reusing Base class functionality
    return 0;
}
```

**Output**:
```
This is the Base class.
```

---

## [4.3.2 Types of Inheritance](#432-types-of-inheritance)

Inheritance can be categorized into the following types:

### [4.3.2.1 Single Inheritance](#4321-single-inheritance)

A derived class inherits from a single base class.  

**Example**:
```cpp
class Animal {
public:
    void eat() {
        cout << "This animal eats food." << endl;
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "The dog barks." << endl;
    }
};

int main() {
    Dog dog;
    dog.eat();
    dog.bark();
    return 0;
}
```

---

### [4.3.2.2 Multiple Inheritance](#4322-multiple-inheritance)

A derived class inherits from more than one base class.  

**Example**:
```cpp
class Base1 {
public:
    void showBase1() {
        cout << "Base1 class function." << endl;
    }
};

class Base2 {
public:
    void showBase2() {
        cout << "Base2 class function." << endl;
    }
};

class Derived : public Base1, public Base2 {
};

int main() {
    Derived obj;
    obj.showBase1();
    obj.showBase2();
    return 0;
}
```

---

### [4.3.2.3 Multilevel Inheritance](#4323-multilevel-inheritance)

A derived class inherits from another derived class, forming a chain.  

**Example**:
```cpp
class A {
public:
    void showA() {
        cout << "Class A function." << endl;
    }
};

class B : public A {
public:
    void showB() {
        cout << "Class B function." << endl;
    }
};

class C : public B {
public:
    void showC() {
        cout << "Class C function." << endl;
    }
};

int main() {
    C obj;
    obj.showA();
    obj.showB();
    obj.showC();
    return 0;
}
```

---

### [4.3.2.4 Hierarchical Inheritance](#4324-hierarchical-inheritance)

Multiple derived classes inherit from a single base class.  

**Example**:
```cpp
class Animal {
public:
    void eat() {
        cout << "This animal eats food." << endl;
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "The dog barks." << endl;
    }
};

class Cat : public Animal {
public:
    void meow() {
        cout << "The cat meows." << endl;
    }
};

int main() {
    Dog dog;
    Cat cat;
    dog.eat();
    dog.bark();
    cat.eat();
    cat.meow();
    return 0;
}
```

---

### [4.3.2.5 Hybrid Inheritance](#4325-hybrid-inheritance)

A combination of two or more types of inheritance, usually involving multiple and hierarchical inheritance.

**Note**: This type of inheritance often leads to the **diamond problem**, which can be resolved using virtual base classes.

---

## [4.3.3 Access Control in Inheritance](#433-access-control-in-inheritance)

The access specifier (`public`, `protected`, `private`) used during inheritance determines how the members of the base class are accessible in the derived class.

| Access Specifier | Base Class Member Access in Derived Class | Outside Access |
|-------------------|------------------------------------------|----------------|
| **Public**       | As-is                                   | Accessible    |
| **Protected**    | Becomes Protected                       | Not Accessible|
| **Private**      | Not Inherited                           | Not Accessible|

---

## [4.3.4 Function Overriding](#434-function-overriding)

Derived classes can redefine methods of the base class to provide specialized behavior. This is possible only with **virtual functions**.

**Example**:
```cpp
class Base {
public:
    virtual void show() {
        cout << "Base class function." << endl;
    }
};

class Derived : public Base {
public:
    void show() override {
        cout << "Derived class function." << endl;
    }
};

int main() {
    Base* basePtr;
    Derived d;
    basePtr = &d;

    basePtr->show();  // Calls Derived's show() due to polymorphism
    return 0;
}
```

---

## [4.3.5 Virtual Base Classes](#435-virtual-base-classes)

Virtual base classes are used to prevent duplicate inheritance of a base class in hybrid inheritance, resolving the **diamond problem**.

**Example**:
```cpp
class A {
public:
    void show() {
        cout << "Base class A." << endl;
    }
};

class B : virtual public A { };
class C : virtual public A { };
class D : public B, public C { };

int main() {
    D obj;
    obj.show();  // Only one copy of class A is inherited
    return 0;
}
```

---

## [4.3.6 Best Practices and Pitfalls](#436-best-practices-and-pitfalls)

### Best Practices:
- Use inheritance only when there is a clear **is-a** relationship.
- Prefer **composition** over inheritance if appropriate.
- Always use **virtual destructors** in base classes when dealing with inheritance and polymorphism.

### Pitfalls:
- Avoid excessive use of multiple inheritance to prevent complexity.
- Be cautious of object slicing when assigning derived objects to base class variables.
- Understand access specifiers to avoid unintentional member exposure.

--- 
