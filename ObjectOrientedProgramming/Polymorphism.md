# [4.4 Polymorphism](#44-polymorphism)

Polymorphism in C++ is the ability of an object to take many forms. It enables a single interface to represent different types of objects or behaviors. Polymorphism is a cornerstone of Object-Oriented Programming (OOP) as it promotes flexibility and reusability.

---

## Types of Polymorphism in C++

1. **[Compile-Time Polymorphism](#441-compile-time-polymorphism)** (Static Binding)  
   The method to be invoked is determined at compile-time.  
   - Achieved using **function overloading** and **operator overloading**.

2. **[Run-Time Polymorphism](#442-run-time-polymorphism)** (Dynamic Binding)  
   The method to be invoked is determined at runtime.  
   - Achieved using **virtual functions** and **function overriding**.

---

## [4.4.1 Compile-Time Polymorphism](#441-compile-time-polymorphism)

### [4.4.1.1 Function Overloading](#4411-function-overloading)
Function overloading allows multiple functions with the same name but different parameter lists.

**Example**:
```cpp
#include <iostream>
using namespace std;

class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
};

int main() {
    Calculator calc;
    cout << "Int addition: " << calc.add(5, 10) << endl;       // Calls int version
    cout << "Double addition: " << calc.add(5.5, 10.5) << endl; // Calls double version
    return 0;
}
```

**Output**:
```
Int addition: 15
Double addition: 16
```

---

### [4.4.1.2 Operator Overloading](#4412-operator-overloading)
Operator overloading allows customizing the behavior of operators for user-defined types.

**Example**:
```cpp
#include <iostream>
using namespace std;

class Complex {
    int real, imag;

public:
    Complex(int r = 0, int i = 0) : real(r), imag(i) {}

    Complex operator+(const Complex& other) {
        return Complex(real + other.real, imag + other.imag);
    }

    void display() {
        cout << real << " + " << imag << "i" << endl;
    }
};

int main() {
    Complex c1(3, 4), c2(1, 2);
    Complex c3 = c1 + c2; // Using overloaded '+' operator
    c3.display();
    return 0;
}
```

**Output**:
```
4 + 6i
```

---

## [4.4.2 Run-Time Polymorphism](#442-run-time-polymorphism)

### [4.4.2.1 Virtual Functions](#4421-virtual-functions)
A virtual function allows a derived class to override a function in the base class, enabling dynamic binding.

**Example**:
```cpp
#include <iostream>
using namespace std;

class Base {
public:
    virtual void display() {
        cout << "Display from Base" << endl;
    }
};

class Derived : public Base {
public:
    void display() override {
        cout << "Display from Derived" << endl;
    }
};

int main() {
    Base* basePtr;
    Derived derivedObj;

    basePtr = &derivedObj;
    basePtr->display(); // Calls Derived's display due to virtual function
    return 0;
}
```

**Output**:
```
Display from Derived
```

In this example:
- The `display()` function is **virtual** in the base class. 
- The function call `basePtr->display()` dynamically binds to the derived class version of `display()` at runtime, even though the type of the pointer `basePtr` is `Base*`.

---

### [4.4.2.2 Function Overriding](#4422-function-overriding)
Function overriding occurs when a derived class provides its own implementation of a base class method with the same signature.

**Example**:
```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void sound() {   // Virtual function in base class
        cout << "Animal makes a sound" << endl;
    }
};

class Dog : public Animal {
public:
    void sound() override {  // Override the base class function
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* animalPtr;
    Dog dogObj;

    animalPtr = &dogObj;
    animalPtr->sound(); // Calls Dog's sound() function due to function overriding
    return 0;
}
```

**Output**:
```
Dog barks
```

In this example:
- The `sound()` function in `Animal` is virtual, and it is overridden by the `Dog` class.
- The call to `animalPtr->sound()` dynamically binds to the `Dog` class's `sound()` function at runtime, demonstrating the power of **function overriding**.

---

## Key Points About Polymorphism

1. **[Compile-Time Polymorphism](#441-compile-time-polymorphism)**:
   - Offers performance benefits as decisions are made at compile time.
   - Limited flexibility compared to runtime polymorphism.

2. **[Run-Time Polymorphism](#442-run-time-polymorphism)**:
   - Provides flexibility and extensibility.
   - Requires virtual functions, introducing a slight performance overhead.

3. **[Virtual Functions](#4421-virtual-functions)**:
   - Always define the destructor as `virtual` in a base class if it has virtual functions to avoid resource leaks.

4. **[Use Cases](#4422-function-overriding)**:
   - **Compile-time polymorphism** is ideal for scenarios where functionality is fixed and does not need to change dynamically.
   - **Run-time polymorphism** is best suited for scenarios requiring dynamic behavior based on object types, enabling greater flexibility and extendibility.

Polymorphism enhances code readability, maintainability, and flexibility, making it a powerful tool in C++ programming.

---
